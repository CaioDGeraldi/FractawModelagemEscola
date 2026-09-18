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

Da mesma forma, possuir um valor temporal não transforma automaticamente um conceito em Parâmetro.

## 3. Referências estruturais iniciais

### REF-001 — Professor

Representa um docente da Empresa Escola.

Pode:

- possuir Habilitações docentes;
- possuir Disponibilidade;
- receber Atribuições docentes;
- aparecer em alocações de uma Grade de Horários;
- possuir associação com uma identidade de acesso à plataforma.

Professor existe como conceito escolar independentemente de possuir usuário no sistema.

```text
Professor estrutural
≠ Cargo Professor
≠ Usuario
≠ EmpresaUsuario
```

`Cargo Professor` representa função organizacional no vínculo empresarial; Professor representa a referência do domínio acadêmico.

A forma técnica de associar Professor e EmpresaUsuario permanece em aberto.

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
- Turno;
- Ofertas de Disciplina;
- alocações da Grade de Horários.

### REF-005 — Período Letivo

Representa uma vigência acadêmica concreta utilizada para organizar Turmas, disponibilidade, ofertas e planejamento.

Pode corresponder, por exemplo, a semestre ou ano letivo conforme a instituição.

Possui identidade e datas efetivas próprias.

Uma regra de duração padrão pode ser candidata a Parâmetros, mas o Período Letivo concreto continua sendo referência estrutural.

### REF-006 — Site

Representa uma unidade física da Empresa Escola.

`Site` é o termo estrutural adotado pela modelagem; “Unidade Escolar” pode ser utilizado como termo de apresentação.

Pode possuir Ambientes e relações de deslocamento com outros Sites.

### REF-007 — Ambiente

Representa um espaço físico alocável dentro de um Site.

Exemplos:

- sala comum;
- laboratório de informática;
- laboratório de química;
- auditório;
- quadra;
- oficina.

Sala e Laboratório não são tratados como entidades estruturais paralelas. São formas de classificar ou caracterizar um Ambiente.

A modelagem ainda deve detalhar como tipos, capacidades e recursos de Ambiente serão expressos conceitualmente.

### REF-008 — Turno

Representa uma organização recorrente de parte da jornada escolar, como manhã, tarde ou noite.

Pode organizar Blocos de Aula e ser utilizado como referência por Turmas e regras de planejamento.

Defaults gerais de horário podem ser políticas, mas o Turno concreto não se torna Parâmetro apenas por possuir horários configuráveis.

### REF-009 — Bloco de Aula

Representa uma faixa concreta e alocável de tempo.

Possui, conceitualmente:

- horário de início;
- horário de término;
- ordem dentro de um Turno;
- identidade utilizada por Disponibilidade e Gestão de Horários.

Uma duração padrão de aula pode ser candidata a Parâmetros, mas um Bloco concreto como `09:00–09:50` permanece referência estrutural.

## 4. Relações acadêmicas estruturais

O detalhamento principal destas relações está em `05-modelo-academico.md`.

### Matriz Curricular — Curso ↔ Disciplina

Representa quais Disciplinas fazem parte de um Curso e quais propriedades pertencem a essa relação curricular.

### Oferta de Disciplina — Turma ↔ Disciplina

Representa uma Disciplina que uma Turma precisa receber em determinado contexto letivo.

A Oferta existe antes da montagem da grade e constitui entrada para Gestão de Horários.

### Habilitação docente — Professor ↔ Disciplina

Representa que um Professor pode lecionar determinada Disciplina.

Habilitação não significa responsabilidade por uma Turma específica.

### Atribuição docente — Professor ↔ Oferta de Disciplina

Representa que um Professor é responsável por uma Oferta de Disciplina concreta.

O momento em que a atribuição é definida ainda deve ser confirmado pelos requisitos de Gestão de Horários.

## 5. Relações físicas estruturais

O detalhamento principal está em `06-modelo-fisico-temporal.md`.

### Deslocamento entre Sites

Representa uma relação direcional concreta entre um Site de origem e um Site de destino.

Nesta modelagem, o tempo específico necessário para esse par faz parte dos dados próprios da relação estrutural:

```text
Site A → Site B = 35 min
Site B → Site A = 45 min
```

O valor pode diferir por sentido.

Isso não impede que a Empresa possua políticas gerais aplicadas sobre esses dados, por exemplo:

```text
margem geral de deslocamento = 10 min
```

A margem geral é candidata natural a Parâmetros.

## 6. Conceitos que não são referências estruturais

### Diretor e Coordenador

Enquanto representarem somente função organizacional, são conceitos compatíveis com `Cargo` do TipoEmpresa Escola e não exigem entidade estrutural paralela.

A classificação poderá ser revista se futuros requisitos demonstrarem identidade de domínio independente.

### Papel empresarial

`PROPRIETARIO`, `ADMINISTRADOR_GERAL` e `MEMBRO` pertencem à plataforma FractawModules.

### Autoridade modular e permissões funcionais

Representam autorização e não referências acadêmicas.

### Disponibilidade declarada

É estado operacional do módulo Disponibilidade.

### Grade, tentativa e exceção

São estados ou resultados pertencentes a Gestão de Horários.

### Políticas, defaults, margens e limites institucionais

Pertencem conceitualmente a Parâmetros quando sua natureza for realmente política/configuração empresarial compartilhável.

Ser configurável, isoladamente, não basta.

## 7. Relação com o Catálogo de Produtos

O Catálogo de Produtos da plataforma FractawModules não é proprietário das referências escolares.

As referências deste documento pertencem ao domínio Escola.

## 8. Relação com módulos

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

Ambiente
└── Gestão de Horários

Site / Deslocamento
└── Gestão de Horários
```

## 9. Invariantes iniciais

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

### INV-REF-007

Ambiente pertence a um Site e não pode ser alocado como se pertencesse a outro Site.

### INV-REF-008

Deslocamento entre Sites não é presumido como zero quando não houver relação conhecida.

### INV-REF-009

Disponibilidade e Gestão de Horários devem referenciar a mesma identidade de Bloco de Aula quando representarem a mesma faixa alocável.

## 10. Questões em aberto

### QR-001 — Atribuição docente

O Professor responsável por uma Oferta de Disciplina deve estar definido antes da geração da grade ou pode ser decidido durante o planejamento?

### QR-002 — Matriz Curricular

Como vigência e versionamento da Matriz Curricular devem ser representados conceitualmente?

### QR-003 — Escopo temporal

A mesma definição de Turno e Blocos pode ser compartilhada entre Sites ou cada Site precisa de sua própria organização temporal?

### QR-004 — Recursos de Ambiente

Como representar requisitos de recursos e capacidades sem criar tipos excessivamente rígidos?

### QR-005 — Turma integral

Como uma Turma que opera em mais de um Turno deve ser representada?

### QR-006 — Intervalos concretos

Intervalos precisam de identidade própria em algum caso real ou podem permanecer como parte da organização temporal concreta sem entidade estrutural independente?

## 11. Próximas definições

A modelagem deverá aprofundar:

- recursos e capacidades de Ambiente;
- escopo de Turnos e Blocos entre Sites;
- atribuição docente;
- vigência de Matriz Curricular;
- regras específicas consumidas por Disponibilidade e Gestão de Horários.