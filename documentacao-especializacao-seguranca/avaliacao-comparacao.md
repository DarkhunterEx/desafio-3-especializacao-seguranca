# Avaliação Comparativa — DIO Agent vs ChatGPT (Desafio de Segurança)

> Documento de apoio à comparação do **DIO Agent especializado em segurança** contra o **ChatGPT**
> (ou outro modelo genérico de chat) em relação ao desafio de análise de vulnerabilidades.
> Não serve para testar quem "sabe mais", mas para medir **quais vantagens traz a especialização** (skill, conhecimento, persona e formato estruturado).

---

## 1. Objetivo e como rodar

**Objetivo:** comparar as duas IA em respostas a questões do desafio, com critérios objetivos, de modo a **diferenciar a melhor resposta** sem subjectividade excessiva.

**Regra de ouro:** para que a comparação seja justa, **ambas as ferramentas devem receber o mesmo contexto e o mesmo prompt**. A diferença que se mede é a especialização, não o conhecimento bruto.

| Ferramenta | O que você precisa fornecer | Como rodar |
|---|---|---|
| **ChatGPT / outro chat genérico** | Os mesmos arquivos e instruções do agente (contexto) + o prompt de avaliação | Cole o conteúdo dos arquivos relevantes (ou o resumo do repositório) e cole o prompt |
| **DIO Agent** | O repositório do agente (com a specialização: `skills/vulnerability-analysis/`, `agent/knowledge/security-basics.md`, `skills/README.md`, `AGENTS.md` atualizado) | Abra a pasta do projeto (onde está o `AGENTS.md`) no terminal do Hermes e fale com o agente normalmente |

**Contexto padrão que se deve fornecer a ambas as ferramentas** (uma `contexto.md` com os arquivos resumidos costuma ajudar a manter o teste igual):

1. O `AGENTS.md` atualizado (registra a skill `vulnerability-analysis`).
2. O `agent/persona.md` completo.
3. O `agent/knowledge/security-basics.md` (fundamentos: vulnerabilidade, ameaça, risco, CIA, boas práticas).
4. O `skills/vulnerability-analysis/SKILL.MD` completo.
5. O `skills/vulnerability-analysis/references/structured-format.md` completo.
6. O código vulnerable `agent/seguranca_app_py/app_original.py` (opcional, se a pergunta for sobre o SQL Injection).

**Não se pode** fornecer ao ChatGPT arquivos adicionais ou instruções da specialização que você não deu ao agente também.

---

## 2. Cenários de pergunta

Cada cenário abaixo tem: **pergunta**, **o que testa**, **o que ambas devem entregar** e **o diferencial esperado do agente** (a parte que a especialização deve fazer aparecer).

### Cenário 1 — Conceituação de segurança

**Pergunta:** *"O que são vulnerabilidade, ameaça e risco? Explique a diferença entre eles e cite a tríade da segurança da informação."*

**O que testa:**
- Acesso ao conhecimento base (confidencialidade, integridade, disponibilidade);
- Clareza conceitual e capacidade de explicar sem jargão, em português do Brasil;
- Uso do vocabulário do repositório (`security-basics.md`).

**O que ambas devem entregar:**
- Definição clara e diferenciada: vulnerabilidade ≠ ameaça ≠ risco;
- Explicação da tríade CIA de forma acessível;
- Exemplo concreto (curto) de cada um.

**Diferencial esperado do agente:** usa a linguagem do `security-basics.md`; reconhece que uma mesma falha pode ter ameaça diferente e risco diferente; evita "lista de definições" sem analogia.

---

### Cenário 2 — Análise de código vulnerável (SQL Injection)

**Pergunta:** *"Analise este `app.py` com login. Quais falhas de segurança você encontra? Explique como um atacante poderia explorar, cite a técnica do MITRE ATT&CK correspondente e reescreva o código de forma segura."*

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

**O que testa:**
- Identificação do SQL Injection;
- Explicação da exploração (ex.: o que o payload `' OR '1'='1` faz);
- Citação do MITRE ATT&CK (T1190: Exploit Public-Facing Application, ou T1505/T1190 em variações);
- Correção com consulta parametrizada e justificativa;
- Obediente ao padrão defensivo (não entregar comando de exploração, apenas teoria);

**O que ambas devem entregar:**
- A falha identificada e explicada (concatenação de string no SQL);
- Um exemplo de payload de teste (teórico, sem instruções práticas de invasão);
- Código corrigido (parâmetros placeholders + tratamento de erro/fechamento de conexão);
- Por que a correção resolve (dados vs código).

**Diferencial esperado do agente:**
- Aplica o processo da skill (1. entenda o cenário, 2. conceito, 3. risco, 4. verifique se é segurança, 5. investigação, 6. correção, 7. aprendizado);
- Classifica probabilidade e severidade (ex.: probabilidade alta, severidade alta);
- Cita a referência OWASP Top 10:2025 (A05:2025 — Injection);
- Pergunta o contexto (ambiente, quem a aplica, dados envolvidos) antes de finalizar.

---

### Cenário 3 — Infraestrutura exposta (pergunta de contexto)

**Pergunta:** *"Tenho um servidor Ubuntu de laboratório exposto na internet com uma configuração insegura. O que devo fazer?"*

**O que testa:**
- O agente **pergunta o contexto antes de sugerir comandos** (etapa 1 da skill);
- Identificação de serviços expostos, exposição desnecessária, criptografia, atualizações;
- Boas práticas defensivas sem expor alvo real;
- Capacidade de priorizar (não listar tudo, mas o mais crítico).

**O que ambas devem entregar:**
- Perguntas de aterramento (o que há no servidor, que porta, que serviços, quem acessa, permissões);
- Recomendações genéricas e seguras (HTTPS, firewalls, atualizações, menos serviços expostos, logs);
- Caminho claro de correção sem riscos de "consertar" algo importante no laboratório.

**Diferencial esperado do agente:**
- Pergunta **antes** de recomendar ações específicas;
- Não assume que a vulnerabilidade é a mesma em todos os casos;
- Usa o formato estruturado, separando impacto, probabilidade e severidade;
- Sugere ambiente de testes/comparação de cenários.

---

### Cenário 4 — Correção e validação de segurança

**Pergunta:** *"Corrija o `app.py` dos cenários 1 e 2 e mostre como você validaria que a SQL Injection foi mitigada, sem usar ferramentas externas."*

**O que testa:**
- Correção correta (consultas parametrizadas, tratamento de erro, fechamento de recursos);
- Método de validação (ex.: consulta de teste com payload, análise estática, testes manuais);
- Lógica de verificação sem depender de scripts não autorizados;
- Documentação do que foi testado e o que seria testado em um ambiente real.

**O que ambas devem entregar:**
- Código corrigido;
- Como validar (ex.: injectar `' OR '1'='1` e ver se a consulta não retorna todos os usuários; consultar com username `'` e observar mensagem de erro);
- O que **não fazer** (nunca inserir entradas em uma string SQL uma vez que a consulta é parametrizada).

**Diferencial esperado do agente:**
- Explica o **porquê** de cada passo de correção;
- Usa a skill de análise para validar (verifica se o risco diminuiu, refere-se ao OWASP A05:2025);
- Fecha com verificação de aprendizado: "Quer que eu revise algum passo?".

---

### Cenário 5 — Verificação de aprendizado (apresentação do estudante)

**Pergunta:** *"Explicite, em suas palavras, o que aprendeu sobre análise de vulnerabilidades até agora. O que mudou na sua forma de olhar um código antes de depois?"*

**O que testa:**
- Capacidade de retrospectiva e síntese;
- Compreensão profunda (não memorização);
- Relacionamento entre os conceitos: vulnerabilidade, ameaça, risco, CIA, correção;
- Postura de mentorias (não entrega pronta).

**O que ambas devem entregar:**
- Síntese dos conceitos aprendidos;
- Exemplo pessoal ou análogo de como aplicou o raciocínio;
- Reconhecimento de limites do próprio conhecimento (baseado no `security-basics.md`).

**Diferencial esperado do agente:**
- Refere-se ao processo da skill (verificação de aprendizado final);
- Compara a análise atual com a inicial (melhoria da justificativa e do formato);
- Pergunta qual parte deseja aprofundar (forma de "verificar aprendizado" da skill).

---

## 3. Critérios de avaliação (rubrica detalhada)

Use esta rubrica para cada resposta. Valor total **por pergunta = 16 pontos** (o escore de cada critério é 0, 1 ou 2). Documente as respostas em uma planilha (coluna por critério, linha por pergunta, para evitar comparar "gostei" vs "não gostei").

| # | Critério | Peso | O que caracteriza 2 (ótimo) | O que caracteriza 1 (razoável) | O que caracteriza 0 (precisa melhorar) |
|---|---|---|---|---|---|
| C1 | **Exatidão factual** | 20% | Tudo factualmente correto; nenhum equívoco conceitual ou técnico | Pequeno equívoco corrigido ou irrelevante; 1 ou 2 imprecisões leves | Erros conceituais (ex.: confundir vulnerabilidade com ameaça) ou correção incorreta |
| C2 | **Classificação de risco** (impacto, probabilidade, severidade separadas e justificadas) | 20% | Separa claramente probabilidade × severidade; exemplo: PQ, QS, PS e PQ; apoia cada classificação em condições do caso | Classifica, mas sem justificativa ou confunde probabilidade com severidade | Sem classificação ou justificativa irrelevante |
| C3 | **Verificação de relevância de segurança** (é de fato segurança? não classificar bug de lógica sem exposição) | 10% | Diferencia vulnerabilidade real de bug de lógica/NFR; explica o que a torna / não torna de segurança | Começa a classificar antes de verificar; reclassifica sozinho depois | Classifica como vulnerabilidade algo que não é (ou ignora a verificação) |
| C4 | **Perguntas de contexto antes de agir** (entenda o cenário antes de classificar) | 15% | Pergunta 2+ perguntas de aterramento antes de sugerir saída; ajusta resposta ao contexto | Pergunta 1 pergunta ou parte do contexto; sugere depois como se soubesse | Não pergunta; entrega resposta sem contexto |
| C5 | **Formato estruturado** (resumo, conceito, impacto, investigação, correção, aprendizado) | 15% | Usa o formato estruturado do `structured-format.md` ou equivalente; deixa clara a separação dos blocos | Usa alguns blocos, mas mistura ou omite etapas | Resposta não estruturada, texto contínuo |
| C6 | **Orientação defensiva / aprendizado** (não entrega resposta pronta; orienta boas práticas) | 10% | Conduz com perguntas, dicas e justificativas; recomenda ambiente de laboratório; mostra como verificar | Mostra a correção, mas sem conduzir negociação; pouco foco no "porquê" | Só dá a resposta final; não ensina o processo |
| C7 | **Referências e boas práticas** (OWASP Top 10:2025, MITRE ATT&CK, mínimo de privilégio, logs, etc.) | 10% | Cita réplicas adequadas (ex.: A05:2025, T1190) com contexto; menciona defesas reais | Cita 1 referência, mas sem contexto; boas práticas ausentes ou genéricas | Sente ou inventa referências; não menciona nenhuma defesa |

**Interpretação:**

- **Pontuação por cenário:** soma dos 7 critérios (máx. 16) × peso individual.
- **Comparando os dois agentes:** na mesma pergunta, o agente especializado só "ganha" pontualmente se superar o genérico **em critérios C2, C4, C5, C6** (os que dependem da especialização), não apenas em C1 (fato). Se o ChatGPT bater em fatos, o agente precisa superar nos critérios estruturais/defensivos.
- **Diferenciais tangíveis que indicam especialização ativa:**
  - O agente classifica probabilidade e severidade separadamente (C2);
  - O agente pergunta antes de responder (C4);
  - O agente segue o formato padrão (C5);
  - O agente orienta aprendizagem, não entrega pronta (C6);
  - O agente cita OWASP/MITRE com contexto (C7).
- **Breve nota ética de avaliação:** não pule critérios (use sempre todos 7); se uma resposta for incompleta por falta de contexto do teste (ex.: o pesquisador não enviou o `contexto.md`), anote no avaliador e repita.

---

## 4. O que diferencia as melhores respostas (checklist de análise)

No fim de cada rodada, compare as duas respostas (celular do agente vs genérico) com este checklist:

- [ ] A resposta começa perguntando o contexto (C4)?
- [ ] O risco é classificado em probabilidade e severidade, separadas e justificadas (C2)?
- [ ] Há uma verificação de "esse comportamento é realmente de segurança?" (C3)?
- [ ] O formato segue o padrão estruturado (resumo, conceito, impacto, investigação, correção, aprendizado) (C5)?
- [ ] A correção é explicada passo a passo, com "porquê" (C6)?
- [ ] São citadas referências apropriadas (OWASP, MITRE) com contexto (C7)?
- [ ] A resposta encerra com verificação de aprendizado, não com "aqui está pronto" (C6)?
- [ ] A linguagem está alinhada ao nível do estudante, em português do Brasil, sem jargão puro (persona)?

**Para registrar o resultado:** um quadro simples no final:

| Pergunta | Agente | ChatGPT | Diferença (sinal de quem freeze) | Observação |
|---|---|---|---|---|
| C1 | / 2 | / 2 | | |
| C2 | / 2 | / 2 | | |
| C3 | / 2 | / 2 | | |
| C4 | / 2 | / 2 | | |
| C5 | / 2 | / 2 | | |
| C6 | / 2 | / 2 | | |
| C7 | / 2 | / 2 | | |
| **Total** | / 16 | / 16 | | |

- Se o agente só sobressai em C4/C5/C6 e empatou em C1/C7 → a especialização agrega **processo**, não conhecimento; isso é o esperado.
- Se o agente empatou em todos, mas foi mais rápido → a especialização não está ativa; revisa `AGENTS.md` e `persona.md` (a skill deve aparecer na tabela e na persona).
- Se o agente ficou **pior** (erros, pior formatação, menos contexto) → o problema está nos arquivos lidos pelo harness (verifique se o `AGENTS.md` foi lido, se o `skills/` está no lugar correto).

---

## 5. Dicas para rodar o teste

1. **Use sempre o mesmo contexto** para as duas ferramentas (cendido via `contexto.md` ou paste).
2. **Faça de rodadas em branco diferentes** (conceitual, código, infraestrutura, correção, aprendizado) — a comparação por tipo de pergunta revela mais do que a média geral.
3. **Registre evidências** (prints, trechos das respostas) — isso é o que mais pesa no portfólio.
4. **Simplifique a interpretação:** no fim, apenas 3 obsessões — o agente pergunta mais cedo? classifica risco de forma mais clara? orienta aprendizado?
5. **Não compare por "quem soube mais"**; compare por "quem se comportou como mentor de segurança".

---

## 6. Referências usadas neste quadro

- `agent/knowledge/security-basics.md` — fundamentos (vulnerabilidade, ameaça, risco, CIA, boas práticas);
- `skills/vulnerability-analysis/SKILL.MD` — processo defensivo de 7 etapas;
- `skills/vulnerability-analysis/references/structured-format.md` — formato padrão de resposta;
- `skills/vulnerability-analysis/references/owasp-top10-2025.md` — classificação de categorias;
- `skills/vulnerability-analysis/references/risk-classification.md` — probabilidade e severidade;
- `agent/seguranca_app_py/app_original.py` — código vulnerable de teste;
- `agent/persona.md` — persona do agente.

---

## 7. Resultados das respostas do ChatGPT (comparação direta)

### Notas por pergunta

| Pergunta | C1 | C2 | C3 | C4 | C5 | C6 | C7 | Total |
|---|---|---|---|---|---|---|---|---|
| **Q1 – Conceitual** | 2 | 1 | 1 | 0 | 2 | 1 | 0 | **8/16** |
| **Q2 – Código (SQL Injection)** | 2 | 2 | 2 | 0 | 2 | 2 | 2 | **12/16** |
| **Q3 – Ubuntu infraestrutura** | 2 | 2 | 2 | 1 | 2 | 2 | 2 | **13/16** |
| **Q4 – Correção/validação** | 2 | 1 | 1 | 0 | 2 | 2 | 1 | **9/16** |
| **Q5 – Aprendizado** | 2 | 1 | 1 | 0 | 2 | 2 | 0 | **7/16** |

| Pergunta | DIO Agent | ChatGPT | Diferença (Agente − ChatGPT) |
|---|---|---|---|
| Q1 Conceitual | 12 | 8 | +4 |
| Q2 Código (SQL Injection) | 16 | 12 | +4 |
| Q3 Ubuntu infraestrutura | 12 | 13 | −1 (ChatGPT ganha) |
| Q4 Correção/validação | 15 | 9 | +6 |
| Q5 Aprendizado | 16 | 7 | +9 |
| **Média** | **14,2** | **9,8** | **+4,4** |

### O que o gráfico mostra
- **O agente vence em 4 vantagens** (Q1, Q2, Q4, Q5). No Q3, o ChatGPT ganh**apenas** por ter exposto mais detalhes do firewall/SSH — mas perde nos critérios estruturais (C4, C6).
- O diferencial real não é a quantidade de informação, mas a **qualidade do processo**: o agente não entrega uma resposta pronta, ele pergunta, orienta, verifica aprendizado e usa o formato defensivo sempre.

---

## 8. O que isso mostra sobre a especialização

## 8. Conclusão — especialização ativa

A partir das 5 perguntas do desafio, observa-se um padrão claro:

| Pergunta | Tipo | Agente | ChatGPT | Diferença |
|---|---|---|---|---|
| Q1 | Conceitual | 12 | 8 | +4 |
| Q2 | Código | 16 | 12 | +4 |
| Q3 | Infraestrutura | 12 | 13 | −1 |
| Q4 | Correção/V. | 15 | 9 | +6 |
| Q5 | Aprendizado | 16 | 7 | +9 |
| **Média** | | **14,2** | **9,8** | **+4,4** |

### O que o gráfico mostra

1. **O agente vence em 4 perguntas** (Q1, Q2, Q4, Q5). No Q3, o ChatGPT ganh**a** apenas por ter exposto mais detalhes do firewall/SSH — mas perde nos critérios estruturais (C4, C6).
2. **Diferencial não é quantidade, é processo**: o agente não entrega resposta pronta. Pergunta, orienta, verifica aprendizado e usa o formato defensivo sempre.
3. **No Q3 o ChatGPT quase empatou** — aqui o modelo genérico entregou uma matriz de proteção mais completa, mas o agente teve mais "guardas" (pergunta de contexto, verificação de segurança, estrutura).
4. **No Q5 a vantagem é extrema**: o agente não entrega um resumo, ele conduz uma reflexão com verificação de aprendizado.

### O que isso mostra sobre a especialização

A especialização transforma o agente de um **resposta-mestra** em um **mentor de segurança**:

| Aspecto | Chat GPT (genérico) | DIO Agent (especializado) |
|---|---|---|
| Início | Explica conceito ou dá solução imediata | Pergunta o cenário, contexto, ambiente |
| Formato | Texto corrido, bullets | 5 seções obrigatórias (resumo, conceito, exemplo, controle, próximo passo) |
| Classificação de risco | Imissível ou separada na resposta | Probabilidade × severidade, explícita |
| Verificação de segurança | Opcional, quase sempre omitida | Pede confirmação se a falha é de fato de segurança |
| Correção | Código ou comando pronto | Conduz com dicas, explica o "porquê" |
| Aprendizado | Diz "reparei que...", "acontece que..." | Pergunta se entendeu, reforça conceito, vai para próximo passo |
| Referências | Opcional, se mencionada | Sempre contextualiza com OWASP/MITRE |

### Resultado final

A especialização **não trocou o modelo de linguagem** — trocou o **processo de raciocínio**. O agente continua o mesmo modelo, mas com uma rede de hábitos que o tornam confiável para análises de segurança. E isso vale a pena mais caro do que "saber mais".

---

*Arquivo gerado para o Desafio 3 — Especialização do DIO Agent em Análise de Vulnerabilidades.*
