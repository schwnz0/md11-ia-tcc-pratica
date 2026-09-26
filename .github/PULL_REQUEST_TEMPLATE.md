## Avaliação Módulo 11 — Everson

PR só para correção (não precisa mergear). Entrega individual: respostas no README, guia do projeto, skill reutilizável e evidência de uso real da IA no `GerenciadorDeTarefas`.

### Resumo

- Dissertativas 1, 2 e 4–10 no `README.md` (questões 3 e 11 ficam nos arquivos abaixo).
- `CLAUDE.md` específico do console C# / .NET 8 (Questão 3).
- Skill: `.claude/skills/revisao-bugs-seguranca/SKILL.md` (revisão de bugs e fragilidades; pasta `minha-skill/` renomeada).
- `EVIDENCIAS.md` (Questão 11): ferramenta, prompt exato, o que a IA fez e se seguiu o guia/skill.

### Como conferir

- [ ] Dissertativas no `README.md`, em primeira pessoa, sem copiar colega
- [ ] `CLAUDE.md` descreve este repo (como rodar, convenções, o que a IA não deve fazer)
- [ ] Skill com `name`/`description` coerentes e instruções reutilizáveis (não amarradas a um arquivo só)
- [ ] Uso real de IA documentado no `EVIDENCIAS.md` (sem evidência inventada)
- [ ] Diff só no escopo da avaliação (README, `CLAUDE.md`, skill, `EVIDENCIAS.md`; este template se tiver sido pedido)

### Escopo deste PR

**Entra:** `README.md`, `CLAUDE.md`, `.claude/skills/revisao-bugs-seguranca/`, `EVIDENCIAS.md`, este template.

**Não entra:** feature nova no `GerenciadorDeTarefas`, refatoração do `Program.cs`, dependências, commit de segredo.

### Evidência (preencher se ainda não estiver no `EVIDENCIAS.md`)

| | |
|---|---|
| Ferramenta | Cursor (agente no código local) |
| Skill aplicada | `revisao-bugs-seguranca` no `GerenciadorDeTarefas` |
| Pediu correção no app? | Não — só revisão |

### Nota para quem avalia

O app de exemplo continua o mesmo de propósito. O ponto da prática é o guia, a skill e a sessão documentada, não um gerenciador mais completo.
