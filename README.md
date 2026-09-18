# FractawModelagemEscola

Repositório de modelagem do **Tipo de Empresa Escola** no FractawModules.

Este projeto registra o domínio escolar, suas referências estruturais, as políticas que precisam ser fornecidas pela capacidade genérica de Parâmetros e os módulos funcionais próprios da Escola.

O objetivo é definir responsabilidades, regras, requisitos e fronteiras de domínio antes das decisões de implementação no FractawModules.

## Relação com o FractawModules

Esta modelagem respeita as decisões arquiteturais vigentes do FractawModules:

- `Empresa` é o tenant;
- `TipoEmpresa` define contexto e aplicabilidade, mas não executa regras de domínio;
- a plataforma não depende de código específico da Escola;
- referências próprias da Escola não pertencem ao Catálogo de Produtos;
- Parâmetros é uma capacidade-base genérica do FractawModules;
- módulos escolares mantêm seus próprios processos e estados operacionais;
- o mecanismo técnico de composição por `TipoEmpresa` é responsabilidade do FractawModules.

O documento `docs/contexto-fractaw.md` registra essas fronteiras.

## Escopo da modelagem

A modelagem do Tipo de Empresa Escola é organizada em áreas complementares:

```text
Escola
├── referências estruturais
├── modelo acadêmico
├── modelo físico e temporal
├── parâmetros aplicáveis
└── módulos
    ├── Disponibilidade
    └── Gestão de Horários
```

Novos módulos escolares poderão ser adicionados quando seus requisitos forem modelados.

## Organização

```text
docs/
├── contexto-fractaw.md
├── 00-visao-geral-escola.md
├── 01-glossario.md
├── 02-atores-e-partes-interessadas.md
├── 03-referencias-estruturais.md
├── 04-parametros.md
├── 05-modelo-academico.md
├── 06-modelo-fisico-temporal.md
│
└── modulos/
    ├── disponibilidade/
    │   └── 00-visao-geral.md
    │
    └── gestao-horarios/
        └── 00-visao-geral.md

diagramas/
└── arquivos de modelagem adicionados conforme necessidade
```

Os arquivos previstos serão adicionados de forma incremental conforme cada etapa for desenvolvida e revisada.

## Regra de classificação

Antes de decidir onde um conceito pertence, a modelagem deve identificar sua natureza:

- **referência estrutural:** algo que existe no domínio e possui identidade própria;
- **política/configuração:** define como uma Empresa decidiu operar;
- **estado operacional:** fato ou resultado pertencente a um processo ou módulo.

A classificação é feita pelo significado do conceito, não pela conveniência de implementação.

## Estado atual

Os primeiros módulos funcionais em modelagem são:

- **Disponibilidade**;
- **Gestão de Horários**.

A base acadêmica já distingue:

- Curso e Disciplina;
- Matriz Curricular e Oferta de Disciplina;
- Habilitação docente e Atribuição docente.

A base física e temporal já distingue:

- Site e Ambiente;
- classificação/capacidades de Ambiente em vez de entidades paralelas para Sala e Laboratório;
- Deslocamento estrutural entre Sites e margem institucional de deslocamento;
- Período Letivo, Turno e Bloco de Aula.

A geração automática da grade é uma responsabilidade de Gestão de Horários, não o nome do módulo como um todo.
