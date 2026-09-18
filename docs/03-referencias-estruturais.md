# Referências Estruturais da Escola

## 1. Objetivo

Identificar e classificar os conceitos do domínio Escola que possuem identidade própria e servem como referência para diferentes processos, módulos ou históricos.

Este documento não define models, tabelas ou APIs.

## 2. Critério de pertencimento

Um conceito é referência estrutural quando:

- representa algo que existe no domínio escolar;
- possui identidade própria;
- pode ser referenciado por diferentes processos;
- precisa permanecer semanticamente compreensível em históricos;
- não representa apenas uma política ou uma execução operacional.

Ser configurável não transforma automaticamente um conceito em Parâmetro.

## 3. Referências estruturais iniciais

### REF-001 — Professor

Representa um docente da Empresa Escola.

Pode:

- possuir Habilitações docentes;
- possuir Disponibilidade;
- receber Atribuições docentes;
- aparecer em alocações de uma Grade de Horários;
- possuir vínculo com uma identidade de acesso à plataforma.

Professor existe como conceito escolar independentemente de possuir usuário no sistema.

`Professor`, `Usuario` e `EmpresaUsuario` não são o mesmo conceito.

### REF-002 — Curso

Representa uma formação ou organização acadêmica.

Pode possuir uma Matriz Curricular e organizar Turmas quando o modelo acadêmico da instituição utilizar Curso.

Curso não é obrigatório para representar toda forma possível de Turma escolar.

### REF-003 — Disciplina

Representa um componente curricular com identidade própria.

A mesma Disciplina pode participar de diferentes Cursos e diferentes Ofertas de Disciplina.

Disciplina não deve ser duplicada apenas porque aparece em mais de um Curso.

### REF-004 — Turma

Representa um grupo acadêmico concreto para o qual atividades são planejadas.

Deve estar relacionada a um Período Letivo.

Pode estar relacionada a:

- Curso, quando aplicável;
- Site de referência;
- Ofertas de Disciplina;
- alocações da Grade de Horários.

### REF-005 — Período Letivo

Representa a vigência acadêmica utilizada para organizar atividades, Turmas, disponibilidade e planejamento.

Pode corresponder, por exemplo, a semestre ou ano letivo conforme a instituição.

Período Letivo não deve ser confundido com turno, período do dia ou Bloco de Aula.

### REF-006 — Site / Unidade Escolar

Representa uma unidade física da Empresa Escola.

Pode possuir:

- Salas;
- Laboratórios;
- Turmas;
- relações de deslocamento com outros Sites.

O termo definitivo entre `Site` e `Unidade Escolar` ainda pode ser refinado no glossário.

### REF-007 — Sala

Representa um ambiente físico em que atividades acadêmicas podem ocorrer.

Pode possuir características relevantes para alocação.

A relação entre Sala e Laboratório permanece em análise.

### REF-008 — Laboratório

Representa um ambiente com recursos ou características específicas necessários a determinadas atividades.

Ainda deve ser decidido se Laboratório é uma especialização de Sala ou uma referência independente.

### REF-009 — Bloco de Aula

Representa uma unidade estrutural de tempo disponível para alocação acadêmica.

Pode possuir:

- horário de início;
- horário de término;
- posição dentro de um turno ou jornada;
- identidade utilizada por Disponibilidade e Gestão de Horários.

Por possuir identidade própria e ser referenciado por processos diferentes, é tratado inicialmente como referência estrutural.

## 4. Relações acadêmicas estruturais

O detalhamento principal destas relações está em `05-modelo-academico.md`.

### Matriz Curricular — Curso ↔ Disciplina

Representa quais Disciplinas fazem parte de um Curso e quais propriedades pertencem a essa relação curricular.

Pode registrar, conforme os requisitos:

- carga horária prevista;
- etapa, módulo ou série;
- obrigatoriedade;
- outras regras curriculares.

### Oferta de Disciplina — Turma ↔ Disciplina

Representa uma Disciplina que uma Turma precisa receber em determinado contexto letivo.

A Oferta existe antes da montagem da grade e constitui entrada para Gestão de Horários.

Não representa uma aula já alocada.

### Habilitação docente — Professor ↔ Disciplina

Representa que um Professor pode lecionar determinada Disciplina.

Habilitação não significa responsabilidade por uma Turma específica.

### Atribuição docente — Professor ↔ Oferta de Disciplina

Representa que um Professor é responsável por uma Oferta de Disciplina concreta.

O momento em que a atribuição é definida ainda deve ser confirmado pelos requisitos de Gestão de Horários.

### Deslocamento entre Sites

Uma relação específica entre Site de origem e Site de destino, com tempo de deslocamento aplicável, é candidata a dado estrutural.

Exemplo:

```text
Site A → Site B = 35 minutos
```

Uma margem geral adicional de deslocamento, por outro lado, representa política e é candidata a Parâmetro.

## 5. Conceitos que não são referências estruturais

### Diretor

Diretor é uma função/ator escolar e não uma referência estrutural necessária ao planejamento.

Caso futuros requisitos exijam identidade de domínio própria para direção escolar, essa classificação poderá ser revista.

### Papel empresarial

`PROPRIETARIO`, `ADMINISTRADOR_GERAL` e `MEMBRO` pertencem à plataforma FractawModules.

### Autorizações funcionais

Representam capacidades de acesso e não referências acadêmicas.

### Disponibilidade declarada

É estado operacional do módulo Disponibilidade.

### Grade, tentativa e exceção

São estados ou resultados pertencentes a Gestão de Horários.

## 6. Relação com o Catálogo de Produtos

O Catálogo de Produtos da plataforma FractawModules não é proprietário das referências escolares.

Conceitos como Professor, Turma e Disciplina pertencem ao domínio Escola.

## 7. Relação com módulos

Módulos podem consumir referências sem assumir ownership sobre elas.

```text
Professor
├── Disponibilidade
└── Gestão de Horários

Oferta de Disciplina
└── Gestão de Horários

Bloco de Aula
├── Disponibilidade
└── Gestão de Horários
```

## 8. Invariantes iniciais

### INV-REF-001

Referências escolares utilizadas em conjunto devem pertencer à mesma Empresa.

### INV-REF-002

Uma referência estrutural não muda de domínio proprietário apenas porque um módulo específico a utiliza com maior frequência.

### INV-REF-003

Dados históricos devem continuar apontando para referências semanticamente identificáveis, mesmo que essas referências deixem de estar ativas para novas operações.

### INV-REF-004

Políticas institucionais não devem ser incorporadas artificialmente às referências estruturais quando pertencem à responsabilidade de Parâmetros.

### INV-REF-005

Habilitação docente e Atribuição docente possuem significados distintos.

### INV-REF-006

Matriz Curricular e Oferta de Disciplina possuem significados distintos.

## 9. Questões em aberto

### QR-001 — Sala e Laboratório

Laboratório é uma especialização de Sala ou conceito independente?

### QR-002 — Bloco de Aula

Blocos são definidos globalmente para a Empresa, por Site, por turno ou por outro contexto?

### QR-003 — Atribuição docente

O Professor responsável por uma Oferta de Disciplina deve estar definido antes da geração da grade ou pode ser decidido durante o planejamento?

### QR-004 — Deslocamento

A relação de deslocamento é direcional e pode possuir tempos diferentes entre A → B e B → A?

### QR-005 — Matriz Curricular

Como vigência e versionamento da Matriz Curricular devem ser representados conceitualmente?

## 10. Próximas definições

A modelagem deverá aprofundar:

- Sala, Laboratório e recursos de ambiente;
- Bloco de Aula e organização temporal;
- Sites e deslocamento;
- regras de vigência acadêmica;
- atribuição docente;
- cardinalidades e ciclo de vida das relações acadêmicas.
