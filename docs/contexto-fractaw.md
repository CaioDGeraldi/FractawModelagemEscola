# Contexto no FractawModules

## Objetivo

Este documento registra as fronteiras do FractawModules que devem ser respeitadas durante a modelagem dos requisitos de horário escolar.

Ele não define implementação, estrutura de banco de dados, padrões de projeto ou mecanismo de composição em runtime.

## Posição do domínio Escola

O sistema de horário escolar será incorporado ao FractawModules dentro do contexto do Tipo de Empresa Escola.

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
    ├── Cadastro de Disponibilidade
    └── Geração de Horários
```

A localização física definitiva dos conceitos escolares será decidida no repositório do FractawModules. Este repositório deve modelar responsabilidades e ownership de domínio, não diretórios Django.

## Tipo de Empresa

`TipoEmpresa` representa o contexto empresarial.

Para esta modelagem, ele responde perguntas como:

- quais capacidades fazem sentido para uma Escola;
- quais conceitos pertencem ao domínio escolar;
- quais políticas podem ser configuradas;
- quais módulos são aplicáveis.

Não deve ser tratado como:

- executor das regras da Escola;
- service locator;
- registry universal;
- objeto que concentra lógica de Catálogo, Parâmetros e módulos.

O mecanismo concreto de resolução desse contexto em runtime permanece uma decisão do FractawModules.

## Três categorias de informação

Durante a modelagem, todo conceito relevante deve ser classificado pela sua natureza.

### Referência estrutural

Responde principalmente:

> O que existe no domínio?

Possui identidade própria e pode ser referenciada por outros registros ou históricos.

Exemplos candidatos:

- Professor;
- Disciplina;
- Curso;
- Turma;
- Site;
- Sala;
- Laboratório;
- Período Letivo;
- Bloco de Aula.

A lista é provisória. A modelagem deve confirmar cada conceito.

### Política ou configuração

Responde principalmente:

> Como esta Empresa decidiu operar?

É candidata natural ao módulo-base Parâmetros.

Exemplos:

- prioridade mínima bloqueante;
- política de interpretação da escala 1–10;
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

O Catálogo da plataforma Fractaw não deve ser tratado neste projeto como um catálogo universal.

Ele possui responsabilidade própria relacionada a produtos e embalagens quando aplicável.

Professor, Turma, Disciplina e outras referências escolares não devem ser colocadas no Catálogo de Produtos apenas para reutilizar uma capacidade da plataforma.

## Parâmetros

Parâmetros é uma capacidade-base genérica do FractawModules.

No contexto Escola, ela pode armazenar políticas e configurações empresariais escolares, desde que o conceito seja de fato uma política e não uma referência estrutural ou estado operacional.

A Empresa mantém os valores efetivamente vigentes.

## Módulos escolares

### Cadastro de Disponibilidade

É proprietário do processo e do estado operacional relacionado à declaração, revisão e disponibilização da disponibilidade docente.

Deve permanecer utilizável independentemente da Geração de Horários.

### Geração de Horários

É proprietária do processo de geração, revisão, exceções e resultados da grade.

Pode consumir informações de Disponibilidade, referências escolares e Parâmetros, mas não deve assumir ownership dessas informações.

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
