# Modelo Físico e Temporal

## 1. Objetivo

Definir as referências estruturais necessárias para representar onde e quando as atividades escolares podem ocorrer, respeitando a decisão de que os valores temporais são configurados pela própria Empresa por meio de Parâmetros.

Este documento trata de domínio e relações conceituais. Não define models Django, tabelas, chaves estrangeiras ou estratégia de persistência.

## 2. Princípio temporal

### DEC-FT-000 — A Empresa define sua configuração temporal

Valores que determinam duração, horário, intervalo, janela ou tempo considerado pela operação escolar são configuráveis pela Empresa.

Parâmetros é a fonte dessas definições e políticas.

As referências temporais concretas continuam existindo apenas quando identidade e histórico próprios forem necessários aos módulos.

```text
Parâmetros da Empresa
        ↓
configuração temporal
        ↓
Período Letivo / Turno / Bloco
+ Intervalos configurados
        ↓
Disponibilidade e Gestão de Horários
```

Assim, duas Empresas do Tipo Escola podem operar com durações, horários, intervalos e deslocamentos diferentes.

---

## 3. Visão geral

```text
Estrutura física                     Estrutura temporal

Site                                 Período Letivo
 └── Ambiente                         └── Turno
                                        └── Bloco de Aula

Site ── Deslocamento ──> Site

Parâmetros temporais
├── duração de aula
├── horários de Turno
├── Intervalos
├── interstícios
└── tempos de deslocamento

              ↓
      Gestão de Horários
```

A Gestão de Horários consome essas referências e configurações, mas não é proprietária delas.

---

## 4. Site

### DEC-FT-001 — `Site` é o termo estrutural adotado

`Site` representa uma unidade física da Empresa Escola em que atividades acadêmicas podem ocorrer.

O termo “Unidade Escolar” pode ser utilizado na interface ou comunicação de negócio, mas a modelagem utiliza `Site` como termo estrutural.

Um Site pode possuir vários Ambientes.

### Invariantes

- um Site pertence a exatamente uma Empresa;
- Sites relacionados em uma mesma operação escolar devem pertencer à mesma Empresa;
- a inativação de um Site não apaga seu significado em históricos.

---

## 5. Ambiente

### DEC-FT-002 — Sala e Laboratório são representados por `Ambiente`

`Ambiente` é a referência física alocável.

Exemplos:

- sala comum;
- laboratório de informática;
- laboratório de química;
- auditório;
- quadra;
- oficina.

Sala e Laboratório não precisam ser entidades paralelas.

A classificação do Ambiente deve permitir expressar tipo, capacidades ou recursos necessários para que Gestão de Horários verifique se ele atende a uma Oferta de Disciplina.

Um Ambiente pode possuir capacidade de ocupação quando relevante.

### Invariantes

- um Ambiente pertence a exatamente um Site;
- Ambiente e Site pertencem à mesma Empresa;
- Ambiente inativo não recebe novas alocações;
- históricos continuam identificando o Ambiente utilizado.

---

## 6. Deslocamento entre Sites

### DEC-FT-003 — A relação é estrutural; o tempo é configurado pela Empresa

A relação identifica um Site de origem e um Site de destino.

```text
Site A → Site B
```

A Empresa define o tempo considerado para essa relação por meio de sua configuração temporal.

Exemplo:

```text
A → B = 35 min
B → A = 45 min
```

A relação é direcional, portanto os valores podem divergir entre os dois sentidos.

A Empresa também pode definir margens adicionais.

```text
tempo A → B = 35 min
margem = 10 min
tempo considerado = 45 min
```

A identidade do par origem/destino pertence à estrutura escolar; os valores temporais associados pertencem à configuração da Empresa.

### Invariantes

- origem e destino pertencem à mesma Empresa;
- origem e destino são Sites diferentes;
- valores temporais não podem ser negativos;
- ausência de configuração não significa automaticamente zero minutos.

---

## 7. Período Letivo

### DEC-FT-004 — A Empresa define a organização temporal dos Períodos Letivos

Período Letivo representa uma vigência acadêmica concreta e possui identidade própria.

Exemplos:

- 1º semestre de 2027;
- ano letivo de 2027.

A Empresa define em Parâmetros como seus períodos são normalmente organizados, podendo estabelecer:

- duração padrão;
- datas ou meses usuais de início e término;
- demais regras de calendário.

Cada Período Letivo materializa uma vigência efetiva concreta.

```text
Configuração da Empresa:
semestre padrão ≈ 5 meses

Período Letivo 2027.1:
início = 03/02/2027
fim    = 02/07/2027
```

A duração concreta é consequência da configuração efetivamente aplicada àquele Período.

Período Letivo não significa manhã, tarde ou noite.

### Invariantes

- pertence a uma Empresa;
- possui início e término concretos;
- término posterior ao início;
- históricos de um Período encerrado permanecem identificáveis.

---

## 8. Turno

### DEC-FT-005 — Turno materializa uma configuração temporal da Empresa

Turno representa uma organização recorrente de parte da jornada escolar.

Exemplos:

- manhã;
- tarde;
- noite;
- integral.

A Empresa define os horários e regras temporais associados ao Turno.

Esses valores podem, quando necessário, divergir por Site ou outro escopo permitido.

Turno possui identidade porque pode ser referenciado por Turmas e Blocos de Aula.

---

## 9. Bloco de Aula

### DEC-FT-006 — Bloco é uma referência concreta derivada da configuração temporal

Bloco de Aula representa uma unidade identificável e alocável de tempo.

A Empresa define a duração e organização de seus Blocos por meio de Parâmetros.

Exemplo:

```text
Parâmetro:
duração padrão da aula = 50 min

Blocos concretos:
Bloco 1 = 07:00–07:50
Bloco 2 = 07:50–08:40
Bloco 3 = 09:00–09:50
```

Uma Empresa pode utilizar durações distintas quando necessário.

O Bloco continua possuindo:

- identidade;
- horário concreto de início;
- horário concreto de término;
- posição ou ordem dentro do Turno;
- contexto suficiente para ser referenciado por Disponibilidade e Gestão de Horários.

A duração concreta é consequência dos valores definidos pela Empresa e aplicados ao Bloco.

### Invariantes

- término posterior ao início;
- blocos do mesmo contexto não possuem sobreposição inválida;
- bloco inativo não recebe novas alocações;
- Disponibilidade e Gestão de Horários usam a mesma identidade de Bloco para a mesma faixa alocável.

---

## 10. Intervalo

### DEC-FT-007 — Intervalo pertence à configuração temporal em Parâmetros

`Intervalo` representa uma faixa de tempo não alocável configurada pela Empresa.

Exemplo:

```text
Bloco 2    07:50–08:40
Intervalo  08:40–09:00
Bloco 3    09:00–09:50
```

Diferentemente de Bloco de Aula, o Intervalo não precisa possuir identidade estrutural própria para ser referenciado por Disponibilidade ou Gestão de Horários.

Ele é uma regra/configuração da organização temporal que informa ao sistema que determinada faixa não pode receber alocações.

Pode variar por:

- Empresa;
- Site;
- Turno;
- outro escopo permitido pela configuração.

Portanto:

```text
Bloco de Aula
→ referência temporal alocável

Intervalo
→ configuração temporal não alocável em Parâmetros
```

---

## 11. Interstício

Interstício também é definido em Parâmetros, mas possui significado diferente de Intervalo.

```text
Intervalo
→ faixa não alocável explicitamente configurada

Interstício
→ tempo mínimo livre exigido entre determinadas atividades
```

Um Intervalo pode satisfazer um Interstício, mas não são equivalentes.

---

## 12. Relações com Turma

Uma Turma pode possuir:

- Período Letivo;
- Site de referência;
- Turno.

Esses vínculos descrevem contexto da Turma, não uma aula já alocada.

---

## 13. Relação com Disponibilidade

Disponibilidade consome principalmente os Blocos de Aula materializados:

```text
Professor
  ↓
Disponibilidade
  ↓
Bloco de Aula
```

Intervalos configurados não são oferecidos como Blocos disponíveis para declaração de disponibilidade ou alocação.

Quando necessário, Site também pode compor o contexto da disponibilidade.

---

## 14. Relação com Gestão de Horários

Gestão de Horários combina:

```text
Oferta de Disciplina
        +
Professor(es) / Disponibilidade
        +
Turma
        +
Bloco de Aula
        +
Ambiente
        +
Deslocamento entre Sites
        +
Parâmetros temporais
          ├── Intervalos
          ├── Interstícios
          ├── durações
          └── deslocamentos
        ↓
Grade de Horários
```

O módulo utiliza os valores definidos pela Empresa sem assumir ownership sobre a configuração temporal.

---

## 15. Escopo e divergência de configuração

A modelagem admite que configurações temporais possam divergir.

Exemplos:

```text
Empresa A
  aula padrão = 50 min
  intervalo = 08:40–09:00
  interstício = 10 min

Empresa B
  aula padrão = 45 min
  intervalo = 09:15–09:30
  interstício = 20 min
```

E, quando permitido:

```text
Empresa A
  Site Centro
    turno manhã = 07:00–12:00
    intervalo = 08:40–09:00

  Site Anexo
    turno manhã = 07:30–12:30
    intervalo = 09:00–09:20
```

A precedência entre configuração global e configuração específica será definida tecnicamente no FractawModules, preservando a semântica estabelecida neste repositório.

---

## 16. Questões em aberto

### QFT-001 — Precedência temporal

Quando existir valor global e valor específico por Site/Turno, qual deve prevalecer?

### QFT-002 — Recursos de Ambiente

É suficiente classificar Ambientes por tipo ou precisamos representar capacidades/recursos independentemente?

### QFT-003 — Capacidade de Ambiente

Turma maior que a capacidade do Ambiente bloqueia a alocação ou pode haver exceção configurável?

### QFT-004 — Disponibilidade por Site

A Disponibilidade do Professor precisa distinguir o Site em determinado Bloco?

### QFT-005 — Turnos compostos

Como representar Turmas que operam em mais de um Turno?

### QFT-006 — Vigência de mudança temporal

Quando a Empresa altera uma configuração temporal, a mudança afeta somente novos Períodos/Blocos ou também estruturas já existentes?

### QFT-007 — Escopo de Intervalos

Intervalos podem ser definidos globalmente para a Empresa ou precisam sempre estar associados a um Turno e, eventualmente, a um Site?

---

## 17. Decisões consolidadas

- a Empresa define seus valores temporais por meio de Parâmetros;
- Empresas podem operar com valores temporais diferentes;
- configurações podem possuir escopo específico quando necessário;
- Site e Ambiente permanecem referências estruturais;
- a relação de Deslocamento entre Sites permanece estrutural, mas seu tempo é configurável;
- Período Letivo permanece referência concreta, materializando uma configuração de vigência;
- Turno permanece referência organizacional, materializando horários configurados;
- Bloco de Aula permanece referência alocável, materializando duração e horários configurados;
- Intervalo pertence a Parâmetros como faixa temporal não alocável;
- Interstício mínimo pertence a Parâmetros como regra de separação;
- Gestão de Horários consome as configurações e referências sem assumir ownership sobre elas.
