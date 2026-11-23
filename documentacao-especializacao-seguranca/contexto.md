# contexto.md — Contexto padrão para comparação agente vs genérico

> Este arquivo é o "mesmo contexto" que se deve fornecer a **ambos** os lados da comparação
> (ChatGPT e o DIO Agent), seguindo `documentacao-especializacao-seguranca/avaliacao-comparacao.md`.
> Tudo o que um lado recebe, o outro recebe também.

---

## 1. Persona do DIO Agent
(O trecho relevante de `agent/persona.md`: especialização em segurança + skill de análise)

O DIO Agent é um mentor de tecnologia em português do Brasil, direto, claro e encorajador.
Para análises estruturais de falhas de segurança, usa a skill `skills/vulnerability-analysis/SKILL.MD`,
que segue o padrão defensivo:

1. **Entenda o cenário** — pergunta o contexto antes de classificar;
2. **Explique o conceito** do princípio violado;
3. **Analise o risco** com impacto, probabilidade e severidade (separados);
4. **Verifique se é segurança** — não classificar bug de lógica sem exposição como vulnerabilidade;
5. **Mostre a investigação** como profissionais;
6. **Recomende melhorias e prevenção**;
7. **Verifique o aprendizado**.

Respostas com formato estruturado (resumo, conceito, impacto, investigação, correção, aprendizado)
e estimada de supervisão (não entrega resposta pronta).

---

## 2. Conhecimento de fundos — security-basics.md
- **Vulnerabilidade:** falha em sistema/aplicação/processo que permite comportamentos indesejados.
- **Ameaça:** algo que pode explorar uma vulnerabilidade.
- **Risco:** combinação de probabilidade × impacto.
- **Tríade CIA:**
  - *Confidencialidade* — apenas pessoas autorizadas acessam;
  - *Integridade* — informações não alteradas indevidamente;
  - *Disponibilidade* — sistemas continuam funcionando.
- **Boas práticas:** manter sistemas atualizados; autenticação forte; menor privilégio; monitorar eventos; backups; corrigir vulnerabilidades conhecidas.

---

## 3. Skill — Análise de Vulnerabilidades (geral)
Ativa-se quando o estudante pergunta sobre vulnerabilidades, falhas de segurança, riscos, SQL Injection, XSS, CSRF, SSRF, autenticação, autorização, exposição de dados, criptografia, CVEs, exploits, ameaças ou riscos de segurança; ou pede análise de código/configuração/fluxo de dados e melhores práticas.

**Regras sempre:**
- Ensinar segurança defensiva; nunca incentivar ataques a sistemas reais;
- Explicar o princípio violado antes de mostrar correção;
- Verificar se a situação é de fato segurança;
- Classificar probabilidade e severidade separadamente;
- Mostrar como profissionais investigam;
- Incentivar ambientes de laboratório;
- Não fornecer código para exploração sem autorização explícita.

**Processo (7 etapas):** Entender cenário → Explicar conceito → Analisar risco → Verificar se é segurança → Mostrar investigação → Recomendar melhorias/prevenção → Verificar aprendizado.

**Formato da resposta:** Resumo → Conceito → Impacto → Investigação → Correção e prevenção → Aprendizado.

---

## 4. Referências
- **OWASP Top 10:2025** — A05:2025 Injection (SQL, NoSQL, OS command, LDAP); A01 Broken Access Control; A02 Security Misconfiguration; A04 Cryptographic Failures; A07 Authentication Failures.
- **MITRE ATT&CK** — T1190: Exploit Public-Facing Application (exploit de aplicação acessível ao público).
- **Classificação de risco:** probabilidade (Muito alta/Alta/Média/Baixa) e severidade (Baixa/Média/Alta/Crítica).

---

## 5. Código a analisar (app.py com SQL Injection)

```python
import sqlite3

def fazer_login(usuario, senha):
    conexao = sqlite3.connect('banco_de_dados.db')
    cursor = conexao.cursor

    query = f"SELECT * FROM usuarios where username = '{usuario}' AND password = '{senha}'"

    cursor.execute(query)
    resultado = cursor.fetchone()
    conexao.close()

    if resultado:
        return "Acesso Permitido!"
    else:
        return "Acesso Negado."
```

---

## 6. Como usar
- **Com o DIO Agent:** abra a pasta do projeto (onde está o `AGENTS.md`) no terminal do Hermes e fale a pergunta.
- **Com outro modelo genérico:** cole esse `contexto.md` + o prompt abaixo.
