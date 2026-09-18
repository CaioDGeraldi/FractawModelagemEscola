# Modelo Físico e Temporal

## 1. Objetivo

Definir as referências estruturais necessárias para representar onde e quando as atividades escolares podem ocorrer.

Este documento trata de domínio e relações conceituais. Não define models Django, tabelas, chaves estrangeiras ou estratégia de persistência.

## 2. Visão geral

```text
Estrutura física                     Estrutura temporal

Site                                 Período Letivo
 └── Ambiente                         └── Turno
                                        ├── Bloco de Aula
                                        └── Intervalo

Site ── Deslocamento ──> Site

              ↓
      Gestão de Horários
```

A Gestão de Horários consome essas referências, mas não é proprietária delas.

---

## 3. Site

### DEC-FT-001 — `Site` é o termo estrutural adotado

`Site` representa uma unidade física da Empresa Escola em que atividades acadêmicas podem ocorrer.

O termo “Unidade Escolar” pode ser utilizado na interface ou comunicação de negócio, mas a modelagem utiliza `Site` como termo estrutural.

Um Site pode possuir vários Ambientes.

### Invariantes

- um Site pertence a exatamente uma Empresa;
- Sites relacionados em uma mesma operação escolar devem pertencer à mesma Empresa;
- a inativação de um Site não apaga seu significado em históricos.

---

## 4. Ambiente

### DEC-FT-002 — Sala e Laboratório são representados por `Ambiente`

`Ambiente` é a referência física alocável.

Exemplos:

- sala comum;
- laboratório de informática;
- laboratório de química;
- auditório;
- quadra;
- oficina.

Sala e Laboratório não precisam ser entidades paralelas.

A classificação do Ambiente deve permitir expressar tipo, capacidades ou recursos necessários para que Gestão de Horários verifique se ele atende a uma Oferta de Disciplina.

Um Ambiente pode possuir capacidade de ocupação quando relevante.

### Invariantes

- um Ambiente pertence a exatamente um Site;
- Ambiente e Site pertencem à mesma Empresa;
- Ambiente inativo não recebe novas alocações;
- históricos continuam identificando o Ambiente utilizado.

---

## 5. Deslocamento entre Sites

### DEC-FT-003 — Deslocamento é uma relação estrutural direcional

A relação registra o tempo concreto necessário para ir de um Site de origem a um Site de destino.

O dado estrutural deve expressar explicitamente o tempo, por exemplo em minutos:

```text
Site A → Site B
tempo de deslocamento = 35 min
```

A relação é direcional:

```text
A → B
≠ necessariamente
B → A
```

Se a instituição considerar os tempos equivalentes, pode registrar o mesmo valor nos dois sentidos.

### Dado estrutural x política

```text
Deslocamento estrutural:
A → B = 35 min

Parâmetro:
margem de segurança = 10 min

Gestão de Horários:
tempo considerado = 45 min
```

O tempo concreto pertence ao domínio estrutural. A margem adicional pertence a Parâmetros.

### Invariantes

- origem e destino pertencem à mesma Empresa;
- origem e destino são Sites diferentes;
- o tempo de deslocamento não pode ser negativo;
- ausência de relação não significa automaticamente zero minutos.

---

## 6. Período Letivo

### DEC-FT-004 — Período Letivo possui início e fim concretos

Período Letivo representa uma vigência acadêmica.

Exemplos:

- 1º semestre de 2027;
- ano letivo de 2027.

Conceitualmente deve possuir:

- data de início;
- data de término;
- identidade própria dentro da Empresa.

Sua duração é derivada dessas datas, e não armazenada como uma política genérica.

```text
Período Letivo
início = 03/02/2027
fim    = 02/07/2027

Duração
→ consequência da vigência definida
```

Uma Empresa pode adotar semestres, anos ou outros ciclos sem alterar o conceito.

Período Letivo organiza referências e processos como:

- Turmas;
- Ofertas de Disciplina;
- Disponibilidade;
- planejamento e publicação de grades.

Período Letivo não significa manhã, tarde ou noite.

### Invariantes

- pertence a uma Empresa;
- data de término deve ser posterior à data de início;
- Turmas e processos associados pertencem à mesma Empresa;
- históricos de um Período encerrado permanecem identificáveis.

---

## 7. Turno

### DEC-FT-005 — Turno é referência organizacional temporal

Turno representa uma organização recorrente de parte da jornada escolar.

Exemplos:

- manhã;
- tarde;
- noite;
- integral.

Pode ser referenciado por Turmas, Blocos de Aula e regras de planejamento.

O modelo não assume que todo Site possua exatamente os mesmos horários para um Turno com o mesmo nome.

---

## 8. Bloco de Aula

### DEC-FT-006 — Bloco de Aula define a duração real de uma aula alocável

Bloco de Aula representa uma unidade concreta de tempo em que uma aula pode ser alocada.

Deve possuir:

- horário de início;
- horário de término;
- posição ou ordem dentro do Turno;
- contexto suficiente para não ser ambíguo.

Exemplo:

```text
Bloco 1 — 07:00–07:50
```

A duração real do Bloco é derivada desses horários:

```text
07:00–07:50
→ 50 minutos
```

Portanto, o tempo de uma aula não precisa ser um número global fixo.

Uma Empresa pode possuir Blocos de durações diferentes quando necessário.

Se existir uma `duração padrão de aula`, ela pode ser um Parâmetro utilizado para auxiliar criação ou validação dos Blocos, mas não substitui os horários reais do Bloco.

### Escopo dos Blocos

Um Bloco não é global para todas as Empresas ou Sites por padrão.

Sua definição pertence a uma organização temporal da Empresa e pode, quando necessário, variar por Site.

### Invariantes

- término posterior ao início;
- blocos do mesmo contexto não possuem sobreposição inválida;
- bloco inativo não recebe novas alocações;
- Disponibilidade e Gestão de Horários usam a mesma identidade de Bloco para a mesma faixa alocável.

---

## 9. Intervalo

### DEC-FT-007 — Intervalo real e Interstício mínimo são conceitos diferentes

`Intervalo` representa uma faixa concreta e não alocável dentro da organização temporal da instituição.

Exemplo:

```text
Bloco 2    07:50–08:40
Intervalo  08:40–09:00
Bloco 3    09:00–09:50
```

Esse Intervalo existe na estrutura temporal real.

Já `Interstício mínimo` representa uma política que exige separação mínima entre determinadas atividades e pertence a Parâmetros.

```text
Intervalo
→ fato estrutural da grade temporal

Interstício mínimo
→ política institucional/contratual
```

Um Intervalo concreto pode satisfazer um Interstício, mas os conceitos não são equivalentes.

---

## 10. Interstício e deslocamento

O planejamento deve considerar separadamente:

- duração real dos Blocos;
- Intervalos existentes;
- tempo de Deslocamento entre Sites;
- margem de deslocamento definida em Parâmetros;
- Interstício mínimo exigido por política.

Exemplo conceitual:

```text
Professor termina aula no Site A às 10:00
próxima aula no Site B começa às 10:40

Deslocamento A → B = 30 min
Margem = 5 min

Tempo necessário = 35 min
Tempo disponível  = 40 min
→ deslocamento possível
```

Se houver também um Interstício mínimo aplicável, Gestão de Horários deverá validar a política correspondente sem confundi-la com o próprio tempo de deslocamento.

---

## 11. Relações com Turma

Uma Turma pode possuir:

- Período Letivo;
- Site de referência;
- Turno.

Esses vínculos descrevem contexto da Turma, não uma aula já alocada.

---

## 12. Relação com Disponibilidade

Disponibilidade consome principalmente a estrutura temporal:

```text
Professor
  ↓
Disponibilidade
  ↓
Bloco de Aula
```

Quando necessário, Site também pode compor o contexto da disponibilidade.

---

## 13. Relação com Gestão de Horários

Gestão de Horários combina:

```text
Oferta de Disciplina
        +
Professor(es) / Disponibilidade
        +
Turma
        +
Bloco de Aula
        +
Ambiente
        +
Deslocamento entre Sites
        +
Intervalos
        +
Parâmetros / Interstício
        ↓
Grade de Horários
```

Gestão de Horários não duplica essas referências em seu domínio.

---

## 14. Cardinalidades conceituais iniciais

```text
Empresa          1 ← 0..* Site
Site             1 ← 0..* Ambiente
Site             0..* ↔ 0..* Site (Deslocamento direcional)
Empresa          1 ← 0..* Período Letivo
Empresa          1 ← 0..* Turno
Turno            1 ← 0..* Bloco de Aula
Turno            1 ← 0..* Intervalo
Período Letivo   1 ← 0..* Turma
Site             0..1 ← 0..* Turma
Turno            0..1 ← 0..* Turma
```

---

## 15. Questões em aberto

### QFT-001 — Escopo de Turno e Bloco

A mesma definição de Turno e seus Blocos pode ser compartilhada entre Sites ou cada Site precisa de organização temporal própria?

### QFT-002 — Recursos de Ambiente

É suficiente classificar Ambientes por tipo ou precisamos representar capacidades/recursos independentemente?

### QFT-003 — Capacidade de Ambiente

Turma maior que a capacidade do Ambiente bloqueia a alocação ou pode haver exceção configurável?

### QFT-004 — Disponibilidade por Site

A Disponibilidade do Professor precisa distinguir o Site em determinado Bloco?

### QFT-005 — Turnos compostos

Como representar Turmas que operam em mais de um Turno?

### QFT-006 — Escopo do Interstício

Em quais situações o Interstício mínimo se aplica: entre aulas, turnos, jornadas ou regras contratuais específicas?

---

## 16. Decisões consolidadas

- `Site` é a referência de unidade física;
- `Ambiente` é a referência física alocável;
- Sala e Laboratório são classificações/capacidades de Ambiente;
- Deslocamento entre Sites possui tempo concreto e é direcional;
- margem geral de deslocamento pertence a Parâmetros;
- Período Letivo possui início e fim e sua duração é derivada;
- Turno representa organização temporal recorrente;
- Bloco de Aula possui início e fim e sua duração é derivada;
- Intervalo é uma faixa real não alocável;
- Interstício mínimo é política em Parâmetros;
- Gestão de Horários consome essas referências sem assumir ownership sobre elas.
