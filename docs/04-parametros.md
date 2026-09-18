# Parâmetros Aplicáveis à Escola

## 1. Objetivo

Identificar as políticas e configurações que o Tipo de Empresa Escola exige da capacidade-base genérica Parâmetros do FractawModules.

Este documento não cria um módulo separado chamado "Parâmetros Escolares".

## 2. Responsabilidade

Parâmetros responde principalmente:

> Como esta Empresa decidiu operar dentro do contexto Escola?

O Tipo de Empresa Escola define a semântica e a aplicabilidade das políticas.

A Empresa mantém os valores efetivamente vigentes e pode defini-los de forma diferente de outras Empresas.

## 3. Princípio de configuração temporal

### DEC-PAR-001 — Valores que definem tempo são configuráveis pela Empresa

Toda regra ou valor que determina duração, intervalo, janela ou tempo considerado pela operação escolar deve ser configurável pela Empresa por meio de Parâmetros.

Isso inclui, conforme aplicável:

- duração padrão de aula;
- duração ou organização dos Blocos de Aula;
- horários de Turnos;
- Intervalos e suas faixas de início e término;
- duração ou configuração padrão de Períodos Letivos;
- interstício mínimo entre atividades;
- tempos considerados para deslocamento entre Sites;
- margens de segurança de deslocamento;
- demais janelas e tolerâncias temporais.

Uma Empresa não deve ser obrigada a adotar os mesmos valores temporais de outra.

Quando necessário, a configuração pode possuir escopo mais específico, por exemplo:

- Empresa inteira;
- Site;
- Turno;
- Período Letivo;
- outro contexto escolar confirmado pelos requisitos.

A forma técnica de representar herança, sobrescrita ou precedência desses valores pertence ao FractawModules.

## 4. Parâmetro x referência concreta

Configurar tempo em Parâmetros não elimina referências estruturais que precisam ser identificadas por outros domínios.

Exemplo:

```text
Parâmetros
  duração padrão de aula = 50 min
  turno manhã = 07:00–12:00
  intervalo = 08:40–09:00

Estrutura temporal concreta
  Bloco 1 = 07:00–07:50
  Bloco 2 = 07:50–08:40
  Bloco 3 = 09:00–09:50
```

O valor temporal é definido/configurável pela Empresa; o Bloco continua possuindo identidade porque Disponibilidade e Gestão de Horários precisam referenciar a mesma faixa alocável.

O Intervalo, por outro lado, não precisa ser uma referência estrutural própria: ele representa uma faixa temporal não alocável definida na configuração da Empresa.

O mesmo princípio de configuração temporal vale para Período Letivo e Deslocamento entre Sites: a Empresa define os valores temporais, enquanto as referências concretas preservam identidade, relacionamento e histórico quando essa identidade for necessária ao domínio.

## 5. Parâmetros já identificados

A modelagem já identificou como candidatos:

- política de interpretação da escala de prioridade de ausência;
- prioridade mínima considerada bloqueante;
- limites institucionais de alocação;
- máximo de aulas consecutivas;
- duração padrão de aula;
- configuração temporal de Turnos e Blocos;
- Intervalos e suas faixas temporais;
- duração/configuração padrão de Período Letivo;
- interstício mínimo entre atividades;
- tempos de deslocamento considerados entre Sites;
- margens ou tolerâncias adicionais de deslocamento;
- outras políticas compartilhadas confirmadas pelos requisitos.

## 6. Política de prioridade

A Disponibilidade registra o valor de prioridade atribuído a um período.

Parâmetros define como esse valor deve ser interpretado pela Empresa.

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

Essa política permanece configurável conforme as regras permitidas para o Tipo de Empresa Escola.

## 7. Intervalo e Interstício

### DEC-PAR-002 — Intervalo é configuração temporal da Empresa

Intervalo representa uma faixa de tempo não alocável definida pela Empresa dentro de sua organização temporal.

Exemplo:

```text
intervalo = 08:40–09:00
```

Ele pode variar conforme Turno, Site ou outro escopo temporal permitido.

O Intervalo faz parte da configuração utilizada para construir e validar os Blocos, mas não precisa possuir identidade estrutural independente.

### DEC-PAR-003 — Interstício é uma regra mínima de separação

Interstício representa o tempo mínimo livre exigido entre atividades quando houver regra institucional, contratual ou operacional.

Exemplo:

```text
interstício mínimo entre atividades = 20 minutos
```

Os conceitos são diferentes:

```text
Intervalo
→ faixa não alocável configurada pela Empresa

Interstício
→ tempo mínimo que determinadas atividades devem respeitar entre si
```

Um Intervalo pode satisfazer um Interstício, mas não são o mesmo conceito.

## 8. Deslocamento entre Sites

O tempo utilizado para deslocamento entre Sites deve ser definido pela Empresa e pode divergir entre pares de Sites e entre sentidos.

Exemplo:

```text
A → B = 35 min
B → A = 45 min
```

A Empresa também pode definir margem adicional:

```text
tempo configurado A → B = 35 min
margem de segurança = 10 min
tempo considerado = 45 min
```

A relação `Site origem → Site destino` continua sendo estrutural; os valores temporais associados a ela são configurações da Empresa.

## 9. Duração da aula e Blocos

A Empresa define sua organização temporal.

Pode existir uma duração padrão de aula, por exemplo:

```text
duração padrão = 50 min
```

Os Blocos concretos materializam essa configuração:

```text
Bloco 1 = 07:00–07:50
Bloco 2 = 07:50–08:40
Intervalo = 08:40–09:00
Bloco 3 = 09:00–09:50
```

A Empresa pode possuir exceções ou Blocos de durações diferentes quando necessário.

Portanto, a duração padrão e os Intervalos orientam a organização temporal, enquanto o Bloco concreto continua sendo a referência alocável utilizada pelos módulos.

## 10. Período Letivo

A Empresa define como seus Períodos Letivos são organizados temporalmente.

Podem existir configurações como:

- duração padrão;
- meses ou datas usuais de início e término;
- regras de composição do calendário.

Cada Período Letivo concreto continua possuindo identidade e vigência efetiva própria.

Exemplo:

```text
Parâmetro/política:
semestre letivo padrão ≈ 5 meses

Período concreto:
2027.1 = 03/02/2027 até 02/07/2027
```

## 11. Limites

Parâmetros não deve assumir ownership das referências estruturais apenas porque define seus valores temporais.

Portanto, Parâmetros não é proprietário de:

- Professor;
- Turma;
- Disciplina;
- Site;
- Bloco de Aula enquanto referência concreta;
- Período Letivo enquanto ocorrência concreta;
- Grade;
- Disponibilidade individual;
- tentativas e exceções de Gestão de Horários.

Intervalo é exceção a essa distinção porque, nesta modelagem, ele é uma configuração temporal e não uma referência estrutural independente.

## 12. Questões em aberto

A modelagem ainda deve confirmar:

- como funciona a precedência entre valores globais e valores por Site/Turno;
- quais configurações temporais aceitam sobrescrita;
- quais mudanças precisam de histórico/versionamento;
- quando uma mudança temporal passa a valer para processos já iniciados;
- em quais contextos o interstício mínimo se aplica;
- se Intervalos podem variar por Site e Turno;
- se valores temporais podem variar por Período Letivo sem alterar o padrão da Empresa.
