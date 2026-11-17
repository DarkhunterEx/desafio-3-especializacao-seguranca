# Formato das Entregas — Revisao de Seguranca de Projeto

Este e o padrao de formato dos tres arquivos gerados pela skill
`security-review-project`.

## 1. relatorio_seguranca.md

```text
# Relatorio de Seguranca - <nome do projeto>
- Data: <data>
- Escopo revisado: <repositorio ou arquivo>, branch <branch>
- Autor: <aluno/mentor>
- Metodo: revisao estatica do codigo enviado
- Resultado: <quantidade total> vulnerabilidades encontradas

## Resumo
- <uma oder explicando o projeto e o que foi encontrado>

## Lista de Vulnerabilidades

### 1. <titulo>
- Local: <arquivo>:<linha>
- Conceito: <descricao em linguagem acessivel>
- Impacto:
  - Dados: <o que seria exposto>
  - Acesso: <conta, funcao ou sistema comprometido>
  - Disponibilidade: <se ha risco de indisponibilidade>
- Probabilidade: <Muito alta/Alta/Media/Baixa>
- Severidade: <Baixa/Media/Alta/Critica>
- Correcao: <como corrigir>

## Recomendacoes Gerais
- <recomendacao 1 com motivo>
- <recomendacao 2 com motivo>
```

## 2. log_modificacoes.md

```text
# Log de Modificacoes - <nome do projeto>
- Data: <data>

## 1. <titulo do achado>
- Local original: <arquivo>:<linha>
- Codigo original:
  <codecant>
- Codigo corrigido:
  <codecant>
- Motivo: <qual classe de vulnerabilidade e por que a mudanca>
```

## 3. codigo_corrigido.<ext>

- Versao do codigo com as falhas corrigidas.
- Apenas alteracoes necessarias; o codigo nao e reescrito.
- Nenhum dado sensivel, token, senha ou credencial e incluido.
- Cabecalho inicial:

```text
// Codigo corrigido por revisao de seguranca.
// Projeto: <nome>. Original: <arquivo>.
// Apenas as alteracoes de seguranca foram aplicadas.
```
