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

## 4. Parâmetros já identificados

A modelagem de Disponibilidade e Gestão de Horários já identificou como candidatos:

- política de interpretação da escala de prioridade de ausência;
- prioridade mínima considerada bloqueante;
- limites institucionais de alocação;
- máximo de aulas consecutivas;
- interstício mínimo entre atividades, quando exigido pela instituição;
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

## 6. Interstício

### DEC-PAR-001 — Interstício mínimo é uma política

Interstício representa o tempo mínimo livre que deve existir entre duas atividades quando uma regra institucional, contratual ou operacional exigir essa separação.

Exemplo conceitual:

```text
interstício mínimo entre atividades = 20 minutos
```

Ele não deve ser confundido com um intervalo real já existente na grade de blocos.

```text
Intervalo estrutural
→ 08:40–09:00 realmente existe na organização temporal

Interstício mínimo
→ regra que exige determinada separação entre atividades
```

A modelagem ainda deve definir em quais contextos o interstício se aplica, por exemplo:

- entre aulas consecutivas;
- entre turnos;
- entre jornadas;
- apenas em determinadas situações contratuais.

## 7. Deslocamento e margem

O tempo concreto de deslocamento entre dois Sites não é Parâmetro: pertence à relação estrutural de Deslocamento entre Sites.

Parâmetros pode definir uma margem adicional de segurança.

Exemplo:

```text
Deslocamento A → B = 35 min
Margem institucional = 10 min
Tempo considerado no planejamento = 45 min
```

Gestão de Horários aplica a política sem assumir ownership do dado estrutural.

## 8. Duração padrão de aula

A duração real de uma aula alocável é definida pelo Bloco de Aula, por seus horários de início e término.

Se a Empresa desejar utilizar uma duração padrão para auxiliar a criação ou validação dos Blocos, esse valor pode ser representado como política/configuração.

Portanto:

```text
Bloco 1 = 07:00–07:50
→ duração real: 50 min

Parâmetro opcional:
duração padrão de aula = 50 min
```

O parâmetro padrão não substitui a identidade e os horários reais dos Blocos.

## 9. Limites

Parâmetros não deve:

- manter Professor, Turma, Disciplina ou outras referências;
- registrar Períodos Letivos concretos;
- registrar Blocos de Aula concretos;
- armazenar tempos concretos de deslocamento entre Sites;
- registrar ausências individuais;
- armazenar uma tentativa de geração;
- armazenar a grade;
- absorver dados operacionais apenas porque mais de um módulo os consulta.

## 10. Questões em aberto

A modelagem ainda deve confirmar:

- quais políticas são realmente compartilhadas;
- quais possuem valor padrão;
- quais podem ser alteradas por Empresa;
- quais exigem histórico ou versionamento;
- em que momento uma política alterada passa a valer para processos já iniciados;
- em quais contextos o interstício mínimo se aplica.
