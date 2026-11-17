# Como Rodar o Agente no Harness

Este documento explica como executar o DIO Agent especializado em segurança no **Hermes**, indicado no README original.

---

## 1. Instalar o Hermes

Siga a documentação oficial do Hermes:

- URL: https://hermes-agent.nousresearch.com/docs

O README do repositório também lista os harnesses disponíveis:

| Harness | Documentação |
|---------|--------------|
| Google Antigravity (recomendado) | https://antigravity.google/docs |
| Claude Code | https://code.claude.com/docs |
| Codex | https://developers.openai.com/codex |
| OpenCode | https://opencode.ai/docs |
| **Hermes** | (documentação do Hermes) |

---

## 2. Baixar o repositório

### Com Git

```bash
git clone https://github.com/digitalinnovationone/dio-agent.git
cd dio-agent
```

### Sem Git (Download ZIP)

1. No repositório do GitHub, clique no botão **Code** → **Download ZIP**.
2. Extraia a pasta.
3. O folder resultante será `dio-agent-main` (ou um nome similar).

---

## 3. Abrir na pasta do projeto

**O passo mais importante:** abra no harness a pasta que tem o arquivo `AGENTS.md`.

- Se fez o **clone** (via Git): a pasta é `dio-agent` (ou o nome do repositório).
- Se fez o **Download ZIP**: a pasta é `dio-agent-main` (ou o nome extraído).

O harness lê o `AGENTS.md` sozinho. A partir daí, o agente já sabe quem é: o mentor de estudos da DIO, que também sabe analisar vulnerabilidades.

> ⚠️ **Atenção:** abra a pasta **do projeto**, não uma subpasta. Se o harness não encontrar os arquivos, verifique se o terminal está apontado para a pasta correta.

---

## 4. Conversar com o agente

Depois de aberto, fale naturalmente. O agente aplicará a skill `vulnerability-analysis` automaticamente quando o contexto exigir.

### Exemplos de comando

**Analisar uma vulnerabilidade:**

```text
Analisa esse app.py que tem login com SQL. O que tem de errado?
```

**Usar a skill de análise:**

```text
Quero aprender a analisar vulnerabilidades.
```

**Perguntar o contexto antes de agir:**

```text
Meu servidor Ubuntu de laboratório está exposto na internet com alguma configuração insegura. O que devo verificar?
```

O agente vai:

1. Identificar que a skill `vulnerability-analysis` é necessária;
2. Perguntar detalhes sobre o cenário (tecnologia, exposição real?, ambiente de estudo?);
3. Aplicar o processo defensivo (entender, explicar, analisar risco, verificar, investigar, recomendar, verificar aprendizado);
4. Entregar análise com impacto, probabilidade e severidade separados.

---

## 5. Dica de boas práticas

Durante um teste de segurança, **nunca** use o agente para ataques em sistemas reais. Sempre use ambientes de laboratório:

- Metasploitable 2 (uso controlado);
- DVWA (Damn Vulnerable Web Application);
- Sua própria aplicação em um ambiente de desenvolvimento.
