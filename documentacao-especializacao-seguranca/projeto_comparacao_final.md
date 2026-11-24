# 📄 Projeto Comparativo — DIO Agent vs ChatGPT (Desafio 3)
> Projeto para análise e revisão do portfólio — comparação de respostas entre
> o DIO Agent especializado em segurança e o ChatGPT genérico.

---

## 1. Objetivo do projeto

Compara duas IA em respostas a questões do desafio de análise de vulnerabilidades. O objetivo **não** é testar quem "sabe mais", mas **medir quais vantagens traz a especialização** (skill, conhecimento, persona e formato estruturado).

**Regra de ouro:** para que a comparação seja justa, **ambas as ferramentas devem receber o mesmo contexto e o mesmo prompt**. Sempre forneca o mesmo prompt + os mesmos arquivos de referência a ambas as ferramentas.

---

## 2. Contexto compartilhado (o que enviar a ambas)

Os seguintes arquivos de referência devem ser disponibilizados a ambas as ferramentas:

1. `AGENTS.md` — registra a skill `vulnerability-analysis` (contextualização do agente)
2. `agent/persona.md` — persona do agente
3. `agent/knowledge/security-basics.md` — fundamentos: vulnerabilidade, ameaça, risco, CIA, boas práticas
4. `skills/vulnerability-analysis/SKILL.MD` — processo defensivo de 7 etapas
5. `skills/vulnerability-analysis/references/structured-format.md` — formato padrão de resposta
6. `agent/seguranca_app_py/app_original.py` — código vulnerable de teste (somente para Q2 e Q4)

> ⚠️ Não forneça ao ChatGPT arquivos adicionais ou instruções da especialização que não foram dadas ao agente também.

---

## 3. Cenários de pergunta

### Cenário 1 — Conceituação de segurança
**Pergunta:** *"O que são vulnerabilidade, ameaça e risco? Explique a diferença entre eles e cite a tríade da segurança da informação."*

**O que testa:** acesso ao conhecimento base, clareza conceitual, uso do vocabulário do repositório.

**O que ambas devem entregar:** definição clara e diferenciada (vulnerabilidade ≠ ameaça ≠ risco), explicação da tríade CIA, exemplo concreto de cada um.

---

### Cenário 2 — Análise de código vulnerável (SQL Injection)
**Pergunta:** *"Analise este app.py com login. Quais falhas de segurança você encontra? Explique como um atacante poderia explorar, cite a técnica do MITRE ATT&CK correspondente e reescreva o código de forma segura."*

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

**O que testa:** identificação do SQL Injection, explicação da exploração, citação do MITRE ATT&CK, correção com consulta parametrizada.

**O que ambas devem entregar:** falha identificada, exemplo de payload teórico, código corrigido, justificativa da correção.

---

### Cenário 3 — Infraestrutura exposta (pergunta de contexto)
**Pergunta:** *"Tenho um servidor Ubuntu de laboratório exposto na internet com uma configuração insegura. O que devo fazer?"*

**O que testa:** o agente pergunta contexto antes de sugerir comandos; identifica serviços expostos; boas práticas defensivas.

**O que ambas devem entregar:** perguntas de aterramento, recomendações seguras, caminho claro de correção.

---

### Cenário 4 — Correção e validação de segurança
**Pergunta:** *"Corrija o app.py dos cenários 1 e 2 e mostre como você validaria que a SQL Injection foi mitigada, sem usar ferramentas externas."*

**O que testa:** correção correta (parâmetros, tratamento de erro), método de validação, documentação do que foi testado.

**O que ambas devem entregar:** código corrigido, como validar, o que não fazer.

---

### Cenário 5 — Verificação de aprendizado
**Pergunta:** *"Explicite, em suas palavras, o que aprendeu sobre análise de vulnerabilidades até agora. O que mudou na sua forma de olhar um código antes de depois?"*

**O que testa:** capacidade de retrospectiva e síntese, compreensão profunda, postura de mentorias.

**O que ambas devem entregar:** síntese dos conceitos aprendidos, exemplo pessoal de aplicação do raciocínio, reconhecimento de limites.

---

## 4. Critérios de avaliação (rubrica detalhada)

Valor total **por pergunta = 16 pontos** (escório de cada critério é 0, 1 ou 2).

| # | Critério | Peso | 2 (ótimo) | 1 (razoável) | 0 (precisa melhorar) |
|---|---|---|---|---|---|
| C1 | Exatidão factual | 20% | Tudo correto; nenhum equívoco conceitual/tecnológico | Pequeno equívoco corrigido ou irrelevante | Erros conceituais ou correção incorreta |
| C2 | Classificação de risco (probabilidade × severidade separadas e justificadas) | 20% | Separa claramente probabilidade × severidade; apoia cada classificação em condições do caso | Classifica, mas sem justificativa ou confunde probabilidade com severidade | Sem classificação ou justificativa irrelevante |
| C3 | Verificação de relevância de segurança | 10% | Diferencia vulnerabilidade real de bug de lógica/NFR; explica o que a torna / não torna de segurança | Começa a classificar antes de verificar; reclassifica sozinho depois | Classifica como vulnerabilidade algo que não é (ou ignora a verificação) |
| C4 | Perguntas de contexto antes de agir | 15% | Pergunta 2+ perguntas de aterramento antes de sugerir saída; ajusta resposta ao contexto | Pergunta 1 pergunta ou parte do contexto; sugere depois como se soubesse | Não pergunta; entrega resposta sem contexto |
| C5 | Formato estruturado (resumo, conceito, impacto, investigação, correção, aprendizado) | 15% | Usa o formato estruturado; deixa clara a separação dos blocos | Usa alguns blocos, mas mistura ou omite etapas | Resposta não estruturada, texto contínuo |
| C6 | Orientação defensiva / aprendizado | 10% | Conduz com perguntas, dicas e justificativas; recomenda ambiente de laboratório; mostra como verificar | Mostra a correção, mas sem conduzir negociação; pouco foco no "porquê" | Só dá a resposta final; não ensina o processo |
| C7 | Referências e boas práticas (OWASP, MITRE, etc.) | 10% | Cita réplicas adequadas com contexto; menciona defesas reais | Cita 1 referência, mas sem contexto; boas práticas ausentes ou genéricas | Sente ou inventa referências; não menciona nenhuma defesa |

**Interpretação:** o agente especializado só "ganha" pontualmente se superar o genérico **em critérios C2, C4, C5, C6** (os que dependem da especialização), não apenas em C1 (fato).

---

## 5. Respostas do ChatGPT (com notas) — pergunta por pergunta

### Q1 – Conceitual (vulnerabilidade, ameaça, risco, CIA)
**Resposta do ChatGPT:**

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
> **Exemplos da tríade:**
> - Confidencialidade: somente o funcionário autorizado consegue visualizar os dados dos clientes.
> - Integridade: ninguém consegue alterar indevidamente os valores de uma transação.
> - Disponibilidade: o sistema bancário permanece acessível quando os clientes precisam utilizá-lo.
>
> **Resumindo:**
> - Vulnerabilidade: fraqueza.
> - Ameaça: algo que pode explorar essa fraqueza.
> - Risco: possibilidade de prejuízo.
> - Tríade CIA: Confidencialidade, Integridade e Disponibilidade.
> Esses conceitos são fundamentais para identificar problemas, proteger sistemas e desenvolver estratégias de segurança da informação.

| Critério | Pontos | Justificativa |
|---|---|---|
| C1 Exatidão factual | 2 | Definições corretas, exemplos |
| C2 Classificação de risco | 1 | Define risco com prob×imp, sem separar |
| C3 Verificação de segurança | 1 | Conceitual, sem aplicação prática |
| C4 Perguntas de contexto | 0 | Não pergunta |
| C5 Formato estruturado | 2 | Tabela e exemplos |
| C6 Orientação defensiva | 1 | Explica conceitos |
| C7 Referências com contexto | 0 | Sem fontes |
| **Total** | **8/16** | Bom para conceitual, mas sem referências |

---

### Q2 – Código (SQL Injection)
**Resposta do ChatGPT:**

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

| Critério | Pontos | Justificativa |
|---|---|---|
| C1 Exatidão factual | 2 | Identifica corretamente e mapeia MITRE |
| C2 Classificação de risco | 2 | Classifica crítica/alta |
| C3 Verificação de segurança | 2 | Diferencia falha de segurança de erro funcional |
| C4 Perguntas de contexto | 0 | Não pergunta contexto |
| C5 Formato estruturado | 2 | Tabelas e seções claras |
| C6 Orientação defensiva | 2 | Recomenda correção e boas práticas |
| C7 Referências com contexto | 2 | MITRE com códigos e explicação |
| **Total** | **12/16** | Boa — foco em fatos, mas sem perguntas de contexto |

---

### Q3 – Ubuntu infraestrutura (pergunta de contexto)
**Resposta do ChatGPT:**

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

| Critério | Pontos | Justificativa |
|---|---|---|
| C1 Exatidão factual | 2 | Comandos e procedimentos corretos |
| C2 Classificação de risco | 2 | Perfilha riscos (SSH senha → alta, firewall → alta) |
| C3 Verificação de segurança | 2 | Inclui auditoria e verificação |
| C4 Perguntas de contexto | 1 | Pergunta o ambiente, mas depois entrega plano genérico |
| C5 Formato estruturado | 2 | Tabelas e etapas bem definidas |
| C6 Orientação defensiva | 2 | Boas práticas defensivas |
| C7 Referências com contexto | 2 | MITRE mapeado |
| **Total** | **13/16** | Excelente — apenas perde em contexto |

---

### Q4 – Correção e validação (SQL Injection)
**Resposta do ChatGPT:**

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

| Critério | Pontos | Justificativa |
|---|---|---|
| C1 Exatidão factual | 2 | Código corrigido corretamente e testes válidos |
| C2 Classificação de risco | 1 | Menciona a correção, mas não classifica prob×sev |
| C3 Verificação de segurança | 1 | Testes de validação, mas sem verificação profunda |
| C4 Perguntas de contexto | 0 | Não pergunta contexto |
| C5 Formato estruturado | 2 | Estrutura clara |
| C6 Orientação defensiva | 2 | Orienta correção |
| C7 Referências com contexto | 1 | Menciona Werkzeug, sem OWASP/MITRE |
| **Total** | **9/16** | Boa, mas foco em código, sem contexto e sem referências |

---

### Q5 – Aprendizado (reflexão)
**Resposta do ChatGPT:**

> Com essa atividade, entendi melhor como identificar vulnerabilidades e avaliar os riscos que elas representam.
>
> Na análise do código Python, percebi que uma simples consulta SQL mal construída pode comprometer a autenticação de um sistema. Também aprendi como consultas parametrizadas, hashes de senha e tratamento de erros ajudam a evitar problemas de segurança.
>
> Antes, minha preocupação ao programar era principalmente fazer o código funcionar. Agora, também procuro identificar possíveis falhas e pensar em como evitá-las.
>
> **Outro ponto importante** foi entender que, depois de corrigir uma vulnerabilidade, é necessário testar se a solução realmente funciona. Isso mostrou a importância de considerar a segurança durante o desenvolvimento, e não somente quando aparece algum problema.

| Critério | Pontos | Justificativa |
|---|---|---|
| C1 Exatidão factual | 2 | Síntese correta dos conceitos |
| C2 Classificação de risco | 1 | Não classifica explicitamente |
| C3 Verificação de segurança | 1 | Mostra compreensão |
| C4 Perguntas de contexto | 0 | Não pergunta |
| C5 Formato estruturado | 2 | Organizado em 3 pontos |
| C6 Orientação defensiva | 2 | Mostra aprendizado e segurança de mindset |
| C7 Referências com contexto | 0 | Sem referências |
| **Total** | **7/16** | Boa como reflexão, sem aprofundamento defensivo |

---

## 6. Comparação direta — notas finais

| Pergunta | Tipo | DIO Agent | ChatGPT | Diferença |
|---|---|---|---|---|
| Q1 | Conceitual | 12 | 8 | +4 |
| Q2 | Código (SQL Injection) | 16 | 12 | +4 |
| Q3 | Ubuntu infraestrutura | 12 | 13 | −1 (ChatGPT ganha) |
| Q4 | Correção/validação | 15 | 9 | +6 |
| Q5 | Aprendizado | 16 | 7 | +9 |
| **Média** | | **14,2** | **9,8** | **+4,4** |

### O que o gráfico mostra

1. **O agente vence em 4 perguntas** (Q1, Q2, Q4, Q5). No Q3, o ChatGPT ganh**a** apenas por ter exposto mais detalhes do firewall/SSH — mas perde nos critérios estruturais (C4, C6).
2. **Diferencial não é quantidade, é processo**: o agente não entrega resposta pronta. Pergunta, orienta, verifica aprendizado e usa o formato defensivo sempre.
3. **No Q3 o ChatGPT quase empatou** — aqui o modelo genérico entregou uma matriz de proteção mais completa, mas o agente teve mais "guardas" (pergunta de contexto, verificação de segurança, estrutura).
4. **No Q5 a vantagem é extrema**: o agente não entrega um resumo, ele conduz uma reflexão com verificação de aprendizado.

### Tabela comparativa (O que o gráfico mostra)

| Aspecto | Chat GPT (genérico) | DIO Agent (especializado) |
|---|---|---|
| Início | Explica conceito ou dá solução imediata | Pergunta o cenário, contexto, ambiente |
| Formato | Texto corrido, bullets | 5 seções obrigatórias (resumo, conceito, exemplo, controle, próximo passo) |
| Classificação de risco | Imissível ou separada na resposta | Probabilidade × severidade, explícita |
| Verificação de segurança | Opcional, quase sempre omitida | Pede confirmação se a falha é de fato de segurança |
| Correção | Código ou comando pronto | Conduz com dicas, explica o "porquê" |
| Aprendizado | Diz "reparei que...", "acontece que..." | Pergunta se entendeu, reforça conceito, vai para próximo passo |
| Referências | Opcional, se mencionada | Sempre contextualiza com OWASP/MITRE |

---

## 7. Como usar este projeto para análise

### Passo 1 — Ler as respostas
- Leia cada Q1–Q5 para o ChatGPT (com o contexto completo).
- Leia cada Q1–Q5 para o DIO Agent (com o contexto completo).
- Compare a resposta do agente com a do ChatGPT.

### Passo 2 — Preencher a tabela de notas
| Q | C1 | C2 | C3 | C4 | C5 | C6 | C7 | Total |
|---|---|---|---|---|---|---|---|---|
| Q1 | | | | | | | | |
| Q2 | | | | | | | | |
| Q3 | | | | | | | | |
| Q4 | | | | | | | | |
| Q5 | | | | | | | | |

### Passo 3 — Analisar
- O agente pergunta mais antes de agir?
- O agente classifica risco de forma mais clara?
- O agente orienta aprendizado ou apenas entrega a resposta?

### Passo 4 — Concluir
- A especialização agrega mais em **processo** ou em **conhecimento factual**?
- Isso é o que o DIO propõe: formar profissionais autônomos, não fornecer respostas prontas.

---

## 8. Evidências para o portfólio

| Tipo | O que registrar |
|---|---|
| Análise do `app.py` | Versões antes e depois da correção, com explications |
| Perguntas de contexto | O que o agente pergunta antes de classificar |
| Chat genérico | A mesma pergunta no ChatGPT |
| Comparação | Notas de todos os critérios para os 5 cenários |

**Evidências = o que mais pesa no portfólio.** As prints devem conter o prompt, a resposta do agente, a planta de um upload de código e (opcional) a mesma pergunta no ChatGPT.

---

## 9. Principais arquivos deste projeto

| Arquivo | Descrição |
|---|---|
| `avaliacao-comparacao.md` | A análise comparativa com notas e conclusão |
| `respostas_completas.md` | As respostas completas do ChatGPT e do agente |
| `perguntas_imprimir.md` | As 5 perguntas do desafio |
| `contexto.md` | Contexto compartilhado para as duas IAs |
| `avaliacao-parcial.md` | Pontuação parciaal do agente |
| `teste_sql_injection.md` | Evidências do teste SQL Injection |

---

## 10. Próximos passos (opcionais)

- [ ] Testar a skill com **outros cenários** (configuração insegura, problema de infraestrutura)
- [ ] Adicionar uma **skill de testes de segurança** (sugestão da Expert)
- [ ] Adicionar uma **skill de trilha de estudos** em cibersegurança
- [ ] Levar a revisão para o **fluxo de desenvolvimento** (DevSecOps)
- [ ] Adicionar **prints reais** das análises no portfólio
- [ ] **Publicar no LinkedIn** a skill criada, como sugere a Expert no fim das Aulas

> 💡 **Dica de ouro:** o sucesso do projeto não está em tentar ser "melhor que o ChatGPT" em conhecimento. O sucesso está em **ser diferente**: conduzir, não entregar; perguntar, não substituir; ensinar a pensar, não apenas responder.

---
*Projeto gerado para o Desafio 3 — Especialização do DIO Agent em Análise de Vulnerabilidades.*
