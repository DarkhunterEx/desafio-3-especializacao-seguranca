# Teste com SQL Injection — Evidência

Este documento registra o teste que serviu de base para a criação da skill. O cenário: um app.py com login que monta o SQL juntando os valores digitados pela pessoa.

---

## 1. Cenário

O app.py tem uma função `fazer_login(usuario, senha)` que busca um usuário no banco de dados. O trecho problemático fica na montagem da query:

```python
query = f"SELECT * FROM usuarios where username = '{usuario}' AND password = '{senha}'"
cursor.execute(query)
```

Os valores de `usuario` e `senha` são inseridos **diretamente** na query por meio de f-string. Isso é o que chamamos de **String Concatenation / Inline Concatenation** e é o padrão clássico de SQL Injection.

---

## 2. Como explorar

Um atacante pode enviar:

```text
usuario = ' OR '1'='1
senha   = ' OR '1'='1
```

A query resultante é:

```sql
SELECT * FROM usuarios WHERE username = '' OR '1'='1' AND password = '' OR '1'='1'
```

A expressão `'1'='1'` é sempre verdadeira, então a query retorna **pelo menos um registro**, mesmo sem usuário ou senha válidos. O resultado: **acesso autorizado sem senha válida**.

---

## 3. O impacto

| Aspecto | Descrição |
|---------|-----------|
| **Impacto** | Violação da confidencialidade (dados do banco expostos), integridade (dados podem ser alterados) e disponibilidade (banco pode ser lento ou inacessível). |
| **Probabilidade** | Alta — a falha está exposta e a exploração é simples. |
| **Severidade** | Crítica — acesso não autorizado ao sistema inteiro. |
| **Classificação OWASP** | A05:2025 — Injection. |

### Por que isso é grave

Ao contornar o login, um atacante:

- Deixa de precisar de credenciais;
- Pode acessar contas de administração;
- Pode ler, alterar ou excluir dados do banco;
- Pode usar o sistema como pivô para ataques internos.

---

## 4. Correção aplicada

A correção simples e eficaz: **consultas parametrizadas**.

```python
query = "SELECT * FROM usuarios WHERE username = ? AND password = ?"
cursor.execute(query, (usuario, senha))
```

Agora o valor de `usuario` e `senha` é passado **separadamente** para o executador da query. O SQLite trata cada valor como **dados**, não como parte da estrutura da query. O `' OR '1'='1` não vira mais código SQL, vira literal de texto.

### A lógica por trás

| Antes | Depois |
|-------|--------|
| `f"SELECT ... WHERE username = '{usuario}'"` | `query = "SELECT ... WHERE username = ?"` |
| O valor vira parte da estrutura | O valor vira dado |
| O SQLite interpreta a entrada | O SQLite não interpreta | 

Ou seja: **nunca concatene entradas do usuário em uma query**. Use placeholders (`?`, `%s`, `@nome`) — a consulta parametrizada é o padrão de segurança. A linguagem/lidar com o banco trata os valores como dados.

---

## 5. Teste de validação

### Antes (vulnerável)

```python
# login malicioso = operações que sempre retornam verdadeiro
usuario = "' OR '1'='1"
senha = "' OR '1'='1"
# Query: WHERE username = '' OR '1'='1' AND password = '' OR '1'='1' → retorna linhas
```

### Depois (corrigido)

```python
# O mesmo login malicioso é tratado como DADOS
query = "SELECT * FROM usuarios WHERE username = ? AND password = ?"
cursor.execute(query, (usuario, senha))
# Resultado: nenhuma linha retornada (não há usuário com esse nome exatamente)
```

---

## 6. Recomendações adicionais

1. **Armazenar senhas com hash** (`bcrypt` ou `argon2`) em vez de texto puro.
   - A senha deve ser hasheada **antes** da comparação.
   - Se o banco for vazado, as senhas não ficam expostas.

2. **Validar/limitar as entradas** do usuário (comprimento, tipo, caracteres permitidos).

3. **Usar um ORM** (ex.: SQLAlchemy) para evitar a construção manual de queries.

4. **Isolar credenciais** — colocar caminhos do banco em variáveis de ambiente, não no código-fonte.

5. **Usar parâmetros nomeados** para consultas mais complexas:

```python
query = "SELECT * FROM usuarios WHERE username = :usuario AND password = :senha"
cursor.execute(query, {"usuario": usuario, "senha": senha})
```

---

## 7. Como confirmationar que o SQL Injection foi mitigado

1. Rode o app.py corrigido (versão em `teste-skill-codigo-corrigido/app.py`).
2. Teste o login com o valor malicioso `admin' OR '1'='1`.
3. O resultado deve ser **Acesso Negado**, não Permitido.
4. Teste com credenciais reais: devia retornar **Acesso Permitido**.

Se o resultado for o esperado em ambos os casos, a falha foi mitigada.
