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
          ▼
  Oferta de Disciplina
          │
          └── 0..* Professores atribuídos
```

As relações possuem significado próprio e não devem ser reduzidas apenas por conveniência de implementação.

## 3. Curso e Disciplina

### DEC-ACA-001 — Disciplina possui identidade independente de Curso

Uma Disciplina não pertence exclusivamente a um Curso.

A mesma Disciplina pode participar de diferentes Cursos, inclusive com regras curriculares diferentes.

### Matriz Curricular

A relação entre Curso e Disciplina representa a presença daquela Disciplina na organização curricular do Curso.

Pode registrar, conforme os requisitos:

- carga horária prevista;
- módulo, série ou etapa;
- obrigatoriedade;
- outras regras curriculares.

## 4. Turma

### DEC-ACA-002 — Turma representa uma ocorrência acadêmica concreta

Turma não é sinônimo de Curso.

Curso representa a formação ou organização acadêmica; Turma representa um grupo concreto dentro de determinado contexto letivo.

Uma Turma deve estar vinculada a um Período Letivo.

Quando o modelo acadêmico da instituição utilizar Curso, a Turma pode estar vinculada ao Curso correspondente.

## 5. Turma e Disciplina

### DEC-ACA-003 — Currículo e oferta concreta são conceitos diferentes

A Matriz Curricular responde:

> Quais Disciplinas fazem parte de um Curso?

A Oferta de Disciplina responde:

> Quais Disciplinas esta Turma precisa receber neste contexto letivo?

### Oferta de Disciplina

`Oferta de Disciplina` representa a necessidade concreta de uma Disciplina para uma Turma.

Pode conter, conforme os requisitos:

- Turma;
- Disciplina;
- carga horária a cumprir;
- requisitos de Ambiente;
- Professores atribuídos;
- outras condições necessárias à oferta.

A Oferta existe antes da montagem da grade e constitui entrada para Gestão de Horários.

Ela não representa uma aula já alocada em determinado dia e Bloco.

## 6. Professor, Cargo e identidade de acesso

### DEC-ACA-004 — Professor estrutural não é Cargo nem identidade de acesso

Professor representa um docente no domínio acadêmico e existe independentemente de acesso ao Fractaw.

Devem permanecer distintos:

```text
Professor estrutural
≠ Cargo Professor
≠ Usuario
≠ EmpresaUsuario
```

`Cargo Professor` classifica a função organizacional de um vínculo empresarial no TipoEmpresa Escola.

A referência estrutural Professor participa do modelo acadêmico, da Disponibilidade e das Grades mesmo quando não existe conta de acesso correspondente.

Quando o docente possui acesso ao sistema, deve ser possível relacionar sua referência Professor ao EmpresaUsuario correspondente sem tornar os conceitos equivalentes.

A forma técnica e a persistência dessa associação permanecem em aberto.

## 7. Professor e Disciplina

### DEC-ACA-005 — Habilitação docente e atribuição docente são conceitos diferentes

### Habilitação docente

Responde:

> Este Professor pode lecionar esta Disciplina?

É uma relação estrutural entre Professor e Disciplina.

A habilitação não significa responsabilidade por uma Turma específica.

### Atribuição docente

Responde:

> Quais Professores estão atribuídos a esta Oferta de Disciplina?

Uma Oferta pode possuir nenhum, um ou vários Professores atribuídos antes do planejamento, dependendo do fluxo adotado.

```text
Oferta de Disciplina
├── Professor A
└── Professor B
```

A existência de mais de um Professor não significa necessariamente que todos participarão de todas as aulas da Oferta.

## 8. Co-docência

### DEC-ACA-006 — Uma aula pode possuir mais de um Professor

Uma aula efetivamente alocada pode possuir um ou mais Professores simultaneamente.

Isso permite representar co-docência sem tornar dois Professores obrigatórios para todas as aulas.

Conceitualmente:

```text
Aula / Alocação
├── Professor A
└── Professor B   ← opcional
```

Para uma aula válida deve existir pelo menos um Professor responsável, salvo casos futuros explicitamente modelados de atividade sem docente.

Quando houver co-docência:

- todos os Professores participantes precisam estar disponíveis no mesmo Bloco;
- todos ficam ocupados durante a mesma alocação;
- restrições de deslocamento e Interstício se aplicam individualmente a cada Professor;
- a alocação continua pertencendo à mesma Oferta de Disciplina.

A modelagem distingue, portanto:

```text
Professores atribuídos à Oferta
≠ necessariamente
Professores participantes de cada Aula
```

Isso permite que uma Oferta tenha dois Professores responsáveis, mas apenas determinadas aulas utilizem os dois simultaneamente.

## 9. Professor e Turma

Não deve existir uma relação direta genérica `Professor → Turma` sem significado específico.

O relacionamento ocorre pela atividade acadêmica:

```text
Professor
   ↓
Oferta de Disciplina / Aula
   ↓
Turma
```

## 10. Relação com Gestão de Horários

Gestão de Horários consome o modelo acadêmico, mas não se torna proprietária dele.

Entradas conceituais relevantes incluem:

- Turma;
- Oferta de Disciplina;
- carga horária exigida;
- Professores habilitados ou previamente atribuídos;
- Período Letivo;
- Blocos de Aula;
- Ambientes e recursos;
- disponibilidade proveniente de fonte compatível;
- políticas fornecidas por Parâmetros.

Gestão de Horários define quais Professores participam de cada alocação quando isso fizer parte do planejamento.

O resultado da geração é operacional e pertence ao módulo.

## 11. Invariantes iniciais

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

Uma aula alocada deve possuir pelo menos um Professor responsável, quando se tratar de atividade docente.

### INV-ACA-007

Todos os Professores de uma aula em co-docência devem estar simultaneamente aptos àquela alocação segundo Disponibilidade, deslocamento e demais restrições aplicáveis.

### INV-ACA-008

A exclusão ou inativação de uma referência não deve tornar históricos acadêmicos ou grades publicadas semanticamente incompreensíveis.

### INV-ACA-009

Cargo Professor, Usuario e EmpresaUsuario não substituem a identidade acadêmica da referência Professor.

## 12. Cardinalidades conceituais iniciais

```text
Curso                0..* ↔ 0..* Disciplina
Curso                0..1 ← 0..* Turma
Período Letivo       1    ← 0..* Turma
Turma                 1    ← 0..* Oferta de Disciplina
Disciplina            1    ← 0..* Oferta de Disciplina
Professor            0..* ↔ 0..* Disciplina (habilitação)
Professor            0..* ↔ 0..* Oferta de Disciplina (atribuição)
Aula / Alocação      1..* ↔ 0..* Professor
```

As cardinalidades podem ser refinadas quando regras institucionais mais específicas forem formalizadas.

A cardinalidade e a representação da associação `Professor ↔ EmpresaUsuario` não são definidas neste documento.

## 13. Questões em aberto

### QA-ACA-001 — Matriz Curricular

A Matriz Curricular precisa de versionamento por vigência ou Período Letivo?

### QA-ACA-002 — Carga horária

A carga horária efetiva para planejamento pertence à Matriz Curricular, à Oferta de Disciplina ou pode ser derivada com possibilidade de ajuste por Turma?

### QA-ACA-003 — Atribuição docente

Os Professores responsáveis por uma Oferta são definidos obrigatoriamente antes da geração ou podem fazer parte do processo de planejamento?

### QA-ACA-004 — Co-docência por aula

Quando uma Oferta possui vários Professores atribuídos, como é determinado quais deles participam juntos de cada Aula?

### QA-ACA-005 — Turma sem Curso

Quais cenários escolares exigem Turmas sem vínculo com Curso e quais regras mudam nesses casos?

### QA-ACA-006 — Associação Professor e EmpresaUsuario

Qual é a forma adequada de relacionar a referência Professor ao vínculo empresarial de acesso sem duplicar identidade ou acoplar o domínio acadêmico à autenticação?