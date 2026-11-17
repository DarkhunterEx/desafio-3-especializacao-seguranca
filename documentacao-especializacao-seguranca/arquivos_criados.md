# Arquivos Criados

Esta seção lista todos os arquivos que foram **gerados** (ou recompletos) durante a evolução do DIO Agent com a especialização de segurança.

## Skill de Análise de Vulnerabilidades

| Arquivo | Descrição |
|---------|-----------|
| `skills/vulnerability-analysis/SKILL.MD` | Skill principal. Contém `name`, `description` e o processo defensivo com 7 etapas. |
| `skills/vulnerability-analysis/README.MD` | README explicando para quem é a skill, qual seu objetivo e qual o fluxo. |
| `skills/vulnerability-analysis/references/structured-format.md` | Formato estruturado de resposta (Resumo, Conceito, Impacto, Investigação, Correção, Aprendizado). |
| `skills/vulnerability-analysis/references/owasp-top10-2025.md` | Tabela do OWASP Top 10:2025 com códigos e descrições. |
| `skills/vulnerability-analysis/references/risk-classification.md` | Classificação de **probabilidade** e **severidade** para análise de risco. |

## Conhecimento de Fundamentos

| Arquivo | Descrição |
|---------|-----------|
| `agent/knowledge/security-basics.md` | Fundamentos de segurança da informação: vulnerabilidade, ameaça, risco, tríade CIA, boas práticas. |

## Personalidade e Registro

| Arquivo | Descrição |
|---------|-----------|
| `agent/persona.md` | Personalidade do agente, incluindo a seção de especialização em segurança e a skill de análise de vulnerabilidades. |
| `AGENTS.md` | Registro da skill `vulnerability-analysis` na tabela de skills, para o agente saber quando usá-la. |

## Evidências de Teste

| Arquivo | Descrição |
|---------|-----------|
| `teste-skill-codigo-corrigido/app.py` | Versão corrigida do app.py com consulta parametrizada. |
| `teste-skill-codigo-corrigido/relatorio_seguranca.md` | Relatório de análise de segurança do app.py (antes da correção). |
| `agent/seguranca_app_py/app_original.py` | Versão original do app.py, com SQL Injection. |

## Outros

| Arquivo | Descrição |
|---------|-----------|
| `skills/README.md` | Documenta o processo de criação de skills no DIO Agent. |

## Estrutura final da pasta

```
skills/vulnerability-analysis/
├── SKILL.MD
├── README.MD
└── references/
    ├── structured-format.md
    ├── owasp-top10-2025.md
    └── risk-classification.md
agent/
├── persona.md
└── knowledge/
    └── security-basics.md
AGENTS.md
```
