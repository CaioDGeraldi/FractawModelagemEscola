# Visão Geral do Sistema

## 1. Propósito do documento

Descrever o problema de planejamento de horários escolares, os objetivos do sistema, seu escopo e os limites considerados nesta modelagem.

Este repositório modela o domínio escolar que será incorporado ao FractawModules, mas não define antecipadamente o mecanismo técnico de composição por Tipo de Empresa.

---

## 2. Problema

A elaboração de horários escolares exige conciliar professores, turmas, disciplinas, carga horária, ambientes e diferentes restrições de disponibilidade e alocação.

Quando esse planejamento é feito manualmente ou com informações dispersas, conflitos podem passar despercebidos e alterações em um horário podem gerar novos problemas em outras partes da grade. A complexidade aumenta quando existem laboratórios, mais de uma unidade, restrições contratuais ou legais e professores com disponibilidade limitada.

O problema central é construir e manter uma grade de horários válida sem perder o controle das restrições envolvidas.

---

## 3. Objetivo geral

Gerar automaticamente uma grade de horários escolares que considere as restrições da instituição, identifique situações que não possam ser resolvidas automaticamente e permita revisar e remontar a grade quando necessário.

---

## 4. Objetivos específicos

### OBJ-001

Considerar de forma conjunta as informações de professores, turmas, disciplinas, horários, ambientes e demais elementos relevantes para o planejamento da grade.

### OBJ-002

Reduzir conflitos de alocação entre professores, turmas, horários e espaços físicos.

### OBJ-003

Permitir que restrições de disponibilidade, infraestrutura, deslocamento e regras institucionais sejam consideradas durante o planejamento.

### OBJ-004

Facilitar a identificação dos motivos que impedem ou dificultam uma determinada alocação.

### OBJ-005

Permitir que a grade seja revisada e ajustada sem perder a possibilidade de verificar sua consistência.

### OBJ-006

Gerar automaticamente a grade de horários a partir das informações e restrições disponíveis.

### OBJ-007

Registrar e apresentar obrigatoriamente as exceções que impeçam uma alocação durante a geração, sem interromper o processamento das demais alocações possíveis.

### OBJ-008

Entregar, quando houver exceções não resolvidas, uma grade parcial acompanhada das situações que exigem intervenção humana.

---

## 5. Escopo

### 5.1 Dentro do escopo

- professores e suas disponibilidades;
- turmas e suas necessidades de horário;
- disciplinas e suas cargas horárias;
- associação entre professores, turmas e disciplinas;
- períodos, turnos e blocos de horário;
- unidades escolares e ambientes físicos;
- laboratórios e demais recursos necessários para determinadas aulas;
- restrições contratuais, legais e institucionais que afetem a alocação;
- preferências de horário quando forem relevantes para o planejamento;
- deslocamento entre unidades;
- cadastro semestral de disponibilidade dos professores;
- edição do cadastro de disponibilidade durante o semestre;
- indicação de prioridade de ausência para períodos específicos;
- identificação das políticas escolares que devem ser configuráveis por Empresa;
- geração automática da grade de horários;
- criação, revisão e remontagem da grade;
- identificação de conflitos de alocação;
- identificação e registro de exceções não resolvidas automaticamente;
- continuidade da geração mesmo quando uma exceção impede uma alocação específica;
- validação da consistência da grade;
- apresentação dos motivos que impedem ou dificultam uma alocação;
- apresentação de uma grade parcial quando existirem alocações que dependam de intervenção humana.

### 5.2 Fora do escopo

- matrícula de alunos;
- controle de frequência;
- lançamento e cálculo de notas;
- emissão de boletins;
- gestão financeira da instituição;
- mensalidades e cobranças;
- folha de pagamento;
- biblioteca;
- ambiente virtual de aprendizagem;
- conteúdo e planejamento pedagógico das aulas;
- comunicação com responsáveis e alunos;
- gestão completa de recursos humanos;
- definição do mecanismo técnico de composição dos módulos pelo Tipo de Empresa;
- implementação interna da plataforma Fractaw;
- Catálogo de Produtos da plataforma.

---

## 6. Fronteira do domínio

O domínio de horários escolares pertence ao contexto do Tipo de Empresa Escola no FractawModules.

A modelagem distingue quatro responsabilidades: referências estruturais da Escola, Parâmetros, Cadastro de Disponibilidade e Geração de Horários.

### Referências estruturais da Escola

Conceitos com identidade própria no domínio escolar, utilizados como referência por processos e históricos, pertencem ao domínio do Tipo de Empresa Escola.

Exemplos candidatos incluem Professor, Disciplina, Curso, Turma, Site, Sala, Laboratório, Período Letivo e Bloco de Aula.

A classificação definitiva de cada conceito deve ser realizada durante a modelagem. Um conceito não deve ser classificado como Parâmetro apenas por ser configurável.

O Catálogo de Produtos da plataforma não é responsável por essas referências escolares.

### Parâmetros

Parâmetros é uma capacidade-base genérica do FractawModules destinada a políticas e configurações empresariais compartilháveis.

No contexto Escola, este repositório deve identificar quais políticas precisam ser configuráveis por Empresa e consumidas pelos módulos escolares.

O Tipo de Empresa Escola define a semântica e a aplicabilidade desses parâmetros; a Empresa mantém os valores efetivamente vigentes.

Exemplos claros de candidatos a Parâmetros são políticas como:

- interpretação da escala de prioridade de ausência;
- prioridade mínima considerada bloqueante;
- limites institucionais de alocação;
- margens ou tolerâncias gerais utilizadas no planejamento.

Conceitos estruturais com identidade própria não devem ser movidos para Parâmetros apenas por possuírem valores configuráveis.

Parâmetros não armazena disponibilidade individual, tentativas de geração, grades, conflitos ou exceções.

### Cadastro de Disponibilidade

Responsável por registrar e manter informações de disponibilidade docente para um período de vigência.

O módulo deve poder ser utilizado de forma independente da Geração de Horários e seus dados devem poder alimentar outros sistemas.

O cadastro deve permitir atribuir prioridade de ausência aos períodos aplicáveis.

A prioridade utiliza uma escala de 1 a 10. Seu significado operacional é determinado pela política vigente da Empresa.

Como política padrão de referência para esta modelagem, prioridades de 1 a 8 são negociáveis com peso crescente, enquanto 9 e 10 são bloqueantes. A Empresa pode utilizar outra política quando o contexto definido para Escola permitir essa configuração.

### Geração de Horários

Responsável por utilizar referências escolares, políticas vigentes e informações de disponibilidade para gerar automaticamente a grade de horários.

Quando o Cadastro de Disponibilidade estiver disponível, seus dados devem poder ser consumidos diretamente. Na ausência desse módulo, a Geração de Horários deve continuar sendo utilizável a partir de uma fonte compatível de disponibilidade.

A Geração de Horários aplica políticas, mas não é proprietária das políticas compartilhadas.

Quando uma situação impedir uma alocação e não houver solução automática possível, deve ser registrada uma exceção de planejamento. A exceção não encerra a geração: as demais alocações possíveis continuam sendo processadas.

Ao término, todas as exceções devem ser apresentadas ao responsável pela montagem dos horários com informação suficiente para identificar a alocação afetada e o motivo do impedimento.

Situações que dependam de negociação, alteração de disponibilidade ou outra decisão externa permanecem sob responsabilidade humana. Após a alteração das condições, a grade pode ser remontada ou gerada novamente.

---

## 7. Relação com o FractawModules

Esta modelagem assume as seguintes fronteiras arquiteturais já decididas no FractawModules:

- `Empresa` é o tenant;
- `TipoEmpresa` define contexto e aplicabilidade de capacidades, mas não executa regras de domínio;
- a plataforma não deve depender do código específico de Escola;
- código específico de Escola pode depender de contratos públicos da plataforma;
- Parâmetros é capacidade-base genérica;
- o Catálogo de Produtos permanece uma capacidade neutra própria da plataforma e não se torna catálogo universal da Escola;
- Disponibilidade e Geração de Horários são responsabilidades operacionais distintas;
- o mecanismo concreto de composição por `TipoEmpresa` em runtime permanece fora do escopo deste repositório.

---

## 8. Premissas

### PRE-001

As referências estruturais necessárias ao planejamento existem no domínio do Tipo de Empresa Escola ou são disponibilizadas por contratos compatíveis antes da geração.

### PRE-002

A disponibilidade dos professores possui período de vigência e pode ser alterada quando permitido pelo processo definido.

### PRE-003

A Geração de Horários deve poder consumir disponibilidade proveniente do Cadastro de Disponibilidade ou de outra fonte compatível.

### PRE-004

A geração pode ser concluída mesmo quando uma ou mais alocações não puderem ser resolvidas automaticamente.

### PRE-005

Toda situação não resolvida automaticamente que impeça uma alocação deve permanecer visível para intervenção humana.

### PRE-006

Após uma negociação ou alteração das condições de disponibilidade, uma nova geração ou remontagem da grade pode ser realizada.

### PRE-007

Cada Empresa pode possuir valores próprios para políticas escolares configuráveis.

### PRE-008

Na ausência de configuração específica para a política de prioridades, a referência inicial desta modelagem considera 9 e 10 como bloqueantes e 1 a 8 como negociáveis com peso crescente.

### PRE-009

Cadastro de Disponibilidade e Geração de Horários devem permanecer funcionais de forma independente entre si.

### PRE-010

Parâmetros é uma capacidade-base genérica do FractawModules; no contexto Escola, fornece políticas e configurações aplicáveis ao domínio escolar.

### PRE-011

Quando módulos compatíveis estiverem disponíveis em conjunto, eles devem poder compartilhar informações por contratos explícitos sem transferir a propriedade dos dados entre domínios.

### PRE-012

Referências estruturais, políticas e estado operacional são categorias distintas e devem ser classificadas pela natureza do conceito, não pela conveniência de implementação.

### PRE-013

O Tipo de Empresa fornece contexto e aplicabilidade, mas não deve ser tratado por esta modelagem como executor de regras ou concentrador da lógica dos módulos.

---

## 9. Questões em aberto

### Q-001 — Unidade da prioridade

A prioridade é atribuída a um dia inteiro, a um bloco de horário específico ou pode ser utilizada nos dois níveis?

### Q-002 — Alteração durante o período vigente

Quando uma disponibilidade já utilizada em uma grade for alterada, a grade atual deve apenas ser marcada para revisão ou deve existir alguma ação imediata sobre ela?

### Q-003 — Remontagem

A remontagem deve tentar preservar o máximo possível da grade anterior ou uma nova geração pode reorganizar livremente todas as alocações?

### Q-004 — Classificação de conceitos estruturais

Quais conceitos escolares possuem identidade própria e devem permanecer como referências do domínio Escola, e quais representam efetivamente políticas configuráveis em Parâmetros?

---

## 10. Critérios de conclusão desta etapa

A visão geral pode ser considerada suficientemente definida quando:

- o problema estiver descrito sem depender de solução técnica;
- o objetivo geral estiver claro;
- os objetivos específicos forem coerentes com o problema;
- o escopo estiver delimitado;
- os principais itens fora de escopo estiverem explícitos;
- as fronteiras entre referências escolares, Parâmetros, Disponibilidade e Geração de Horários estiverem claras;
- a modelagem não tratar o Catálogo de Produtos como catálogo universal da Escola;
- a modelagem não atribuir ao Tipo de Empresa a execução das regras dos módulos;
- as premissas principais estiverem registradas;
- as questões que afetam diretamente as regras de geração estiverem identificadas.
