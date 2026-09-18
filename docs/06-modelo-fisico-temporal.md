# Modelo Físico e Temporal

## 1. Objetivo

Definir as referências estruturais necessárias para representar onde e quando as atividades escolares podem ocorrer.

Este documento trata de domínio e relações conceituais. Não define models Django, tabelas, chaves estrangeiras ou estratégia de persistência.

## 2. Visão geral

A estrutura física e temporal é dividida em dois eixos independentes, que se encontram durante o planejamento:

```text
Estrutura física                     Estrutura temporal

Site                                 Período Letivo
 └── Ambiente                         └── Turno
                                        └── Bloco de Aula

Site ── Deslocamento ── Site

              ↓
      Gestão de Horários
              ↓
 professor + turma + disciplina
 + ambiente + bloco de aula
```

A Gestão de Horários consome essas referências, mas não é proprietária delas.

---

## 3. Site

### DEC-FT-001 — `Site` é o termo estrutural adotado

`Site` representa uma unidade física da Empresa Escola em que atividades acadêmicas podem ocorrer.

O termo “Unidade Escolar” pode ser utilizado na interface ou comunicação de negócio, mas a modelagem utilizará `Site` para manter um termo único e compatível com o vocabulário já existente no Fractaw.

Exemplos:

- unidade principal;
- prédio anexo;
- campus distinto;
- outra localização física operada pela mesma Empresa.

Um Site pode possuir vários Ambientes.

```text
Empresa Escola
  └── Site
        └── Ambiente
```

### Invariantes

- um Site pertence a exatamente uma Empresa;
- Sites relacionados em uma mesma operação escolar devem pertencer à mesma Empresa;
- a inativação de um Site não deve apagar seu significado em históricos já produzidos.

---

## 4. Ambiente

### DEC-FT-002 — Sala e Laboratório são representados pelo conceito estrutural `Ambiente`

A modelagem adota `Ambiente` como referência física alocável.

Um Ambiente representa um espaço dentro de um Site em que uma atividade escolar pode ocorrer.

Exemplos:

- sala de aula comum;
- laboratório de informática;
- laboratório de química;
- auditório;
- quadra;
- oficina;
- outro espaço utilizável por uma atividade acadêmica.

Assim, `Sala` e `Laboratório` não precisam ser entidades estruturais paralelas.

```text
Site
 └── Ambiente
      ├── sala comum
      ├── laboratório
      ├── auditório
      └── ...
```

### Tipo e capacidades do Ambiente

A classificação do Ambiente deve permitir expressar sua natureza e, quando necessário, os recursos relevantes para uma atividade.

A modelagem ainda não determina se isso será representado por:

- tipo de Ambiente;
- capacidades/recursos;
- ou combinação dos dois.

O requisito de domínio é que Gestão de Horários possa verificar se um Ambiente atende às necessidades de uma Oferta de Disciplina.

Exemplo conceitual:

```text
Oferta de Disciplina
  exige: laboratório de informática

Ambiente 12
  atende: laboratório de informática
```

### Capacidade física

Um Ambiente pode possuir capacidade de ocupação quando essa informação for relevante para o planejamento.

A capacidade é propriedade do Ambiente, enquanto a regra sobre como utilizá-la pertence ao domínio consumidor.

### Invariantes

- um Ambiente pertence a exatamente um Site;
- Ambiente e Site pertencem à mesma Empresa;
- um Ambiente inativo não deve ser usado em novas alocações;
- históricos devem continuar identificando o Ambiente utilizado.

---

## 5. Deslocamento entre Sites

### DEC-FT-003 — Deslocamento é uma relação estrutural direcional

Uma relação de deslocamento representa o tempo necessário para ir de um Site de origem a um Site de destino.

```text
Site A ── 35 min ──> Site B
```

A relação é conceitualmente direcional porque o tempo pode não ser idêntico no sentido contrário.

Portanto:

```text
A → B
≠ necessariamente
B → A
```

Se a instituição considerar os tempos iguais, poderá registrar valores equivalentes para os dois sentidos.

### Responsabilidade

O tempo concreto entre dois Sites é dado estrutural.

Uma margem adicional aplicada pela instituição é política e pertence a Parâmetros.

Exemplo:

```text
Dado estrutural:
A → B = 35 min

Parâmetro:
margem de segurança = 10 min

Gestão de Horários:
tempo considerado = 45 min
```

### Invariantes

- origem e destino devem pertencer à mesma Empresa;
- origem e destino devem ser Sites diferentes;
- o tempo de deslocamento não pode ser negativo;
- ausência de uma relação não deve ser interpretada automaticamente como deslocamento de zero minutos.

---

## 6. Período Letivo

### DEC-FT-004 — Período Letivo representa vigência acadêmica, não faixa do dia

Período Letivo delimita uma vigência acadêmica concreta.

Exemplos:

- 1º semestre de 2027;
- ano letivo de 2027;
- outra vigência adotada pela Empresa.

Ele organiza referências e processos que precisam de contexto temporal acadêmico, como:

- Turmas;
- Ofertas de Disciplina;
- Disponibilidade;
- planejamento e publicação de grades.

Período Letivo não significa manhã, tarde ou noite.

### Invariantes

- um Período Letivo pertence a uma Empresa;
- deve possuir uma vigência temporal identificável;
- Turmas e processos associados devem pertencer à mesma Empresa;
- históricos de um Período Letivo encerrado permanecem identificáveis.

---

## 7. Turno

### DEC-FT-005 — Turno é referência organizacional temporal

Turno representa uma organização recorrente de parte da jornada escolar.

Exemplos:

- manhã;
- tarde;
- noite;
- integral;
- outro turno definido pela instituição.

Turno não é apenas uma string de apresentação porque pode ser referenciado por Turmas, Blocos de Aula e regras de planejamento.

```text
Turno
 └── Blocos de Aula
```

O vínculo exato entre Turno e Site ainda pode variar entre instituições. A modelagem, portanto, não assume que todo Site obrigatoriamente possui os mesmos horários para um Turno com o mesmo nome.

---

## 8. Bloco de Aula

### DEC-FT-006 — Bloco de Aula é referência estrutural alocável

Bloco de Aula representa uma unidade concreta de tempo em que uma aula pode ser alocada.

Deve possuir, conceitualmente:

- horário de início;
- horário de término;
- posição ou ordem dentro do Turno;
- contexto temporal suficiente para não ser ambíguo.

Exemplo:

```text
Turno: Manhã

Bloco 1 — 07:00–07:50
Bloco 2 — 07:50–08:40
Bloco 3 — 09:00–09:50
```

O intervalo entre 08:40 e 09:00 não precisa ser tratado como Bloco de Aula porque não é uma faixa alocável.

### Escopo dos Blocos

Um Bloco de Aula não deve ser considerado global para todas as Empresas ou todos os Sites por padrão.

Sua definição deve estar contextualizada por uma organização temporal da Empresa e, quando necessário, pelo Site ao qual aquela grade de blocos se aplica.

A implementação exata desse escopo permanece aberta.

### Invariantes

- horário de término deve ser posterior ao horário de início;
- blocos utilizados em um mesmo contexto não devem possuir sobreposição inválida;
- um bloco inativo não recebe novas alocações;
- Disponibilidade e Gestão de Horários devem referenciar a mesma identidade de Bloco quando estiverem falando da mesma faixa alocável.

---

## 9. Relações com Turma

Uma Turma pode possuir referências que restringem sua operação normal, como:

- Período Letivo;
- Site de referência;
- Turno.

Conceitualmente:

```text
Turma
├── Período Letivo
├── Site de referência
└── Turno
```

Esses vínculos representam contexto da Turma, não uma alocação de aula.

Uma aula concreta pode utilizar um Ambiente específico dentro do Site e um Bloco de Aula específico durante a montagem da Grade.

---

## 10. Relação com Disponibilidade

Disponibilidade consome principalmente a estrutura temporal.

```text
Professor
  ↓
Disponibilidade
  ↓
Bloco de Aula
```

Quando a instituição exigir granularidade por Site, o Site também pode compor o contexto da disponibilidade.

Essa necessidade deve ser confirmada pelos requisitos do módulo Disponibilidade.

---

## 11. Relação com Gestão de Horários

Gestão de Horários combina os eixos acadêmico, físico e temporal.

```text
Oferta de Disciplina
        +
Professor / Disponibilidade
        +
Turma
        +
Bloco de Aula
        +
Ambiente
        +
Deslocamento entre Sites
        +
Parâmetros
        ↓
Grade de Horários
```

Gestão de Horários não deve duplicar essas referências em seu próprio domínio.

---

## 12. Cardinalidades conceituais iniciais

```text
Empresa          1 ← 0..* Site
Site             1 ← 0..* Ambiente
Site             0..* ↔ 0..* Site (Deslocamento direcional)
Empresa          1 ← 0..* Período Letivo
Empresa          1 ← 0..* Turno
Turno            1 ← 0..* Bloco de Aula
Período Letivo   1 ← 0..* Turma
Site             0..1 ← 0..* Turma
Turno            0..1 ← 0..* Turma
```

A cardinalidade de Site e Turno em Turma pode ser refinada conforme os casos reais da instituição.

---

## 13. Questões em aberto

### QFT-001 — Escopo de Turno e Bloco

A mesma definição de Turno e seus Blocos pode ser compartilhada entre Sites ou cada Site precisa possuir sua própria organização temporal?

### QFT-002 — Recursos de Ambiente

É suficiente classificar Ambientes por tipo ou precisamos representar capacidades/recursos de forma independente?

### QFT-003 — Capacidade de Ambiente

A quantidade de alunos da Turma deve bloquear alocação em Ambiente com capacidade insuficiente ou essa regra pode admitir exceções configuráveis?

### QFT-004 — Disponibilidade por Site

A disponibilidade do Professor precisa distinguir o Site em que ele pode estar em determinado Bloco ou o Site é consequência apenas das aulas atribuídas?

### QFT-005 — Turnos compostos

Como representar instituições em que uma Turma opera em mais de um Turno, como período integral?

---

## 14. Decisões consolidadas nesta etapa

- `Site` é a referência de unidade física;
- `Ambiente` é a referência física alocável;
- Sala comum e Laboratório são classificações/capacidades de Ambiente, não entidades paralelas;
- Deslocamento entre Sites é dado estrutural e direcional;
- margem geral de deslocamento pertence a Parâmetros;
- Período Letivo representa vigência acadêmica;
- Turno representa organização temporal recorrente;
- Bloco de Aula representa faixa concreta alocável;
- Gestão de Horários consome essas referências sem assumir ownership sobre elas.
