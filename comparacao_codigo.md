# Comparação: Código Original vs Código Corrigido

## 1. Contexto

Este documento compara o código **vulnerável** (com SQL Injection) com o código **corrigido** (com consulta parametrizada), conforme o desafio 3 da DIO.

---

## 2. Código Vulnerável (Original)

### Arquivo: `agent/seguranca_app_py/app_original.py`
### Arquivo: `teste-skill-codigo-corrigido/.git` referencia: `app_original.py`

```python
import sqlite3

def fazer_login(usuario, senha):
    conexao=sqlite3.connect('banco_de_dados.db')
    cursor=conexao.cursor

    query=f"SELECT * FROM usuarios where username = '{usuario}' AND password = '{senha}'"

    cursor.execute(query)
    resultado=cursor.fetchone()
    conexao.close()

    if resultado:
        return "Acesso Permitido!"
    else:
        return "Acesso Negado."
```

### O que está errado

A linha crítica é:

```python
query = f"SELECT * FROM usuarios where username = '{usuario}' AND password = '{senha}'"
```

A entrada do usuário (`usuario` e `senha`) é **inserida diretamente** na query SQL por meio de **f-string** (concatenação). Isso é chamado de **String Concatenation / Inline Concatenation** e é o padrão clássico de **SQL Injection**.

---

## 2. Código Corrigido

### Arquivo: `teste-skill-codigo-corrigido/app.py`

```python
import sqlite3

def fazer_login(usuario, senha):
    conexao = None
    try:
        conexao = sqlite3.connect("banco_de_dados.db")
        cursor = conexao.cursor()

        # Consulta parametrizada: os valores são passados separadamente,
        # impedindo que o usuário altere a estrutura da query (SQL Injection).
        query = "SELECT * FROM usuarios WHERE username = ? AND password = ***"
        cursor.execute(query, (usuario, senha))

        resultado = cursor.fetchone()
        return "Acesso Permitido!" if resultado else "Acesso Negado."
    finally:
        if conexao is not None:
            conexao.close()
```

### O que mudou

| Antes (vulnerável) | Depois (corrigido) |
|---|---|
| `f"SELECT ... WHERE username = '{usuario}'"` | `query = "SELECT * FROM usuarios WHERE username = ? AND password = ***"` |
| O valor vira **parte da estrutura** da query | O valor vira **dado** passado separadamente |
| O SQLite interpreta a entrada | O SQLite trata o parâmetro como **valor** |

---

## 3. Comparação Detalhada

### Diferença 1: Montagem da query

**ANTES:**
```python
query = f"SELECT * FROM usuarios where username = '{usuario}' AND password = '{senha}'"
```
- A entrada do usuário é **concatenada** diretamente na query
- Se o usuário digitar `' OR '1'='1`, a query vira:
  ```sql
  SELECT * FROM usuarios WHERE username = '' OR '1'='1' AND password = '***' OR '1'='1'
  ```
- Isso força a consulta a retornar true, ignorando as credenciais

**DEPOIS:**
```python
query = "SELECT * FROM usuarios WHERE username = ? AND password = ***"
cursor.execute(query, (usuario, senha))
```
- O valor é passado **separadamente** para o executador da query
- O SQLite trata cada valor como **dados**, não como parte da estrutura
- O `' OR '1'='1` não vira mais código SQL, vira literal de texto

### Diferença 2: Execução da query

**ANTES:**
```python
cursor.execute(query)
```
- Executa a query montada em uma única string

**DEPOIS:**
```python
cursor.execute(query, (usuario, senha))
```
- Executa a query com **parâmetros separados**
- O banco de dados sabe exatamente quais valores são dados e não código

### Diferença 3: Fechamento da conexão

**ANTES:**
```python
conexao.close()
```
- Se ocorrer uma exceção antes do `close()`, a conexão pode ficar aberta

**DEPOIS:**
```python
finally:
    if conexao is not None:
        conexao.close()
```
- O fechamento é garantido **mesmo quando uma exceção ocorre**
- Pattern `try/finally` é a boa prática

### Diferença 4: Alias de variáveis

**ANTES:**
```python
conexao=sqlite3.connect('banco_de_dados.db')
cursor=conexao.cursor
```
- Sem espaço em branco (estilo antigo)

**DEPOIS:**
```python
conexao = None
try:
    conexao = sqlite3.connect("banco_de_dados.db")
    cursor = conexao.cursor()
```
- Padrão PEP 8 (espaços em branco)
- Inicialização explícita de `conexao = None`
- Bloco `try` para garantir fechamento

---

## 4. Exploração (Exemplo)

### Cenário: Login malicioso

**Dados de entrada:**
```python
usuario = "' OR '1'='1"
senha   = "' OR '1'='1"
```

**Com o código VULNERÁVEL:**
```sql
SELECT * FROM usuarios WHERE username = '' OR '1'='1' AND password = '***' OR '1'='1'
```
Resultado: Retorna linhas → **Acesso Permitido!** (sem senha!)

**Com o código CORRIGIDO:**
```sql
SELECT * FROM usuarios WHERE username = ? AND password = ?
```
Parâmetros: `(' OR \'1\'=\'1', ' OR \'1\'=\'1')`
Resultado: Nenhuma linha retornada (não há usuário com esse nome exatamente) → **Acesso Negado.**

---

## 5. Técnica do MITRE ATT&CK

Esta falha é classificada como:

| Categoria | Técnica |
|-----------|---------|
| **Initial Access** | T1190 - Exploit Public-Facing Application |
| **Credential Access** | T1078 - Valid Accounts (contornando a autenticação) |
| **Injection** | T1059.001 - Command and Scripting Interpreter: Shell |

Referência: https://attack.mitre.org/techniques/T1190/

---

## 6. Recomendações Adicionais

1. **Armazenar senhas com hash** (`bcrypt` ou `argon2`) em vez de texto puro
2. **Validar/limitar as entradas** do usuário (comprimento, tipo, caracteres permitidos)
3. **Usar um ORM** (ex.: SQLAlchemy) para evitar a construção manual de queries
4. **Proteger configurações sensíveis** — manter senhas e tokens fora do código-fonte
5. **Usar parâmetros nomeados** para consultas mais complexas

---

## 7. Comoconfirmar que a injeção foi mitigada

1. Rode o `app.py` corrigido (`teste-skill-codigo-corrigido/app.py`)
2. Teste o login com o valor malicioso `' OR '1'='1`
3. O resultado deve ser **Acesso Negado**, não Permitido
4. Teste com credenciais reais: deve retornar **Acesso Permitido**

Se os dois testes apresentarem o comportamento esperado, a injeção foi bloqueada.

