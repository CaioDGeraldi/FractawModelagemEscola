# Visão Geral — Gestão de Horários

## 1. Propósito

Modelar o módulo escolar responsável pelo planejamento e gestão da grade de horários.

A geração automática é uma responsabilidade central do módulo, mas não representa todo o seu escopo.

## 2. Problema

A elaboração de horários escolares exige conciliar professores, turmas, disciplinas, carga horária, ambientes e diferentes restrições de disponibilidade e alocação.

Quando o planejamento é feito manualmente ou com informações dispersas, conflitos podem passar despercebidos e alterações podem gerar novos problemas em outras partes da grade.

O problema central é construir e manter uma grade válida sem perder o controle das restrições envolvidas.

## 3. Objetivo geral

Permitir o planejamento, a geração, a revisão e a remontagem de grades de horários escolares, considerando referências estruturais, políticas institucionais e informações de disponibilidade.

## 4. Objetivos específicos

### OBJ-GH-001

Considerar conjuntamente professores, turmas, disciplinas, horários, ambientes e demais elementos relevantes.

### OBJ-GH-002

Reduzir conflitos de alocação entre professores, turmas, horários e espaços físicos.

### OBJ-GH-003

Considerar restrições de disponibilidade, infraestrutura, deslocamento e regras institucionais.

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

## 5. Entradas conceituais

Gestão de Horários pode consumir:

- referências estruturais da Escola;
- políticas vigentes fornecidas por Parâmetros;
- disponibilidade docente;
- demandas de aula;
- informações de ambientes e recursos;
- dados de deslocamento quando aplicáveis.

Consumir uma informação não transfere sua propriedade para Gestão de Horários.

## 6. Disponibilidade

Quando o módulo Disponibilidade estiver disponível, Gestão de Horários deve poder consumir seus dados.

Na ausência dele, a gestão da grade deve continuar utilizável a partir de outra fonte compatível de disponibilidade.

## 7. Políticas

Gestão de Horários aplica políticas institucionais, mas não é proprietária das políticas compartilhadas.

Por exemplo, o módulo pode utilizar a prioridade informada pela Disponibilidade e consultar a política vigente para decidir o peso ou bloqueio daquela restrição.

## 8. Exceções

Quando uma situação impedir uma alocação e não houver solução automática possível, deve ser registrada uma exceção de planejamento.

A exceção não deve encerrar toda a geração.

O processo continua para as demais alocações possíveis e apresenta ao responsável:

- a alocação afetada;
- o motivo do impedimento;
- o contexto necessário para intervenção humana.

## 9. Intervenção humana

Negociação com professores, alteração de disponibilidade ou outra decisão externa não deve ser mascarada como solução automática.

Após a mudança das condições, uma nova geração ou remontagem pode ocorrer.

## 10. Ownership

Pertencem a Gestão de Horários, quando confirmados pela modelagem:

- demandas de planejamento;
- tentativas de geração;
- alocações produzidas;
- conflitos detectados;
- exceções de planejamento;
- resultados e versões da grade.

Não pertencem ao módulo:

- Professor, Turma, Disciplina e demais referências estruturais;
- política institucional de prioridades;
- disponibilidade docente original.

## 11. Questões em aberto

### QGH-001 — Remontagem

A remontagem deve preservar o máximo possível da grade anterior ou pode reorganizar livremente todas as alocações?

### QGH-002 — Efeito de alterações externas

Como uma alteração em disponibilidade, referência estrutural ou política vigente afeta uma grade já produzida?

### QGH-003 — Histórico e publicação

Quais estados precisam ser versionados, publicados ou congelados para garantir rastreabilidade e reproduzibilidade?

### QGH-004 — Resultado da geração

Quais estados formais uma tentativa de geração pode assumir e como distinguir falha de validação, inviabilidade comprovada e busca inconclusiva?
