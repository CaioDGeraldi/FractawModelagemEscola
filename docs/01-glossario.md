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

Pode estar associado a disciplinas, turmas, disponibilidade e grades.

Quando um Professor utiliza o sistema, sua referência escolar deve poder ser relacionada à identidade de acesso correspondente sem que os dois conceitos se tornem a mesma coisa.

### Curso

Referência estrutural que representa uma formação ou organização acadêmica à qual turmas e componentes curriculares podem estar relacionados.

Sua necessidade e granularidade definitiva devem ser confirmadas pelos requisitos.

### Disciplina

Referência estrutural que representa um componente curricular que pode possuir carga horária, requisitos de ambiente e relações com professores e turmas.

### Turma

Referência estrutural que representa um grupo acadêmico para o qual aulas e disciplinas são planejadas dentro de determinado contexto letivo.

### Período Letivo

Referência estrutural que delimita uma vigência acadêmica, como semestre ou ano letivo.

Não deve ser confundido com turno ou bloco de aula.

### Site / Unidade Escolar

Local físico ou unidade da Empresa Escola em que atividades acadêmicas podem ocorrer.

Pode possuir salas, laboratórios e relações de deslocamento com outros Sites.

### Sala

Ambiente físico utilizado para atividades acadêmicas.

Sua relação com Laboratório ainda deve ser refinada pela modelagem.

### Laboratório

Ambiente físico com características ou recursos específicos exigidos por determinadas atividades ou disciplinas.

A modelagem ainda deve decidir se Laboratório é uma especialização de Sala ou um conceito estrutural independente.

### Bloco de Aula

Intervalo estrutural de tempo no qual uma aula pode ser alocada.

Se possuir identidade e for referenciado por disponibilidade, turmas e grades, deve ser tratado como referência estrutural e não como simples Parâmetro.

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

Resultado estruturado da alocação de aulas em professores, turmas, horários e ambientes.

### Exceção de planejamento

Situação identificada durante o planejamento que impede ou compromete uma alocação e exige tratamento explícito ou intervenção humana.

## Regra terminológica

Os seguintes conceitos devem permanecer distintos:

```text
papel empresarial
≠ função escolar
≠ autorização funcional
```

Da mesma forma:

```text
Professor
≠ Usuario
≠ EmpresaUsuario
```

Os conceitos podem estar relacionados, mas não representam a mesma responsabilidade de domínio.
