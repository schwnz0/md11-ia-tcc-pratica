# Questão 11 — Evidência de uso real da IA

Não inventei esta sessão: ela aconteceu neste repositório, no Cursor, enquanto eu pedia para completar a avaliação na ordem combinada (README → `CLAUDE.md` → skill → uso da skill no código → este arquivo).

## Ferramenta

**Cursor** (agente conectado ao código local do workspace `md11-ia-tcc-pratica`). O modelo da sessão identificou-se como Cursor Grok 4.6. Não usei Claude Code nem Copilot neste passo.

## Prompt exato enviado

O recado que eu mandei no chat (texto integral):

```
Trabalhe neste repositório de avaliação seguindo exatamente esta ordem:

Responda as questões dissertativas 1, 2, 4, 5, 6, 7, 8, 9 e 10 direto no README.md, abaixo de cada uma. Escreva em primeira pessoa, como se eu (Everson) tivesse escrito — com minhas próprias palavras, tom natural, mostrando entendimento real dos conceitos (agentes, guidelines, escolha de modelo/effort, estrutura de prompt, iteração, zero-shot vs few-shot, memória entre sessões, avaliação de resposta de IA, divisão de tarefas em etapas). Não deixe as respostas genéricas — conecte pelo menos uma delas ao contexto do meu TCC/projeto quando fizer sentido.
Não responda as Questões 3 e 11 aqui — elas são respondidas no CLAUDE.md e no EVIDENCIAS.md, respectivamente.
Analise o código do projeto GerenciadorDeTarefas/ (estrutura, linguagem C#, dependências, como rodar — dotnet run) e crie um CLAUDE.md completo na raiz do repositório, específico a este projeto (não um template genérico). Isso responde a Questão 3, então o CLAUDE.md deve demonstrar por que aquele conteúdo/estrutura é uma boa forma de orientar um assistente de IA neste projeto.
Crie a skill em .claude/skills/minha-skill/SKILL.md, mas renomeie a pasta minha-skill/ para um nome que reflita sua função real: uma skill reutilizável para revisar código em busca de bugs e fragilidades de segurança (ex: validação de entrada, tratamento de erros, exposição de dados sensíveis). Escreva instruções realmente reutilizáveis (não específicas de um único arquivo), com nome e descrição da skill coerentes com o que ela faz.
Conecte um assistente de IA ao código local (Claude Code, GitHub Copilot, Cursor ou outro) e use-o de verdade pelo menos uma vez, aplicando o CLAUDE.md e/ou a skill criada em uma tarefa real do projeto — por exemplo, rodar a skill de revisão de segurança sobre o código do GerenciadorDeTarefas e ver o que ela encontra. Não é necessário adicionar funcionalidade nova ao app; o foco é essa sessão real de uso da IA.
Documente essa experiência real no EVIDENCIAS.md, respondendo a Questão 11: qual ferramenta foi usada, o prompt exato enviado, o que a IA fez/encontrou, e se ela seguiu (ou não) as instruções do CLAUDE.md/skill.

Restrições: não copie respostas, CLAUDE.md ou skill de colega; não invente nenhuma evidência que não tenha ocorrido de fato; não altere nenhum arquivo fora do escopo pedido (README, CLAUDE.md, a pasta da skill renomeada, e EVIDENCIAS.md).
```

## O que a IA fez nesta sessão (antes da evidência)

1. Leu o `README.md`, o `Program.cs`, o `.csproj`, o `CLAUDE.md` vazio e a skill placeholder.
2. Rodou o app de verdade, a partir de `GerenciadorDeTarefas`, com `"" | dotnet run` (para não travar no `Console.ReadLine`). A saída bateu com o que depois foi descrito no `CLAUDE.md`: três tarefas, depois a `#1` com `[X]`.
3. Escreveu as dissertativas no README, o `CLAUDE.md` específico e a skill em `.claude/skills/revisao-bugs-seguranca/SKILL.md` (a pasta `minha-skill/` foi removida).
4. **Aplicou a skill** no código do GerenciadorDeTarefas: li o `SKILL.md` e o `CLAUDE.md` e percorri o checklist (validação, erros silenciosos, exposição de dado) só no que existe — um console com títulos hardcoded, sem autenticação e sem banco. Não alterei o `Program.cs`.

## O que a revisão encontrou

Superfície atual: um executável .NET 8, estado em `List<(int, string, bool)>`, sem input de menu. Risco geral **baixo** (não há rede nem segredo no repo), mas já tem buraco de robustez se alguém ligar esses métodos a `ReadLine` depois.

| Severidade | Local | Achado |
|---|---|---|
| média | `GerenciadorDeTarefas/Program.cs:9` | `Concluir(int id)` não valida id e não avisa se ninguém foi encontrado. `Concluir(99)` simplesmente não faz nada; o chamador acha que concluiu. |
| média | `GerenciadorDeTarefas/Program.cs:4` | `Adicionar(string titulo)` não recusa `""` nem whitespace. Com os títulos atuais (literais no código) não quebra; no dia em que o título vier do console, a lista enche de lixo. |
| baixa | `GerenciadorDeTarefas/Program.cs:11` | O `for` de `Concluir` não dá `break` depois de achar o id. Hoje o `proximoId` evita duplicata; se alguém inserir id repetido, marca todos. |
| baixa | `GerenciadorDeTarefas/Program.cs:20` | `Listar()` não trata lista vazia (fica só o cabeçalho, sem “nenhuma tarefa”). Não é falha de segurança; é comportamento mudo, no mesmo espírito da skill (erro/ausência sem feedback). |
| baixa | `GerenciadorDeTarefas/Program.cs:25` | O título vai cru para o console. Aqui os textos são fixos; se no futuro o usuário digitar o título, qualquer conteúdo (inclusive dado pessoal colado por engano) aparece na saída sem filtro. |

Não classifiquei como vulnerabilidade a falta de login, HTTPS ou Entity Framework: o programa **não tem** essa camada, e o `CLAUDE.md` pede para não inventar arquitetura.

Confirmei o caminho feliz na prática: depois do `dotnet run`, a tarefa `#1` saiu `[X]` — o `Concluir(1)` do script de demo funciona; o problema é o caminho de erro, que o demo não exercita.

**Não feito (skill):** nenhum patch, nenhuma feature (`Remover`, menu, arquivo), nenhum commit.

## A IA seguiu o CLAUDE.md e a skill?

**Sim, no que eu pude conferir nesta sessão.**

Do `CLAUDE.md`:

- Tratou o projeto como console C# / `net8.0` / um `Program.cs`, não como API.
- Usou `dotnet run` como validação, não inventou `npm`.
- **Não** adicionou método novo nem refatorou para classes.
- **Não** editou arquivo fora do escopo (README, `CLAUDE.md`, skill, `EVIDENCIAS.md`).
- Respeitou a skill apontada no guia (`.claude/skills/revisao-bugs-seguranca/`).

Da skill:

- Relatório com tabela, local `arquivo:linha`, severidade, superfície real.
- Achados amarrados no código; sem lista genérica tipo “siga o OWASP”.
- Recusou corrigir sozinha.

O que eu teria que ajustar se fosse repetir: o prompt original era enorme (toda a avaliação). A skill pede uma tarefa de revisão; o agente só aplicou o checklist **depois** de escrever os arquivos. Da próxima vez eu mandaria um segundo recado só com “segue `CLAUDE.md` e a skill `revisao-bugs-seguranca` no `GerenciadorDeTarefas/` — não mexa no código”. Ficaria mais fácil de auditar o turno da revisão isolado. Mesmo assim, a revisão acima foi feita neste workspace, neste código, sem achado inventado.
