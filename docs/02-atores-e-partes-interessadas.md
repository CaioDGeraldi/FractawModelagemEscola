# Atores e Partes Interessadas

## 1. Objetivo

Identificar quem interage com os módulos do Tipo de Empresa Escola e separar função de negócio, identidade de plataforma e autorização funcional.

Este documento não define a implementação técnica das permissões.

## 2. Princípio de autorização

A modelagem adota três dimensões independentes:

```text
Papel empresarial
→ PROPRIETARIO / ADMINISTRADOR_GERAL / MEMBRO

Função escolar
→ Diretor / Professor / Coordenador / outras

Autorização funcional
→ capacidades específicas dos módulos
```

Nenhuma dessas dimensões deve ser usada como sinônimo das outras.

Um usuário com papel empresarial `MEMBRO` pode, por exemplo, possuir autorização para gerenciar horários.

Da mesma forma, possuir papel `PROPRIETARIO` ou `ADMINISTRADOR_GERAL` não implica automaticamente autorização funcional sobre Gestão de Horários.

## 3. Atores principais

### AT-001 — Professor

Pessoa representada pela referência estrutural Professor e que, quando possui acesso ao Fractaw, atua sobre sua própria disponibilidade.

Responsabilidade principal nesta modelagem:

- consultar sua disponibilidade;
- informar sua disponibilidade;
- alterar sua própria disponibilidade quando o processo permitir.

O Professor não é responsável, no fluxo normal, por montar ou gerar a grade institucional.

A forma técnica de relacionar Professor, Usuario e EmpresaUsuario ainda não é definida por este repositório.

### AT-002 — Responsável pela Gestão de Horários

Usuário da Empresa com autorização funcional para operar o módulo Gestão de Horários.

Pode exercer uma função escolar como Diretor, Coordenador ou outra função definida pela instituição.

Responsabilidades candidatas:

- iniciar geração de horários;
- revisar resultados;
- analisar exceções;
- solicitar ou executar remontagem;
- realizar demais operações administrativas do módulo que venham a ser confirmadas pelos requisitos.

A autorização deriva da capacidade funcional concedida ao usuário, e não do nome de seu cargo ou papel empresarial.

### AT-003 — Diretor

Ator de negócio que exerce função de direção escolar.

Na modelagem atual, Diretor é um exemplo relevante de usuário que pode receber autorização para Gestão de Horários.

Entretanto:

- Diretor não é sinônimo de `PROPRIETARIO`;
- Diretor não é sinônimo de `ADMINISTRADOR_GERAL`;
- Diretor pode possuir papel empresarial `MEMBRO`;
- ser Diretor não concede automaticamente acesso a Gestão de Horários.

O ator Diretor só executa operações do módulo quando possuir a autorização funcional correspondente.

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

A Gestão de Horários é operada por usuários explicitamente autorizados para essa capacidade.

### RA-003

Papel empresarial não concede, por si só, autorização funcional para módulos escolares.

### RA-004

Função escolar não concede, por si só, autorização funcional.

### RA-005

A autorização deve expressar a capacidade necessária, evitando regras do tipo "é Diretor, então pode gerar horários".

## 7. Questões em aberto

### QA-001 — Administração excepcional da disponibilidade

Deve existir uma operação administrativa para registrar ou corrigir disponibilidade em nome de um Professor? Em quais situações?

### QA-002 — Granularidade das autorizações

Gestão de Horários terá uma autorização ampla ou capacidades separadas, como gerar, revisar, remontar e publicar?

### QA-003 — Outras funções escolares

Quais funções além de Diretor e Professor precisam ser reconhecidas pelo domínio Escola, mesmo que não impliquem permissões automáticas?
