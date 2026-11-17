# Melhoria Escolhida

O desafio propôs várias melhorias para a evolução do DIO Agent. Esta seção registra **qual foi escolhida** e **por quê**.

---

## As opções apresentadas

| Opção | Descrição |
|-------|-----------|
| 1. Skill de testes de segurança | Uma skill focada em testes de segurança (uma das sugestões da Expert). |
| 2. Skill que monta uma trilha de estudos | Uma skill que orienta a montagem de uma trilha de estudos em cibersegurança. |
| 3. Testar a skill com outros cenários | Testar a skill com configurações inseguras ou problemas de infraestrutura. |
| 4. **Padronizar o formato da análise** | **Tornar a análise sair sempre no mesmo formato**, com impacto, probabilidade e severidade, para comparar casos. |
| 5. Levar a revisão para o fluxo de desenvolvimento (DevSecOps) | Revisar cada alteração antes da produção, conforme o padrão de segurança. |

---

## A melhoria escolhida

A melhoria escolhida foi a **opção 4**:

> **Tornar a análise sair sempre no mesmo formato**, com impacto, probabilidade e severidade, para comparar casos.

Em conjunto com a opção 1 (skill de testes de segurança), foi construído um **roteiro defensivo** (não entregar respostas prontas, sempre conduzir o estudante).

---

## Por que essa melhoria?

### 1. Tornar o processo reprodutível

Embora análises de segurança sejam naturais de serem contextuais, a **estrutura** da análise deve ser previsível. Isso permite:

- Comparar múltiplas vulnerabilidades com um mesmo critério;
- Ensinar o estudante a **pensar** sobre segurança, em vez de apenas nomear falhas;
- Permitir que o estudante acompanhe o raciocínio passo a passo.

### 2. Focar na qualidade da análise, não apenas no resultado

Separar **impacto**, **probabilidade** e **severidade** obriga o estudante a:

- Perguntar: *qual é a probabilidade de exploração?*
- Quebrar o impacto: *o que seria exposto? qual a amplitude?*
- Avaliar o risco real: *probabilidade × severidade = risco*.

Isso constrói **pensamento analítico**, não apenas memorização.

### 3. Conduzir o estudante, não entregar a resposta

O roteiro defensivo previne que o agente entregue código ou comandos prontos. Em vez disso:

1. Entender o cenário;
2. Explicar o conceito;
3. Analisar o risco (probabilidade × severidade);
4. Verificar se é segurança;
5. mostre a investigação;
6. Recomende melhorias;
7. Verifique o aprendizado.

---

## Por que não as outras opções?

| Opção | Reflexão sobre a escolha |
|-------|--------------------------|
| 1. Skill de testes de segurança | Uma opção válida a curto prazo, mas **exige laboratório real** (fernando VMs, redes, etc.), o que aumenta a barreira de entrada. O processo de aprendizado seria mais difícil de validar sem ambiente. |
| 2. Skill de trilha de estudos | Já existe uma skill de trilha de cibersegurança (`skills/cyber-study-path`). Criar outra duplicaria esforço. |
| 3. Testar com outros cenários | Importante para validação, mas **é uma fase de validação**, não uma melhoria de projeto. Depois da materialização, o próximo passo seria testar. |
| 5. DevSecOps | Excelente para um projeto de longo prazo, mas **fora do escopo do desafio**. Nesse momento, a especialização educacional é mais importante. |

---

## O resultado da melhoria

A specialização construída possui:

| Elemento | Descrição |
|----------|-----------|
| **Skill defensiva** | `skills/vulnerability-analysis/SKILL.MD` com 7 etapas. |
| **Formato estruturado** | `references/structured-format.md` — Resumo, Conceito, Impacto, Investigação, Correção, Aprendizado. |
| **Classificação de risco** | `references/risk-classification.md` — Probabilidade e Severidade, separados. |
| **Referência OWASP** | `references/owasp-top10-2025.md` — com códigos e classificação. |
| **Conhecimento** | `agent/knowledge/security-basics.md` — CIA, risco, boas práticas. |
| **Registro** | `AGENTS.md` e `agent/persona.md` — para o agente usar a skill. |

O resultado: um agente que **conduz uma análise de segurança como um profissional**, perguntando antes de agir e entregando análise estruturada — não uma resposta pronta para copiar.
