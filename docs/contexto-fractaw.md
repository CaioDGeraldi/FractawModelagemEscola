# Contexto no FractawModules

## Objetivo

Este documento registra as fronteiras vigentes do FractawModules que devem ser respeitadas durante toda a modelagem do Tipo de Empresa Escola.

Ele não define implementação, estrutura de banco de dados, padrões de projeto ou mecanismo de composição em runtime.

As decisões vivas em `FractawModules/docs/decisions/` e `FractawModules/docs/roadmap.md` prevalecem sobre o corpus histórico em `docs/reconstrucao/`.

## Posição do Tipo de Empresa Escola

A modelagem deste repositório representa o contexto Escola dentro do FractawModules.

Conceitualmente:

```text
FractawModules
├── Plataforma
│   ├── Identidade / Empresas / Equipe
│   ├── Launcher
│   ├── Modularidade
│   ├── Catálogo de Produtos
│   └── Parâmetros (F08)
│
└── Tipo de Empresa: Escola
    ├── referências estruturais escolares
    └── módulos
        ├── Disponibilidade
        └── Gestão de Horários
```

A localização física definitiva dos conceitos escolares será decidida no repositório do FractawModules. Este repositório modela requisitos, responsabilidades e ownership de domínio, não diretórios Django.

## Empresa e TipoEmpresa

`Empresa` é o tenant.

`TipoEmpresa` representa contexto e aplicabilidade. Para esta modelagem, determina conceitualmente:

- quais conceitos pertencem ao domínio Escola;
- quais políticas/configurações fazem sentido;
- quais módulos são aplicáveis;
- quais Cargos organizacionais são compatíveis com o contexto Escola.

`TipoEmpresa` não deve ser tratado como:

- executor das regras da Escola;
- service locator;
- Registry ou Manifest;
- objeto que concentra lógica de referências, Parâmetros ou módulos;
- mecanismo que habilita módulos automaticamente.

O mecanismo concreto de composição permanece responsabilidade do FractawModules.

## Cargo, identidade e autorização

O FractawModules já possui `Cargo` como classificação da função organizacional exercida pela pessoa dentro de um `TipoEmpresa`.

Exemplos escolares compatíveis com Cargo incluem:

```text
Professor
Diretor
Coordenador
```

Cargo não concede autorização.

Também não deve ser criado, sem requisito próprio, um subsistema paralelo `FunçãoEscolar` que represente novamente a mesma classificação organizacional.

Uma distinção especial deve ser preservada para Professor:

```text
Professor
→ referência estrutural do domínio Escola

Cargo Professor
→ função organizacional de um EmpresaUsuario
```

Esses conceitos podem estar associados quando o docente possui acesso ao Fractaw, mas não são equivalentes. Um Professor pode existir sem `Usuario`, sem `EmpresaUsuario` e sem acesso ao sistema.

A representação técnica da associação `Professor ↔ EmpresaUsuario` permanece em aberto.

## Modularidade: aplicabilidade, habilitação e concessão

Os módulos escolares obedecem ao contrato vigente de Modularidade:

```text
módulo aplicável ao TipoEmpresa Escola
≠
módulo habilitado para determinada Empresa
≠
módulo concedido a determinado MEMBRO
```

Ser aplicável nunca habilita automaticamente um módulo.

A presença do código no deploy também não habilita a Empresa.

Para Membros, uma concessão somente produz autoridade efetiva quando o vínculo está ativo, o módulo é aplicável e o módulo está efetivamente habilitado para a mesma Empresa.

Conceitualmente:

```text
Gestão de Horários
→ aplicável a Escola

Escola A
→ pode habilitar Gestão de Horários

Escola B
→ pode permanecer sem Gestão de Horários habilitado

MEMBRO da Escola A
→ depende de concessão modular
```

## Papel empresarial e autoridade modular

O contrato atual do FractawModules distingue papel empresarial de autoridade modular.

Depois dos gates de vínculo ativo, aplicabilidade e habilitação:

```text
PROPRIETARIO
→ autoridade modular efetiva ADMINISTRADOR

ADMINISTRADOR_GERAL
→ autoridade modular efetiva ADMINISTRADOR

MEMBRO
→ depende de concessão UsuarioModulo
   ├── USUARIO
   └── ADMINISTRADOR
```

Cargo não participa dessa resolução.

Esse contrato não encerra a futura necessidade de permissões funcionais granulares dentro dos módulos escolares.

Gestão de Horários pode vir a distinguir operações como gerar, revisar, remontar, publicar e tratar exceções. Disponibilidade pode vir a distinguir operações sobre a própria disponibilidade e eventual administração da disponibilidade de terceiros.

Permanece em aberto como essas futuras permissões se relacionarão com a autoridade modular `ADMINISTRADOR`, inclusive se Proprietário e Administrador Geral receberão automaticamente todas as operações funcionais ou autoridade administrativa suficiente para gerenciá-las.

Este repositório registra a necessidade, mas não inventa schema ou mecanismo de permissão.

## Capacidade empresarial não é permissão

No FractawModules, `Capacidade` possui significado arquitetural específico: entitlement empresarial para recurso opcional, limite ou vigência.

Portanto:

```text
Capacidade empresarial
≠ autoridade modular
≠ permissão funcional do usuário
```

Na modelagem Escola, quando o assunto for autorização de uma operação, devem ser utilizados termos como `autoridade modular`, `permissão funcional` ou `operação autorizada`, sem redefinir `Capacidade`.

## Três categorias de informação

Durante a modelagem, todo conceito relevante deve ser classificado pela sua natureza.

### Referência estrutural

Responde principalmente:

> O que existe no domínio?

Possui identidade própria e pode ser referenciada por outros registros ou históricos.

Exemplos consolidados:

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

É candidata à capacidade-base Parâmetros quando sua natureza for realmente política, default, limite ou configuração empresarial compartilhável.

Exemplos fortes:

- prioridade mínima bloqueante;
- política de interpretação da prioridade;
- duração padrão de aula;
- máximo de aulas consecutivas;
- interstício institucional;
- margem/tolerância geral de deslocamento.

`Configurável` não é critério suficiente para transformar uma referência ou ocorrência concreta em Parâmetro.

### Estado operacional

Responde principalmente:

> O que aconteceu ou está acontecendo em um processo?

Pertence ao módulo responsável pelo processo.

Exemplos:

- disponibilidade declarada;
- tentativa de geração;
- exceção de planejamento;
- grade gerada;
- versão publicada.

## Catálogo de Produtos

O Catálogo de Produtos da plataforma não é catálogo universal.

Professor, Turma, Disciplina e outras referências escolares permanecem no domínio Escola e não devem ser movidas para o Catálogo de Produtos por conveniência de reutilização.

## Parâmetros e F08

Parâmetros é uma capacidade-base genérica planejada para a F08.

A modelagem Escola já revela que políticas podem exigir semântica ou escopo contextual, por exemplo Empresa, Site, Turno ou Período Letivo.

Isso é requisito para a futura F08, não uma decisão de implementação.

A solução futura deve preservar:

```text
plataforma
    ↑
Escola
```

A plataforma não pode passar a depender diretamente de models escolares apenas para representar o escopo de uma política.

Permanecem deliberadamente em aberto schema, persistência, precedência, versionamento e mecanismo de resolução de Parâmetros.

## Módulos escolares

### Disponibilidade

É proprietária do processo e do estado operacional relacionado à disponibilidade docente.

Deve permanecer utilizável independentemente de Gestão de Horários.

### Gestão de Horários

É proprietária do planejamento, geração, revisão, exceções e resultados da grade.

Pode consumir referências escolares, Parâmetros e uma fonte compatível de disponibilidade sem assumir ownership dessas informações.

Gestão de Horários não possui dependência obrigatória do módulo Disponibilidade.

## Regra de dependência

A direção conceitual deve preservar:

```text
plataforma
    ↑
Escola
```

A plataforma não deve importar nem conhecer código específico da Escola.

## Regra para este repositório

Sempre que surgir um novo conceito, a modelagem deve responder, nesta ordem:

1. O conceito possui identidade própria no domínio?
2. Sua natureza é realmente política/configuração da Empresa?
3. É um fato ou estado produzido por um processo?
4. Qual domínio é proprietário desse conceito?
5. Outros módulos apenas consultam esse conceito ou também são responsáveis por ele?
6. O conceito já possui significado arquitetural próprio no FractawModules?

Somente depois dessa classificação deve ser discutida sua futura representação técnica.