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

Relações conceituais já identificadas:

- pode estar habilitado a lecionar uma ou mais Disciplinas;
- pode possuir Disponibilidade;
- pode aparecer em alocações de uma Grade de Horários;
- pode possuir vínculo com uma identidade de acesso à plataforma.

Professor existe como conceito escolar independentemente de possuir usuário no sistema.

`Professor`, `Usuario` e `EmpresaUsuario` não são o mesmo conceito.

### REF-002 — Curso

Representa uma organização ou formação acadêmica utilizada para estruturar a oferta escolar.

Pode organizar Turmas e Disciplinas conforme o modelo acadêmico adotado pela Empresa.

A obrigatoriedade e a granularidade de Curso ainda devem ser confirmadas.

### REF-003 — Disciplina

Representa um componente curricular.

Pode possuir:

- carga horária;
- relação com Professores;
- relação com Turmas ou Cursos;
- requisitos específicos de ambiente ou recurso.

Regras de carga horária e requisitos ainda serão detalhadas.

### REF-004 — Turma

Representa um grupo acadêmico para o qual aulas são planejadas.

Pode estar relacionada a:

- Curso;
- Período Letivo;
- Site;
- Disciplinas;
- alocações da Grade de Horários.

### REF-005 — Período Letivo

Representa a vigência acadêmica utilizada para organizar atividades, turmas, disponibilidade e planejamento.

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

Questão em aberto:

- Laboratório é uma especialização de Sala;
- ou deve permanecer como referência independente?

A modelagem deverá escolher com base nas regras do domínio, não na estrutura de banco de dados.

### REF-009 — Bloco de Aula

Representa uma unidade estrutural de tempo disponível para alocação acadêmica.

Pode possuir:

- horário de início;
- horário de término;
- posição dentro de um turno ou jornada;
- identidade utilizada por Disponibilidade e Gestão de Horários.

Por possuir potencial identidade e ser referenciado por processos diferentes, é tratado inicialmente como referência estrutural.

## 4. Relações estruturais candidatas

Algumas relações também possuem relevância de domínio e podem exigir identidade própria.

### Professor ↔ Disciplina

Representa quais Disciplinas um Professor está apto ou designado a lecionar.

Ainda deve ser definido se existe diferença entre habilitação, preferência e atribuição efetiva.

### Turma ↔ Disciplina

Representa quais Disciplinas precisam ser ofertadas para uma Turma e em qual carga horária.

Essa relação provavelmente será uma entrada importante de Gestão de Horários.

### Deslocamento entre Sites

Uma relação específica entre Site de origem e Site de destino, com tempo de deslocamento aplicável, é candidata a dado estrutural.

Exemplo conceitual:

```text
Site A → Site B = 35 minutos
```

Já uma margem geral adicionada ao deslocamento representa política e é candidata a Parâmetro.

## 5. Conceitos que não são referências estruturais

### Diretor

Diretor é, inicialmente, uma função/ator escolar e não uma referência estrutural necessária ao planejamento.

Caso futuros requisitos exijam dados próprios de direção com identidade de domínio, essa classificação poderá ser revista.

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

Turma
└── Gestão de Horários

Bloco de Aula
├── Disponibilidade
└── Gestão de Horários
```

## 8. Invariantes iniciais

### INV-REF-001

Referências escolares utilizadas em conjunto devem pertencer à mesma Empresa.

### INV-REF-002

Uma referência estrutural não deve mudar de domínio proprietário apenas porque um módulo específico a utiliza com maior frequência.

### INV-REF-003

Dados históricos devem continuar apontando para referências semanticamente identificáveis, mesmo que essas referências deixem de estar ativas para novas operações.

### INV-REF-004

Políticas institucionais não devem ser incorporadas artificialmente às referências estruturais quando pertencem à responsabilidade de Parâmetros.

## 9. Questões em aberto

### QR-001 — Curso

Toda Turma necessariamente pertence a um Curso?

### QR-002 — Sala e Laboratório

Laboratório é uma especialização de Sala ou conceito independente?

### QR-003 — Bloco de Aula

Blocos são definidos globalmente para a Empresa, por Site, por turno ou por outro contexto?

### QR-004 — Professor e Disciplina

A relação representa habilitação, atribuição, preferência ou mais de um desses conceitos?

### QR-005 — Deslocamento

A relação de deslocamento é direcional e pode possuir tempos diferentes entre A → B e B → A?

## 10. Próximas definições

Para cada referência confirmada, a modelagem deverá aprofundar:

- identidade;
- relações;
- cardinalidades conceituais;
- invariantes;
- ciclo de vida;
- consumidores;
- regras que pertencem à própria referência;
- regras que pertencem a Parâmetros ou módulos.
