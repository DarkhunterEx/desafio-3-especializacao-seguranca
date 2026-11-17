---
name: security-review-project
description: Use quando o estudante quer analisar a segurança de um projeto de código (repositorio ou arquivo), pedir o fluxo de envio para o GitHub, revisar o codigo procurando falhas de seguranca, gerar um relatorio local, entregar o codigo corrigido e criar um Pull Request. O analise e defensiva: nunca incentiva ataques, e o codigo de terceiros so e analisado com autorizacao explicita.
---

# Skill: Revisao de Seguranca de Projeto

## Quando usar

Ative esta skill quando o estudante:

- pedir para revisar a seguranca de um projeto ou repositorio de codigo;
- querer enviar um projeto para o GitHub com codigo revisado e corrigido;
- pedir para analisar um repositorio, aplicacao, API ou sistema;
- pedir para encontrar falhas de seguranca no codigo;
- perguntar sobre vulnerabilidades em um projeto especifico;
- pedir recomendacoes de seguranca, correcao ou prevencao de um projeto;
- querer gerar um Pull Request com o codigo revisado.

## Regras sempre

- Ensine seguranca de forma defensiva; nunca incentive ataques contra sistemas reais.
- Nao forneça codigo, comandos ou instrucoes para exploracao, salvo com autorizacao explicita e limitada.
- Codigo de terceiros so pode ser analisado com autorizacao explicita do proprietario.
- Nao prometa que o codigo esta 100% seguro; sempre deixe claro que a revisao e um processo, nao uma garantia.
- A revisao so e feita do codigo enviado (repositorio ou arquivo), nao demais partes do repositorio ou sistema.
- Antes de enviar para o GitHub, confirme sempre a origem, a branch e a forma de envio (push, Pull Request, upload).
- Para o seu envio final, o envio para o GitHub do código corrigido só ocorre com autorização explícita e sem expor dados sensíveis.
- Cada achado de seguranca e classificado separadamente em probabilidade e severidade.

## Processo

### 1. Entenda o escopo

Antes de analisar, o agente pergunta o estudante, e so comeca a analisar quando tiver:

- Qual projeto ou repositorio esta sendo revisado? (URL do GitHub, ou arquivo enviado)
- Qual branch ou versao?
- O codigo e seu ou de terceiros?
- Se e de terceiros: tem autorizacao explicita para analisar? (confirme antes de prosseguir)
- O que o projeto faz? (tecnologias, stack, linguagem)
- O que o estudante quer com o resultado? (revisao local, Pull Request, upload para repo)
- Como quer que o codigo seja enviado? (push para branch, criar Pull Request, ou apenas arquivo local)

## 2. Confirme a origem

- Se o codigo e seu: confirme que quer avali-lo.
- Se o codigo e de terceiros: peça autorizacao explicita e registre que foi dada.
- Se nao ha autorizacao: pare e pergunte.

### 3. Colete o codigo

- Do repositorio (clonar ou baixar apenas o que foi solicitado), ou do arquivo enviado.
- Analise apenas o que foi enviado, com foco em seguranca.
- Nao leia ou envie demais arquivos, dados, ou divisas do repositorio sem permissao.

### 4. Analise o codigo

- Procure por padroes comuns de vulnerabilidades: SQL injection, XSS, CSRF, SSRF,
  exposicao de dados, autenticacao e autorizacao fraca, seguranca de dependencias,
  tratamento de erro, sensibilidade de arquivos, e criptografia fraca.
- Para cada achado, descreva: o que e, qual princípio de seguranca foi violado,
  qual o risco, e como corrigir.
- Nao adivinhe: baseie cada achado em evidencias no codigo.
- Liste ate mesmo os achados de menor impacto, mas classifique a sua severidade.

### 5. Classifique o risco

Para cada vulnerabilidade encontrada, use os dois campos separados:

- Probabilidade (Muito alta, Alta, Media, Baixa): chance de explotacao.
- Severidade (Baixa, Media, Alta, Critica): magnitude do dano.

Compare com o padrao OWASP Top 10, quando aplicável.

### 6. Produzir as entregas

O agente gera, no diretorio de saida, os tres arquivos de entrega:

#### a) relatorio_seguranca.md

Contem:

- resumo do projeto e do escopo revisado;
- lista de todas as vulnerabilidades encontradas, cada uma com:
  - nome e local;
  - conceito e o que acontece;
  - impacto (dados, acesso, disponibilidade);
  - classificacao de probabilidade e severidade;
  - como corrigir;
- secao de recomendacoes gerais.

#### b) log_modificacoes.md

Para cada falha, registra:

- o que era no codigo original;
- o que foi alterado;
- o motivo da alteracao (quem ou que classe de vulnerabilidade);
- o preciso do codigo antes e depois.

#### c) codigo_corrigido.<ext>

- Versao revisada do codigo, com as falhas corrigidas.
- Apenas as alteracoes necessarias; o codigo nao e reescrito para mais.
- Nao inclui dados sensiveis, credenciais ou secrets de produo nesse arquivo.

### 7. Enviar para o GitHub

Depois de produzir as entregas e antes de enviar:

1. Verifique que o token tem permissao de escrita (`repo`). Leitura (clone)
   so funciona com tokens mais limitados; push exige `repo`.
   ```bash
   # Teste sobre o GitHub:
   curl -X POST -H "Authorization: token SEU_TOKEN" \
        https://api.github.com/user/repos -d '{"name":"verifica-escrita"}'
   # - HTTP 201 => token valido e com permissao de escrita (pode push)
   # - HTTP 401 => token invalido ou expirado
   # - HTTP 403 => token sem escopo `repo` (só le, push falhara)
   ```

2. Defina o remoto e envie:
   ```bash
   git remote add origin https://SEU_TOKEN@github.com/SEU_USUARIO/SEU_REPO.git
   git push -u origin main
   ```

3. Aviso crítico: se o repositorio ja existia antes que este token o criou —
   por exemplo, foi criado por outro token ou conta — o push falhara com 403
   "Permission denied" mesmo que o clone funcione. Nesse caso, o push deve ir
   para o repositório criado por este token ou o token precisa ter escopo `repo`.

### 8. Gerar o Pull Request (se pedido)

- Apenas se o estudante pedir explicitamente e tiver autorizacao.
- Indique o repositorio de destino, a branch de destino e o titulo do Pull Request.
- O Pull Request nao e criado automaticamente; o agente so prepara o resumo e pede confirmacao.
- Confirme sempre que o estudante quer efetivamente criar o Pull Request antes de fazer algo.

### 9. Verificar aprendizado

- Pergunte ao estudante se entendeu as falhas e como corrigir.
- "Voce gostaria de entender melhor algum achado ou analisar outro projeto?"

## Entregas

Nomes de arquivos padrao, no diretorio de saida:

- relatorio_seguranca.md
- codigo_corrigido.<ext>
- log_modificacoes.md

Cada arquivo começa com cabecalho indicando: projeto, data, escopo revisado e autor.
Nao inclua credenciais, tokens, senhas ou dados nao-publicos nos arquivos gerados.

## Limites

- Nenhuma revisao estatica garante 100% de seguranca.
- A revisao e limitada ao codigo fornecido; nao e um escaneamento completo do sistema.
- Novo codigo escrito depois da revisao pode introduzir falhas.
- A classificacao de risco eh uma estimativa, nao uma certeza.
- Se o estudante pedir mudancas de comportamento ou design, concentre-se na seguranca, em vez de reescrever a funcionalidade.
