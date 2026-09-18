# Modelo Físico e Temporal

## 1. Objetivo

Definir as referências estruturais necessárias para representar onde e quando as atividades escolares podem ocorrer, distinguindo referências e ocorrências concretas de políticas/defaults candidatos a Parâmetros.

Este documento trata de domínio e relações conceituais. Não define models Django, tabelas, chaves estrangeiras ou estratégia de persistência.

## 2. Princípio temporal

### DEC-FT-000 — Natureza antes de configurabilidade

A Empresa pode definir sua organização temporal, mas o fato de um valor ser temporal ou configurável não o transforma automaticamente em Parâmetro.

A distinção central é:

```text
política/default temporal
≠
referência/ocorrência temporal concreta
```

Exemplos:

```text
duração padrão de aula = 50 min
→ candidata a Parâmetros

Bloco 3 = 09:00–09:50
→ referência concreta

Período Letivo 2027.1 = datas efetivas
→ referência concreta

margem geral de deslocamento = 10 min
→ candidata a Parâmetros
```

A futura capacidade Parâmetros pode fornecer políticas/defaults consumidos pela estrutura temporal, sem assumir ownership das referências concretas.

---

## 3. Visão geral

```text
Estrutura física                     Estrutura temporal

Site                                 Período Letivo
 └── Ambiente                         └── Turno
                                        └── Bloco de Aula

Site ── Deslocamento ──> Site

Políticas/defaults possíveis em Parâmetros
├── duração padrão de aula
├── interstício institucional
├── margem geral de deslocamento
├── regras/defaults de calendário
└── outros limites temporais compartilháveis

              ↓
      Gestão de Horários
```

A Gestão de Horários consome referências e políticas sem assumir ownership delas.

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

### DEC-FT-003 — Relação e tempo específico são dados estruturais

Deslocamento identifica uma relação direcional entre um Site de origem e um Site de destino e o tempo concreto associado àquele par.

```text
Site A → Site B = 35 min
Site B → Site A = 45 min
```

A relação é direcional, portanto os tempos podem divergir entre os sentidos.

O tempo específico faz parte do dado próprio daquela relação estrutural. Ele não é classificado como Parâmetro apenas por ser temporal ou configurável.

A Empresa pode possuir uma política geral adicional, candidata a Parâmetros:

```text
margem geral de deslocamento = 10 min
```

Gestão de Horários pode aplicar:

```text
deslocamento estrutural A → B = 35 min
margem institucional = 10 min
tempo considerado = 45 min
```

### Invariantes

- origem e destino pertencem à mesma Empresa;
- origem e destino são Sites diferentes;
- o tempo específico não pode ser negativo;
- ausência de relação conhecida não significa automaticamente zero minutos.

---

## 7. Período Letivo

### DEC-FT-004 — Período Letivo é vigência concreta

Período Letivo representa uma vigência acadêmica concreta e possui identidade própria.

Exemplos:

- 1º semestre de 2027;
- ano letivo de 2027.

Cada Período Letivo possui datas efetivas próprias:

```text
Período Letivo 2027.1
início = 03/02/2027
fim    = 02/07/2027
```

Essas datas são dados da ocorrência concreta.

Podem existir políticas ou defaults compartilháveis candidatos a Parâmetros, por exemplo:

- duração padrão de um semestre;
- regra geral de composição do calendário;
- defaults de início ou encerramento, quando houver requisito real.

A política não substitui a vigência concreta.

Período Letivo não significa manhã, tarde ou noite.

### Invariantes

- pertence a uma Empresa;
- possui início e término concretos;
- término posterior ao início;
- históricos de um Período encerrado permanecem identificáveis.

---

## 8. Turno

### DEC-FT-005 — Turno é referência organizacional temporal

Turno representa uma organização recorrente de parte da jornada escolar.

Exemplos:

- manhã;
- tarde;
- noite;
- integral.

Pode ser referenciado por Turmas e organizar Blocos de Aula.

Horários concretos de um Turno fazem parte de sua organização temporal. Defaults ou políticas gerais de horário podem ser candidatos a Parâmetros quando houver necessidade compartilhável.

A modelagem não assume que todos os Sites obrigatoriamente utilizem os mesmos horários para um Turno com o mesmo nome.

---

## 9. Bloco de Aula

### DEC-FT-006 — Bloco é referência concreta e alocável

Bloco de Aula representa uma unidade identificável de tempo em que uma aula pode ser alocada.

Possui:

- identidade;
- horário concreto de início;
- horário concreto de término;
- posição ou ordem dentro do Turno;
- contexto suficiente para ser referenciado por Disponibilidade e Gestão de Horários.

Exemplo:

```text
Bloco 1 = 07:00–07:50
Bloco 2 = 07:50–08:40
```

A duração concreta decorre dos horários do próprio Bloco.

Uma política como:

```text
duração padrão de aula = 50 min
```

pode ser candidata a Parâmetros e orientar a construção/validação da organização temporal, mas não substitui os Blocos concretos.

### Invariantes

- término posterior ao início;
- blocos do mesmo contexto não possuem sobreposição inválida;
- bloco inativo não recebe novas alocações;
- Disponibilidade e Gestão de Horários usam a mesma identidade de Bloco para a mesma faixa alocável.

---

## 10. Intervalo

### DEC-FT-007 — Intervalo não é automaticamente Parâmetro

Intervalo representa uma faixa não alocável da organização temporal escolar.

Exemplo concreto:

```text
Bloco 2    07:50–08:40
Intervalo  08:40–09:00
Bloco 3    09:00–09:50
```

A faixa `08:40–09:00` é uma ocorrência concreta da organização temporal.

Ela não deve ser classificada como Parâmetro apenas porque a Empresa pode configurá-la.

Ao mesmo tempo, a modelagem ainda não exige que Intervalo tenha identidade estrutural independente. Ele pode permanecer como parte da organização temporal concreta de um Turno enquanto nenhum requisito exigir referência própria.

Políticas/defaults gerais podem ser candidatos a Parâmetros, por exemplo:

```text
duração padrão do intervalo = 20 min
```

Portanto:

```text
Intervalo concreto
→ parte da organização temporal

política/default de Intervalo
→ candidata a Parâmetros
```

---

## 11. Interstício

Interstício possui natureza diferente de Intervalo.

Representa uma regra mínima de separação entre determinadas atividades.

Quando institucional ou contratual, é candidato natural a Parâmetros:

```text
interstício mínimo = 20 min
```

```text
Intervalo
→ faixa não alocável concreta

Interstício
→ regra mínima de separação
```

Um Intervalo pode satisfazer um Interstício, mas os conceitos não são equivalentes.

---

## 12. Relações com Turma

Uma Turma pode possuir:

- Período Letivo;
- Site de referência;
- Turno.

Esses vínculos descrevem contexto da Turma, não uma aula já alocada.

---

## 13. Relação com Disponibilidade

Disponibilidade consome principalmente Blocos de Aula concretos:

```text
Professor
  ↓
Disponibilidade
  ↓
Bloco de Aula
```

Faixas não alocáveis da organização temporal não devem ser oferecidas como Blocos disponíveis.

Quando necessário, Site também pode compor o contexto da disponibilidade. Essa necessidade permanece em aberto.

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
políticas aplicáveis de Parâmetros
        ↓
Grade de Horários
```

O módulo não assume ownership dessas referências ou políticas.

---

## 15. Pressão de requisitos sobre Parâmetros

Algumas políticas escolares podem precisar de escopo contextual, por exemplo:

- Empresa;
- Site;
- Turno;
- Período Letivo.

Essa necessidade deve informar a futura F08, mas não define como o escopo será persistido ou resolvido.

A solução futura deve preservar:

```text
plataforma
    ↑
Escola
```

Parâmetros não pode depender diretamente de código específico da Escola para cumprir esse requisito.

---

## 16. Questões em aberto

### QFT-001 — Escopo de Turno e Bloco

A mesma definição de Turno e seus Blocos pode ser compartilhada entre Sites ou cada Site precisa de organização temporal própria?

### QFT-002 — Recursos de Ambiente

É suficiente classificar Ambientes por tipo ou precisamos representar capacidades/recursos independentemente?

### QFT-003 — Capacidade de Ambiente

Turma maior que a capacidade do Ambiente bloqueia a alocação ou pode haver exceção configurável?

### QFT-004 — Disponibilidade por Site

A Disponibilidade do Professor precisa distinguir o Site em determinado Bloco?

### QFT-005 — Turnos compostos

Como representar Turmas que operam em mais de um Turno?

### QFT-006 — Vigência de mudanças

Como alterações em políticas temporais ou referências concretas afetam processos ou resultados já iniciados?

### QFT-007 — Intervalos concretos

Intervalos precisam de identidade própria em algum caso real ou podem permanecer como parte da organização temporal de Turno?

### QFT-008 — Escopo de políticas temporais

Quais políticas realmente precisam variar por Site, Turno ou Período Letivo?

---

## 17. Decisões consolidadas

- Site e Ambiente permanecem referências estruturais;
- Deslocamento entre Sites é relação estrutural direcional;
- o tempo específico de cada relação de deslocamento é dado estrutural;
- margem geral de deslocamento é candidata a Parâmetros;
- Período Letivo permanece referência concreta com datas efetivas;
- Turno permanece referência organizacional temporal;
- Bloco de Aula permanece referência concreta e alocável;
- duração padrão de aula é candidata a Parâmetros;
- Intervalo concreto não é automaticamente Parâmetro;
- Interstício institucional é candidato natural a Parâmetros;
- configurabilidade não substitui classificação pela natureza do conceito;
- Gestão de Horários consome referências e políticas sem assumir ownership.