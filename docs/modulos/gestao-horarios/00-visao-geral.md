# Visão Geral — Gestão de Horários

## 1. Propósito

Modelar o módulo escolar responsável pelo planejamento e gestão da grade de horários.

A geração automática é uma responsabilidade central do módulo, mas não representa todo o seu escopo.

## 2. Problema

A elaboração de horários escolares exige conciliar professores, turmas, disciplinas, carga horária, ambientes, tempos de aula, deslocamentos e diferentes restrições de disponibilidade e alocação.

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

Considerar restrições de disponibilidade, infraestrutura, deslocamento, interstício e demais regras institucionais.

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

## 5. Entradas conceituais

Gestão de Horários pode consumir:

- referências estruturais da Escola;
- Oferta de Disciplina;
- Professores habilitados e/ou atribuídos;
- políticas vigentes fornecidas por Parâmetros;
- disponibilidade docente;
- Blocos de Aula e Intervalos;
- Ambientes e recursos;
- tempos de Deslocamento entre Sites;
- Período Letivo;
- demais demandas necessárias ao planejamento.

Consumir uma informação não transfere sua propriedade para Gestão de Horários.

## 6. Disponibilidade

Quando o módulo Disponibilidade estiver disponível, Gestão de Horários deve poder consumir seus dados.

Na ausência dele, a gestão da grade deve continuar utilizável a partir de outra fonte compatível de disponibilidade.

## 7. Políticas e restrições temporais

Gestão de Horários aplica políticas institucionais, mas não é proprietária das políticas compartilhadas.

Entre elas podem estar:

- interpretação de prioridade de ausência;
- prioridade mínima bloqueante;
- máximo de aulas consecutivas;
- Interstício mínimo;
- margem de deslocamento;
- outros limites institucionais.

O módulo deve distinguir os dados estruturais das políticas.

Exemplo:

```text
Bloco real:            07:00–07:50
Deslocamento A → B:    35 min
Margem configurada:    10 min
Interstício aplicável: conforme política
```

A duração real da aula é determinada pelo Bloco alocado, não por uma constante interna do módulo.

## 8. Co-docência

Uma alocação pode possuir mais de um Professor.

Quando isso ocorrer, todos os Professores participantes devem simultaneamente satisfazer:

- disponibilidade;
- habilitação ou vínculo acadêmico aplicável;
- ausência de conflito com outra alocação;
- deslocamento entre Sites;
- Interstício e demais políticas aplicáveis.

A existência de dois Professores atribuídos à Oferta não obriga que todas as aulas utilizem os dois, salvo quando os requisitos da Oferta determinarem isso.

## 9. Exceções

Quando uma situação impedir uma alocação e não houver solução automática possível, deve ser registrada uma exceção de planejamento.

A exceção não encerra toda a geração.

O processo continua para as demais alocações possíveis e apresenta ao responsável:

- a alocação afetada;
- o motivo do impedimento;
- o contexto necessário para intervenção humana.

## 10. Intervenção humana

Negociação com professores, alteração de disponibilidade ou outra decisão externa não deve ser mascarada como solução automática.

Após a mudança das condições, uma nova geração ou remontagem pode ocorrer.

## 11. Ownership

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
- política institucional de prioridades, Interstício ou margem;
- disponibilidade docente original;
- Blocos, Sites, Ambientes e Deslocamentos estruturais.

## 12. Questões em aberto

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
