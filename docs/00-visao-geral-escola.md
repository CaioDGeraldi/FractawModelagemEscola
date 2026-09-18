# Visão Geral — Tipo de Empresa Escola

## 1. Propósito

Definir o escopo conceitual do Tipo de Empresa Escola no FractawModules e estabelecer as fronteiras que orientarão a modelagem de seus dados, políticas, atores e módulos.

Este documento não define estrutura de banco de dados, classes Django, APIs ou mecanismo de composição em runtime.

## 2. Papel do Tipo de Empresa Escola

O Tipo de Empresa Escola fornece o contexto de negócio necessário para representar instituições escolares dentro do FractawModules.

Esse contexto determina quais conceitos, políticas e módulos fazem sentido para uma Empresa classificada como Escola.

`TipoEmpresa` não executa regras de negócio, não concentra implementação dos módulos e não habilita módulos automaticamente. Ele define contexto e aplicabilidade.

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

A modelagem da Escola identifica políticas, defaults, limites e configurações empresariais compartilháveis cuja natureza realmente responda a como a Empresa decidiu operar.

Exemplos fortes:

- política de interpretação da prioridade de ausência;
- prioridade mínima bloqueante;
- duração padrão de aula;
- máximo de aulas consecutivas;
- interstício institucional;
- margem ou tolerância geral de deslocamento.

`Configurável` não é critério suficiente para transformar um conceito em Parâmetro.

Assim:

```text
duração padrão de aula = 50 min
→ política/configuração candidata a Parâmetros

Bloco 3 = 09:00–09:50
→ referência concreta

Período Letivo 2027.1 = datas concretas
→ referência concreta
```

### 3.3 Módulos funcionais

Até o momento, dois módulos escolares estão em modelagem:

#### Disponibilidade

Responsável pelo processo de declaração, manutenção e disponibilização da disponibilidade docente.

#### Gestão de Horários

Responsável pelo planejamento, geração, revisão, exceções, remontagem e resultados da grade de horários.

Os módulos mantêm ownership de seu próprio estado operacional e podem consumir referências e políticas por contratos adequados.

## 4. Identidade organizacional e autorização

A modelagem respeita as dimensões já existentes no FractawModules:

```text
Cargo
→ função organizacional

Papel empresarial
→ PROPRIETARIO / ADMINISTRADOR_GERAL / MEMBRO

Autoridade modular
→ USUARIO / ADMINISTRADOR, conforme o contrato vigente

Permissão funcional
→ operação concreta, quando requisitos futuros exigirem granularidade adicional
```

Cargo não concede autorização.

Diretor e Coordenador, salvo requisito futuro de entidade de domínio independente, são funções organizacionais compatíveis com `Cargo` no TipoEmpresa Escola.

Professor exige uma distinção adicional:

```text
Professor estrutural
≠ Cargo Professor
≠ Usuario
≠ EmpresaUsuario
```

Um Professor pode existir no domínio sem possuir acesso ao Fractaw.

Quando possuir acesso, a associação entre Professor e EmpresaUsuario deverá ser representável, mas sua persistência permanece em aberto.

## 5. Contrato modular

A Escola deve preservar:

```text
aplicabilidade
≠ habilitação
≠ concessão
```

Ser aplicável ao TipoEmpresa Escola não habilita o módulo para todas as Empresas Escola.

A existência do código também não habilita uma Empresa.

Depois dos gates de vínculo ativo, aplicabilidade e habilitação, o contrato atual é:

```text
PROPRIETARIO
→ autoridade modular ADMINISTRADOR

ADMINISTRADOR_GERAL
→ autoridade modular ADMINISTRADOR

MEMBRO
→ depende de concessão modular USUARIO ou ADMINISTRADOR
```

Isso não resolve antecipadamente permissões funcionais granulares dentro de Disponibilidade ou Gestão de Horários.

Ainda deve ser decidido, quando os requisitos exigirem, como operações como gerar, revisar, publicar ou corrigir disponibilidade se relacionam com a autoridade modular atual.

## 6. Fronteiras com a plataforma

A Escola pode depender de capacidades públicas da plataforma FractawModules.

A plataforma não deve depender do domínio específico da Escola.

```text
plataforma
    ↑
Escola
```

O Catálogo de Produtos permanece uma capacidade própria da plataforma e não é o repositório das referências escolares.

Parâmetros permanece uma capacidade-base genérica e recebe semântica concreta conforme os requisitos do contexto Escola, sem depender diretamente de models escolares.

## 7. Escopo atual

Faz parte da modelagem atual:

- referências e relações acadêmicas;
- estrutura física e temporal;
- atores e responsabilidades;
- relação entre Cargo, Professor estrutural e identidade de acesso;
- requisitos de autorização e modularidade aplicáveis aos módulos escolares;
- políticas escolares configuráveis;
- módulo Disponibilidade;
- módulo Gestão de Horários;
- integração conceitual entre esses elementos;
- regras de negócio e restrições compartilhadas quando houver ownership claro.

Não faz parte deste repositório:

- implementação do FractawModules;
- decisões internas de persistência;
- escolha de padrões técnicos para resolver o contexto de `TipoEmpresa`;
- schema de permissões funcionais granulares;
- schema ou mecanismo de Parâmetros;
- redefinição do Catálogo de Produtos;
- funcionalidades escolares ainda não modeladas e sem requisitos definidos.

## 8. Princípios de modelagem

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

### MOD-006 — Cargo não é autorização

Função organizacional e autorização respondem a perguntas diferentes.

### MOD-007 — Aplicabilidade não habilita

Um módulo aplicável ao TipoEmpresa Escola somente é utilizável por uma Empresa quando o contrato vigente de habilitação e autoridade também for satisfeito.

### MOD-008 — Configurável não significa Parâmetro

Um valor ou conceito configurável continua estrutural ou operacional quando sua natureza assim determinar.

## 9. Questões estruturais em aberto

### QE-001 — Atribuição docente

Em que momento o Professor responsável por uma Oferta de Disciplina deve ser definido?

### QE-002 — Matriz Curricular

Como vigência e versionamento curricular devem funcionar?

### QE-003 — Escopo temporal

Turnos e Blocos podem ser compartilhados entre Sites ou cada Site precisa de sua própria organização temporal?

### QE-004 — Recursos de Ambiente

Como representar tipos, capacidades e recursos de Ambiente sem criar classificações rígidas demais?

### QE-005 — Granularidade das permissões funcionais

Quais operações de Disponibilidade e Gestão de Horários exigirão permissões mais granulares que a autoridade modular mínima atual?

### QE-006 — Relação com autoridade administrativa

Quando permissões granulares existirem, `PROPRIETARIO` e `ADMINISTRADOR_GERAL`, hoje administradores derivados de todo módulo habilitado e aplicável, receberão automaticamente todas as operações funcionais ou apenas autoridade administrativa suficiente para gerenciá-las?

### QE-007 — Evolução do Company Type

Novos módulos escolares deverão ser incorporados sem exigir que os módulos existentes assumam responsabilidades que não lhes pertencem.