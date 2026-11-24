# 📋 Respostas Completas — DIO Agent vs ChatGPT (Desafio 3)

> Reunião de todas as respostas dos 5 cenários, com notas e classificação.
> Use este documento para análise e revisão.

---

## Q1 – Vulnerabilidade, ameaça, risco e tríade CIA

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

**Notas:** C1=2, C2=1, C3=1, C4=0, C5=2, C6=1, C7=0 → **8/16**

---

## Q2 – Análise de vulnerabilidades em código Python

### ChatGPT
> O código apresenta algumas falhas importantes de segurança. A principal é a SQL Injection, mas também existem problemas na conexão com o banco de dados e no armazenamento e tratamento das senhas.
>
> **1. Análise das vulnerabilidades**
>
> **1.1. SQL Injection**
> **Crítica**
> Trecho vulnerável:
> ```python
> query = f"SELECT * FROM usuarios where username = '{usuario}' AND password = '{senha}'"
> ```
> **Problema:** o código utiliza uma f-string para inserir diretamente os dados fornecidos pelo usuário dentro de uma consulta SQL.
>
> **Como poderia ser explorada:** Um atacante pode fornecer entradas especialmente construídas para modificar a lógica da consulta SQL, em vez de informar simplesmente um nome de usuário e uma senha.
>
> **MITRE ATT&CK:**
> - **T1190** – Exploit Public-Facing Application, quando a aplicação está exposta e a vulnerabilidade é explorada por meio dela.
> - T1059 não é a classificação adequada para SQL Injection nesse cenário.
>
> **1.2. Senhas armazenadas em texto puro**
> **Alta**
> O código compara diretamente a senha informada com o campo password do banco de dados.
> **Problema:** sugere que as senhas estão armazenadas sem hash.
> Se alguém conseguir acessar o banco de dados, poderá visualizar as senhas dos usuários.
>
> **1.3. Tratamento de erros ausente e cursor incorreto**
> O código usa `conexao.cursor` em vez de `conexao.cursor()`, o que gera um erro de execução. Falhas na conexão podem expor informações técnicas.
>
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

**Notas:** C1=2, C2=2, C3=2, C4=0, C5=2, C6=2, C7=2 → **12/16**

---

## Q3 – Proteção de um servidor Ubuntu

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

**Notas:** C1=2, C2=2, C3=2, C4=1, C5=2, C6=2, C7=2 → **13/16**

---

## Q4 – Correção e validação da SQL Injection

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

**Notas:** C1=2, C2=1, C3=1, C4=0, C5=2, C6=2, C7=1 → **9/16**

---

## Q5 – O que aprendi

### ChatGPT
> Com essa atividade, entendi melhor como identificar vulnerabilidades e avaliar os riscos que elas representam.
>
> Na análise do código Python, percebi que uma simples consulta SQL mal construída pode comprometer a autenticação de um sistema. Também aprendi como consultas parametrizadas, hashes de senha e tratamento de erros ajudam a evitar problemas de segurança.
>
> Antes, minha preocupação ao programar era principalmente fazer o código funcionar. Agora, também procuro identificar possíveis falhas e pensar em como evitá-las.
>
> **Outro ponto importante** foi entender que, depois de corrigir uma vulnerabilidade, é necessário testar se a solução realmente funciona. Isso mostrou a importância de considerar a segurança durante o desenvolvimento, e não somente quando aparece algum problema.

**Notas:** C1=2, C2=1, C3=1, C4=0, C5=2, C6=2, C7=0 → **7/16**

---

## Resumo comparativo

| Pergunta | Tipo | DIO Agent | ChatGPT | Diferença |
|---|---|---|---|---|
| Q1 Conceitual | Conceitual | 12 | 8 | +4 |
| Q2 Código (SQL Injection) | Técnico | 16 | 12 | +4 |
| Q3 Ubuntu infraestrutura | Contextual | 12 | 13 | −1 |
| Q4 Correção/validação | Técnico | 15 | 9 | +6 |
| Q5 Aprendizado | Reflexiva | 16 | 7 | +9 |
| **Média** | | **14,2** | **9,8** | **+4,4** |

---

*Arquivo gerado para o Desafio 3 — Especialização do DIO Agent em Análise de Vulnerabilidades.*
