# Atores e Partes Interessadas

## 1. Objetivo

Identificar quem interage com os módulos do Tipo de Empresa Escola e separar função organizacional, identidade de plataforma, papel empresarial, autoridade modular e futuras permissões funcionais.

Este documento não define schema nem implementação técnica de permissões.

## 2. Dimensões de identidade e autorização

A modelagem respeita as dimensões existentes no FractawModules:

```text
Cargo
→ função organizacional exercida no contexto do TipoEmpresa

Papel empresarial
→ PROPRIETARIO / ADMINISTRADOR_GERAL / MEMBRO

Autoridade modular
→ autoridade efetiva dentro de um módulo habilitado e aplicável

Permissão funcional
→ operação concreta, quando requisitos futuros exigirem granularidade adicional
```

Essas dimensões não são sinônimas.

### Cargo no contexto Escola

Diretor, Coordenador e outras funções puramente organizacionais devem ser tratados conceitualmente como Cargos do TipoEmpresa Escola, salvo quando um requisito futuro demonstrar a necessidade de entidade de domínio independente.

Professor exige uma distinção adicional:

```text
Professor estrutural
≠ Cargo Professor
```

A referência Professor existe no domínio mesmo sem Usuario ou EmpresaUsuario.

### Contrato atual de autoridade modular

Depois de satisfeitos vínculo ativo, aplicabilidade e habilitação do módulo:

```text
PROPRIETARIO
→ ADMINISTRADOR no módulo

ADMINISTRADOR_GERAL
→ ADMINISTRADOR no módulo

MEMBRO
→ depende de concessão modular
   ├── USUARIO
   └── ADMINISTRADOR
```

Cargo não cria nem substitui essa autoridade.

Permissões mais granulares dentro dos módulos escolares ainda não foram definidas.

## 3. Atores principais

### AT-001 — Professor

Pessoa representada pela referência estrutural Professor e que, quando possui acesso compatível ao Fractaw, atua sobre sua própria disponibilidade.

Responsabilidades principais nesta modelagem:

- consultar sua disponibilidade;
- informar sua disponibilidade;
- alterar sua própria disponibilidade quando o processo permitir.

O Professor não é responsável, no fluxo normal, por montar ou gerar a grade institucional.

Quando um docente possui acesso ao sistema, pode existir associação entre sua referência Professor e um EmpresaUsuario, possivelmente com Cargo Professor.

A forma de persistir `Professor ↔ EmpresaUsuario` permanece em aberto.

### AT-002 — Responsável pela Gestão de Horários

Ator que possui autoridade suficiente para operar Gestão de Horários no contexto da Empresa.

Pode ser, por exemplo:

- Proprietário;
- Administrador Geral;
- Membro com concessão modular adequada.

Organizacionalmente, esse usuário pode possuir Cargo Diretor, Coordenador ou outro Cargo compatível com o TipoEmpresa Escola.

Responsabilidades candidatas:

- iniciar geração de horários;
- revisar resultados;
- analisar exceções;
- solicitar ou executar remontagem;
- publicar resultados, se esse caso de uso for confirmado;
- realizar outras operações do módulo confirmadas pelos requisitos.

A granularidade final dessas operações permanece em aberto. A autoridade modular atual não deve ser silenciosamente convertida em um catálogo de permissões que o FractawModules ainda não definiu.

### AT-003 — Diretor

Ator de negócio que exerce função organizacional de direção escolar.

Nesta modelagem, Diretor é compatível conceitualmente com Cargo do TipoEmpresa Escola.

Diretor:

- não é sinônimo de `PROPRIETARIO`;
- não é sinônimo de `ADMINISTRADOR_GERAL`;
- pode possuir papel empresarial `MEMBRO`;
- não recebe autorização apenas por possuir Cargo Diretor.

Um Diretor `MEMBRO` precisa satisfazer o contrato modular aplicável, inclusive concessão quando necessária.

Um Diretor que também seja `PROPRIETARIO` ou `ADMINISTRADOR_GERAL` segue a autoridade modular derivada vigente desses papéis.

## 4. Atores secundários potenciais

### AT-004 — Fonte externa de disponibilidade

Sistema ou processo externo capaz de fornecer informações de disponibilidade em formato compatível com Gestão de Horários.

Sua existência é relevante porque Gestão de Horários não deve depender obrigatoriamente do módulo Disponibilidade.

Os contratos e formatos dessa integração ainda não estão definidos.

## 5. Partes interessadas

### Empresa Escola

Responsável institucional pelos dados, políticas e processos executados no contexto da Empresa.

### Responsáveis pelo planejamento acadêmico

Pessoas interessadas na validade, qualidade e viabilidade da grade, mesmo quando não operam diretamente o sistema.

### Professores

Além de atores do módulo Disponibilidade, são diretamente afetados pelas decisões de planejamento e alocação.

## 6. Regras iniciais de responsabilidade

### RA-001

O Professor é responsável por informar sua própria disponibilidade no fluxo normal.

### RA-002

Gestão de Horários somente pode ser utilizada quando o módulo for aplicável e estiver efetivamente habilitado para a Empresa.

### RA-003

Um `MEMBRO` depende de concessão modular para obter autoridade efetiva em Gestão de Horários ou Disponibilidade quando esses módulos utilizarem o contrato modular habilitável.

### RA-004

`PROPRIETARIO` e `ADMINISTRADOR_GERAL` seguem o contrato vigente de autoridade modular derivada `ADMINISTRADOR` nos módulos habilitados e aplicáveis.

### RA-005

Cargo não concede autorização. Regras do tipo “é Diretor, então pode gerar horários” não devem substituir o contrato de autoridade.

### RA-006

Autoridade modular administrativa e permissão para uma operação funcional concreta não devem ser tratadas como conceitos necessariamente idênticos antes da definição dos requisitos de permissões granulares.

## 7. Questões em aberto

### QA-001 — Administração excepcional da disponibilidade

Deve existir uma operação administrativa para registrar ou corrigir disponibilidade em nome de um Professor? Em quais situações?

### QA-002 — Granularidade das permissões funcionais

Gestão de Horários precisará distinguir permissões como gerar, revisar, remontar, publicar e tratar exceções?

### QA-003 — Efeito da autoridade ADMINISTRADOR

Se permissões funcionais granulares forem introduzidas, `PROPRIETARIO`, `ADMINISTRADOR_GERAL` e Membros com autoridade modular `ADMINISTRADOR` receberão todas essas operações automaticamente ou apenas autoridade administrativa para gerenciá-las?

### QA-004 — Cargos escolares

Quais Cargos, além de Professor, Diretor e Coordenador, são realmente necessários ao TipoEmpresa Escola?

### QA-005 — Professor e acesso

Como a associação entre Professor estrutural e EmpresaUsuario será representada sem tornar os conceitos equivalentes?