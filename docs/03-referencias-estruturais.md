# Referências Estruturais da Escola

## 1. Objetivo

Identificar e classificar os conceitos do domínio Escola que possuem identidade própria e servem como referência para diferentes processos, módulos ou históricos.

Este documento não define models, tabelas ou APIs.

## 2. Critério de pertencimento

Um conceito é candidato a referência estrutural quando:

- representa algo que existe no domínio escolar;
- possui identidade própria;
- pode ser referenciado por diferentes processos;
- precisa permanecer semanticamente compreensível em históricos;
- não representa apenas uma política ou uma execução operacional.

Ser configurável não transforma automaticamente um conceito em Parâmetro.

## 3. Candidatos atuais

A modelagem já identificou os seguintes candidatos:

- Professor;
- Disciplina;
- Curso;
- Turma;
- Site ou Unidade Escolar;
- Sala;
- Laboratório;
- Período Letivo;
- Bloco de Aula.

A presença nesta lista não encerra a modelagem de cada conceito.

## 4. Relação com o Catálogo de Produtos

O Catálogo de Produtos da plataforma FractawModules não é proprietário das referências escolares.

Conceitos como Professor, Turma e Disciplina pertencem ao domínio Escola e não devem ser colocados no Catálogo de Produtos apenas para reutilizar uma capacidade existente.

## 5. Relação com módulos

Módulos escolares podem consumir essas referências sem assumir ownership sobre elas.

Exemplo:

```text
Professor
├── Disponibilidade
└── Gestão de Horários
```

O fato de um módulo utilizar Professor não significa que Professor pertence a esse módulo.

## 6. Questões de classificação

Alguns conceitos exigem análise adicional.

### Bloco de Aula

Se possuir identidade, horários próprios e for referenciado por disponibilidade, turmas ou grades, comporta-se como referência estrutural.

### Deslocamento entre Sites

Uma relação específica entre dois Sites pode representar dado estrutural do domínio.

Uma margem geral ou política de deslocamento pode, por outro lado, pertencer a Parâmetros.

A modelagem deve separar o dado concreto da política aplicada sobre ele.

## 7. Próximas definições

Para cada referência confirmada, a modelagem deverá registrar:

- significado no domínio;
- identidade;
- relações com outras referências;
- invariantes;
- ownership;
- quais módulos a consomem;
- regras que não pertencem à própria referência.
