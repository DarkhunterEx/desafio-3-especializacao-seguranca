# 🤖 DIO Agent — Análise de Vulnerabilidades

> **Especialização em segurança**: transformando o DIO Agent em um mentor que analisa falhas de segurança do jeito que um profissional faria.

---

## 1. Especialização dada ao agente

Criei uma **skill de análise de vulnerabilidades** que transforma o DIO Agent em um mentor de segurança. Em vez de responder direto, o agente agora:

1. **Entende o cenário** antes de classificar;
2. **Explica o conceito** do princípio violado;
3. **Analisa o risco** com impacto, probabilidade e severidade;
4. **Verifica se é segurança** (não classificar bug de lógica sem exposição);
5. **Mostra como profissionais investigam**;
6. **Recomenda melhorias e prevenção**;
7. **Verifica o aprendizado**.

**Por que escolhi esse caminho:** a especialização em segurança exige pensamento defensivo, não respostas prontas. O estudante precisa entender o porquê de cada classificação e correção.

---

## 2. Arquivos criados ou alterados

| Arquivo | Ação | Descrição |
|---|---|---|
| `skills/vulnerability-analysis/SKILL.MD` | ✅ Criado | Skill principal: processo defensivo em 7 etapas + formato estruturado de resposta |
| `skills/vulnerability-analysis/README.MD` | ✅ Criado | Explicação para estudantes do que a skill faz e como usá-la |
| `skills/vulnerability-analysis/references/` | ✅ Criado | Referências: OWASP Top 10:2025, classificação de risco, formato estruturado |
| `agent/knowledge/security-basics.md` | ✅ Criado | Fundamentos: vulnerabilidade, ameaça, risco, CIA (confidencialidade/integridade/disponibilidade), boas práticas |
| `agent/persona.md` | ✅ Alterado | Adicionadas seções: "Skill de análise de vulnerabilidades" e "Mentoria de cibersegurança" |
| `AGENTS.md` | ✅ Alterado | Tabela de skills atualizada com a linha da nova skill |

---

## 3. Como rodar no Hermes

### 1. Instale o Hermes

Siga a documentação oficial: https://hermes-agent.nousresearch.com/docs

### 2. Baixe ou clone o repositório

```bash
# Com Git
git clone https://github.com/DarkhunterEx/desafio-3-especializacao-seguranca.git

# Sem Git
# Clique em "Code" > "Download ZIP" e extraia
```

### 3. Abra o projeto no Hermes

- No VS Code, abra a pasta `documentação desafio 3`
- O arquivo `AGENTS.md` é lido automaticamente pelo harness
- O agente já sabe quem é: seu mentor de estudos com especialização em segurança

### 4. Comece a conversa

```
Analise o login deste app.py e explique quais problemas de segurança você encontrou.
```

```
Quero aprender a identificar e corrigir uma vulnerabilidade de SQL Injection.
```

O agente fará perguntas de contexto, explicará a falha, avaliará o risco e indicará a correção.

---

## 4. Melhoria escolhida

Escolhi **padronizar o formato da análise**, separando:
- **Impacto** — o que pode acontecer se a falha for explorada;
- **Probabilidade** — as condições que favorecem a exploração;
- **Severidade** — a gravidade atribuída ao problema.

Isso permite comparar casos diferentes e entender por que uma falha merece mais atenção que outra.

---

## 5. Diferenças entre o original e o corrigido

### SQL Injection (app.py)

| Linha | Original (vulnerável) | Corrigido |
|---|---|---|
| 7 | `query=f"SELECT * FROM usuarios where username = '{usuario}' AND password = '***'"` | `query = "SELECT * FROM usuarios WHERE username = ? AND password = ***"` |
| 9 | `cursor.execute(query)` | `cursor.execute(query, (usuario, senha))` |
| 4-9 | Sem `try/finally` | `try/finally` garante fechamento da conexão |
| 5 | `conexao=sqlite3.connect('banco_de_dados.db')` | `conexao = None` + `conexao = sqlite3.connect(...)` |

### Explicação

**Antes (vulnerável):**
```python
query = f"SELECT * FROM usuarios where username = '{usuario}' AND password = '{senha}'"
```
A entrada do usuário é **inserida diretamente** na query SQL por meio de **f-string** (concatenação). Isso é chamado de **String Concatenation / Inline Concatenation** e é o padrão clássico de **SQL Injection**.

**Depois (corrigido):**
```python
query = "SELECT * FROM usuarios WHERE username = ? AND password = ***"
cursor.execute(query, (usuario, senha))
```
A entrada do usuário deixou de fazer parte da **estrutura da query**. Agora é passada como **parâmetro**, tratada como **dados**. O SQLite trata cada valor como dado, não como código.

---

## 📸 Evidências

- Prints da análise do `app.py` (vulnerável vs corrigido);
- Perguntas de contexto que o agente faz antes de analisar;
- Comparação de respostas: agente genérico vs agente especializado.

---

## 📚 Referências

- OWASP Top 10:2025 — https://owasp.org/www-project-top-ten/
- MITRE ATT&CK — T1190: Exploit Public-Facing Application
- DIO Agent — https://github.com/digitalinnovationone/dio-agent

---

Bons estudos e bom projeto! 🚀
