# ✂️ Auditoria de Ablação — curso INEMA

Curso HTML self-contained (formato INEMA.CLUB dark âmbar, camada de aprendizagem v2) que ensina a auditar e enxugar a própria configuração do Claude Code — `CLAUDE.md`, skills e hooks — pelo método de **ablação**.

**Curso online:** https://inematds.github.io/curso-ablacao/

> Toda linha da sua configuração é **culpada de complexidade até provar utilidade**.
> Mas o objetivo não é cortar o máximo: é maximizar **qualidade + autonomia + verificabilidade ÷ complexidade**.

## Para quem

Quem **já usa Claude Code** e acumulou config própria. Não é curso de introdução ao Claude Code.

**Pré-requisitos:** Claude Code em uso; pelo menos um `CLAUDE.md` (global ou de projeto) e/ou skills próprias; noção básica de git e terminal.

## Estrutura

4 trilhas · 8 módulos · 48 tópicos · ~6h30

| Trilha | Módulos | Pergunta que responde |
|---|---|---|
| **1. Por que apagar** (emerald) | 1.1 Configuração envelhece · 1.2 O método de ablação | Por que uma config que funcionava virou peso morto? |
| **2. Como diagnosticar** (blue) | 2.1 A taxonomia · 2.2 De microgerenciamento a critério e verificação | Como dizer o que fica, o que sai e o que precisa de teste? |
| **3. Auditar de verdade** (purple) | 3.1 Rodando a skill `audit-ablacao` · 3.2 Do relatório aos cortes | Como rodar a auditoria e transformar o relatório em cortes seguros? |
| **4. Provar e manter** (amber) | 4.1 O plano de ablação A/B/C · 4.2 O ciclo contínuo | Como provar que a versão mínima não piorou — e manter assim? |

Cada módulo tem 6 tópicos, exercício prático na **sua própria config** e critério de saída verificável.

## Projeto final

Quatro entregáveis sobre a sua configuração: (1) relatório da skill com as 10 seções; (2) `CLAUDE.md` antes/depois com diff; (3) tabela A/B/C com ≥2 tarefas reais; (4) política de ablação pessoal.

**Aprovação:** os 4 itens existem; toda remoção aplicada tem trecho citado + risco + como testar; toda instrução devolvida tem falha repetida registrada; a versão final tem pelo menos uma verificação objetiva onde antes não havia nenhuma.

## Skill que o curso ensina a usar

[`inematds/audit-ablacaocc`](https://github.com/inematds/audit-ablacaocc) — skill de diagnóstico (somente leitura) que audita `CLAUDE.md`, skills e hooks, classifica cada instrução e propõe uma versão mínima, sem tocar em nenhum arquivo.

## Estrutura de arquivos

```
curso-ablacao/
├── index.html                  # landing do curso
├── assets/
│   ├── learn.css               # camada de aprendizagem (temas, prose, controles)
│   ├── learn.js                # window.INEMA (progresso, dúvida, notas, jornada)
│   └── curso.css               # base v1 + light mode completo
├── curso/
│   ├── trilha1/  index.html + modulo-1-1.html + modulo-1-2.html
│   ├── trilha2/  index.html + modulo-2-1.html + modulo-2-2.html
│   ├── trilha3/  index.html + modulo-3-1.html + modulo-3-2.html
│   └── trilha4/  index.html + modulo-4-1.html + modulo-4-2.html
├── capa/capa.png               # capa 1280x720
├── doc/                        # plano do curso, contrato de página e material de base
└── .github/workflows/pages.yml # deploy do GitHub Pages via Actions
```

Sem build, sem backend, sem framework: HTML + Tailwind via CDN + JS inline. Abre até em `file://`.

## Créditos

Método e visão: **Boris Cherny** (Anthropic), palestra na Y Combinator sobre o corte de 80% do prompt de sistema do Claude Code. Resumos e material de base em [`doc/`](doc/). Curso montado pelo INEMA no formato `formato-curso-v2`.
