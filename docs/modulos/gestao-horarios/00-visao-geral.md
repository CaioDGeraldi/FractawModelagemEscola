# Visão Geral — Gestão de Horários

## 1. Propósito

Modelar o módulo escolar responsável pelo planejamento e gestão da grade de horários.

A geração automática é uma responsabilidade central do módulo, mas não representa todo o seu escopo.

## 2. Problema

A elaboração de horários escolares exige conciliar Professores, Turmas, Disciplinas, carga horária, Ambientes, Blocos de Aula, deslocamentos e diferentes restrições de disponibilidade e alocação.

Quando o planejamento é feito manualmente ou com informações dispersas, conflitos podem passar despercebidos e alterações podem gerar novos problemas em outras partes da grade.

O problema central é construir e manter uma grade válida sem perder o controle das restrições envolvidas.

## 3. Objetivo geral

Permitir o planejamento, a geração, a revisão e a remontagem de grades de horários escolares, considerando referências estruturais, políticas institucionais e informações de disponibilidade.

## 4. Modularidade e autoridade

Gestão de Horários deve respeitar o contrato vigente do FractawModules:

```text
aplicável ao TipoEmpresa Escola
≠ habilitado para determinada Empresa
≠ concedido a determinado MEMBRO
```

Ser aplicável ao TipoEmpresa Escola ou existir no código não habilita o módulo automaticamente para uma Empresa.

Depois dos gates de vínculo ativo, aplicabilidade e habilitação, o contrato atual de autoridade modular é:

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

Cargo não concede essa autoridade.

Um usuário com Cargo Diretor ou Coordenador, por exemplo, somente obtém autoridade como Membro quando existir a concessão modular necessária. Se o mesmo vínculo for Proprietário ou Administrador Geral, aplica-se a autoridade modular derivada desses papéis.

A modelagem ainda não define permissões funcionais granulares dentro de Gestão de Horários.

## 5. Objetivos específicos

### OBJ-GH-001

Considerar conjuntamente Professores, Turmas, Disciplinas, horários, Ambientes e demais elementos relevantes.

### OBJ-GH-002

Reduzir conflitos de alocação entre Professores, Turmas, horários e espaços físicos.

### OBJ-GH-003

Considerar restrições de disponibilidade, infraestrutura, deslocamento, Interstício e demais regras institucionais.

### OBJ-GH-004

Identificar os motivos que impedem ou dificultam uma alocação.

### OBJ-GH-005

Permitir revisão e remontagem sem perder a possibilidade de verificar consistência.

### OBJ-GH-006

Gerar automaticamente a grade a partir das entradas disponíveis.

### OBJ-GH-007

Registrar exceções que impeçam uma alocação sem interromper o processamento das demais alocações possíveis.

### OBJ-GH-008

Quando necessário, concluir com uma grade parcial acompanhada das situações que exigem intervenção humana.

### OBJ-GH-009

Permitir aulas com um ou mais Professores quando a Oferta e as regras acadêmicas exigirem co-docência.

## 6. Entradas conceituais

Gestão de Horários pode consumir:

- referências estruturais da Escola;
- Oferta de Disciplina;
- Professores habilitados e/ou atribuídos;
- políticas vigentes fornecidas por Parâmetros;
- disponibilidade docente proveniente de fonte compatível;
- Blocos de Aula concretos e demais elementos da organização temporal;
- Ambientes e recursos;
- relações estruturais de Deslocamento entre Sites e seus tempos específicos;
- Período Letivo;
- demais demandas necessárias ao planejamento.

Consumir uma informação não transfere sua propriedade para Gestão de Horários.

## 7. Disponibilidade

Quando o módulo Disponibilidade estiver efetivamente habilitado e disponível no contexto da Empresa, Gestão de Horários deve poder consumir seus dados por contrato compatível.

Na ausência desse módulo, a gestão da grade deve continuar utilizável a partir de outra fonte compatível de disponibilidade.

Portanto, não existe dependência obrigatória:

```text
Gestão de Horários → Disponibilidade
```

A integração é uma possibilidade, não requisito de existência do módulo.

## 8. Políticas e restrições

Gestão de Horários aplica políticas institucionais, mas não é proprietária das políticas compartilhadas.

Entre os candidatos já identificados estão:

- interpretação de prioridade de ausência;
- prioridade mínima bloqueante;
- duração padrão de aula;
- máximo de aulas consecutivas;
- Interstício institucional;
- margem geral de deslocamento;
- outros limites confirmados pelos requisitos.

O módulo deve distinguir política/default de referência ou ocorrência concreta.

Exemplo:

```text
Bloco concreto:                 07:00–07:50
Deslocamento estrutural A → B:  35 min
Margem geral de deslocamento:   10 min
Interstício aplicável:          conforme política
```

A duração real da aula alocada decorre do Bloco concreto, ainda que uma duração padrão possa orientar a organização temporal.

## 9. Co-docência

Uma alocação pode possuir mais de um Professor.

Quando isso ocorrer, todos os Professores participantes devem simultaneamente satisfazer:

- disponibilidade;
- habilitação ou vínculo acadêmico aplicável;
- ausência de conflito com outra alocação;
- restrições de deslocamento entre Sites;
- Interstício e demais políticas aplicáveis.

A existência de dois Professores atribuídos à Oferta não obriga que todas as aulas utilizem os dois, salvo quando os requisitos da Oferta determinarem isso.

## 10. Operações e permissões funcionais

Gestão de Horários pode vir a possuir operações funcionalmente distintas, como:

- gerar grade;
- revisar resultado;
- tratar exceções;
- remontar grade;
- publicar uma versão, caso publicação seja confirmada;
- outras operações futuras.

A necessidade dessa granularidade é real para a modelagem, mas o FractawModules ainda não definiu um catálogo ou schema de permissões granulares.

Portanto, permanece em aberto:

- quais operações exigirão permissão funcional própria;
- se autoridade modular `USUARIO` será suficiente para alguma delas;
- quais operações exigirão `ADMINISTRADOR`;
- se Proprietário e Administrador Geral, por possuírem `ADMINISTRADOR` derivado, receberão automaticamente todas as futuras operações funcionais ou apenas autoridade administrativa suficiente para gerenciá-las.

Este documento não antecipa essa decisão.

## 11. Exceções

Quando uma situação impedir uma alocação e não houver solução automática possível, deve ser registrada uma exceção de planejamento.

A exceção não encerra toda a geração.

O processo continua para as demais alocações possíveis e apresenta ao responsável:

- a alocação afetada;
- o motivo do impedimento;
- o contexto necessário para intervenção humana.

## 12. Intervenção humana

Negociação com Professores, alteração de disponibilidade ou outra decisão externa não deve ser mascarada como solução automática.

Após a mudança das condições, uma nova geração ou remontagem pode ocorrer.

## 13. Ownership

Pertencem a Gestão de Horários, quando confirmados pela modelagem:

- demandas de planejamento;
- tentativas de geração;
- alocações produzidas;
- Professores participantes de cada alocação;
- conflitos detectados;
- exceções de planejamento;
- resultados e versões da grade.

Não pertencem ao módulo:

- Professor, Turma, Disciplina e demais referências estruturais;
- Cargo e identidade de acesso;
- autoridade modular e concessões da plataforma;
- política institucional de prioridades, Interstício ou margem;
- disponibilidade docente original;
- Blocos, Sites, Ambientes e relações de Deslocamento estruturais.

## 14. Questões em aberto

### QGH-001 — Remontagem

A remontagem deve preservar o máximo possível da grade anterior ou pode reorganizar livremente todas as alocações?

### QGH-002 — Efeito de alterações externas

Como uma alteração em disponibilidade, referência estrutural ou política vigente afeta uma grade já produzida?

### QGH-003 — Histórico e publicação

Quais estados precisam ser versionados, publicados ou congelados para garantir rastreabilidade e reproduzibilidade?

### QGH-004 — Resultado da geração

Quais estados formais uma tentativa de geração pode assumir e como distinguir falha de validação, inviabilidade comprovada e busca inconclusiva?

### QGH-005 — Co-docência

Como a Oferta determina se todos os Professores atribuídos devem participar juntos ou se a participação pode variar entre as aulas?

### QGH-006 — Permissões granulares

Quais operações precisarão de permissão funcional própria além da autoridade modular mínima vigente?

### QGH-007 — Autoridade administrativa e operação concreta

Como futuras permissões funcionais se relacionarão com a autoridade modular `ADMINISTRADOR` de Membros e com a autoridade derivada de Proprietário e Administrador Geral?