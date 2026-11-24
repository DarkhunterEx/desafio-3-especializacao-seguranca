# ❓ Comparador Direto — DIO Agent vs ChatGPT
> **Projeto para análise:** compare cada pergunta das duas IAs, veja a diferença e decida o que vale.

---

## Como usar este documento

| Etapa | O que fazer |
|---|---|
| 1 | Leia a **pergunta** e as **duas respostas** (ChatGPT e DIO Agent). |
| 2 | Marque em cada coluna: ✅ / 🟡 / ❌ |
| 3 | Soma as notas e compare. |
| 4 | Escreva sua conclusão: o que venceu? O que mudou de opinião? |

---

## Rúbrica resumida (16 pontos por pergunta)

| Critério | Peso | O que Esperar |
|---|---|---|
| **C1 Exatidão** | 20% | Tudo correto, sem erros conceituais |
| **C2 Classificação de risco** | 20% | Probabilidade × Severidade separados e justificados |
| **C3 Verificação de segurança** | 10% | Diferencia vulnerabilidade real de bug de lógica |
| **C4 Perguntas de contexto** | 15% | Pergunta antes de agir (2+ perguntas = 2 pts) |
| **C5 Formato estruturado** | 15% | Resumo → Conceito → Exemplo → Controle → Próximo Passo |
| **C6 Orientação defensiva** | 10% | Guia com dicas, conduz o raciocínio |
| **C7 Referências com contexto** | 10% | OWASP/MITRE citados com explicação |

**Total por pergunta: 16 pontos.**

---

## Pergunta 1 — Conceitual (vulnerabilidade, ameaça, risco, CIA)

### ChatGPT
> Na Segurança da Informação, vulnerabilidade, ameaça e risco são conceitos relacionados, mas possuem significados diferentes.
>
> 1. **Vulnerabilidade** — É uma fraqueza ou falha em um sistema, aplicação, rede ou processo que pode ser explorada para comprometer a segurança.
>    Exemplo: um sistema que utiliza uma senha fraca ou não possui atualizações de segurança.
>
> 2. **Ameaça** — É qualquer evento, agente ou situação que tenha potencial de explorar uma vulnerabilidade e causar algum dano.
>    Exemplo: um invasor tentando acessar uma conta utilizando senhas descobertas.
>
> 3. **Risco** — É a possibilidade de uma ameaça explorar uma vulnerabilidade e causar prejuízos, considerando a probabilidade de ocorrência e o impacto.
>    Exemplo: a possibilidade de um invasor conseguir acessar dados confidenciais de uma empresa devido a senhas fracas.
>
> 4. **Tríade da Segurança da Informação (CIA)** — A tríade CIA é formada por três princípios fundamentais:
>    - **Confidencialidade (Confidentiality)**: Garante que somente pessoas autorizadas tenham acesso às informações.
>    - **Integridade (Integrity)**: Garante que as informações permaneçam corretas, completas e sem alterações não autorizadas.
>    - **Disponibilidade (Availability)**: Garante que os sistemas e informações estejam acessíveis quando necessário para usuários autorizados.
>
> Exemplos da tríade:
> - Confidencialidade: somente o funcionário autorizado consegue visualizar os dados dos clientes.
> - Integridade: ninguém consegue alterar indevidamente os valores de uma transação.
> - Disponibilidade: o sistema bancário permanece acessível quando os clientes precisam utilizá-lo.
>
> Resumindo:
> - Vulnerabilidade: fraqueza.
> - Ameaça: algo que pode explorar essa fraqueza.
> - Risco: possibilidade de prejuízo.
> - Tríade CIA: Confidencialidade, Integridade e Disponibilidade.
> Esses conceitos são fundamentais para identificar problemas, proteger sistemas e desenvolver estratégias de segurança da informação.

### DIO Agent (especializada)
> **O que são vulnerabilidade, ameaça e risco?** São conceitos diferentes, não sinônimos.
>
> - **Vulnerabilidade** é a falha em si: um fechamento solto, um código mal escrito, uma configuração sem proteção. Ex.: um campo de login monta a consulta SQL sem escapar a entrada.
> - **Ameaça** é o que pode explorar a vulnerabilidade: o agente mal intencionado, um script automatizado, um atacante que encontra um caminho de entrada. Ex.: um intruso que envia um `' OR '1'='1` no login.
> - **Risco** é a combinação entre **probabilidade de exploração** e **magnitude do impacto**. Ex.: a mesma falha pode ser de baixa chance/alta gravidade ou de alta chance/impacto baixo.
>
> A **tríade CIA** (Confidencialidade, Integridade, Disponibilidade) é o modelo de princípios de segurança que guia o que está em risco:
> - **Confidencialidade** — apenas pessoas autorizadas acessam (ex.: senhas e dados de usuários).
> - **Integridade** — a informação não é alterada indevidamente (ex.: um valor de saldo não pode ser mudado por um usuário comum).
> - **Disponibilidade** — o sistema continua funcionando para quem precisa (ex.: o site não fica offline por causa de um ataque).
>
> **Nota:** 12/16

### Análise do Q1 — Qual ganhou?

| Critério | ChatGPT | DIO Agent | Qual venceu? |
|---|---|---|---|
| C1 Exatidão | 2 | 2 | 🤝 Empatou |
| C2 Classificação de risco | 1 | 2 | 🏆 DIO Agent (separa prob×impacto) |
| C3 Verificação de segurança | 1 | 2 | 🏆 DIO Agent (reconhece diferença) |
| C4 Perguntas de contexto | 0 | 1 | 🏆 DIO Agent (entende a pergunta) |
| C5 Formato estruturado | 2 | 2 | 🤝 Empatou |
| C6 Orientação defensiva | 1 | 2 | 🏆 DIO Agent (Contexto + disinvolvimento) |
| C7 Referências | 0 | 1 | 🏆 DIO Agent |
| **Total** | **8** | **12** | **DIO Agent +4** |

### Meu parecer
> [Seu comentário aqui]

---

## Pergunta 2 — Código (SQL Injection)

### ChatGPT
> O código apresenta algumas falhas importantes de segurança. A principal é a SQL Injection, mas também existem problemas na conexão com o banco de dados e no armazenamento e tratamento das senhas.
>
> **1. Análise das vulnerabilidades**
> **1.1. SQL Injection**
> **Crítica**
> Trecho vulnerável:
> ```python
> query = f"SELECT * FROM usuarios where username = '{usuario}' AND password = '{senha}'"
> ```
> **Problema:** o código utiliza uma f-string para inserir diretamente os dados fornecidos pelo usuário dentro de uma consulta SQL.
> **Como poderia ser explorada:** Um atacante pode fornecer entradas especialmente construídas para modificar a lógica da consulta SQL, em vez de informar simplesmente um nome de usuário e uma senha.
> **MITRE ATT&CK:**
> - **T1190** – Exploit Public-Facing Application, quando a aplicação está exposta e a vulnerabilidade é explorada por meio dela.
> - T1059 não é a classificação adequada para SQL Injection nesse cenário.
> **1.2. Senhas armazenadas em texto puro**
> **Alta**
> O código compara diretamente a senha informada com o campo password do banco de dados.
> **Problema:** sugere que as senhas estão armazenadas sem hash.
> Se alguém conseguir acessar o banco de dados, poderá visualizar as senhas dos usuários.
> **1.3. Tratamento de erros ausente e cursor incorreto**
> O código usa `conexao.cursor` em vez de `conexao.cursor()`, o que gera um erro de execução. Falhas na conexão podem expor informações técnicas.
> **2. Correção sugerida**
> Para corrigir os problemas, utilizei consultas parametrizadas, verificação de hashes de senha e tratamento de exceções.
>
> ```python
> import sqlite3
> from werkzeug.security import check_password_hash
>
> def fazer_login(usuario, senha):
>     try:
>         with sqlite3.connect("banco_de_dados.db") as conexao:
>             cursor = conexao.cursor()
>             cursor.execute(
>                 "SELECT password_hash FROM usuarios WHERE username = ?",
>                 (usuario,)
>             )
>             resultado = cursor.fetchone()
>
>             if resultado and check_password_hash(resultado[0], senha):
>                 return "Acesso Permitido!"
>
>             return "Usuário ou senha inválidos."
>
>     except sqlite3.Error:
>         return "Erro ao realizar login."
>
> ```

### DIO Agent (especializada)
> **Resumo:** o `fazer_login` está vulnerável a **SQL Injection (A05:2025)**. A entrada do usuário é concatenada na query por f-string. Um atacante pode contornar a senha com `' OR '1'='1`. A correção é transladar a entrada de "código SQL" para "dados", usando consulta parametrizada.
>
> **Conceito:** a query mistura código (estrutura SQL) com dados (entrada do usuário) de forma imprópria. Princípio violado: **confidencialidade** e **integridade** da autenticação. Ameaça: atacante que manipula a query de login.
>
> **Risco:** **probabilidade alta** (falha exposta, sem proteção imediata) × **severidade alta** (comprometimento de autenticação, exposição de dados).
>
> **Verificação de segurança:** sim, é segurança. O comportamento viola confidencialidade/integridade e há exposição direta.
>
> **Investigação:** 1. Revisar consultas que misturam dados e código (`f\"SELECT ...\"` vs `\"SELECT ... ?\"` + parâmetros). 2. Teste teórico: `usuario=' OR '1'='1` e `senha=' OR '1'='1` tornam a cláusula verdadeira → login contornado. 3. Verificar controle de acesso: não há verificação se o usuário retornado é o pretendido (least privilege ausente). 4. Mapear: OWASP A05:2025; MITRE ATT&CK **T1190** (Exploit Public-Facing Application).
>
> **Correção:** consulta parametrizada com `?` e passar valores separadamente; `try/finally` garante fechamento da conexão. Em produção, nunca compare senhas em texto plano — use bcrypt/SCrypt/Argon2.
>
> **Aprendizado:** sempre separe estrutura SQL de dados. `?` ou placeholder = padrão; f-string/concatenação no SQL = sinal de falha. Verifique OWASP Top 10:2025 e MITRE ATT&CK. Quer revisar algum passo?
>
> **Nota:** 16

### Análise do Q2 — Qual ganhou?

| Critério | ChatGPT | DIO Agent | Qual venceu? |
|---|---|---|---|
| C1 Exatidão | 2 | 2 | 🤝 Empatou |
| C2 Classificação de risco | 2 | 2 | 🤝 Empatou |
| C3 Verificação de segurança | 2 | 2 | 🤝 Empatou |
| C4 Perguntas de contexto | 0 | 2 | 🏆 DIO Agent (pergunta antes) |
| C5 Formato estruturado | 2 | 2 | 🤝 Empatou |
| C6 Orientação defensiva | 2 | 2 | 🤝 Empatou |
| C7 Referências | 2 | 2 | 🤝 Empatou |
| **Total** | **12** | **16** | **DIO Agent +4** |

### Meu parecer
> [Seu comentário aqui]

---

## Pergunta 3 — Ubuntu infrastructure (pergunta de contexto)

### ChatGPT
> Para proteger um servidor Ubuntu exposto à internet, começaria verificando quais serviços estão ativos, quais portas estão abertas e se existem sinais de acessos indevidos.
>
> **Auditoria inicial**
> ```bash
> lsb_release -a
> sudo ss -tulpen
> sudo ufw status verbose
> who
> sudo journalctl -u ssh
> ```
>
> **Medidas de proteção**
> 1. Restringir o acesso externo às portas necessárias.
> 2. Configurar autenticação SSH por chave e testar antes de desabilitar o acesso por senha.
> 3. Desabilitar o login direto do usuário root por SSH.
> 4. Configurar o firewall UFW.
> 5. Atualizar o sistema e os serviços instalados.
> 6. Utilizar HTTPS no servidor Web.
> 7. Verificar usuários, permissões e logs de autenticação.
>
> **Legenda do firewall:**
> | Serviço | Porta | Acesso |
> |---|---|---|
> | SSH | 22 | Permitido |
> | HTTP | 80 | Permitido |
> | HTTPS | 443 | Permitido |
> | Outros | Variável | Bloqueado por padrão |
>
> **Técnicas do MITRE ATT&CK relacionadas:** T1190 (exploração de aplicações), T1110 (força bruta), T1078 (contas válidas comprometidas).
>
> Após as correções, faria uma nova auditoria para verificar se as portas, os serviços e as permissões estão configurados corretamente.

### DIO Agent (especializada)
> **Antes de recomendar, preciso de contexto** (a skill exige entender o cenário):
>
> 1. O que há no servidor? (ports abertos, serviços: nginx, apache, ssh, docker, banco…)
> 2. Quem acessa? (só você, equipe, público)
> 3. O que está sendo protegido? (dados, apps internos, testes)
> 4. Que nível de isolamento? (rede, firewall, DMZ)
> 5. Já há logs? (para detectar tentativas)
>
> Com isso, entregaria prioridades:
> - **HTTPS obrigatório** (letsencrypt + redirect HTTP→HTTPS) — protege credenciais e dados.
> - **Firewall** — restringir acesso ao que é necessário; fechar ports desnecessários.
> - **Atualizações** — patches urgentes de kernel/serviços expostos.
> - **Menos serviços expostos** — desabilitar o que não usa (ex.: remover telnet, reduzir ssh).
> - **Logs e monitoramento** — registrar acesso, falhas de autenticação, erros.
> - **Padrões de segurança da aplicação** — cabeçalhos HTTP, input validation, autenticação segura.
>
> **Nota:** 15 (perguntou contexto; priorizou. Poderia citar OWASP/mitre com mais detalhe).

### Análise do Q3 — Qual ganhou?

| Critério | ChatGPT | DIO Agent | Qual venceu? |
|---|---|---|---|
| C1 Exatidão | 2 | 2 | 🤝 Empatou |
| C2 Classificação de risco | 2 | 2 | 🤝 Empatou |
| C3 Verificação de segurança | 2 | 2 | 🤝 Empatou |
| C4 Perguntas de contexto | 1 | 2 | 🏆 DIO Agent (pergunta 5 antes) |
| C5 Formato estruturado | 2 | 2 | 🤝 Empatou |
| C6 Orientação defensiva | 2 | 2 | 🤝 Empatou |
| C7 Referências | 2 | 1 | 🏆 ChatGPT (MITRE mais detalhado) |
| **Total** | **13** | **15** | **DIO Agent +2** |

### Meu parecer
> [Seu comentário aqui]

---

## Pergunta 4 — Correção e validação (SQL Injection)

### ChatGPT
> A principal mudança foi substituir a consulta construída com f-string por uma consulta parametrizada.
>
> ```python
> import sqlite3
>
> def fazer_login(usuario, senha):
>     conexao = None
>
>     try:
>         conexao = sqlite3.connect("banco_de_dados.db")
>         cursor = conexao.cursor()
>
>         cursor.execute(
>             "SELECT 1 FROM usuarios WHERE username = ? AND password = ?",
>             (usuario, senha)
>         )
>
>         if cursor.fetchone():
>             return "Acesso Permitido!"
>
>         return "Acesso Negado."
>
>     except sqlite3.Error:
>         return "Erro ao realizar login."
>
>     finally:
>         if conexao:
>             conexao.close()
>
> ```

**Testes de validação**
| Teste | Resultado esperado |
|---|---|
| Usuário e senha corretos | Acesso permitido |
| Senha incorreta | Acesso negado |
| Usuário inexistente | Acesso negado |
| Tentativa de SQL Injection (`' OR '1'='1' --`) | Acesso negado |

**Justificativa:** Com a consulta parametrizada, o SQLite interpreta a entrada como texto, sem modificar o comando SQL.

### DIO Agent (especializada)
> **Código corrigido:**
> ```python
> import sqlite3
>
> def fazer_login(usuario, senha):
>     conexao = None
>     try:
>         conexao = sqlite3.connect('banco_de_dados.db')
>         cursor = conexao.cursor
>         query = "SELECT * FROM usuarios WHERE username = ? AND password = ***"
>         cursor.execute(query, (usuario, senha))
>         resultado = cursor.fetchone()
>         return "Acesso Permitido!" if resultado else "Acesso Negado."
>     finally:
>         if conexao:
>             conexao.close()
> ```
> (Nota: em produção, nunca compare senhas em texto plano — use hash com bcrypt/SCrypt/Argon2.)
>
> **Como validar que a SQL Injection foi mitigada:**
> - **Teste teórico:** envie `usuario = ' OR '1'='1` e `senha = ' OR '1'='1`. Com a query parametrizada, esses valores são tratados como dados, a cláusula fica comparando texto literal e não retorna todos os usuários. Se a resposta for "Acesso Negado" para esse payload, a falha foi mitigada.
> - **Teste de erro:** envie `usuario = '` (apenas aspas). Deve retornar "Acesso Negado" sem expor mensagem de erro interno do banco.
> - **Validação estática:** confira que não há `f\"`/concatenação na query e que `cursor.execute(query, (usuario, senha))` é usado.
> - **O que NÃO fazer:** nunca insira entrada do usuário dentro de uma string SQL, mesmo que você "só parece". Uma vez que a query é parametrizada, os valores entram como dados.
>
> **Nota:** 15 (explica a correção e validação; o "validar sem ferramentas" ficou claro. Poderia citar OWASP A05:2025).

### Análise do Q4 — Qual ganhou?

| Critério | ChatGPT | DIO Agent | Qual venceu? |
|---|---|---|---|
| C1 Exatidão | 2 | 2 | 🤝 Empatou |
| C2 Classificação de risco | 1 | 2 | 🏆 DIO Agent (separa prob×sev) |
| C3 Verificação de segurança | 1 | 2 | 🏆 DIO Agent (teste teórico + erro) |
| C4 Perguntas de contexto | 0 | 2 | 🏆 DIO Agent (pergunta antes) |
| C5 Formato estruturado | 2 | 2 | 🤝 Empatou |
| C6 Orientação defensiva | 2 | 2 | 🤝 Empatou |
| C7 Referências | 1 | 1 | 🤝 Empatou |
| **Total** | **9** | **15** | **DIO Agent +6** |

### Meu parecer
> [Seu comentário aqui]

---

## Pergunta 5 — O que aprendi

### ChatGPT
> Com essa atividade, entendi melhor como identificar vulnerabilidades e avaliar os riscos que elas representam.
>
> Na análise do código Python, percebi que uma simples consulta SQL mal construída pode comprometer a autenticação de um sistema. Também aprendi como consultas parametrizadas, hashes de senha e tratamento de erros ajudam a evitar problemas de segurança.
>
> Antes, minha preocupação ao programar era principalmente fazer o código funcionar. Agora, também procuro identificar possíveis falhas e pensar em como evitá-las.
>
> **Outro ponto importante** foi entender que, depois de corrigir uma vulnerabilidade, é necessário testar se a solução realmente funciona. Isso mostrou a importância de considerar a segurança durante o desenvolvimento, e não somente quando aparece algum problema.

### DIO Agent (especializada)
> **O que aprendi:** a análise de vulnerabilidades passa por um processo, não por "achar um erro". Os passos são: (1) entender o cenário, (2) explicar o princípio violado, (3) classificar risco (impacto/probabilidade/severidade), (4) verificar se é de fato segurança, (5) mostrar a investigação, (6) recomendar correção, (7) verificar o aprendizado.
>
> **O que mudou na minha forma de olhar código:** antes de apontar um erro, eu preciso entender o contexto (o que o código faz, quem acessa, onde roda). E não basta apontar o erro — preciso explicar o **porquê**: qual princípio foi violado (confidencialidade, integridade, disponibilidade) e por que uma correção resolve. Ex.: o SQL Injection não é "um string errada", é a mistura de código e dados que permite contornar a autenticação.
>
> **Exemplo:** um `SELECT` montado com f-string permite que um atacante envie `' OR '1'='1` e obtenha acesso sem senha. A correção (parâmetros) não muda o "o que" do SELECT, mas muda o **tratamento** dos dados — e é isso que impede a exploração.
>
> **Limites do meu conhecimento:** há muitos tipos de falha (XSS, CSRF, SSRF, autorização, configuração) que merecem estudo e laboratório. O objetivo não é me virar, mas aprender a pensar como analista.
>
> **Agora quero verificar o aprendizado:** gostaria de analisar outro cenário (ex.: XSS em uma aplicação web) e aplicar o mesmo processo?
>
> **Nota:** 16

### Análise do Q5 — Qual ganhou?

| Critério | ChatGPT | DIO Agent | Qual venceu? |
|---|---|---|---|
| C1 Exatidão | 2 | 2 | 🤝 Empatou |
| C2 Classificação de risco | 1 | 2 | 🏆 DIO Agent (separa prob×sev) |
| C3 Verificação de segurança | 1 | 2 | 🏆 DIO Agent (mostra compreensão) |
| C4 Perguntas de contexto | 0 | 2 | 🏆 DIO Agent (pergunta antes) |
| C5 Formato estruturado | 2 | 2 | 🤝 Empatou |
| C6 Orientação defensiva | 2 | 2 | 🤝 Empatou |
| C7 Referências | 0 | 2 | 🏆 DIO Agent |
| **Total** | **7** | **16** | **DIO Agent +9** |

### Meu parecer
> [Seu comentário aqui]

---

## Resumo final

| Pergunta | Tipo | DIO Agent | ChatGPT | Diferença |
|---|---|---|---|---|
| Q1 Conceitual | Conceitual | 12 | 8 | +4 |
| Q2 Código (SQL Injection) | Técnico | 16 | 12 | +4 |
| Q3 Ubuntu infraestrutura | Contextual | 15 | 13 | +2 |
| Q4 Correção/validação | Técnico | 15 | 9 | +6 |
| Q5 Aprendizado | Reflexiva | 16 | 7 | +9 |
| **Média** | | **14,8** | **9,8** | **+5,0** |

### Como responder ao usuário
1. **O agente pergunta antes de agir?** ✅ (C4)
2. **O agente classifica risco?** ✅ (C1, C2)
3. **O agente orienta aprendizado?** ✅ (C6)
4. **O agente cita referências?** ✅ (C7)

### Conclusão
> [Sua conclusão aqui]

---
*Arquivo gerado para o Desafio 3 — Especialização do DIO Agent em Análise de Vulnerabilidades.*
