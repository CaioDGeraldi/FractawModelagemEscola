# Parâmetros Aplicáveis à Escola

## 1. Objetivo

Identificar políticas, defaults, limites e configurações empresariais que o Tipo de Empresa Escola exige da capacidade-base genérica Parâmetros do FractawModules.

Este documento não cria um módulo separado chamado “Parâmetros Escolares” e não define o schema da futura F08.

## 2. Responsabilidade

Parâmetros responde principalmente:

> Como esta Empresa decidiu operar dentro do contexto Escola?

O Tipo de Empresa Escola define a semântica e a aplicabilidade das políticas.

A Empresa mantém os valores efetivamente vigentes quando a política for configurável por tenant.

## 3. Regra principal de classificação

### DEC-PAR-001 — Configurável não é critério suficiente

Um conceito não pertence a Parâmetros apenas porque pode ser configurado pela Empresa.

A classificação deve considerar sua natureza.

```text
política/default
≠
referência/ocorrência concreta
```

Exemplos:

```text
duração padrão de aula = 50 min
→ política/configuração candidata a Parâmetros

Bloco 3 = 09:00–09:50
→ referência concreta

Período Letivo 2027.1 = 03/02/2027 até 02/07/2027
→ referência concreta

margem geral de deslocamento = 10 min
→ política/configuração candidata a Parâmetros
```

O fato de Bloco, Período Letivo ou outra referência possuir horários ou datas não os transforma em Parâmetros.

## 4. Candidatos fortes já identificados

A modelagem atualmente considera fortes candidatos a Parâmetros:

- política de interpretação da escala de prioridade de ausência;
- prioridade mínima considerada bloqueante;
- limites institucionais de alocação;
- máximo de aulas consecutivas;
- duração padrão de aula;
- interstício institucional ou contratual;
- margem ou tolerância geral de deslocamento;
- defaults temporais compartilháveis;
- outras políticas empresariais compartilhadas confirmadas pelos requisitos.

Cada novo candidato deve ser classificado individualmente.

## 5. Política de prioridade

A Disponibilidade registra o valor informado pelo Professor.

Parâmetros define como a Empresa interpreta esse valor quando essa interpretação for uma política institucional compartilhada.

```text
Disponibilidade
    registra prioridade

Parâmetros
    define significado e efeito

Gestão de Horários
    aplica fato + política
```

A referência atual de modelagem considera:

- níveis 1 a 8 como negociáveis, com peso crescente;
- níveis 9 e 10 como bloqueantes.

Essa política permanece configurável conforme os requisitos definitivos da Escola.

## 6. Duração de aula, Turno e Bloco

Uma duração padrão de aula é candidata natural a Parâmetros:

```text
duração padrão = 50 min
```

Ela pode orientar a organização temporal da Empresa.

Entretanto, as ocorrências concretas continuam pertencendo à estrutura temporal:

```text
Turno Manhã
Bloco 1 = 07:00–07:50
Bloco 2 = 07:50–08:40
```

A modelagem não afirma que todos os horários concretos de Turnos ou Blocos sejam armazenados em Parâmetros.

O requisito de Parâmetros é a possibilidade de representar políticas/defaults temporais quando eles realmente existirem como configuração compartilhável.

## 7. Intervalo e Interstício

### Intervalo

`Intervalo` representa uma faixa não alocável da organização temporal escolar.

A classificação depende da natureza do requisito.

Exemplo de política/default candidata a Parâmetros:

```text
duração padrão do intervalo = 20 min
```

Exemplo de ocorrência concreta:

```text
intervalo do Turno Manhã = 08:40–09:00
```

A faixa concreta não deve ser classificada como Parâmetro apenas por ser configurável.

Também permanece em aberto se Intervalo precisa de identidade estrutural própria ou se pode ser parte da organização temporal concreta de um Turno.

### Interstício

Interstício representa uma regra de tempo mínimo livre exigido entre determinadas atividades.

Quando definido como política institucional ou contratual, é candidato natural a Parâmetros:

```text
interstício mínimo = 20 min
```

Intervalo e Interstício continuam conceitos diferentes.

## 8. Deslocamento entre Sites

A relação específica entre dois Sites é estrutural.

Nesta modelagem, o tempo concreto necessário para o par é tratado como dado próprio da relação:

```text
Site A → Site B = 35 min
Site B → Site A = 45 min
```

O fato de o valor ser temporal não o transforma em Parâmetro.

Uma política geral aplicada sobre essas relações é candidata natural a Parâmetros:

```text
margem geral de deslocamento = 10 min
```

Gestão de Horários pode então combinar dado estrutural e política:

```text
deslocamento A → B = 35 min
margem institucional = 10 min
tempo considerado = 45 min
```

## 9. Período Letivo

Período Letivo é referência estrutural concreta e possui vigência efetiva própria.

Exemplo:

```text
2027.1
03/02/2027 → 02/07/2027
```

Uma política geral ou default de calendário pode ser candidata a Parâmetros, por exemplo:

```text
duração padrão de semestre
regra geral de composição do calendário
```

As datas concretas de um Período Letivo não pertencem a Parâmetros apenas porque a Empresa pode escolhê-las.

## 10. Escopo contextual exigido pela Escola

A modelagem revela que algumas políticas escolares podem precisar de semântica ou escopo contextual, por exemplo:

- Empresa;
- Site;
- Turno;
- Período Letivo;
- outros contextos futuros confirmados pelos requisitos.

Essa necessidade deve orientar a futura F08.

Ela não autoriza criar dependência da plataforma para o domínio Escola.

Conceitualmente deve permanecer:

```text
plataforma
    ↑
Escola
```

A futura capacidade Parâmetros precisa oferecer semântica contextual suficiente para casos escolares sem importar diretamente models da Escola.

Este repositório não escolhe como isso será implementado.

## 11. Limites

Parâmetros não deve assumir ownership de referências ou estados operacionais apenas porque eles são configuráveis.

Portanto, não pertencem a Parâmetros por esse motivo:

- Professor;
- Turma;
- Disciplina;
- Site;
- Turno enquanto referência concreta;
- Bloco de Aula concreto;
- Período Letivo concreto;
- relação concreta de Deslocamento entre Sites e seu tempo específico;
- disponibilidade individual;
- Grade;
- tentativas e exceções de Gestão de Horários.

## 12. Decisões deliberadamente não tomadas

Esta modelagem não escolhe:

- EAV;
- chave/valor;
- `JSONField`;
- Registry;
- Manifest;
- Strategy;
- Factory;
- definition objects;
- generic foreign key;
- schema específico por parâmetro;
- mecanismo de precedência;
- mecanismo de resolução runtime por TipoEmpresa;
- mecanismo concreto de escopo contextual.

Essas decisões pertencem à futura F08 do FractawModules.

## 13. Questões em aberto

A modelagem ainda deve confirmar:

- quais políticas realmente precisam de escopo por Site, Turno ou Período Letivo;
- como funcionará a precedência entre valores globais e específicos;
- quais configurações aceitam sobrescrita;
- quais mudanças precisam de histórico/versionamento;
- quando uma alteração de política passa a valer para processos já iniciados;
- em quais contextos o interstício mínimo se aplica;
- se Intervalos concretos precisam de identidade própria;
- quais resultados históricos precisam preservar snapshot da política utilizada.