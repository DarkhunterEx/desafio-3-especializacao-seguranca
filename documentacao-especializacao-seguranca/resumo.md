# Resumo Executivo

Este projeto evoluiu o **DIO Agent** — um agente de IA que auxilia estudantes da DIO em seus estudos — para que ele passe a atuar como **mentor de cibersegurança**.

O objetivo do desafio era transformar o agente em alguém que, ao invés de responder direto, **entenda o contexto, avalie impacto, probabilidade e severidade e oriente a correção**.

## O que foi feito

| Item | Descrição |
|------|-----------|
| **Skill de análise de vulnerabilidades** | `skills/vulnerability-analysis/SKILL.MD` — roteiro defensivo com 7 etapas: entender cenário, explicar conceito, analisar risco, verificar se é segurança, mostrar investigação, recomendar melhorias, verificar aprendizado |
| **README da skill** | `skills/vulnerability-analysis/README.MD` — explica para quem o skill serve |
| **Referências da skill** | `skills/vulnerability-analysis/references/` — arquivos com OWASP Top 10:2025, classificação de risco e formato estruturado de resposta |
| **Conhecimento de fundamentos** | `agent/knowledge/security-basics.md` — vulnerabilidade, ameaça, risco, tríade CIA (confidencialidade, integridade, disponibilidade) e boas práticas |
| **Persona ajustada** | `agent/persona.md` — adiciona a seção sobre skill de análise de vulnerabilidades e trilha de cibersegurança |
| **Registro no AGENTS.md** | Adiciona a skill `vulnerability-analysis` à tabela de skills, para o agente reconhecê-la e usá-la quando o contexto exigir |

## O que foi testado

O principal teste foi com **SQL Injection**:

1. **Antes** — `agent/seguranca_app_py/app_original.py`: consulta SQL montada com f-string, vulnerável a SQL Injection.
2. **Depois** — `agent/app.py` e `teste-skill-codigo-corrigido/app.py`: consulta parametrizada com `?` placeholders, mitigação da falha.
3. **Relatório** — `teste-skill-codigo-corrigido/relatorio_seguranca.md`: análise detalhada de impacto, correção e recomendações.

## A melhoria escolhida

Dentro do desafio, foram sugeridas várias melhorias:

- Uma skill de testes de segurança (opção sugerida pela Expert);
- Uma skill que monte uma trilha de estudos em cibersegurança;
- Testar a skill com outros cenários;
- **Padronizar o formato da análise** com impacto, probabilidade e severidade;
- Levar a revisão para o fluxo de desenvolvimento (DevSecOps).

A melhoria escolhida foi **padronizar o formato da análise** com os três campos (impacto, probabilidade, severidade), em conjunto com a vector **de um roteiro defensivo** (nunca entregar a resposta pronta, sempre conduzir o estudante).

## Por que

A especialização é importante porque:

- Ensina o estudante a **pensar como analista de segurança**, não apenas a usar ferramentas;
- **Não entrega respostas prontas** — o agente conduz o estudante pelo raciocínio;
- **Verifica se a situação é de segurança** antes de classificar como vulnerabilidade;
- **Pergunta o contexto antes de sugerir comandos**, evitando que o estudante compreenda a segurança de forma superficial;
- Torna o processo **reprodutível**, graças ao formato estruturado de resposta.

## Como rodar

1. Instale o **Hermes** seguindo a documentação indicada no `README.md`.
2. Baixe ou clone o repositório.
3. Abra a pasta do projeto (onde está o `AGENTS.md`) em um terminal.
4. Converse com o agente, por exemplo:

   - "Analisa essa app.py que tem login com SQL";
   - "Quero aprender a analisar vulnerabilidades";
   - "Meu servidor Ubuntu de laboratório expôs uma configuração insegura, o que fazer?"

O agente vai procurar a skill `vulnerability-analysis`, perguntar o contexto quando necessário e aplicar o roteiro.
