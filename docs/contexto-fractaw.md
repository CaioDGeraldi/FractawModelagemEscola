# Contexto no FractawModules

## Objetivo

Este documento registra as fronteiras do FractawModules que devem ser respeitadas durante toda a modelagem do Tipo de Empresa Escola.

Ele não define implementação, estrutura de banco de dados, padrões de projeto ou mecanismo de composição em runtime.

## Posição do Tipo de Empresa Escola

A modelagem deste repositório representa o contexto Escola dentro do FractawModules.

Conceitualmente:

```text
FractawModules
├── Plataforma
│   ├── Launcher
│   ├── Catálogo de Produtos
│   └── Parâmetros
│
└── Tipo de Empresa: Escola
    ├── referências estruturais escolares
    └── módulos
        ├── Disponibilidade
        └── Gestão de Horários
```

A localização física definitiva dos conceitos escolares será decidida no repositório do FractawModules. Este repositório modela responsabilidades e ownership de domínio, não diretórios Django.

## Tipo de Empresa

`TipoEmpresa` representa o contexto empresarial.

Para esta modelagem, ele determina conceitualmente:

- quais capacidades fazem sentido para uma Escola;
- quais conceitos pertencem ao domínio escolar;
- quais políticas podem ser configuradas;
- quais módulos são aplicáveis.

Não deve ser tratado como:

- executor das regras da Escola;
- service locator;
- registry universal;
- objeto que concentra lógica de referências, Parâmetros e módulos.

O mecanismo concreto de resolução desse contexto em runtime permanece uma decisão do FractawModules.

## Três categorias de informação

Durante a modelagem, todo conceito relevante deve ser classificado pela sua natureza.

### Referência estrutural

Responde principalmente:

> O que existe no domínio?

Possui identidade própria e pode ser referenciada por outros registros ou históricos.

Exemplos consolidados nesta modelagem:

- Professor;
- Curso;
- Disciplina;
- Turma;
- Período Letivo;
- Site;
- Ambiente;
- Turno;
- Bloco de Aula.

Também podem existir relações estruturais com significado próprio, como Matriz Curricular, Oferta de Disciplina, Habilitação docente, Atribuição docente e Deslocamento entre Sites.

### Política ou configuração

Responde principalmente:

> Como esta Empresa decidiu operar?

É candidata natural à capacidade-base Parâmetros.

Exemplos:

- prioridade mínima bloqueante;
- política de interpretação da escala de prioridade;
- máximo de aulas consecutivas;
- tolerância ou margem geral de deslocamento.

### Estado operacional

Responde principalmente:

> O que aconteceu ou está acontecendo em um processo?

Pertence ao módulo responsável pelo processo.

Exemplos:

- disponibilidade declarada;
- submissão de disponibilidade;
- tentativa de geração;
- exceção de planejamento;
- grade gerada;
- versão publicada.

## Catálogo de Produtos

O Catálogo da plataforma Fractaw não deve ser tratado neste projeto como catálogo universal.

Ele possui responsabilidade própria relacionada a produtos e embalagens quando aplicável.

Professor, Turma, Disciplina e outras referências escolares não devem ser colocadas no Catálogo de Produtos apenas para reutilizar uma capacidade da plataforma.

## Parâmetros

Parâmetros é uma capacidade-base genérica do FractawModules.

No contexto Escola, ela pode armazenar políticas e configurações empresariais escolares quando o conceito for de fato uma política e não uma referência estrutural ou estado operacional.

A Empresa mantém os valores efetivamente vigentes.

## Módulos escolares

### Disponibilidade

É proprietária do processo e do estado operacional relacionado à declaração, revisão e disponibilização da disponibilidade docente.

Deve permanecer utilizável independentemente de Gestão de Horários.

### Gestão de Horários

É proprietária do planejamento, geração, revisão, exceções e resultados da grade.

Pode consumir informações de Disponibilidade, referências escolares e Parâmetros, mas não assume ownership dessas informações.

## Regra de dependência

A direção conceitual deve preservar:

```text
plataforma
    ↑
Escola
```

A plataforma não deve precisar conhecer conceitos específicos da Escola.

## Regra para este repositório

Sempre que surgir um novo conceito, a modelagem deve responder, nesta ordem:

1. O conceito possui identidade própria no domínio?
2. É uma política/configuração da Empresa?
3. É um fato ou estado produzido por um processo?
4. Qual domínio é proprietário desse conceito?
5. Outros módulos apenas consultam esse conceito ou também são responsáveis por ele?

Somente depois dessa classificação deve ser discutida sua futura representação no FractawModules.
