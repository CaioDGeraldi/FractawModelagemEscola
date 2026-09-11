# Requisitos de Horário Escolar

Repositório destinado à modelagem e especificação de requisitos do domínio de planejamento de horários escolares que será incorporado ao FractawModules no contexto do Tipo de Empresa Escola.

O foco deste projeto é compreender o domínio, definir responsabilidades e especificar requisitos antes das decisões de implementação.

## Relação com o FractawModules

A modelagem respeita as decisões arquiteturais vigentes do FractawModules, especialmente a separação entre:

- plataforma compartilhada;
- Tipo de Empresa;
- referências estruturais do domínio;
- Parâmetros;
- módulos operacionais.

Este repositório não define o mecanismo técnico de composição por `TipoEmpresa`.

O documento `docs/contexto-fractaw.md` registra as fronteiras que devem orientar a modelagem.

## Objetivo do repositório

Registrar, de forma incremental e rastreável:

- visão geral do sistema;
- contexto de integração com o FractawModules;
- glossário do domínio;
- atores e partes interessadas;
- regras de negócio;
- casos de uso;
- requisitos funcionais;
- requisitos não funcionais;
- modelo conceitual;
- fluxos e estados;
- rastreabilidade entre os artefatos.

## Organização prevista

```text
docs/
├── contexto-fractaw.md
├── 00-visao-geral.md
├── 01-glossario.md
├── 02-atores-e-partes-interessadas.md
├── 03-regras-de-negocio.md
├── 04-casos-de-uso.md
├── 05-requisitos-funcionais.md
├── 06-requisitos-nao-funcionais.md
├── 07-modelo-conceitual.md
├── 08-fluxos-e-estados.md
└── 09-rastreabilidade.md

diagramas/
├── casos-de-uso.drawio
├── atividades.drawio
├── estados.drawio
└── modelo-conceitual.drawio
```

Os arquivos serão adicionados conforme cada etapa da modelagem for desenvolvida e revisada.

## Ordem de modelagem

1. Problema, objetivos, escopo e fronteira do domínio
2. Glossário do domínio
3. Atores e partes interessadas
4. Regras de negócio
5. Casos de uso
6. Requisitos funcionais
7. Requisitos não funcionais
8. Modelo conceitual
9. Fluxos e estados
10. Rastreabilidade e critérios de aceitação

Durante todas as etapas, novos conceitos devem ser classificados como referência estrutural, política/configuração ou estado operacional antes de qualquer decisão de implementação.
