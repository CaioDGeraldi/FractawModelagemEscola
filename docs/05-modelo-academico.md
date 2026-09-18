# Modelo Acadêmico Estrutural

## 1. Objetivo

Definir as relações acadêmicas mínimas entre Curso, Turma, Disciplina e Professor no Tipo de Empresa Escola.

Este documento representa o domínio e suas relações conceituais. Não define models Django, chaves estrangeiras, tabelas intermediárias ou estratégia de persistência.

## 2. Visão geral

A cadeia acadêmica não deve ser tratada como uma hierarquia rígida em que uma entidade simplesmente pertence à anterior.

O modelo inicial é:

```text
Curso ──────── Disciplina
  │                 │
  │                 │
  └── Turma ────────┘
          │
          │ oferta disciplinas
          ▼
  Oferta de Disciplina
          │
          │ pode exigir / receber docente
          ▼
       Professor
```

As relações possuem significado próprio e não devem ser reduzidas apenas por conveniência de implementação.

## 3. Curso e Disciplina

### DEC-ACA-001 — Disciplina possui identidade independente de Curso

Uma Disciplina não pertence exclusivamente a um Curso.

A mesma Disciplina pode participar de diferentes Cursos, inclusive com regras curriculares diferentes.

Exemplo conceitual:

```text
Disciplina: Programação Web

Curso A ── utiliza Programação Web
Curso B ── utiliza Programação Web
```

Isso evita duplicar a identidade da Disciplina apenas porque ela aparece em mais de uma formação.

### Relação curricular

A relação entre Curso e Disciplina representa a presença daquela Disciplina na organização curricular do Curso.

Essa relação pode precisar registrar informações próprias, como:

- carga horária prevista;
- módulo, série ou etapa;
- obrigatoriedade;
- outras regras curriculares que venham a ser confirmadas.

O nome definitivo dessa relação ainda pode ser refinado. `Matriz Curricular` é o termo conceitual inicial.

```text
Curso
  │
  └── Matriz Curricular
          └── Disciplina
```

## 4. Turma

### DEC-ACA-002 — Turma representa uma ocorrência acadêmica concreta

Turma não é sinônimo de Curso.

Curso representa a formação ou organização acadêmica; Turma representa um grupo concreto de alunos dentro de determinado contexto letivo.

Uma Turma deve estar vinculada a um Período Letivo.

Quando o modelo acadêmico da instituição utilizar Curso, a Turma pode estar vinculada ao Curso correspondente.

Conceitualmente:

```text
Curso
  └── Turma
        └── Período Letivo
```

A obrigatoriedade de Curso para toda Turma permanece dependente do modelo acadêmico da Empresa Escola. O domínio não deve impedir instituições que organizem Turmas sem o conceito de Curso.

## 5. Turma e Disciplina

### DEC-ACA-003 — Currículo e oferta concreta são conceitos diferentes

A Matriz Curricular responde:

> Quais Disciplinas fazem parte de um Curso?

A relação Turma–Disciplina responde:

> Quais Disciplinas esta Turma precisa receber neste contexto letivo?

Por isso, Gestão de Horários não deve depender diretamente apenas da Matriz Curricular.

É necessário representar a oferta concreta da Disciplina para a Turma.

### Oferta de Disciplina

`Oferta de Disciplina` é o nome conceitual inicial para a relação entre Turma e Disciplina que precisa ser atendida no planejamento.

Pode conter, conforme os requisitos forem confirmados:

- Turma;
- Disciplina;
- carga horária a cumprir;
- requisitos de ambiente;
- outras condições necessárias à oferta.

Exemplo:

```text
Turma 3-DS
  ├── Programação Web — 4 aulas/semana
  ├── Banco de Dados — 3 aulas/semana
  └── Matemática — 2 aulas/semana
```

A Oferta de Disciplina existe antes da montagem da grade e constitui uma entrada acadêmica para Gestão de Horários.

Ela não representa uma aula já alocada em determinado dia e bloco.

## 6. Professor e Disciplina

### DEC-ACA-004 — Habilitação docente e atribuição docente são conceitos diferentes

A relação entre Professor e Disciplina não deve possuir significado ambíguo.

Devem ser distinguidas pelo menos duas ideias:

### Habilitação docente

Responde:

> Este Professor pode lecionar esta Disciplina?

É uma relação estrutural entre Professor e Disciplina.

```text
Professor
   └── habilitado para
         └── Disciplina
```

A habilitação não significa que o Professor foi escolhido para uma Turma específica.

### Atribuição docente

Responde:

> Este Professor é o responsável por esta Oferta de Disciplina para esta Turma?

É diferente de habilitação.

```text
Professor
   └── atribuído a
         └── Oferta de Disciplina
```

Ainda deve ser definido pelos requisitos se a atribuição docente:

- sempre existe antes da geração da grade;
- pode ser escolhida durante o planejamento;
- pode ser sugerida automaticamente e confirmada por uma pessoa autorizada;
- ou admite mais de um desses fluxos.

## 7. Professor e Turma

Não deve existir uma relação direta genérica `Professor → Turma` sem significado específico.

O relacionamento normalmente ocorre por meio da atividade acadêmica:

```text
Professor
   ↓
Oferta de Disciplina
   ↓
Turma
```

Isso evita registrar que um Professor "pertence" a uma Turma sem explicar qual Disciplina ou responsabilidade origina a relação.

## 8. Relação com Gestão de Horários

Gestão de Horários consome o modelo acadêmico, mas não se torna proprietária dele.

Entradas conceituais relevantes incluem:

- Turma;
- Oferta de Disciplina;
- carga horária exigida;
- Professores habilitados ou previamente atribuídos;
- Período Letivo;
- Blocos de Aula;
- ambientes e recursos;
- Disponibilidade;
- políticas fornecidas por Parâmetros.

O resultado da geração é operacional e pertence a Gestão de Horários.

```text
Modelo acadêmico
      │
      ▼
Gestão de Horários
      │
      ▼
Grade / alocações / exceções
```

## 9. Invariantes iniciais

### INV-ACA-001

Curso, Turma, Disciplina, Professor e demais referências relacionadas em uma mesma oferta devem pertencer à mesma Empresa.

### INV-ACA-002

Uma Oferta de Disciplina deve referenciar exatamente uma Turma e uma Disciplina.

### INV-ACA-003

Uma Turma deve possuir um Período Letivo definido.

### INV-ACA-004

Habilitação docente não equivale a atribuição docente.

### INV-ACA-005

Uma alocação de horário não altera a identidade da Oferta de Disciplina que originou a necessidade.

### INV-ACA-006

A exclusão ou inativação de uma referência não deve tornar históricos acadêmicos ou grades publicadas semanticamente incompreensíveis.

## 10. Cardinalidades conceituais iniciais

```text
Curso                0..* ↔ 0..* Disciplina
Curso                0..1 ← 0..* Turma
Período Letivo       1    ← 0..* Turma
Turma                 1    ← 0..* Oferta de Disciplina
Disciplina            1    ← 0..* Oferta de Disciplina
Professor            0..* ↔ 0..* Disciplina (habilitação)
Professor            0..* ↔ 0..* Oferta de Disciplina (atribuição, se aplicável)
```

Essas cardinalidades são conceituais e podem ser refinadas quando regras institucionais mais específicas forem formalizadas.

## 11. Questões em aberto

### QA-ACA-001 — Matriz Curricular

A Matriz Curricular precisa de versionamento por vigência ou Período Letivo?

### QA-ACA-002 — Carga horária

A carga horária efetiva para planejamento pertence à Matriz Curricular, à Oferta de Disciplina ou pode ser derivada com possibilidade de ajuste por Turma?

### QA-ACA-003 — Atribuição docente

O Professor responsável por uma Oferta de Disciplina é definido obrigatoriamente antes da geração da grade ou pode fazer parte do processo de planejamento?

### QA-ACA-004 — Múltiplos professores

Uma mesma Oferta de Disciplina pode possuir mais de um Professor responsável?

### QA-ACA-005 — Turma sem Curso

Quais cenários escolares exigem Turmas sem vínculo com Curso e quais regras mudam nesses casos?
