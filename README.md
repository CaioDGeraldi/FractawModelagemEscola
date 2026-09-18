# FractawModelagemEscola

Repositório de modelagem do **Tipo de Empresa Escola** no FractawModules.

Este projeto registra o domínio escolar, suas referências estruturais, políticas/configurações reais, atores e módulos funcionais.

O objetivo é definir responsabilidades, regras, requisitos e fronteiras de domínio antes das decisões de implementação no FractawModules.

## Relação com o FractawModules

Esta modelagem respeita as decisões arquiteturais vigentes do FractawModules:

- `Empresa` é o tenant;
- `TipoEmpresa` define contexto e aplicabilidade, mas não executa regras de domínio;
- a plataforma não depende de código específico da Escola;
- referências próprias da Escola não pertencem ao Catálogo de Produtos;
- Parâmetros é uma capacidade-base genérica planejada para a F08;
- Cargo representa função organizacional e não autorização;
- módulos escolares mantêm seus próprios processos e estados operacionais;
- aplicabilidade, habilitação empresarial e concessão ao Membro são conceitos distintos;
- o mecanismo técnico de composição e persistência pertence ao FractawModules.

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
```

## Regra de classificação

Antes de decidir onde um conceito pertence, a modelagem deve identificar sua natureza:

- **referência estrutural:** algo que existe no domínio e possui identidade própria;
- **política/configuração:** define como uma Empresa decidiu operar;
- **estado operacional:** fato ou resultado pertencente a um processo ou módulo.

A classificação é feita pelo significado do conceito, não pela conveniência de implementação.

Em particular:

```text
configurável
≠ automaticamente Parâmetro

política/default
≠ referência/ocorrência concreta
```

## Identidade e autorização

A modelagem preserva:

```text
Cargo
≠ Papel empresarial
≠ Autoridade modular
≠ Permissão funcional
```

E também:

```text
Professor estrutural
≠ Cargo Professor
≠ Usuario
≠ EmpresaUsuario
```

Diretor e Coordenador são tratados como funções organizacionais compatíveis com Cargo enquanto não existir requisito para entidade de domínio independente.

## Contrato modular

Os módulos escolares respeitam:

```text
aplicabilidade
≠ habilitação
≠ concessão
```

Ser aplicável ao TipoEmpresa Escola ou existir no código não habilita automaticamente o módulo para uma Empresa.

No contrato vigente, depois dos gates de vínculo ativo, aplicabilidade e habilitação:

```text
PROPRIETARIO          → ADMINISTRADOR no módulo
ADMINISTRADOR_GERAL   → ADMINISTRADOR no módulo
MEMBRO                 → depende de concessão USUARIO ou ADMINISTRADOR
```

Permissões funcionais mais granulares permanecem uma necessidade a ser definida quando casos de uso concretos exigirem.

## Estado atual

Os primeiros módulos funcionais em modelagem são:

- **Disponibilidade**;
- **Gestão de Horários**.

A base acadêmica distingue:

- Curso e Turma;
- Matriz Curricular e Oferta de Disciplina;
- Habilitação docente e Atribuição docente;
- Professores atribuídos à Oferta e Professores participantes de cada aula;
- co-docência opcional.

A base física e temporal distingue:

- Site e Ambiente;
- Sala/Laboratório como classificações ou capacidades de Ambiente;
- Deslocamento estrutural específico entre Sites e margem geral de deslocamento;
- Período Letivo, Turno e Bloco de Aula como referências/ocorrências concretas;
- políticas/defaults temporais reais de ocorrências concretas configuráveis.

A geração automática da grade é uma responsabilidade de Gestão de Horários, não o nome do módulo como um todo.

## Limite deste repositório

Este projeto não escolhe prematuramente schema de Parâmetros, persistência de permissões granulares, associação técnica Professor ↔ EmpresaUsuario, Registry, Manifest, Dependency Graph, service locator ou mecanismos de runtime por TipoEmpresa.

Questões sem requisito suficiente permanecem explicitamente abertas.