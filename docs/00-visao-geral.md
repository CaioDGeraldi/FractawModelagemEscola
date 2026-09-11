# Visão Geral do Sistema

## 1. Propósito do documento

Descreva brevemente por que este documento existe e qual aspecto do sistema ele delimita.

> Perguntas de apoio:
> - O que este documento pretende estabelecer?
> - Que decisões devem estar claras ao final desta etapa?

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
- gestão completa de recursos humanos.

---

## 6. Fronteira do sistema

O sistema é responsável por gerar automaticamente a grade de horários com base nas informações e restrições fornecidas pela instituição.

Quando uma situação impedir uma alocação e não houver solução automática possível, essa situação deve ser registrada como uma exceção de planejamento. A exceção não deve encerrar a geração da grade: o sistema deve continuar processando as demais alocações possíveis e concluir o processo com os resultados obtidos.

Ao término da geração, todas as exceções devem ser apresentadas de forma explícita ao responsável pela montagem dos horários, incluindo informações suficientes para compreender qual alocação foi afetada e por que ela não pôde ser concluída.

A resolução de situações que dependam de negociação, mudança de disponibilidade ou decisão externa permanece sob responsabilidade humana. Após essas decisões, a grade pode ser remontada ou gerada novamente com as novas condições.

Assim, o sistema automatiza a montagem e a validação da grade, mas não substitui decisões humanas quando o problema exige alteração das condições fornecidas.

---

## 7. Premissas

Registre apenas condições que estejam sendo assumidas como verdadeiras para permitir a modelagem.

Uma premissa não deve ser confundida com requisito ou regra de negócio.

### PRE-001

[Preencher]

---

## 8. Questões em aberto

Registre dúvidas cuja resposta ainda possa alterar o escopo ou a compreensão do problema.

### Q-001

[Preencher]

### Q-002

[Preencher]

---

## 9. Critérios de conclusão desta etapa

A visão geral pode ser considerada suficientemente definida quando:

- o problema estiver descrito sem depender de solução técnica;
- o objetivo geral estiver claro;
- os objetivos específicos forem coerentes com o problema;
- o escopo estiver delimitado;
- os principais itens fora de escopo estiverem explícitos;
- a fronteira do sistema puder ser explicada sem ambiguidade;
- premissas e dúvidas relevantes estiverem registradas.
