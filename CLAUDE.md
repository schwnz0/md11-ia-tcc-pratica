# CLAUDE.md — GerenciadorDeTarefas

Este arquivo é o guia permanente do repositório para um assistente de IA. Ele existe porque um modelo sem contexto trata este console como “app genérico” e começa a inventar API, banco e classes. Aqui está o que importa de verdade neste projeto — não um template copiado.

## O que é este repositório

Avaliação prática do Módulo 11 (IA) em cima de um **console app C#** chamado GerenciadorDeTarefas: lista de tarefas **em memória**, sem persistência, sem UI web, sem pacotes NuGet extras.

O TCC do autor (Everson) é outro sistema (**No Prumo**, gestão de obras). Este app é só a base da avaliação. Não misture os dois: não traga Entity Framework, JWT nem cifra de CPF para cá a menos que alguém peça explicitamente.

## Mapa do código (é pequeno — leia antes de editar)

| Caminho | Papel |
|---|---|
| `GerenciadorDeTarefas.sln` | Solution Visual Studio 17 / .NET |
| `GerenciadorDeTarefas/GerenciadorDeTarefas.csproj` | SDK-style, **net8.0**, `OutputType=Exe`, `ImplicitUsings` e `Nullable` ligados, `RootNamespace=GerenciadorDeTarefas` |
| `GerenciadorDeTarefas/Program.cs` | **Único** arquivo de código: top-level statements (não há `class Program` / `Main`) |

Não existem pastas `Controllers`, `Services` ou testes. Se a tarefa não pedir, não crie.

## Modelo de dados e comportamento atual

Estado global no `Program.cs`:

- `List<(int Id, string Titulo, bool Concluida)> tarefas`
- `int proximoId` começando em `1`

Funções (nomes em português, `void`, estilo procedural):

- `Adicionar(string titulo)` — incrementa o id e inclui `(id, titulo, false)`
- `Concluir(int id)` — percorre a lista e, se achar o id, marca `Concluida = true` (não avisa se o id não existe)
- `Listar()` — imprime `[ ]` / `[X]`, `#id` e o título

O `Main` implícito cadastra 3 tarefas de demo, lista, conclui a `#1`, lista de novo e bloqueia em `Console.ReadLine()`.

Saída esperada ao rodar (a ordem das 3 tarefas é fixa no código):

```
=== Gerenciador de Tarefas ===
[ ] #1 — Estudar para a avaliação do Módulo 11
[ ] #2 — Configurar o CLAUDE.md do projeto
[ ] #3 — Criar uma Skill reutilizável

=== Depois de concluir a tarefa #1 ===
[X] #1 — Estudar para a avaliação do Módulo 11
[ ] #2 — Configurar o CLAUDE.md do projeto
[ ] #3 — Criar uma Skill reutilizável
```

## Como rodar

Na máquina precisa ter o SDK do **.NET 8**.

```bash
cd GerenciadorDeTarefas
dotnet run
```

O processo espera um Enter no `ReadLine` final. Em automação, envie uma linha vazia para a entrada padrão (no PowerShell: `"" | dotnet run`).

Abrir `GerenciadorDeTarefas.sln` no Visual Studio também vale. Não use `npm`, `python` nem `java`.

## Convenções se for alterar código (só quando pedirem)

- Continue em **top-level statements**. Não “organize” em classes, interfaces ou DI por conta própria.
- Novos métodos no mesmo espírito: `void NomeEmPortugues(...)`, loop `for`/`foreach` simples, tupla como registro da tarefa.
- Mensagens de console em português, no padrão já usado (`=== ... ===`, `[X]` / `[ ]`).
- Sem dependências novas no `.csproj` sem pedido explícito.
- Nullable está ligado: não desligue pra silenciar warning.

## O que a IA NÃO deve fazer

- Não adicionar funcionalidade nova (remover tarefa, menu interativo, arquivo JSON, etc.) a menos que o usuário peça.
- Não refatorar “pra ficar profissional”.
- Não commitar, abrir PR ou alterar git config sem pedido.
- Não editar arquivos fora do que a tarefa listou. Nesta avaliação, o código do app em geral **não** é o entregável — o entregável é README, este guia, a skill e `EVIDENCIAS.md`.
- Não colar segredos. Aqui não há User Secrets; se alguém misturar com o No Prumo, chaves de cifra/JWT **nunca** vão para o Git.
- Não inventar evidência de uso da IA. Se rodou um comando ou uma skill, documente o que realmente aconteceu.

## Skills deste repo

- `.claude/skills/revisao-bugs-seguranca/SKILL.md` — revisão reutilizável de bugs e fragilidades (validação, erros, dados sensíveis). Use quando pedirem auditoria/revisão; **não corrija** o código a menos que peçam o conserto depois.

## Por que este guia está assim (Questão 3)

Um bom `CLAUDE.md` não é um manual genérico de “escreva código limpo”. Ele responde às perguntas que a IA erra neste repo:

1. **O que é o sistema** — console, memória, um arquivo.
2. **Como validar** — `dotnet run` e a saída acima.
3. **Qual o estilo legal** — tupla + métodos `void` em português.
4. **O que é proibido** — a lista de NÃO, porque o modelo tende a superengenheirar.
5. **Onde está a exceção** — skill de revisão vs. pedido de feature.

Isso é contexto barato e estável. Detalhe de uma conversa (“hoje só revisa, não mexe”) continua no prompt; decisão de projeto fica aqui para a próxima sessão não recomeçar do zero.
