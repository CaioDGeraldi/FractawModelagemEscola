# Glossário do Domínio Escola

## Objetivo

Estabelecer um vocabulário comum para a modelagem do Tipo de Empresa Escola no FractawModules.

Os termos deste documento descrevem conceitos de domínio e contratos arquiteturais vigentes. Eles não determinam classes, tabelas, endpoints ou mecanismos técnicos além do que já foi decidido no FractawModules.

## Termos

### Empresa

Tenant do FractawModules ao qual pertencem vínculos, configurações e dados de negócio.

Uma Empresa pode estar associada ao Tipo de Empresa Escola.

### Tipo de Empresa Escola

Contexto de negócio que define quais conceitos, políticas, Cargos e módulos são aplicáveis a uma Empresa escolar.

Não executa regras de negócio e não habilita módulos automaticamente.

### Usuario

Identidade global da pessoa no FractawModules.

Não representa, por si só, Professor, Cargo, vínculo empresarial ou autorização dentro de uma Empresa.

### EmpresaUsuario

Vínculo entre um Usuario e uma Empresa.

É o contexto empresarial ao qual Cargo, papel empresarial e concessões modulares pertencem.

### Cargo

Classificação da função organizacional exercida pela pessoa dentro de um TipoEmpresa.

Exemplos escolares podem incluir `Professor`, `Diretor` e `Coordenador`.

Cargo não concede autorização.

O termo descritivo “função escolar” não representa um subsistema separado de Cargo.

### Papel empresarial

Posição administrativa transversal do vínculo empresarial:

- `PROPRIETARIO`;
- `ADMINISTRADOR_GERAL`;
- `MEMBRO`.

Papel empresarial não é Cargo.

No contrato modular vigente, Proprietário e Administrador Geral derivam autoridade `ADMINISTRADOR` nos módulos efetivamente habilitados e aplicáveis.

### Autoridade modular

Nível de autoridade dentro de um módulo.

Para Membros, os níveis mínimos vigentes são:

- `USUARIO`;
- `ADMINISTRADOR`.

Proprietário e Administrador Geral recebem autoridade modular `ADMINISTRADOR` por derivação do papel empresarial quando o módulo satisfaz os gates de aplicabilidade e habilitação.

### Permissão funcional

Autorização para uma operação concreta dentro de um módulo, quando um requisito exigir granularidade além da autoridade modular mínima.

Exemplos potenciais em Gestão de Horários incluem gerar, revisar, remontar ou publicar uma grade.

O schema e a relação dessas permissões com a autoridade modular ainda não estão definidos.

### Capacidade

Entitlement empresarial do FractawModules para recurso opcional, limite ou vigência.

Não deve ser utilizado como sinônimo de autoridade modular ou permissão funcional do usuário.

### Aplicabilidade

Compatibilidade conceitual de um módulo com determinado TipoEmpresa.

Ser aplicável ao TipoEmpresa Escola não significa estar habilitado para todas as Empresas Escola.

### Habilitação de módulo

Estado empresarial que torna um módulo efetivamente habilitado para determinada Empresa, desde que permaneça aplicável.

A existência do código ou a aplicabilidade não substituem a habilitação.

### Concessão modular

Concessão pertencente a um EmpresaUsuario `MEMBRO` para um módulo habilitado e aplicável.

Pode resultar em autoridade modular `USUARIO` ou `ADMINISTRADOR` conforme o contrato vigente.

### Diretor

Função organizacional escolar compatível com Cargo.

Diretor não é sinônimo de `PROPRIETARIO` nem de `ADMINISTRADOR_GERAL`.

Um Diretor com papel empresarial `MEMBRO` pode receber concessão modular e operar Gestão de Horários conforme a autoridade e as futuras permissões funcionais aplicáveis.

Não existe, nesta modelagem, uma entidade estrutural `Diretor` separada apenas para representar a função organizacional.

### Professor

Referência estrutural que representa um docente no domínio Escola.

Pode estar associado a disciplinas, disponibilidade, ofertas de disciplina e grades.

A referência estrutural Professor permanece distinta de `Cargo Professor`, `Usuario` e `EmpresaUsuario`.

Quando o docente possui acesso ao Fractaw, pode existir uma associação entre Professor e EmpresaUsuario. A forma técnica dessa associação permanece em aberto.

### Curso

Referência estrutural que representa uma formação ou organização acadêmica.

Pode possuir uma Matriz Curricular e organizar Turmas quando esse conceito fizer parte do modelo acadêmico da instituição.

### Disciplina

Referência estrutural que representa um componente curricular com identidade própria.

Uma mesma Disciplina pode participar de diferentes Cursos e Ofertas de Disciplina.

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

A co-docência é opcional. Professores atribuídos a uma Oferta não precisam necessariamente participar juntos de todas as aulas dessa Oferta.

### Período Letivo

Referência estrutural que delimita uma vigência acadêmica concreta, como semestre ou ano letivo.

Possui identidade e datas efetivas próprias.

Uma política de duração padrão ou regra geral de calendário pode ser candidata a Parâmetros, mas as datas concretas do Período Letivo não se tornam Parâmetro apenas por serem configuráveis.

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

Relação estrutural direcional entre dois Sites da mesma Empresa.

O tempo específico necessário para o par, como `Site A → Site B = 35 min`, é tratado nesta modelagem como dado próprio da relação estrutural.

Uma margem ou tolerância geral aplicada aos deslocamentos é política candidata a Parâmetros.

### Turno

Referência temporal organizacional, como manhã, tarde ou noite.

Pode ser utilizada por Turmas e organizar Blocos de Aula.

Horários concretos pertencem à organização do Turno; defaults ou políticas gerais de horário podem ser candidatos a Parâmetros quando houver requisito compartilhável.

### Bloco de Aula

Referência concreta e alocável de tempo utilizada por Disponibilidade e Gestão de Horários.

Possui identidade, horário de início e horário de término.

Uma duração padrão de aula pode ser Parâmetro, mas `Bloco 3 = 09:00–09:50` continua sendo referência concreta.

### Intervalo

Faixa concreta não alocável dentro da organização temporal escolar, como recreio ou pausa entre Blocos.

A modelagem não exige que Intervalo possua entidade estrutural independente.

Uma duração padrão, regra geral ou política de Intervalo pode ser candidata a Parâmetros; uma faixa concreta como `08:40–09:00` não deve ser classificada como Parâmetro somente por ser configurável.

### Interstício

Regra de tempo mínimo livre exigido entre determinadas atividades.

Quando definido como política institucional ou contratual compartilhável, é candidato natural a Parâmetros.

Interstício não é sinônimo de Intervalo.

### Disponibilidade

Módulo funcional responsável pelo processo de declaração e manutenção da disponibilidade docente.

Também pode designar, em contexto específico, a informação operacional produzida por esse processo.

### Prioridade de ausência

Valor informado no contexto de Disponibilidade para representar a importância ou força de uma restrição de ausência.

Seu significado operacional é definido pela política vigente da Empresa.

### Parâmetro

Política ou configuração empresarial compartilhável que responde a como a Empresa decidiu operar.

Exemplos fortes no contexto Escola incluem duração padrão de aula, prioridade mínima bloqueante, interpretação da prioridade, máximo de aulas consecutivas, interstício institucional e margem geral de deslocamento.

`Configurável` não é critério suficiente para classificar um conceito como Parâmetro.

### Gestão de Horários

Módulo funcional responsável pelo planejamento, geração, revisão, exceções, remontagem e resultados da grade escolar.

### Grade de Horários

Resultado estruturado da alocação de aulas em Professores, Turmas, Blocos de Aula e Ambientes.

### Exceção de planejamento

Situação identificada durante o planejamento que impede ou compromete uma alocação e exige tratamento explícito ou intervenção humana.

## Regras terminológicas

```text
Cargo
≠ Papel empresarial
≠ Autoridade modular
≠ Permissão funcional
```

```text
Capacidade empresarial
≠ autorização do usuário
```

```text
aplicabilidade
≠ habilitação
≠ concessão
```

```text
Professor estrutural
≠ Cargo Professor
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

Intervalo
≠ Interstício
```

E, para classificação:

```text
política/default
≠ referência/ocorrência concreta
```

Os conceitos podem estar relacionados, mas não representam a mesma responsabilidade de domínio.