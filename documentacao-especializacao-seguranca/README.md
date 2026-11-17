# Documentação da Especialização em Segurança

Este diretório reúne toda a documentação do processo de evolução do DIO Agent para **especialista em análise de vulnerabilidades de segurança**.

## O que está aqui

| Arquivo | O que explica |
|---------|---------------|
| `README.md` | Este arquivo — resumo do diretório |
| `resumo.md` | Resumo executivo do que foi feito |
| `arquivos_criados.md` | Lista de todos os arquivos novos gerados |
| `arquivos_modificados.md` | Lista dos arquivos que foram alterados ou completos |
| `teste_sql_injection.md` | Documentação do teste com SQL Injection (antes e depois) |
| `como_roda.md` | Como rodar o agente no Hermes |
| `recomendacao_escolhida.md` | A melhoria que foi escolhida dentro do desafio |
| `evidencias/` | Código-fonte dos programas de teste (before/after) |

## Como ler

Siga em ordem:

1. `resumo.md` — o que foi feito de resumo
2. `arquivos_criados.md` e `arquivos_modificados.md` — o que foi gerado
3. `teste_sql_injection.md` — a evidência do teste com código vulnerável
4. `como_roda.md` — como rodar o agente no harness
5. `evidencias/` — os arquivos de código reais

## Estrutura do repositório

```
dio-agent/
├── agent/
│   ├── persona.md            # Personalidade do agente + ref. à skill de segurança
│   ├── knowledge/
│   │   ├── security-basics.md      # Fundamentos: vulnerabilidade, ameaça, risco, CIA, boas práticas
│   │   ├── vulnerability-learning.md  # Trilha de estudos de segurança
│   │   ├── dio-platform.md
│   │   └── learning-experiences.md
│   └── seguranca_app_py/
│       └── app_original.py     # App com SQL Injection (antes da correção)
├── skills/
│   └── vulnerability-analysis/
│       ├── SKILL.MD            # Skill de análise de vulnerabilidades
│       ├── README.MD           # README da skill
│       └── references/         # Blueprints de resposta
│           ├── structured-format.md
│           ├── owasp-top10-2025.md
│           └── risk-classification.md
├── AGENTS.md                 # Tabela de skills (registra a nova skill)
└── README.md                 # README do repositório (atualizado com a skill)
```
