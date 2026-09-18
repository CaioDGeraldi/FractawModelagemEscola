# Glossário do Domínio Escola

## Objetivo

Estabelecer um vocabulário comum para a modelagem do Tipo de Empresa Escola no FractawModules.

Os termos deste documento descrevem conceitos de domínio. Eles não determinam classes, tabelas, endpoints ou mecanismos técnicos.

## Termos

### Empresa

Tenant do FractawModules ao qual pertencem usuários, configurações e dados de negócio.

Uma Empresa pode estar associada ao Tipo de Empresa Escola.

### Tipo de Empresa Escola

Contexto de negócio que define quais conceitos, políticas e módulos são aplicáveis a uma Empresa escolar.

Não executa regras de negócio e não substitui os módulos do domínio.

### EmpresaUsuario

Vínculo entre um usuário da plataforma e uma Empresa.

É parte da plataforma FractawModules e não deve ser confundido com uma referência escolar como Professor.

### Papel empresarial

Papel de autoridade geral existente no FractawModules, como `PROPRIETARIO`, `ADMINISTRADOR_GERAL` ou `MEMBRO`.

O papel empresarial não determina sozinho quais operações escolares um usuário pode executar.

### Função escolar

Função exercida por uma pessoa dentro do contexto da instituição, como Diretor, Professor ou Coordenador.

Função escolar não é sinônimo de papel empresarial nem de autorização funcional.

### Autorização funcional

Permissão para executar uma capacidade específica de um módulo ou domínio.

Exemplos conceituais:

- gerenciar horários;
- gerar horários;
- informar a própria disponibilidade.

A forma técnica de representar essas autorizações pertence ao FractawModules e está fora do escopo desta modelagem.

### Diretor

Ator de negócio que exerce função de direção escolar.

Um Diretor pode operar Gestão de Horários quando possuir a autorização funcional necessária.

Ser Diretor não implica ser `PROPRIETARIO` ou `ADMINISTRADOR_GERAL`.

### Professor

Referência estrutural que representa um docente no domínio Escola.

Pode estar associado a disciplinas, disponibilidade, ofertas de disciplina e grades.

Quando um Professor utiliza o sistema, sua referência escolar deve poder ser relacionada à identidade de acesso correspondente sem que os dois conceitos se tornem a mesma coisa.

### Curso

Referência estrutural que representa uma formação ou organização acadêmica.

Pode possuir uma Matriz Curricular e organizar Turmas quando esse conceito fizer parte do modelo acadêmico da instituição.

### Disciplina

Referência estrutural que representa um componente curricular com identidade própria.

Uma mesma Disciplina pode participar de diferentes Cursos e diferentes Ofertas de Disciplina.

### Matriz Curricular

Relação acadêmica que descreve quais Disciplinas fazem parte de um Curso e quais regras curriculares se aplicam nessa relação.

Pode conter carga horária prevista, etapa, módulo ou outras informações curriculares confirmadas pelos requisitos.

### Turma

Referência estrutural que representa um grupo acadêmico concreto dentro de determinado Período Letivo.

Pode estar associada a Curso, Site e Turno conforme o modelo da instituição.

### Oferta de Disciplina

Relação que representa uma Disciplina que uma Turma precisa receber em determinado contexto letivo.

É uma entrada acadêmica para o planejamento de horários e não representa uma aula já alocada na grade.

### Habilitação docente

Relação entre Professor e Disciplina que indica que o Professor pode lecionar aquela Disciplina.

Não significa que ele esteja atribuído a uma Turma específica.

### Atribuição docente

Relação entre Professor e Oferta de Disciplina que indica responsabilidade por uma oferta concreta.

Uma Oferta pode possuir mais de um Professor atribuído.

### Co-docência

Situação em que uma mesma aula/alocação possui mais de um Professor simultaneamente.

A co-docência é opcional: a regra geral continua permitindo uma aula com apenas um Professor.

Professores atribuídos a uma Oferta não precisam necessariamente participar juntos de todas as aulas dessa Oferta.

### Período Letivo

Referência estrutural que delimita uma vigência acadêmica, como semestre ou ano letivo.

Possui início e término concretos; sua duração é consequência dessas datas.

Não deve ser confundido com Turno ou Bloco de Aula.

### Site

Referência estrutural que representa uma unidade física da Empresa Escola.

“Unidade Escolar” pode ser utilizado como termo de apresentação, mas `Site` é o termo adotado pela modelagem.

Um Site pode possuir Ambientes e relações de deslocamento com outros Sites.

### Ambiente

Referência estrutural que representa um espaço físico alocável dentro de um Site.

Sala comum, laboratório, auditório, quadra e oficina são exemplos de classificações ou características de Ambiente.

### Laboratório

Classificação ou conjunto de capacidades de um Ambiente preparado para atividades específicas.

Não é tratado como entidade estrutural paralela a Ambiente.

### Deslocamento entre Sites

Relação estrutural direcional que representa o tempo concreto necessário para ir de um Site de origem a outro Site da mesma Empresa.

Uma margem institucional adicional de deslocamento é política e pertence a Parâmetros.

### Turno

Referência temporal organizacional, como manhã, tarde ou noite.

Pode ser utilizada por Turmas e para organizar Blocos de Aula e Intervalos.

### Bloco de Aula

Faixa concreta e alocável de tempo utilizada por Disponibilidade e Gestão de Horários.

Possui horário de início e término. Sua duração real é derivada desses horários.

### Intervalo

Faixa concreta e não alocável dentro da organização temporal, como recreio ou pausa entre Blocos.

Intervalo é parte da estrutura temporal real da instituição.

### Interstício

Tempo mínimo livre exigido por política entre determinadas atividades.

Interstício não é sinônimo de Intervalo: um Intervalo pode satisfazer um Interstício, mas o primeiro é dado estrutural e o segundo é política/configuração.

### Disponibilidade

Módulo funcional responsável pelo processo de declaração e manutenção da disponibilidade docente.

Também pode designar, em contexto específico, a informação operacional produzida por esse processo.

### Prioridade de ausência

Valor informado no contexto de Disponibilidade para representar a importância ou força de uma restrição de ausência.

Seu significado operacional é definido pela política vigente da Empresa.

### Parâmetro

Política ou configuração que define como uma Empresa decidiu operar dentro do contexto Escola.

Parâmetros não devem absorver referências estruturais ou estado operacional.

### Gestão de Horários

Módulo funcional responsável pelo planejamento, geração, revisão, exceções, remontagem e resultados da grade escolar.

### Grade de Horários

Resultado estruturado da alocação de aulas em Professores, Turmas, Blocos de Aula e Ambientes.

### Exceção de planejamento

Situação identificada durante o planejamento que impede ou compromete uma alocação e exige tratamento explícito ou intervenção humana.

## Regra terminológica

```text
papel empresarial
≠ função escolar
≠ autorização funcional
```

```text
Professor
≠ Usuario
≠ EmpresaUsuario
```

```text
Matriz Curricular
≠ Oferta de Disciplina

Habilitação docente
≠ Atribuição docente
```

```text
Período Letivo
≠ Turno
≠ Bloco de Aula
≠ Intervalo

Intervalo
≠ Interstício
```

Os conceitos podem estar relacionados, mas não representam a mesma responsabilidade de domínio.
