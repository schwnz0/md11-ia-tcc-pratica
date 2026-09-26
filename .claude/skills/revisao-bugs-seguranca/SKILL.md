---
name: revisao-bugs-seguranca
description: Revisa código em busca de bugs e fragilidades de segurança (validação de entrada, tratamento de erros, exposição de dados). Use quando o usuário pedir revisão de segurança, auditoria de validação/erros, code review de bugs, ou checagem de dado sensível em logs/saída — em qualquer linguagem ou arquivo do workspace.
---

# Revisão de bugs e segurança

Skill **reutilizável**: não amarre a análise a um arquivo único nem a um produto específico. Leia o código real do workspace e relate o que existir. Não invente CVE, não invente linha, não “complete” o app.

## Quando usar

- Pedidos do tipo: revisar segurança, achar bug, auditoria, validação de entrada, tratamento de erro, vazamento de dado, OWASP em miniatura.
- Antes de aceitar patch que mexe em input, auth, arquivo, log ou serialização.

Não use quando o usuário só quer explicação didática sem olhar o código, ou quando ele pediu explicitamente só para implementar feature.

## Passos

1. Identifique a superfície real: entrypoints (CLI, API, UI), inputs (`ReadLine`, query, arquivo, ambiente), saídas (console, HTTP, log) e estado (lista em memória, banco, disco).
2. Percorra o checklist abaixo no código **que existe**. Marque “não aplicável” internamente; não encha o relatório de item verde inútil.
3. Confirme cada achado no arquivo (trecho + linha). Se não achar âncora, descarte.
4. Classifique severidade no contexto do programa (um console de demo não é um banco exposto na internet, mas falha silenciosa e input sem validação ainda contam).
5. Entregue o relatório no formato da seção **Saída**. **Não altere código** a menos que o usuário peça correção num passo seguinte.

## Checklist (aplicável a C#, e a outras stacks com o equivalente)

### Validação de entrada

- Valores vazios, nulos, só espaço, tipo errado (parse de `int` sem `TryParse`).
- Identificadores inexistentes (id que não está na coleção) e ids inválidos (`<= 0`).
- Tamanho absurdo de string / coleção se o input puder crescer.
- Confiança em dado que o usuário (ou o console) controla sem checagem.

### Tratamento de erros e comportamento

- Falha silenciosa (método `void` que não faz nada quando deveria recusar).
- Exceção sem tratamento em caminho previsível.
- Loop que deveria parar ao achar o item e continua sem necessidade (efeito colateral se houver duplicata).
- Estado inconsistente depois de operação parcial.

### Exposição de dados e higiene

- Segredo, token, connection string, documento pessoal (CPF/CNPJ, senha) em log, console, mensagem de erro ou repositório.
- Interpolação de input não confiável em comando, SQL, path ou HTML — mesmo em app pequeno, sinalize o risco se o padrão aparecer.
- Dado interno impresso sem necessidade.

### Outros bugs clássicos

- Condição de corrida só se houver concorrência real.
- Recurso não disposto (arquivo, stream) se houver I/O.
- Comparação de string insegura para senha (se houver auth).

## Severidade

- **alta** — exploração ou vazamento plausível no desenho atual (segredo impresso, comando montado com input, dado pessoal em claro na saída).
- **média** — bug de correção ou validação que já quebra o comportamento (id inexistente engolido, parse que derruba o processo, estado mentiroso).
- **baixa** — higiene, usabilidade ligada a erro, dívida que vira problema se o código ganhar input interativo depois.

## Saída

Comece com 2–4 frases: o que foi olhado e o nível de risco geral.

Depois uma tabela, uma linha por achado, severidade alta primeiro:

| Severidade | Local | Achado |
|---|---|---|
| média | `arquivo:linha` | frase objetiva do problema e do efeito |

Se não houver achado: uma linha dizendo isso, e o que foi coberto.

Feche com **Não feito**: o que a skill recusou (ex.: não aplicou patch, não adicionou feature).

## O que não fazer

- Não refatorar nem adicionar método “para ficar seguro” sem pedido.
- Não copiar relatório de outro projeto.
- Não listar teoria OWASP sem amarrar num trecho deste código.
- Não tratar ausência de banco/auth como vulnerabilidade se o programa simplesmente não tem essa camada — descreva a superfície real.
