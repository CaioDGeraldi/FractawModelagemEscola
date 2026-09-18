# Visão Geral — Tipo de Empresa Escola

## 1. Propósito

Definir o escopo conceitual do Tipo de Empresa Escola no FractawModules e estabelecer as fronteiras que orientarão a modelagem de seus dados, políticas, atores e módulos.

Este documento não define estrutura de banco de dados, classes Django, APIs ou mecanismo de composição em runtime.

## 2. Papel do Tipo de Empresa Escola

O Tipo de Empresa Escola fornece o contexto de negócio necessário para representar instituições escolares dentro do FractawModules.

Esse contexto determina quais conceitos, políticas e módulos fazem sentido para uma Empresa classificada como Escola.

`TipoEmpresa` não executa regras de negócio e não concentra a implementação dos módulos. Ele define contexto e aplicabilidade.

## 3. Composição conceitual

A modelagem da Escola distingue três categorias principais:

```text
Tipo de Empresa Escola
│
├── referências estruturais
│   └── o que existe no domínio
│
├── parâmetros aplicáveis
│   └── como a Empresa decidiu operar
│
└── módulos funcionais
    └── processos e estado operacional
```

### 3.1 Referências estruturais

São conceitos do domínio escolar que possuem identidade própria e podem ser referenciados por diferentes processos ou históricos.

As referências estruturais atualmente consolidadas incluem:

- Professor;
- Curso;
- Disciplina;
- Turma;
- Período Letivo;
- Site;
- Ambiente;
- Turno;
- Bloco de Aula.

Também existem relações estruturais com significado próprio, como:

- Matriz Curricular;
- Oferta de Disciplina;
- Habilitação docente;
- Atribuição docente;
- Deslocamento entre Sites.

Sala comum, laboratório, auditório e outros espaços são tratados como classificações ou capacidades de Ambiente, não como entidades estruturais paralelas.

Essas referências não pertencem ao Catálogo de Produtos da plataforma.

### 3.2 Parâmetros aplicáveis à Escola

Parâmetros é uma capacidade-base genérica do FractawModules.

A modelagem da Escola identifica quais políticas e configurações são necessárias no contexto escolar, qual sua semântica e quais módulos podem consumi-las.

A Empresa mantém os valores efetivamente vigentes.

Exemplos já identificados como candidatos:

- política de interpretação da prioridade de ausência;
- prioridade mínima bloqueante;
- limites institucionais de alocação;
- máximo de aulas consecutivas;
- margens ou tolerâncias gerais utilizadas no planejamento.

Um conceito não se torna parâmetro apenas porque é configurável.

### 3.3 Módulos funcionais

Até o momento, dois módulos escolares estão em modelagem:

#### Disponibilidade

Responsável pelo processo de declaração, manutenção e disponibilização da disponibilidade docente.

#### Gestão de Horários

Responsável pelo planejamento, geração, revisão, exceções, remontagem e resultados da grade de horários.

Os módulos mantêm ownership de seu próprio estado operacional e podem consumir referências e políticas por contratos adequados.

## 4. Atores e autorização

A modelagem separa três dimensões:

```text
papel empresarial
≠ função escolar
≠ autorização funcional
```

Um Diretor pode possuir papel empresarial `MEMBRO` e, separadamente, receber autorização para operar Gestão de Horários.

Da mesma forma, possuir papel `PROPRIETARIO` ou `ADMINISTRADOR_GERAL` não significa automaticamente poder gerar ou gerenciar horários.

O Professor é uma referência estrutural do domínio e, quando atua como usuário do sistema, é o ator responsável por informar sua própria disponibilidade no fluxo normal.

A forma técnica de autorização permanece responsabilidade do FractawModules.

## 5. Fronteiras com a plataforma

A Escola pode depender de capacidades públicas da plataforma FractawModules.

A plataforma não deve depender do domínio específico da Escola.

```text
plataforma
    ↑
Escola
```

O Catálogo de Produtos permanece uma capacidade própria da plataforma e não é o repositório das referências escolares.

Parâmetros permanece uma capacidade-base genérica e recebe semântica concreta conforme os requisitos do contexto Escola.

## 6. Escopo atual

Faz parte da modelagem atual:

- referências e relações acadêmicas;
- estrutura física e temporal;
- atores e responsabilidades;
- separação entre função escolar e autorização funcional;
- políticas escolares configuráveis;
- módulo Disponibilidade;
- módulo Gestão de Horários;
- integração conceitual entre esses elementos;
- regras de negócio e restrições compartilhadas quando houver ownership claro.

Não faz parte deste repositório:

- implementação do FractawModules;
- decisões internas de persistência;
- escolha de padrões técnicos para resolver o contexto de `TipoEmpresa`;
- implementação do mecanismo de autorização;
- redefinição do Catálogo de Produtos;
- funcionalidades escolares ainda não modeladas e sem requisitos definidos.

## 7. Princípios de modelagem

### MOD-001 — Ownership explícito

Todo conceito deve possuir um domínio proprietário claro.

### MOD-002 — Contexto não executa domínio

`TipoEmpresa` define contexto e aplicabilidade, mas não executa as regras dos módulos.

### MOD-003 — Plataforma independente da Escola

Nenhuma regra exclusiva da Escola deve ser necessária para o funcionamento da plataforma compartilhada.

### MOD-004 — Natureza antes da implementação

A classificação entre referência, política e estado operacional deve ser feita pelo significado do conceito.

### MOD-005 — Autonomia modular

Módulos escolares não devem assumir ownership do estado interno de outros módulos.

Integrações podem existir por contratos explícitos.

### MOD-006 — Autorização por capacidade

Papel empresarial e função escolar não devem ser usados como substitutos de autorização funcional.

Operações de módulo devem depender da capacidade funcional necessária, sem acoplamento obrigatório a títulos como Diretor ou a papéis empresariais específicos.

## 8. Questões estruturais em aberto

### QE-001 — Atribuição docente

Em que momento o Professor responsável por uma Oferta de Disciplina deve ser definido?

### QE-002 — Matriz Curricular

Como vigência e versionamento curricular devem funcionar?

### QE-003 — Escopo temporal

Turnos e Blocos podem ser compartilhados entre Sites ou cada Site precisa de sua própria organização temporal?

### QE-004 — Recursos de Ambiente

Como representar tipos, capacidades e recursos de Ambiente sem criar classificações rígidas demais?

### QE-005 — Granularidade das autorizações

Quais capacidades funcionais precisam ser distinguidas em Disponibilidade e Gestão de Horários?

### QE-006 — Evolução do Company Type

Novos módulos escolares deverão ser incorporados sem exigir que os módulos existentes assumam responsabilidades que não lhes pertencem.
