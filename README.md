# Avaliação Individual — Módulo 11 — Tecnologias Emergentes e IA

**Data de entrega:** 25/09/2026
**Formato:** individual, de consulta aberta — use slides, anotações e a própria IA à vontade para pesquisar e testar suas respostas.

## Como participar

1. Faça um **fork** deste repositório.
2. Clone o seu fork localmente.
3. Responda as questões teóricas **direto neste README**, abaixo de cada uma.
4. Complete a parte prática (veja abaixo) editando `CLAUDE.md`, `.claude/skills/minha-skill/SKILL.md` e `EVIDENCIAS.md`.
5. Abra um **Pull Request** do seu fork de volta para este repositório.

> O PR não será mergeado — ele existe só para eu avaliar o seu diff. Pode deixar aberto depois de enviar.

O objetivo não é decorar definições, e sim demonstrar que você entende os conceitos e sabe aplicá-los para ganhar eficiência ao usar IA no seu projeto de TCC. Responda com suas próprias palavras — copiar e colar resposta pronta de IA sem entender não demonstra o aprendizado esperado.

---

## Questões dissertativas

### Questão 1 — O que é um "agent"?
O que é um "agent" (agente de IA)? Explique com suas próprias palavras e dê um exemplo de situação em que faz mais sentido usar um agente do que um chat comum.

**Sua resposta:**

Pra mim, um agente é uma IA que não fica só no papo: ela consegue planejar uma sequência de passos e executar coisas no ambiente (abrir arquivo, rodar `dotnet run`, editar código, seguir uma skill). O chat comum me devolve texto e eu que aplico. O agente entra no projeto e trabalha em cima dele.

Faz mais sentido usar agente do que chat quando a tarefa depende do código local de verdade. Nesta avaliação, por exemplo, não adianta eu colar um pedaço do `Program.cs` no ChatGPT e torcer: eu precisava que a IA lesse o GerenciadorDeTarefas, respeitasse o `CLAUDE.md` e rodasse uma revisão de bugs/segurança no repositório. Isso é trabalho de agente. Se eu só quisesse uma definição rápida de “o que é few-shot”, um chat já resolveria.

### Questão 2 — O que são guidelines?
O que são "guidelines" (diretrizes) ao usar uma IA generativa? Qual é o papel delas na qualidade das respostas geradas pelo modelo?

**Sua resposta:**

Guidelines são as regras do jogo que eu coloco antes (ou junto) do pedido: o que o projeto é, como o código é escrito, o que a IA pode e não pode fazer. No Cursor isso aparece como `CLAUDE.md`, rules e skills. Sem isso o modelo improvisa no estilo dele — cria classe, puxa NuGet, “melhora” o que ninguém pediu.

O papel delas na qualidade é cortar o espaço de chute. O modelo continua gerando, mas com um trilho: se o guia diz que o GerenciadorDeTarefas é um console em C# com top-level statements e métodos em português, a resposta tende a continuar nesse padrão em vez de virar API REST do nada. Qualidade, aqui, é menos “texto bonito” e mais “resposta utilizável no meu contexto”.

### Questão 4 — Escolha de modelo e nível de esforço
Qual modelo de IA utilizar para cada tipo de tarefa? Dê um exemplo de tarefa simples e outra mais complexa, explicando como você escolheria o modelo em cada caso. O que é o "nível de esforço" (effort level) e quando faz sentido aumentá-lo ou diminuí-lo?

**Sua resposta:**

Eu não uso o modelo mais pesado pra tudo. Tarefa simples — renomear um método, explicar o que o `Listar()` faz, montar o comando `dotnet run` — um modelo rápido já dá conta e eu não fico esperando. Tarefa complexa — revisar segurança de um fluxo que cifra CPF/CNPJ no No Prumo, ou desenhar como quebrar um módulo grande em etapas — aí eu escolho um modelo mais capaz, porque o risco de alucinar API, pular um caso de erro ou sugerir criptografia errada é alto.

O nível de esforço é o quanto a IA “pensa” antes de fechar a resposta (mais análise, mais checagem, mais tempo). Eu aumento quando o custo de um erro é grande: revisão de segurança, decisão de arquitetura, algo que vai pro TCC. Eu diminuo quando o pedido é estreito e fácil de conferir na hora, tipo “qual o TargetFramework desse `.csproj`?”. Effort alto em pergunta bobinha só gasta tempo; effort baixo numa auditoria é economia falsa.

### Questão 5 — Como estruturar um bom prompt
Descreva os elementos que tornam um prompt mais eficaz (ex.: contexto, objetivo, formato esperado, exemplos, restrições).

**Sua resposta:**

O que mais muda o resultado pra mim é deixar explícito cinco coisas:

1. **Contexto** — onde a IA está (console C# .NET 8, um `Program.cs`, lista em memória). Sem isso ela inventa web app.
2. **Objetivo** — o que eu quero no final (“revisar bugs e segurança”, não “olha esse código”).
3. **Formato** — tabela com severidade e arquivo:linha, por exemplo. Senão vem um texto solto difícil de usar.
4. **Restrições** — o que não fazer: não adicionar feature, não alterar arquivo fora do escopo, não commitar sozinho.
5. **Exemplos** (quando o padrão importa) — um recorte do estilo já usado, tipo o jeito do `Adicionar`/`Concluir`.

No Cursor, parte disso eu tiro do prompt e coloco no `CLAUDE.md`/skill, porque senão eu fico repetindo as mesmas regras em toda conversa.

### Questão 6 — Iteração de prompt
O que significa "iterar" um prompt? Por que a primeira resposta de uma IA geralmente não é a versão final, e como você usaria a resposta recebida para melhorar o próximo prompt?

**Sua resposta:**

Iterar o prompt é tratar a primeira resposta como rascunho: eu olho o que veio, vejo o que está vago, errado ou incompleto, e mando um pedido mais apertado em cima disso — não começo do zero como se nada tivesse acontecido.

A primeira resposta quase nunca é a final porque o modelo preenche lacunas com o “mais provável”. Se eu pedi “revisa o gerenciador” sem dizer que é só `Program.cs` e que não quero feature nova, ele pode sugerir Entity Framework. Eu pego exatamente esse desvio e corrijo no próximo turno: “não proponha persistência; aponte só validação e falha silenciosa no `Concluir`”. Ou seja, eu uso o erro da IA como diagnóstico do que o prompt ainda não travou.

### Questão 7 — Zero-shot vs. few-shot
Qual é a diferença entre um prompt "zero-shot" e um prompt "few-shot"? Dê um exemplo de situação em que vale a pena incluir exemplos dentro do próprio prompt.

**Sua resposta:**

Zero-shot é pedir a tarefa só com a instrução, sem mostrar um caso resolvido. Few-shot é colocar um ou alguns exemplos do formato/estilo que eu espero, e aí pedir o resto no mesmo padrão.

Vale a pena few-shot quando “certo” é um padrão meu, não uma verdade universal. Se eu fosse pedir um método novo no GerenciadorDeTarefas, eu colaria o `Adicionar` e o `Concluir` e diria “siga exatamente esse estilo: `void`, português, sem classe extra”. Sem exemplo, a IA tende a criar `TaskService` e record. No TCC, few-shot também ajuda pra padronizar mensagem de erro da API (um JSON de exemplo de 400/401) em vez de cada endpoint nascer com um formato diferente.

### Questão 8 — Memória e contexto entre sessões
O que significa uma IA "ter memória" entre sessões diferentes de conversa? Por que, em um projeto longo como o TCC, é importante decidir o que precisa ser "lembrado" e como fornecer esse contexto para a IA a cada nova conversa?

**Sua resposta:**

“Ter memória” entre sessões, na prática, não é a IA lembrar da minha vida. É algum contexto sobreviver: um arquivo de guia no repo, rules do Cursor, User Secrets que eu não colo no chat, ou um resumo que eu mesmo trago. Cada conversa nova começa burra em relação ao que aconteceu ontem, a menos que eu deixe isso escrito em algum lugar que ela leia.

No TCC (No Prumo — gestão de obras) isso pesa. Tem decisão que não pode se perder: documentos (CPF/CNPJ) cifrados com AES-256-GCM, HMAC, JWT e connection string em User Secrets, nunca no Git. Se eu não decidir o que é “memória oficial” e jogar isso num `CLAUDE.md`/regra, cada sessão a IA vai sugerir guardar CPF em texto puro “só pra facilitar o teste”. Eu escolho lembrar o que é invariante (segurança, camadas, como rodar) e deixo detalhe passageiro fora, senão o contexto enche de ruído e a IA se apega em coisa velha.

### Questão 9 — Avaliar a resposta da IA
Antes de aplicar a sugestão de uma IA no seu projeto, como você verifica se ela está correta? Descreva pelo menos 2 formas práticas de checar a confiabilidade de uma resposta gerada por IA.

**Sua resposta:**

Eu não aplico no piloto automático. Duas checagens que eu realmente uso:

1. **Conferir no código e rodar.** Se a IA diz que o `Concluir` marca a tarefa, eu abro o trecho e, se fizer sentido, rodo `dotnet run` pra ver o `[X]` sair no console. No No Prumo eu iria além: se ela mexer em cifra de documento, eu testaria o endpoint e olharia se o valor no banco continua ilegível.
2. **Desconfiar de afirmação sem âncora.** Se ela cita um método, uma linha ou uma API do .NET, eu localizo isso no arquivo ou na documentação. Achado de segurança sem `arquivo:linha` eu trato como opinião. Outro filtro simples: se a sugestão viola uma regra que eu já escrevi (não adicionar pacote, não commitar segredo), está errada pro meu projeto mesmo que o código “compile na cabeça”.

### Questão 10 — Dividir tarefas complexas em etapas
Por que, em tarefas mais complexas, pode ser melhor dividir o trabalho em um fluxo de etapas (ex.: primeiro classificar/organizar, depois processar, depois revisar) em vez de pedir tudo em um único prompt? Dê um exemplo aplicado a uma tarefa do seu TCC.

**Sua resposta:**

Porque um prompt gigante mistura objetivos e a IA prioriza o que é mais fácil de gerar (código bonito) e deixa passar o que é chato (caso de erro, dado sensível, o que não fazer). Dividindo, cada etapa tem um critério de pronto e eu consigo recusar um passo sem jogar o resto fora.

No No Prumo, se eu pedisse “faz o módulo de documentos das empresas (CPF/CNPJ) completo”, viria um CRUD misturado com criptografia pela metade. O fluxo que funciona melhor pra mim é: (1) mapear o que entra/sai e o que nunca pode aparecer em log; (2) implementar a cifra/hash com as chaves em User Secrets; (3) expor a API sem devolver o documento em claro; (4) só então uma revisão de segurança. É o mesmo motivo desta avaliação estar quebrada em README, `CLAUDE.md`, skill e evidência — cada peça é um checkpoint.

> **Questão 3** (como escrever um bom CLAUDE.md) e a **Questão 11** (prática, evidência de uso real da IA) são respondidas nos próprios arquivos `CLAUDE.md` e `EVIDENCIAS.md` — veja a parte prática abaixo.

---

## Parte prática

1. **Complete o `CLAUDE.md`** na raiz deste repositório — é onde você responde a Questão 3, documentando o projeto para orientar um assistente de IA.
2. **Complete a Skill** em `.claude/skills/minha-skill/SKILL.md`, com instruções reutilizáveis para uma tarefa recorrente do projeto. Renomeie a pasta `minha-skill/` para o nome real da sua skill.
3. **Conecte um assistente de IA ao código local** (Claude Code, GitHub Copilot, Cursor, ou outro de sua escolha) e use-o pelo menos uma vez de verdade, aplicando o `CLAUDE.md` e/ou a Skill que você criou em uma tarefa real do projeto `GerenciadorDeTarefas`.
4. **Complete o `EVIDENCIAS.md`** — é onde você responde a Questão 11, documentando essa experiência (ferramenta usada, prompt exato, o que a IA fez, se seguiu suas instruções).

### O que NÃO fazer

- ❌ Copiar as respostas, o CLAUDE.md ou a Skill de um colega
- ❌ Inventar uma evidência que não aconteceu de verdade
- ❌ Alterar arquivos fora do escopo pedido

## Sobre o projeto de exemplo

Dentro de `GerenciadorDeTarefas/` tem um console app simples em C# — um gerenciador de tarefas fictício — que serve de base para você praticar. Não é necessário adicionar funcionalidades novas ao app; o foco é a configuração e o uso da IA em cima desse código.

Abra `GerenciadorDeTarefas.sln` no Visual Studio, ou rode pelo terminal:

```bash
cd GerenciadorDeTarefas
dotnet run
```

---

## Critérios de avaliação (10 pontos)

| Critério | Pontos |
|---|---|
| Questões dissertativas (conjunto) | 4 |
| `CLAUDE.md` bem estruturado e específico ao projeto (Questão 3) | 2 |
| Skill funcional e realmente reutilizável | 2 |
| `EVIDENCIAS.md` — uso real da IA, seguindo (ou não) o CLAUDE.md/Skill (Questão 11) | 1 |
| Qualidade do Pull Request (descrição clara, organizado, dentro do escopo) | 1 |

## Entrega

Envie o **link do seu Pull Request** pelo Akademos até a data acima.
