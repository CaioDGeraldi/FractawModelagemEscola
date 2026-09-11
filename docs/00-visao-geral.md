# Visão Geral do Sistema

## 1. Propósito do documento

Descrever o problema de planejamento de horários escolares, os objetivos do sistema, seu escopo e os limites considerados nesta modelagem.

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
- tempo de deslocamento entre unidades;
- cadastro semestral de disponibilidade dos professores;
- edição do cadastro de disponibilidade durante o semestre;
- indicação de prioridade de ausência para períodos específicos;
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
- manutenção dos cadastros gerais compartilhados da empresa, tratados pelo Catálogo.

---

## 6. Fronteira do sistema

O domínio de horários escolares é dividido em duas responsabilidades principais:

### Cadastro de Disponibilidade

Responsável por registrar, para cada semestre, a disponibilidade dos professores utilizada no planejamento da grade.

O cadastro pode ser alterado durante o semestre e deve permitir atribuir uma prioridade de ausência a períodos específicos.

### Geração de Horários

Responsável por utilizar os dados acadêmicos e estruturais disponíveis, juntamente com o Cadastro de Disponibilidade, para gerar automaticamente a grade de horários.

Quando uma situação impedir uma alocação e não houver solução automática possível, essa situação deve ser registrada como uma exceção de planejamento. A exceção não deve encerrar a geração da grade: o sistema deve continuar processando as demais alocações possíveis e concluir o processo com os resultados obtidos.

Ao término da geração, todas as exceções devem ser apresentadas de forma explícita ao responsável pela montagem dos horários, incluindo informações suficientes para compreender qual alocação foi afetada e por que ela não pôde ser concluída.

A resolução de situações que dependam de negociação, mudança de disponibilidade ou decisão externa permanece sob responsabilidade humana. Após essas decisões, a disponibilidade pode ser alterada e a grade pode ser remontada ou gerada novamente.

### Catálogo

Os cadastros gerais utilizados pela escola, como professores, turmas, disciplinas, unidades e demais dados compartilhados da empresa, são fornecidos pelo Catálogo.

A modelagem deste repositório considera esses dados como entradas disponíveis e não cobre as regras internas de manutenção do Catálogo.

---

## 7. Premissas

### PRE-001

Os dados gerais necessários para a geração da grade estão previamente disponíveis no Catálogo.

### PRE-002

A disponibilidade dos professores é cadastrada por semestre e pode ser alterada quando necessário.

### PRE-003

A Geração de Horários utiliza o Cadastro de Disponibilidade vigente como uma de suas fontes obrigatórias de informação.

### PRE-004

A geração pode ser concluída mesmo quando uma ou mais alocações não puderem ser resolvidas automaticamente.

### PRE-005

Toda situação não resolvida automaticamente que impeça uma alocação deve permanecer visível para intervenção humana.

### PRE-006

Após uma negociação ou alteração das condições de disponibilidade, uma nova geração ou remontagem da grade pode ser realizada.

---

## 8. Questões em aberto

### Q-001 — Prioridade de ausência

Como a escala de 1 a 10 deve ser interpretada?

É necessário definir quais valores representam condições negociáveis e qual valor, ou faixa de valores, representa uma ausência que não pode ser violada.

### Q-002 — Unidade da prioridade

A prioridade é atribuída a um dia inteiro, a um bloco de horário específico ou pode ser utilizada nos dois níveis?

### Q-003 — Alteração durante o semestre

Quando uma disponibilidade já utilizada em uma grade for alterada, a grade atual deve apenas ser marcada para revisão ou deve existir alguma ação imediata sobre ela?

### Q-004 — Remontagem

A remontagem deve tentar preservar o máximo possível da grade anterior ou uma nova geração pode reorganizar livremente todas as alocações?

---

## 9. Critérios de conclusão desta etapa

A visão geral pode ser considerada suficientemente definida quando:

- o problema estiver descrito sem depender de solução técnica;
- o objetivo geral estiver claro;
- os objetivos específicos forem coerentes com o problema;
- o escopo estiver delimitado;
- os principais itens fora de escopo estiverem explícitos;
- a fronteira entre Catálogo, Cadastro de Disponibilidade e Geração de Horários estiver clara;
- as premissas principais estiverem registradas;
- as questões que afetam diretamente as regras de geração estiverem identificadas.
