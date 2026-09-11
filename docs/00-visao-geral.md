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
- configuração de políticas escolares compartilhadas;
- configuração, pela própria empresa, da política de interpretação das prioridades de ausência;
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

O domínio de horários escolares é dividido em três responsabilidades principais: Parâmetros Escolares, Cadastro de Disponibilidade e Geração de Horários.

Os módulos funcionais devem permanecer independentes entre si. Eles podem depender de módulos-base do domínio escolar e se integrar quando estiverem disponíveis em conjunto.

### Parâmetros Escolares

Responsável por manter políticas e configurações institucionais compartilhadas pelos módulos do tipo de empresa Escola.

Esse módulo define como a escola opera em aspectos que precisam ser compreendidos por mais de um módulo, como turnos, blocos de horário, políticas de prioridade, limites institucionais e regras gerais de deslocamento.

Parâmetros Escolares não deve armazenar ocorrências operacionais de outros módulos. Disponibilidades individuais pertencem ao Cadastro de Disponibilidade; resultados de geração, conflitos e exceções pertencem à Geração de Horários.

### Cadastro de Disponibilidade

Responsável por registrar, para cada semestre, a disponibilidade dos professores utilizada no planejamento da grade.

O módulo deve poder ser utilizado de forma independente da Geração de Horários e seus dados devem poder alimentar outros sistemas.

O cadastro pode ser alterado durante o semestre e deve permitir atribuir uma prioridade de ausência a períodos específicos.

A prioridade é registrada em uma escala de 1 a 10. O significado operacional dessa escala é definido pela política vigente da empresa em Parâmetros Escolares.

Na política padrão, as prioridades de 1 a 8 são tratadas como restrições negociáveis com peso crescente, enquanto as prioridades 9 e 10 são tratadas como proibições de alocação.

A política padrão não substitui uma política definida pela empresa.

### Geração de Horários

Responsável por utilizar os dados acadêmicos e estruturais disponíveis, as políticas escolares vigentes e as informações de disponibilidade para gerar automaticamente a grade de horários.

Quando o Cadastro de Disponibilidade estiver disponível, seus dados devem poder ser utilizados diretamente. Na ausência desse módulo, a Geração de Horários deve continuar sendo utilizável a partir de dados de disponibilidade obtidos por outro meio compatível.

A Geração de Horários aplica as políticas definidas em Parâmetros Escolares, mas não é responsável por definir essas políticas.

Quando uma situação impedir uma alocação e não houver solução automática possível, essa situação deve ser registrada como uma exceção de planejamento. A exceção não deve encerrar a geração da grade: o sistema deve continuar processando as demais alocações possíveis e concluir o processo com os resultados obtidos.

Ao término da geração, todas as exceções devem ser apresentadas de forma explícita ao responsável pela montagem dos horários, incluindo informações suficientes para compreender qual alocação foi afetada e por que ela não pôde ser concluída.

A resolução de situações que dependam de negociação, mudança de disponibilidade ou decisão externa permanece sob responsabilidade humana. Após essas decisões, a disponibilidade pode ser alterada e a grade pode ser remontada ou gerada novamente.

### Catálogo

Os cadastros gerais utilizados pela escola, como professores, turmas, disciplinas, unidades e demais dados compartilhados da empresa, são fornecidos pelo Catálogo.

O Catálogo mantém dados mestres. Ele não define políticas de geração, disponibilidade ou comportamento específico dos módulos escolares.

A modelagem deste repositório considera esses dados como entradas disponíveis e não cobre as regras internas de manutenção do Catálogo.

---

## 7. Premissas

### PRE-001

Os dados gerais necessários para a geração da grade estão previamente disponíveis no Catálogo.

### PRE-002

A disponibilidade dos professores é cadastrada por semestre e pode ser alterada quando necessário.

### PRE-003

A Geração de Horários deve poder consumir disponibilidade proveniente do Cadastro de Disponibilidade ou de outra fonte compatível.

### PRE-004

A geração pode ser concluída mesmo quando uma ou mais alocações não puderem ser resolvidas automaticamente.

### PRE-005

Toda situação não resolvida automaticamente que impeça uma alocação deve permanecer visível para intervenção humana.

### PRE-006

Após uma negociação ou alteração das condições de disponibilidade, uma nova geração ou remontagem da grade pode ser realizada.

### PRE-007

Cada empresa pode definir como os níveis de prioridade de ausência devem ser interpretados durante a geração.

### PRE-008

Na ausência de uma política própria da empresa, será utilizada uma política padrão em que as prioridades 9 e 10 proíbem a alocação e as prioridades de 1 a 8 permanecem negociáveis, com peso crescente.

### PRE-009

Cadastro de Disponibilidade e Geração de Horários devem permanecer funcionais de forma independente entre si.

### PRE-010

Parâmetros Escolares é um módulo-base do tipo de empresa Escola e concentra políticas compartilhadas entre os módulos escolares.

### PRE-011

Quando módulos compatíveis estiverem disponíveis em conjunto, eles devem poder compartilhar informações para reduzir duplicação de cadastro e melhorar o fluxo de trabalho.

### PRE-012

O Catálogo é responsável por dados mestres; Parâmetros Escolares é responsável por políticas institucionais; módulos funcionais são responsáveis por seus próprios dados operacionais e processos.

---

## 8. Questões em aberto

### Q-001 — Unidade da prioridade

A prioridade é atribuída a um dia inteiro, a um bloco de horário específico ou pode ser utilizada nos dois níveis?

### Q-002 — Alteração durante o semestre

Quando uma disponibilidade já utilizada em uma grade for alterada, a grade atual deve apenas ser marcada para revisão ou deve existir alguma ação imediata sobre ela?

### Q-003 — Remontagem

A remontagem deve tentar preservar o máximo possível da grade anterior ou uma nova geração pode reorganizar livremente todas as alocações?

---

## 9. Critérios de conclusão desta etapa

A visão geral pode ser considerada suficientemente definida quando:

- o problema estiver descrito sem depender de solução técnica;
- o objetivo geral estiver claro;
- os objetivos específicos forem coerentes com o problema;
- o escopo estiver delimitado;
- os principais itens fora de escopo estiverem explícitos;
- a responsabilidade de Catálogo, Parâmetros Escolares, Cadastro de Disponibilidade e Geração de Horários estiver clara;
- as premissas principais estiverem registradas;
- as questões que afetam diretamente as regras de geração estiverem identificadas.
