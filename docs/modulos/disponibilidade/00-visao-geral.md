# Visão Geral — Disponibilidade

## 1. Propósito

Modelar o módulo escolar responsável pela disponibilidade docente.

Disponibilidade é um módulo funcional do Tipo de Empresa Escola e deve permanecer independente de Gestão de Horários.

## 2. Responsabilidade

O módulo é proprietário do processo e do estado operacional relacionados à declaração e manutenção da disponibilidade dos Professores.

Seu resultado deve poder ser consumido por Gestão de Horários ou por outro consumidor compatível.

## 3. Modularidade

Disponibilidade deve respeitar o contrato vigente do FractawModules:

```text
aplicável ao TipoEmpresa Escola
≠ habilitado para determinada Empresa
≠ concedido a determinado MEMBRO
```

A existência do módulo no código ou sua aplicabilidade à Escola não o habilita automaticamente para todas as Empresas Escola.

Depois dos gates de vínculo ativo, aplicabilidade e habilitação, `PROPRIETARIO` e `ADMINISTRADOR_GERAL` possuem autoridade modular `ADMINISTRADOR` derivada. Membros dependem de concessão modular.

Isso não define ainda permissões funcionais granulares para operações como informar, alterar ou administrar disponibilidade.

## 4. Escopo atual

A modelagem contempla:

- disponibilidade por período de vigência;
- cadastro ou declaração da disponibilidade;
- alteração quando permitida;
- prioridade de ausência;
- autoria e responsabilidade sobre a informação;
- disponibilização dos dados para consumidores compatíveis.

O detalhamento de revisão, aprovação, publicação ou versionamento deve ser confirmado pela modelagem específica do módulo antes de ser tratado como requisito definitivo.

## 5. Professor e identidade de acesso

A disponibilidade pertence conceitualmente ao Professor estrutural.

Entretanto:

```text
Professor estrutural
≠ Cargo Professor
≠ Usuario
≠ EmpresaUsuario
```

Quando o Professor opera o módulo, sua referência estrutural deve poder ser associada ao vínculo empresarial que está executando a operação.

A representação técnica dessa associação permanece em aberto.

Possuir Cargo Professor, por si só, não concede acesso ao módulo.

## 6. Prioridade de ausência

A prioridade utiliza uma escala de 1 a 10.

Disponibilidade registra o valor informado.

O significado operacional da escala pertence à política vigente da Empresa quando essa interpretação for configurada em Parâmetros.

O módulo não decide sozinho quais níveis são bloqueantes ou negociáveis.

## 7. Independência

Disponibilidade não depende de Gestão de Horários para existir.

Conceitualmente:

```text
Disponibilidade
    ├── pode alimentar Gestão de Horários
    └── pode alimentar outro consumidor compatível
```

Da mesma forma, Gestão de Horários pode aceitar disponibilidade proveniente de outra fonte compatível.

Não deve existir dependência obrigatória `Gestão de Horários → Disponibilidade`.

## 8. Ownership

Pertencem a Disponibilidade:

- declarações de disponibilidade;
- prioridades registradas;
- alterações do processo de disponibilidade;
- demais estados próprios desse processo que forem confirmados.

Não pertencem a Disponibilidade:

- Professor enquanto referência estrutural;
- Cargo Professor;
- definição institucional do significado das prioridades;
- autoridade modular e concessões da plataforma;
- tentativas de geração de grade;
- conflitos e exceções de planejamento;
- versões da grade.

## 9. Operações e autorização

O fluxo normal prevê que o Professor informe e mantenha sua própria disponibilidade quando estiver autorizado a operar o módulo.

Uma futura necessidade de permissões mais granulares pode distinguir, por exemplo:

- informar a própria disponibilidade;
- alterar a própria disponibilidade;
- consultar histórico próprio;
- administrar ou corrigir disponibilidade de terceiros.

Essas permissões ainda não estão definidas.

A modelagem também não decide se autoridade modular `ADMINISTRADOR` implica automaticamente todas essas operações ou apenas autoridade administrativa suficiente para gerenciá-las.

## 10. Questões em aberto

### QD-001 — Granularidade

A prioridade é atribuída a um dia inteiro, a um Bloco específico ou pode existir nos dois níveis?

### QD-002 — Alteração durante a vigência

Quando uma disponibilidade já utilizada por uma grade for alterada, qual efeito deve ser produzido sobre os consumidores desse dado?

### QD-003 — Ciclo de publicação

O módulo exige submissão, revisão, aprovação e snapshot publicado ou pode disponibilizar diretamente o estado vigente?

Essa questão deve ser decidida pelos requisitos, sem importar automaticamente o comportamento do baseline histórico.

### QD-004 — Administração por terceiros

Deve existir operação administrativa para corrigir ou registrar disponibilidade em nome de um Professor? Quem pode executá-la?

### QD-005 — Permissões granulares

Quais operações do módulo exigirão permissão funcional própria além da autoridade modular mínima vigente?