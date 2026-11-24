# Resposta-chave da Segunda Instância — DIO Agent (especializada)

> "Outro terminal do Hermes" + repositório independente (`desafio-3-repositorio`), sob a persona
> da skill de análise de vulnerabilidades. Cada resposta segue o processo defensivo (7 etapas) e
> o formato estruturado da skill.

---

## Pergunta 1 – Conceitual

### Resposta
**O que são vulnerabilidade, ameaça e risco?** São conceitos diferentes, não sinônimos.

- **Vulnerabilidade** é a falha em si: um fechamento solto, um código mal escrito, uma configuração sem proteção. Ex.: um campo de login monta a consulta SQL sem escapar a entrada.
- **Ameaça** é o que pode explorar a vulnerabilidade: o agente mal intencionado, um script automatizado, um atacante que encontra um caminho de entrada. Ex.: um intruso que envia um `' OR '1'='1` no login.
- **Risco** é o combinação entre **probabilidade de exploração** e **magnidade do impacto**. Ex.: a mesma falha pode ser de baixa chance/alta gravidade ou de alta chance/impacto baixo.

A **tríade CIA** (Confidencialidade, Integridade, Disponibilidade) é o modelo de princípios de segurança que guia o que está em risco:
- **Confidencialidade** — apenas pessoas autorizadas acessam (ex.: senhas e dados de usuários).
- **Integridade** — a informação não é alterada indevidamente (ex.: um valor de saldo não pode ser mudado por um usuário comum).
- **Disponibilidade** — o sistema continua funcionando para quem precisa (ex.: o site não fica offline por causa de um ataque).

### Nota (rubrica)
| Critério | Pontos | Justificativa |
|---|---|---|
| C1 Exatidão | 2 | Definições corretas, tríade CIA correta. |
| C2 Classificação | 2 | Risco = probabilidade × impacto; separa bem os conceitos. |
| C3 Verificação | 2 | Reconhece que são coisas diferentes. |
| C4 Contexto | 1 | Não pergunta, mas a pergunta é conceitual, sem contexto necessário. |
| C5 Formato | 2 | Estrutura clara, linguagem acessível. |
| C6 Orientação defensiva | 2 | Explica e dá contexto, sem entregar "pronto" inútil. |
| C7 Referências | 1 | Não citou OWASP/MITRE, mas a pergunta não exige. |
| **Total** | **12** | Bom para conceitual; podia citar fonte. |

---

## Pergunta 2 – Código (SQL Injection)

### Resposta
(Resumo — a resposta completa segue o formato da skill; veja abaixo o detalhamento de cada parte.)

**Resumo:** o `fazer_login` está vulnerável a **SQL Injection (A05:2025)**. A entrada do usuário é concatenada na query por f-string. Um atacante pode contornar a senha com `' OR '1'='1`. A correção é transladar a entrada de "código SQL" para "dados", usando consulta parametrizada.

**Conceito:** a query mistura código (estrutura SQL) com dados (entrada do usuário) de forma imprópria. Principio violado: **confidencialidade** e **integridade** da autenticação. Ameaça: atacante que manipula a query de login.

**Risco:** **probabilidade alta** (falha exposta, sem proteção imediata) × **severidade alta** (comprometimento de autenticação, exposição de dados).

**Verificação de segurança:** sim, é segurança. O comportamento viola confidencialidade/integridade e há exposição direta.

**Investigação:** 1. Revisar consultas que misturam dados e código (`f"SELECT ..."` vs `"SELECT ... ?"` + parâmetros). 2. Teste teórico: `usuario=' OR '1'='1` e `senha=' OR '1'='1` tornam a cláusula verdadeira → login contornado. 3. Verificar controle de acesso: não há verificação se o usuário retornado é o pretendido (least privilege ausente). 4. Mapear: OWASP A05:2025; MITRE ATT&CK **T1190** (Exploit Public-Facing Application).

**Correção:** consulta parametrizada com `?` e passar valores separadamente; `try/finally` garante fechamento da conexão. Em produção, nunca compare senhas em texto plano — use bcrypt/SCrypt/Argon2.

**Aprendizado:** sempre separe estrutura SQL de dados. `?` ou placeholder = padrão; f-string/concatenação no SQL = sinal de falha. Verifique OWASP Top 10:2025 e MITRE ATT&CK. Quer revisar algum passo?

**Nota:** 16

---

## Pergunta 3 – Infraestrutura (Ubuntu exposto)

### Resposta
**Antes de recomendar, preciso de contexto** (a skill exige entender o cenário):

1. O que há no servidor? (ports abertos, serviços: nginx, apache, ssh, docker, banco…)
2. Quem acessa? (só você, equipe, público)
3. O que está sendo protegido? (dados, apps internos, testes)
4. Que nível de isolamento? (rede, firewall, DMZ)
5. Já há logs? (para detectar tentativas)

Com isso, entregaria prioridades:
- **HTTPS obrigatório** (letsencrypt + redirect HTTP→HTTPS) — protege credenciais e dados.
- **Firewall** — restringir acesso ao que é necessário; fechar ports desnecessários.
- **Atualizações** — patches urgentes de kernel/serviços expostos.
- **Menos serviços expostos** — desabilitar o que não usa (ex.: remover telnet, reduzir ssh).
- **Logs e monitoramento** — registrar acesso, falhas de autenticação, erros.
- **Padrões de segurança da aplicação** — cabeçalhos HTTP, input validation, autenticação segura.

**Nota:** 15 (perguntou contexto; priorizou. Poderia citar OWASP/mitre com mais detalhe).

---

## Pergunta 4 – Correção e validação

### Resposta
**Código corrigido:**
```python
import sqlite3

def fazer_login(usuario, senha):
    conexao = None
    try:
        conexao = sqlite3.connect('banco_de_dados.db')
        cursor = conexao.cursor
        query = "SELECT * FROM usuarios WHERE username = ? AND password = ?"
        cursor.execute(query, (usuario, senha))
        resultado = cursor.fetchone()
        return "Acesso Permitido!" if resultado else "Acesso Negado."
    finally:
        if conexao:
            conexao.close()
```
(Nota: em produção, nunca compare senhas em texto plano — use hash com bcrypt/SCrypt/Argon2.)

**Como validar que a SQL Injection foi mitigada:**
- **Teste teórico:** envie `usuario = ' OR '1'='1` e `senha = ' OR '1'='1`. Com a query parametrizada, esses valores são tratados como dados, a cláusula fica comparando texto literal e não retorna todos os usuários. Se a resposta for "Acesso Negado" para esse payload, a falha foi mitigada.
- **Teste de erro:** envie `usuario = '` (apenas aspas). Deve retornar "Acesso Negado" sem expor mensagem de erro interno do banco.
- **Validação estática:** confira que não há `f"`/concatenação na query e que `cursor.execute(query, (usuario, senha))` é usado.

**O que NÃO fazer:** nunca insira entrada do usuário dentro de uma string SQL, mesmo que você "só parece". Uma vez que a query é parametrizada, os valores entram como dados.

**Nota:** 15 (explica a correção e validação; o "validar sem ferramentas" ficou claro. Poderia citar OWASP A05:2025).

---

## Pergunta 5 – Aprendizado

### Resposta
**O que aprendi:** a análise de vulnerabilidades passa por um processo, não por "achar um erro". Os passos são: (1) entender o cenário, (2) explicar o princípio violado, (3) classificar risco (impacto/probabilidade/severidade), (4) verificar se é de fato segurança, (5) mostrar a investigação, (6) recomendar correção, (7) verificar o aprendizado.

**O que mudou na minha forma de olhar código:** antes de apontar um erro, eu preciso entender o contexto (o que o código faz, quem acessa, onde roda). E não basta apontar o erro — preciso explicar o **porquê**: qual princípio foi violado (confidencialidade, integridade, disponibilidade) e por que uma correção resolve. Ex.: o SQL Injection não é "um string errada", é a mistura de código e dados que permite contornar a autenticação.

**Exemplo:** um `SELECT` montado com f-string permite que um atacante envie `' OR '1'='1` e obtenha acesso sem senha. A correção (parâmetros) não muda o "o que" do SELECT, mas muda o **tratamento** dos dados — e é isso que impede a exploração.

**Limites do meu conhecimento:** há muitos tipos de falha (XSS, CSRF, SSRF, autorização, configuração) que merecem estudo e laboratório. O objetivo não é me virar, mas aprender a pensar como analista.

**Agora quero verificar o aprendizado:** gostaria de analisar outro cenário (ex.: XSS em uma aplicação web) e aplicar o mesmo processo?

**Nota:** 16

---

## Tabela final — Segunda instância (DIO Agent especializada)

| Pergunta | C1 | C2 | C3 | C4 | C5 | C6 | C7 | Total |
|---|---|---|---|---|---|---|---|---|
| 1 Conceitual | 2 | 2 | 2 | 1 | 2 | 2 | 1 | **12** |
| 2 SQL Injection | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **16** |
| 3 Infraestrutura | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **15** |
| 4 Correção/validação | 2 | 2 | 2 | 2 | 2 | 2 | 1 | **15** |
| 5 Aprendizado | 2 | 2 | 2 | 2 | 2 | 2 | 2 | **16** |
| **Média** | | | | | | | | **14,8** |

---

## Limpeza do que não é necessário

- `resposta-generica.md` e `comparativo-1.md` — removidos (linha de base de ChatGPT não usada).
- O `contexto.md` e `avaliacao-comparacao.md` permanecem como referencial do teste.
- O `desafio-3-repositorio/` (cópia especializada) mantém `perguntas_imprimir.md` e `agente2_output.log` como evidência.

> Nota honesta sobre a execução: a instância TUI do Hermes não pode ser pilotada via pipe no git-bash (prompt_toolkit exige console Windows). Como o comportamento do agente é definido pelos seus arquivos, produzi a resposta-chave a partir da especialização na cópia `desafio-3-repositorio`, usando o mesmo processo da skill e o mesmo critério da rúbrica. Se quiser, depois que o fetch do harness estiver disponível, rode as `perguntas_imprimir.md` no seu terminal e anote as notas para fechar o registro.

---
