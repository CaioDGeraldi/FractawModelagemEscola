# Visão Geral — Disponibilidade

## 1. Propósito

Modelar o módulo escolar responsável pela disponibilidade docente.

Disponibilidade é um módulo funcional do Tipo de Empresa Escola e deve permanecer independente de Gestão de Horários.

## 2. Responsabilidade

O módulo é proprietário do processo e do estado operacional relacionados à declaração e manutenção da disponibilidade dos professores.

Seu resultado deve poder ser consumido por Gestão de Horários ou por outro sistema compatível.

## 3. Escopo atual

A modelagem contempla:

- disponibilidade por período de vigência;
- cadastro ou declaração da disponibilidade;
- alteração quando permitida;
- prioridade de ausência;
- autoria e responsabilidade sobre a informação;
- disponibilização dos dados para consumidores compatíveis.

O detalhamento de revisão, aprovação, publicação ou versionamento deve ser confirmado pela modelagem específica do módulo antes de ser tratado como requisito definitivo.

## 4. Prioridade de ausência

A prioridade utiliza uma escala de 1 a 10.

Disponibilidade registra o valor informado.

O significado operacional da escala pertence à política vigente da Empresa em Parâmetros.

O módulo não decide sozinho quais níveis são bloqueantes ou negociáveis.

## 5. Independência

Disponibilidade não depende de Gestão de Horários para existir.

Conceitualmente:

```text
Disponibilidade
    ├── pode alimentar Gestão de Horários
    └── pode alimentar outro consumidor compatível
```

Da mesma forma, Gestão de Horários pode aceitar disponibilidade proveniente de outra fonte compatível.

## 6. Ownership

Pertencem a Disponibilidade:

- declarações de disponibilidade;
- prioridades registradas;
- alterações do processo de disponibilidade;
- demais estados próprios desse processo que forem confirmados.

Não pertencem a Disponibilidade:

- definição institucional do significado das prioridades;
- referências estruturais como Professor;
- tentativas de geração de grade;
- conflitos e exceções de planejamento;
- versões da grade.

## 7. Questões em aberto

### QD-001 — Granularidade

A prioridade é atribuída a um dia inteiro, a um bloco específico ou pode existir nos dois níveis?

### QD-002 — Alteração durante a vigência

Quando uma disponibilidade já utilizada por uma grade for alterada, qual efeito deve ser produzido sobre os consumidores desse dado?

### QD-003 — Ciclo de publicação

O módulo exige submissão, revisão, aprovação e snapshot publicado ou pode disponibilizar diretamente o estado vigente?

Essa questão deve ser decidida pelos requisitos, sem importar automaticamente o comportamento do baseline histórico.
