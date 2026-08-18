# Plano de curso — Auditoria de Ablação: enxugue seu Claude Code sem perder qualidade

> Toda linha da sua configuração é **culpada de complexidade até provar utilidade.**

## Ficha do curso

| Item | Definição |
|---|---|
| **Título** | Auditoria de Ablação — enxugue seu `CLAUDE.md`, skills e hooks sem perder qualidade |
| **Público** | Quem **já usa Claude Code** no dia a dia e acumulou `CLAUDE.md`, skills e hooks próprios — do usuário avançado ao dev. Não é curso de introdução ao Claude Code. |
| **Pré-requisitos** | Ter o Claude Code instalado e em uso; ter pelo menos um `CLAUDE.md` (global ou de projeto) e/ou skills próprias; noção básica de git e terminal. |
| **Objetivo geral** | Ao final, o aluno sabe **diagnosticar** a própria configuração de agente (classificar cada instrução, achar peso morto, redundância e microgerenciamento), **propor uma versão mínima** e **provar por teste de ablação** que a versão mínima não perdeu qualidade — repetindo isso a cada lançamento de modelo. |
| **Base** | Palestra de Boris Cherny (Y Combinator, "cortamos 80% do prompt do Claude Code") + skill `audit-ablacao` (somente diagnóstico) deste repositório. |
| **Formato** | Curso HTML INEMA em **trilhas → módulos**, cada módulo com: objetivo, conteúdo, exercício prático e **critério de saída verificável**. Compatível com `formato-curso-v2` e `formato-curso-v5` (no v5, os módulos 3 e 5 precisam de simplificação de vocabulário e mais analogias). |
| **Carga estimada** | ~6 h de conteúdo + ~4 h de prática na própria config (total ~10 h, distribuível em 1 semana). |
| **Fio condutor** | Uma **config real do aluno** (a global `~/.claude/` ou a de um projeto) atravessa o curso inteiro: é auditada no módulo 5, cortada no 6, testada no 7 e colocada em ciclo no 8. |
| **Regra do curso** | Assim como a skill, o curso **não manda apagar nada por impulso**: primeiro diagnóstico, depois proposta, depois teste — a aplicação é sempre decisão consciente do aluno. |

---

## Mapa das trilhas

| Trilha | Módulos | Pergunta que responde |
|---|---|---|
| **1. Por que apagar** | 1, 2 | Por que uma config que funcionava virou peso morto — e como a Anthropic lidou com isso? |
| **2. Como diagnosticar** | 3, 4 | Como olhar linha por linha e dizer o que fica, o que sai e o que precisa de teste? |
| **3. Como auditar de verdade** | 5, 6 | Como rodar a auditoria na minha config e transformar o relatório em cortes seguros? |
| **4. Como provar e manter** | 7, 8 | Como provar que a versão mínima não piorou — e não deixar o sedimento voltar? |

---

## Trilha 1 — Por que apagar

### Módulo 1 — Configuração envelhece

**Objetivo:** entender que cada instrução é o conserto da fraqueza de um modelo específico e por que ela vira peso morto quando o modelo muda.

**Conteúdo:**
- Quem é Boris Cherny e o contexto da palestra (YC, um dia após o Opus 5).
- O harness do Claude Code nunca está pronto: prompt de sistema, ferramentas e prompts de ferramenta mudam a cada modelo.
- O corte de **80% do prompt de sistema** — o que saiu (correções de comportamento) e o que ficou (segurança, permissões, análise estática, interface).
- O custo composto de uma linha zumbi: contexto queimado em toda execução, autonomia reduzida, comportamento inconsistente, regra que ninguém ousa apagar.
- Tabela "sintoma → o que costuma ser" (CLAUDE.md de 300 linhas, regra em 3 lugares, skill que ensina a pensar, passo a passo de 12 etapas, regras que se contradizem, nada dizendo como conferir).

**Exercício:** abrir o próprio `~/.claude/CLAUDE.md` (e/ou o de um projeto) e marcar, à mão, **três instruções que existem para corrigir um comportamento antigo** — anotar para qual modelo/época cada uma foi escrita.

**Critério de saída:** o aluno consegue apontar as três linhas e dizer, para cada uma, "o que ela tenta evitar" e "quando ela nasceu".

---

### Módulo 2 — O método de ablação

**Objetivo:** dominar o ciclo de apagar e reconstruir com base em evidência, não em previsão.

**Conteúdo:**
- Ablação como termo de pesquisa: remover para medir impacto.
- O conselho para o usuário comum: **a cada ~6 meses e a cada lançamento grande de modelo, apague `CLAUDE.md`, skills e hooks e veja o que o modelo faz sem eles.**
- A reconstrução em 4 etapas: apagar → usar em **trabalho real** → observar onde tropeça → devolver uma instrução **só depois de ver a mesma falha repetir**.
- Por que a disciplina: você é péssimo previsor de qual linha o modelo precisa; cada linha mantida custa contexto sempre.
- Ferramentas de linha de base: flag de prompt de sistema na inicialização e `CLAUDE_CODE_SIMPLE=1` (remove todos os prompts, inclusive das ferramentas). O achado contraintuitivo: sem prompts o modelo fica *ligeiramente mais inteligente*; os prompts existem para o produto se comportar como a pessoa espera.
- Evals também envelhecem (1 a 3 gerações de modelo) — crie evals onde viu o modelo tropeçar, aposente as saturadas.
- A regra de reintrodução (4 condições): falha real → mesma classe se repete → uma instrução específica resolve → na forma mais curta possível.

**Exercício:** rodar uma tarefa real do próprio trabalho com `CLAUDE_CODE_SIMPLE=1` (ou com o `CLAUDE.md` renomeado temporariamente) e registrar num arquivo `ablacao-diario.md`: o que foi bem, onde tropeçou, e se o tropeço se repetiu numa segunda tentativa.

**Critério de saída:** diário com pelo menos 1 tarefa real registrada, com "acertos / tropeços / repetiu?" preenchidos. Nenhuma instrução devolvida ainda.

---

## Trilha 2 — Como diagnosticar

### Módulo 3 — A taxonomia: o que cada linha é

**Objetivo:** classificar qualquer instrução de config em uma categoria e uma decisão, com base nas 7 perguntas da skill.

**Conteúdo:**
- As 10 categorias: `CONTEXTO` · `GUARDRAIL` · `CRITÉRIO DE QUALIDADE` · `VERIFICAÇÃO` · `INTEGRAÇÃO/FERRAMENTA` · `PROCEDIMENTO REPETÍVEL` · `MICROGERENCIAMENTO` · `REDUNDÂNCIA` · `LEGADO/OBSOLETA` · `AMBÍGUA/NÃO COMPROVADA` — com um exemplo real de cada.
- As 6 decisões: `KEEP` · `SIMPLIFY` · `MOVE` · `MERGE` · `TEST` · `REMOVE`. **Na dúvida, `TEST`, não `KEEP`.**
- As 7 perguntas por instrução (o que evita/garante? o modelo atual ainda precisa? diz *o que* ou dirige *como pensar*? duplicada? limita autonomia? tem forma mais curta? o que quebra se sumir?).
- O que **nunca** cortar por reflexo: identidade do projeto, caminhos e fontes de verdade, branding, segurança, compliance, contratos de interface, integrações, convenções internas — o modelo não infere isso sozinho.
- A função-objetivo: não é "reduzir ao máximo", é maximizar **qualidade + autonomia + verificabilidade ÷ complexidade**.
- Sinais a procurar: passo a passo desnecessário, regra duplicada entre `CLAUDE.md` e skills, skill grande demais, excesso de exemplos, formatação rígida sem motivo, exceções acumuladas, contradições, contexto global que serve a poucas tarefas, **verificações ausentes**.

**Exercício:** pegar 15 instruções do próprio `CLAUDE.md` e montar uma tabela `Trecho | Categoria | Decisão | Pergunta que decidiu | O que quebra se sumir`.

**Critério de saída:** tabela com 15 linhas, todas com categoria e decisão; pelo menos uma linha em `TEST` (se não houver nenhuma, o aluno provavelmente está fazendo `KEEP` por conforto).

---

### Módulo 4 — De microgerenciamento a critério + verificação

**Objetivo:** reescrever instruções do tipo "faça A, depois B, depois C" como **objetivo + guardrails + critérios de saída + verificação**, e entender por que verificação é a alavanca que quase todo mundo erra.

**Conteúdo:**
- O modo de falha mais comum (e mais frequente em quem tem anos de engenharia): especificação excessiva. Funcionava com modelos antigos; hoje impede o caminho melhor.
- A conversão canônica: de "faça A, depois B, depois C" para **"Produza X. Respeite Y. O resultado deve atingir Z. Verifique usando W. Escolha a estratégia."**
- O template de prompt recomendado: Objetivo · Contexto · Guardrails · Critérios de qualidade · Verificação · Autonomia.
- **Verificação:** prompt curto com um jeito real de o modelo conferir o próprio trabalho vence prompt gigante sem verificação. O exemplo da reescrita Electron → Swift: rodar o original numa VM, screenshot, comparar pixel a pixel, não parar até bater. Produzir → observar → comparar → condição de parada.
- "Mire acima do teto": dê tarefa um pouco mais difícil do que parece e reteste fracassos antigos a cada versão.
- Nível certo de instrução = o que você daria a um colega de trabalho. Trabalhar com modelo é ciência empírica, não teórica.
- Não existe truque secreto: tarefa difícil → meios de verificar → observar onde trava → corrigir *aquilo* → repetir.

**Exercício:** escolher a instrução mais "receita de bolo" da própria config (ou de uma skill própria) e reescrevê-la em duas versões: (a) 50% menor, (b) mínima com o template — obrigatoriamente com um **passo de verificação objetivo** (comando, teste, comparação, checagem de arquivo).

**Critério de saída:** as duas versões escritas lado a lado da original; a versão mínima tem uma verificação que **um terceiro conseguiria executar** sem perguntar nada ao aluno.

---

## Trilha 3 — Como auditar de verdade

### Módulo 5 — Rodando a skill `audit-ablacao`

**Objetivo:** instalar e rodar a skill na própria config e **ler** o relatório de 10 seções sabendo o que cada seção pede de você.

**Conteúdo:**
- O que a skill faz (lê, classifica, propõe) e o que **nunca** faz (editar, mover, apagar, commitar) — e por que auditoria que já sai mexendo é auditoria em que você não confia.
- Fronteira com `memory-audit`: memória da sessão ≠ configuração do agente.
- Instalação (global em `~/.claude/skills/audit-ablacao/` ou por projeto em `.claude/skills/`) e chamada: `/audit-ablacao` ou por gatilho em linguagem natural.
- Escopos que valem a pena: global (a cada ~6 meses / modelo novo), um projeto (`CLAUDE.md` > ~150 linhas), um conjunto de skills (3+ brigando pelo mesmo gatilho).
- Anatomia do relatório, seção a seção: 1 resumo executivo · 2 métricas (contagens e % de redução) · 3 problemas por arquivo · 4 candidatas à remoção (com *como testar*) · 5 redundâncias e conflitos · 6 skills · 7 `CLAUDE.md` proposto · 8 skills propostas · 9 plano de ablação A/B/C · 10 top 10 por impacto ÷ risco.
- Como cobrar a skill: toda recomendação cita arquivo e trecho; nada de opinião sobre arquivo não lido; "curto não é sempre melhor" — cada remoção relevante vem com risco.
- Auditoria de skill: as 7 decisões (`KEEP` · `SIMPLIFY` · `MERGE` · `SPLIT` · `LOAD-ON-DEMAND` · `CONVERT-TO-CONTEXT` · `DELETE-CANDIDATE`) e as perguntas ("precisa existir?", "que problema recorrente resolve?", "caberia numa instrução curta?", "contém coisas que o modelo já faz sozinho?").

**Exercício:** rodar `/audit-ablacao` na config global (ou no projeto escolhido no módulo 1) pedindo saída em `.md`; conferir 3 recomendações de `REMOVE` **abrindo o arquivo e o trecho citado** e anotar se concorda, discorda ou muda para `TEST`.

**Critério de saída:** relatório salvo com as 10 seções; para as 3 remoções conferidas, uma frase de veredito cada, apontando arquivo:linha.

---

### Módulo 6 — Do relatório aos cortes: skills como unidade certa

**Objetivo:** transformar o Top 10 do relatório em mudanças aplicadas com segurança, usando `MOVE`/`LOAD-ON-DEMAND` para skills como principal ferramenta de emagrecimento.

**Conteúdo:**
- Por que aplicar **em outra sessão/pedido**, separado da auditoria.
- Ordem de ataque: Top 10 por impacto ÷ risco; primeiro redundâncias e conflitos (risco quase zero), depois legado, depois microgerenciamento → critério, e por último o que ficou em `TEST`.
- **Skill é procedimento, não instrução global**: regra no `CLAUDE.md` é lida em 100% das execuções; a mesma regra numa skill custa contexto só quando a tarefa é aquela.
- Invocação direta (`/nome-da-skill`) elimina a loteria do gatilho e mata a necessidade de regra de roteamento no `CLAUDE.md`.
- Skill no projeto (`.claude/skills/`) versiona junto com o código e não vaza para outros projetos.
- Os três remédios quando o modelo tropeça: prompt melhor (instrução obscura) · **skill** (falta procedimento repetível) · MCP (falta contexto que ele não alcança). Escolher o remédio certo evita o reflexo "mais uma regra no `CLAUDE.md`".
- Skill tem fronteira, nome e escopo: dá pra desativar e medir; "aquele parágrafo do meio" não.
- Resultado esperado: `CLAUDE.md` só com o que é verdade **sempre** (identidade, guardrails, fontes de verdade, segurança); todo o resto vira skill.

**Exercício:** aplicar as 3 primeiras mudanças do Top 10 (numa sessão nova, com a config sob git para poder voltar), sendo pelo menos uma um `MOVE` de bloco procedimental do `CLAUDE.md` para uma skill; registrar antes/depois em linhas e em número de regras.

**Critério de saída:** commit (ou snapshot) "antes" e "depois"; `CLAUDE.md` menor; a skill nova responde à invocação direta e o comportamento da tarefa correspondente não mudou.

---

## Trilha 4 — Como provar e manter

### Módulo 7 — O plano de ablação A/B/C

**Objetivo:** provar por teste que a versão mínima não perdeu qualidade — em tarefas reais, com critérios comparáveis.

**Conteúdo:**
- As três versões: **A** config atual · **B** simplificada (~50% menos instruções) · **C** mínima (contexto essencial + objetivo + guardrails + critérios + verificação).
- Escolher 5 a 10 tarefas **reais e representativas** do projeto — não teste hipotético.
- O que medir em cada rodada: qualidade, aderência, autonomia, número de correções humanas, consistência, tempo, uso desnecessário de ferramentas, complexidade e **capacidade de o modelo verificar o próprio trabalho**.
- Como registrar sem se enganar: mesma tarefa, mesmo modelo, mesmo prompt de entrada; anotar antes de olhar a resposta o que seria "bom".
- Ler o resultado: onde C empatou com A, a instrução era peso morto; onde C piorou de forma **repetida**, aplique a regra de reintrodução (e devolva na forma mais curta).
- Evals pessoais: cada falha observada vira um caso do seu conjunto; aposente evals que o modelo saturou.

**Exercício:** montar a grade A/B/C com 5 tarefas reais e rodar pelo menos 2 delas nas três versões; preencher a tabela de comparação.

**Critério de saída:** tabela com ≥2 tarefas × 3 versões preenchida; para cada instrução que voltou (se alguma voltou), a falha que a justificou está registrada e se repetiu ao menos duas vezes.

---

### Módulo 8 — O ciclo contínuo (e o que vem depois)

**Objetivo:** transformar a auditoria em rotina — não em faxina heroica de uma vez só.

**Conteúdo:**
- O ciclo recomendado: rodar `/audit-ablacao` → cortar Top 10 (sessão separada) → usar em trabalho real por dias → anotar falhas e devolver só quando repetir → repetir a cada ~6 meses ou lançamento grande de modelo.
- Gatilhos de re-auditoria: modelo novo, `CLAUDE.md` passou de ~150 linhas, 3+ skills brigando pelo mesmo gatilho, "ninguém sabe mais o que essa regra segura".
- Mentalidade: esqueça pressupostos de modelos anteriores e da teoria de "o que deveria ser difícil"; overengineering é o modo de falha recorrente do builder experiente.
- Onde ainda não está resolvido (sistemas profundos, distribuídos, verificação visual fina) — para calibrar expectativa, não para desistir.
- **Bônus (opcional):** loops e routines — a Anthropic mantém os próprios produtos com 20–30 routines diárias de **uma frase** (limpar código morto, apagar testes inúteis, unificar abstrações duplicadas). A própria re-auditoria pode virar um lembrete/routine periódica.

**Exercício:** escrever a "política de ablação" pessoal em ≤10 linhas (quando re-auditar, o que nunca cortar, regra de reintrodução, onde ficam os diários e evals) e colocá-la **onde ela será relida** — como skill ou nota, não como mais um parágrafo no `CLAUDE.md`.

**Critério de saída:** política escrita, com data da próxima re-auditoria marcada; o `CLAUDE.md` final do curso é menor que o inicial e o aluno sabe dizer **por que cada linha restante sobreviveu**.

---

## Projeto final (avaliação)

O aluno entrega, sobre a própria config:

1. Relatório da skill (10 seções) — módulo 5.
2. `CLAUDE.md` antes/depois com diff — módulo 6.
3. Tabela A/B/C com ≥2 tarefas reais — módulo 7.
4. Política de ablação pessoal — módulo 8.

**Aprovação:** os 4 itens existem; toda remoção aplicada tem trecho citado + risco + como testar; toda instrução devolvida tem falha repetida registrada; a versão final tem pelo menos uma verificação objetiva onde antes não havia nenhuma.

---

## Materiais de apoio (já no repo)

| Material | Uso no curso |
|---|---|
| `SKILL.md` | Módulos 3, 5, 6 — texto-fonte da taxonomia e do relatório |
| `README.md` §1–§7 | Módulos 1, 2, 5, 6, 8 |
| `doc/boris_cherny_cutting_80_percent_claude_code_prompt_pt.md` | Módulos 1, 2, 4, 8 (e bônus routines) |
| `doc/recomendacao_pratica_prompts_skills.md` | Módulo 4 (template de prompt) e 7 (A/B/C) |
| `doc/Prompt para auditar CLAUDE.md e Skills.md` | Módulo 5 — versão longa da skill, para quem quer ver a origem |
| `doc/Texto colado(...).txt` | Transcrição da palestra — leitura opcional |
| `guia/index.html` | Landing do projeto — link de referência |
| `capa/capa.png` | Capa do curso/card no portal |

Fora do escopo do curso: `README.md` §8 (receita de git/GitHub Pages/portal) — é sobre publicar o repo, não sobre ablação. Pode virar apêndice de uma linha.

---

## Sugestão de vídeos por módulo (se for gerar com `videos-cursos-inema`)

Um vídeo curto (3–6 min) por módulo + um de landing. Gancho de cada um:

1. "Sua config está te deixando mais burro."
2. "Apague tudo. Sério. Depois a gente conversa."
3. "10 tipos de linha — e a maioria não merece ficar."
4. "Pare de dar receita: dê o critério."
5. "Uma skill que audita e não toca em nada."
6. "Skill é onde a regra vai morar."
7. "A/B/C: prove que não piorou."
8. "Ablação não é faxina, é rotina."
