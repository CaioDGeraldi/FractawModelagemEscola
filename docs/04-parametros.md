# Parâmetros Aplicáveis à Escola

## 1. Objetivo

Identificar as políticas e configurações que o Tipo de Empresa Escola exige da capacidade-base genérica Parâmetros do FractawModules.

Este documento não cria um módulo separado chamado "Parâmetros Escolares".

## 2. Responsabilidade

Parâmetros responde principalmente:

> Como esta Empresa decidiu operar dentro do contexto Escola?

O Tipo de Empresa Escola define a semântica e a aplicabilidade das políticas.

A Empresa mantém os valores efetivamente vigentes.

## 3. Critério de pertencimento

Um conceito é candidato a Parâmetro quando representa:

- política empresarial;
- limite configurável;
- regra geral de operação;
- preferência institucional compartilhada;
- valor padrão ou tolerância que afeta o comportamento dos módulos.

Não pertencem a Parâmetros apenas por serem configuráveis:

- referências estruturais;
- fatos operacionais;
- disponibilidade individual;
- grades;
- conflitos;
- tentativas de geração;
- exceções.

## 4. Candidatos atuais

A modelagem de Disponibilidade e Gestão de Horários já identificou candidatos como:

- política de interpretação da escala de prioridade de ausência;
- prioridade mínima considerada bloqueante;
- limites institucionais de alocação;
- máximo de aulas consecutivas, quando definido como política;
- margens ou tolerâncias gerais de deslocamento;
- outras políticas compartilhadas que venham a ser confirmadas pelos requisitos.

## 5. Política de prioridade

A Disponibilidade registra o valor de prioridade atribuído a um período.

Parâmetros define como esse valor deve ser interpretado pela Empresa.

Conceitualmente:

```text
Disponibilidade
    registra prioridade

Parâmetros
    define significado e efeito

Gestão de Horários
    aplica a política
```

A referência atual de modelagem considera:

- níveis 1 a 8 como negociáveis, com peso crescente;
- níveis 9 e 10 como bloqueantes.

Essa política permanece sujeita à definição formal dos requisitos da Escola e à configuração permitida por Empresa.

## 6. Limites

Parâmetros não deve:

- manter Professor, Turma, Disciplina ou outras referências;
- registrar ausências individuais;
- armazenar uma tentativa de geração;
- armazenar a grade;
- absorver dados operacionais apenas porque mais de um módulo os consulta.

## 7. Questões em aberto

A modelagem ainda deve confirmar:

- quais políticas são realmente compartilhadas;
- quais possuem valor padrão;
- quais podem ser alteradas por Empresa;
- quais exigem histórico ou versionamento;
- em que momento uma política alterada passa a valer para processos já iniciados.
