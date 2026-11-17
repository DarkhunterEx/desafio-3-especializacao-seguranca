# Arquivos Modificados ou Recompletos

Esta seção lista os arquivos que foram **alterados, complementados ou reorganizados** para a especialização.

## 1. `AGENTS.md`

### O que mudou

A tabela de skills (seção **Skills**) ganhou uma nova linha para a skill de análise de vulnerabilidades:

```markdown
|| Análise de vulnerabilidades | quiser analisar uma falha de segurança, entender o risco e aprender correção defensiva | `skills/vulnerability-analysis/SKILL.MD` |
```

### Por que

O `AGENTS.md` é o arquivo que o harness lê para identificar quem é o agente. Ao registrar a skill nele, o agente passa a saber **quando** usar a skill `vulnerability-analysis` automaticamente, sem que o estudante precise chamá-la.

### Verificação

A tabela agora tem 8 skills, incluindo:

- Plano de estudos
- Destravar desafio
- Explicar conceito
- Pesquisa na web
- Revisão de portfólio
- Caminho AI Builder
- **Análise de vulnerabilidades** (nova)
- Trilha de cibersegurança

## 2. `agent/persona.md`

### O que mudou

Foram adicionadas duas seções:

1. **Skill de análise de vulnerabilidades** (linhas 43–52)
   - Instrui o agente a usar a skill `skills/vulnerability-analysis/SKILL.MD` para análises estruturadas.
   - Define o padrão defensivo: entender o cenário, explicar princípio violado, analisar risco com impacto/probabilidade/severidade, verificar se é segurança, mostrar investigação, recomendar melhorias, verificar aprendizado.

2. **Mentoria de cibersegurança** (linhas 54–61)
   - Refere a trilha de estudos, skill de planejamento e unidades de laboratório.

### Por que

A persona define o "caráter" do agente. Sem essa seção, o agente saberia que existe a skill (pela tabela no AGENTS.md) mas não saberia **para que ela serve** quando o estudante fizer uma pergunta de segurança.

## 3. `agent/knowledge/security-basics.md`

### O que foi feito

Este arquivo **pertencia ao projeto desde o começo**, mas é o documento de fundamento da especialização. Ele foi mantido e usado como base para o contexto do agente.

### Conteúdo

| Tópico | O que explica |
|--------|---------------|
| Vulnerabilidade | Falha em sistema, aplicação ou processo que permite comportamentos indesejados. |
| Ameaça | Algo que pode explorar uma vulnerabilidade. |
| Risco | Combinação entre probabilidade e impacto. |
| Confidencialidade | Apenas pessoas autorizadas acessam. |
| Integridade | Informações não alteradas indevidamente. |
| Disponibilidade | Sistemas continuam funcionando. |
| Boas práticas | 6 itens essenciais. |

### Por que

O agente lê o `agent/knowledge/security-basics.md` sempre que a conversa envolver segurança. Ele fornece o vocabulário e os conceitos mínimos para a análise.

## 4. `agent/seguranca_app_py/app_original.py`

### O que foi feito

Este arquivo **contém a versão original do app.py**, com a vulnerabilidade de SQL Injection. Ele serve como evidência do problema que a skill foi criada para encontrar.

### Código vulnerable (linha 7)

```python
query = f"SELECT * FROM usuarios where username = '{usuario}' AND password = '{senha}'"
```

### Impacto

Um atacante pode enviar:

```text
usuario = ' OR '1'='1
senha   = ' OR '1'='1
```

A query resultante é sempre verdadeira e retorna pelo menos um registro → **acesso autorizado sem senha válida**.

## 5. `teste-skill-codigo-corrigido/`

### O que foi feito

Esta pasta contém a **versão corrigida** do app.py e o relatório de análise. Foi criada para servir como teste do processo: o estudante (ou o agente) analisa o código original, aplica a skill e verifica a correção.

### Estrutura

```
teste-skill-codigo-corrigido/
├── app.py                       # Versão corrigida (consulta parametrizada)
├── relatorio_seguranca.md       # Relatório de análise
└── desafio-1/
    ├── README.md                # Descrição do desafio de força bruta
    ├── ambiente/README.md
    ├── 01-recon/README.md
    ├── 02-forca-bruta-ftp/README.md
    ├── 03-password-spraying/README.md
    ├── 04-forca-bruta-web/README.md
    ├── wordlists/               # (senham.txt, smb_users.txt, usuariom.txt)
    └── reflexoes.md
```

### O que é relevante

- `app.py`: consulta parametrizada com `?` placeholders.
- `relatorio_seguranca.md`: relatório completo de análise de segurança.

## 6. `skills/README.md`

### O que foi feito

Este arquivo **já existia** no repositório e documenta o processo de criação de skills. Foi mantido e é útil para quem quer entender como o repositório funciona.

### Por que

A skill de análise de vulnerabilidades segue exatamente o processo descrito neste README:

1. Crie uma pasta dentro de `skills/` com um nome curto em inglês.
2. Adicione um `SKILL.md` com `name` e `description` e o processo passo a passo.
3. Registre a skill em dois lugares: `AGENTS.md` e `README.md`.

## Resumo

| Tipo | Arquivos |
|------|----------|
| **Criados** | 6 |
| **Modificados/Completos** | 3 |
| **Evidências de teste** | 3 |

Total de **12 arquivos envolvidos** na especialização.
