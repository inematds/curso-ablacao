# CONTRATO DE PÁGINA — curso "Auditoria de Ablação" (formato-curso-v2)

Todas as páginas do curso copiam os blocos abaixo **verbatim**, trocando só o que está marcado como `<<...>>`.

## Identidade

- `courseId` = `ablacao` · emoji = `✂️` · nome = `Auditoria de Ablação`
- Trilhas: T1 emerald `Por que apagar` · T2 blue `Como diagnosticar` · T3 purple `Auditar de verdade` · T4 amber `Provar e manter`
- Módulos: 1-1, 1-2, 2-1, 2-2, 3-1, 3-2, 4-1, 4-2 — **6 tópicos cada**
- `REL` = `.` na landing (`index.html` da raiz) · `../..` nas páginas dentro de `curso/trilhaN/`

## BLOCO A — `<head>` (anti-FOUC + manifesto + CSS). Copiar inteiro.

```html
<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="inema-course" content="ablacao">
  <title><<TITULO DA PAGINA>> | Auditoria de Ablação</title>
  <script>
  (function () {
    try {
      var html = document.documentElement;
      var DEF = { theme: 'inema-dark', font: 'inter', fontScale: 100, lineWidth: 68, leading: 1.7, accent: 'emerald' };
      function clone(o) { var r = {}; for (var x in o) r[x] = o[x]; return r; }
      var p = clone(DEF);
      try {
        var raw = localStorage.getItem('inema.prefs');
        if (raw) { var parsed = JSON.parse(raw); if (parsed && typeof parsed === 'object') { for (var k in DEF) if (parsed[k] != null) p[k] = parsed[k]; } }
        else { var legacy = localStorage.getItem('theme'); if (legacy === 'light') p.theme = 'claro'; else if (legacy === 'dark') p.theme = 'inema-dark'; }
      } catch (e) { p = clone(DEF); }
      var THEMES = { 'inema-dark': { dark: true, attr: null, cs: 'dark' }, 'claro': { dark: false, attr: null, cs: 'light' }, 'sepia': { dark: false, attr: 'sepia', cs: 'light' }, 'foco': { dark: null, attr: 'foco', cs: null }, 'contraste': { dark: true, attr: 'contraste', cs: 'dark' } };
      var t = THEMES[p.theme] || THEMES['inema-dark'];
      if (t.dark === true) html.classList.add('dark'); else if (t.dark === false) html.classList.remove('dark');
      if (t.attr) html.setAttribute('data-theme', t.attr); else html.removeAttribute('data-theme');
      html.style.colorScheme = (t.cs ? t.cs : (html.classList.contains('dark') ? 'dark' : 'light'));
      html.setAttribute('data-font', p.font || 'inter'); html.setAttribute('data-accent', p.accent || 'emerald');
      var s = html.style, scale = (+p.fontScale || 100);
      s.setProperty('--inema-font-scale', (scale / 100).toString()); s.setProperty('font-size', scale + '%');
      s.setProperty('--measure', (+p.lineWidth || 68) + 'ch'); s.setProperty('--lh-body', (+p.leading || 1.7).toString());
      var fam = p.font === 'system' ? 'system-ui, -apple-system, "Segoe UI", Roboto, sans-serif' : (p.font === 'leitura' ? '"Atkinson Hyperlegible", "Inter", system-ui, sans-serif' : '"Inter", system-ui, sans-serif');
      s.setProperty('--font-body', fam);
      var ACC = { emerald: [152,76,45], blue: [217,91,60], purple: [258,90,66], amber: [38,92,50], teal: [174,72,41], rose: [350,89,60] };
      var a = ACC[p.accent] || ACC.emerald;
      s.setProperty('--accent-h', a[0]+''); s.setProperty('--accent-s', a[1]+'%'); s.setProperty('--accent-l', a[2]+'%');
      s.setProperty('--accent', 'hsl(' + a[0] + ' ' + a[1] + '% ' + a[2] + '%)');
    } catch (err) { try { document.documentElement.classList.add('dark'); document.documentElement.style.colorScheme = 'dark'; } catch (e) {} }
  })();
  </script>
  <script type="application/json" data-inema-manifest>
  {
    "course": "ablacao",
    "tracks": [
      { "n": "1", "title": "Por que apagar", "modules": [
        { "id": "1-1", "title": "Configuracao envelhece", "topics": 6, "href": "curso/trilha1/modulo-1-1.html" },
        { "id": "1-2", "title": "O metodo de ablacao", "topics": 6, "href": "curso/trilha1/modulo-1-2.html" }
      ]},
      { "n": "2", "title": "Como diagnosticar", "modules": [
        { "id": "2-1", "title": "A taxonomia: o que cada linha e", "topics": 6, "href": "curso/trilha2/modulo-2-1.html" },
        { "id": "2-2", "title": "De microgerenciamento a criterio", "topics": 6, "href": "curso/trilha2/modulo-2-2.html" }
      ]},
      { "n": "3", "title": "Auditar de verdade", "modules": [
        { "id": "3-1", "title": "Rodando a skill audit-ablacao", "topics": 6, "href": "curso/trilha3/modulo-3-1.html" },
        { "id": "3-2", "title": "Do relatorio aos cortes", "topics": 6, "href": "curso/trilha3/modulo-3-2.html" }
      ]},
      { "n": "4", "title": "Provar e manter", "modules": [
        { "id": "4-1", "title": "O plano de ablacao A/B/C", "topics": 6, "href": "curso/trilha4/modulo-4-1.html" },
        { "id": "4-2", "title": "O ciclo continuo", "topics": 6, "href": "curso/trilha4/modulo-4-2.html" }
      ]}
    ]
  }
  </script>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>tailwind.config = { darkMode: 'class', theme: { extend: { colors: { primary: '#FACC15', dark: { 900: '#111827', 800: '#1f2937', 700: '#374151', 600: '#4b5563' } } } } }</script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="<<REL>>/assets/learn.css">
  <link rel="stylesheet" href="<<REL>>/assets/curso.css">
</head>
<body class="bg-dark-900 text-neutral-100 min-h-screen">

<a href="#conteudo" class="sr-only focus:not-sr-only focus:absolute focus:z-[60] focus:m-2 focus:px-4 focus:py-2 focus:rounded-lg focus:bg-dark-700">Pular para o conteudo</a>
```

## BLOCO B — `<nav>` global. Copiar inteiro; trocar só a classe da trilha ATIVA.

Trilha ativa = `text-<cor>-400 bg-<cor>-500/10` (sem hover). Trilhas inativas = `text-neutral-400 hover:text-<cor>-400 hover:bg-<cor>-500/10 transition-colors`.

`<<HREF_T1>>` etc.: dentro de `curso/trilhaN/` use `index.html` para a própria trilha e `../trilhaM/index.html` para as outras. Na landing use `curso/trilhaN/index.html`. `<<HREF_HOME>>` = `../../index.html` (páginas internas) ou `index.html` (landing).

```html
<nav class="sticky top-0 z-50 bg-dark-900/95 backdrop-blur-sm border-b border-dark-600">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex justify-between items-center h-14 gap-2">
      <div class="flex items-center space-x-3 flex-shrink-0">
        <a href="<<HREF_HOME>>" class="flex items-center space-x-2 text-yellow-400 hover:text-yellow-300">
          <span class="text-2xl">✂️</span><span class="font-bold text-lg hidden lg:inline">Auditoria de Ablação</span>
        </a>
        <span class="text-neutral-600 hidden sm:inline">|</span>
        <a href="https://inema.club" target="_blank" class="text-sky-400 hover:text-sky-300 text-sm font-medium hidden sm:inline">INEMA.CLUB</a>
        <span class="text-neutral-600 hidden sm:inline">-</span>
        <a href="https://inema.pro" target="_blank" class="text-amber-700 hover:text-amber-600 dark:text-slate-300 dark:hover:text-slate-200 text-sm font-medium hidden sm:inline">PRO</a>
      </div>
      <div class="flex items-center space-x-1">
        <a href="<<HREF_T1>>" class="px-2 sm:px-3 py-1.5 rounded-lg text-sm font-semibold text-neutral-400 hover:text-emerald-400 hover:bg-emerald-500/10 transition-colors"><span class="lg:hidden">T1</span><span class="hidden lg:inline">Por que apagar</span></a>
        <a href="<<HREF_T2>>" class="px-2 sm:px-3 py-1.5 rounded-lg text-sm font-semibold text-neutral-400 hover:text-blue-400 hover:bg-blue-500/10 transition-colors"><span class="lg:hidden">T2</span><span class="hidden lg:inline">Diagnosticar</span></a>
        <a href="<<HREF_T3>>" class="px-2 sm:px-3 py-1.5 rounded-lg text-sm font-semibold text-neutral-400 hover:text-purple-400 hover:bg-purple-500/10 transition-colors"><span class="lg:hidden">T3</span><span class="hidden lg:inline">Auditar</span></a>
        <a href="<<HREF_T4>>" class="px-2 sm:px-3 py-1.5 rounded-lg text-sm font-semibold text-neutral-400 hover:text-amber-400 hover:bg-amber-500/10 transition-colors"><span class="lg:hidden">T4</span><span class="hidden lg:inline">Provar e manter</span></a>
        <button type="button" data-inema-journey-open class="hidden sm:inline-flex items-center gap-1 px-2 py-1.5 rounded-lg text-sm text-neutral-400 hover:text-neutral-100 transition-colors" title="Minha jornada">
          <span aria-hidden="true">◷</span><span class="hidden xl:inline">Jornada</span><span class="inema-journey-badge hidden" aria-hidden="true"></span>
        </button>
        <button id="theme-toggle" class="p-2 rounded-lg bg-dark-700 hover:bg-dark-600 transition-colors ml-1" aria-label="Alternar tema claro/escuro">
          <svg id="theme-toggle-dark-icon" class="hidden w-5 h-5 text-neutral-300" fill="currentColor" viewBox="0 0 20 20"><path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z"></path></svg>
          <svg id="theme-toggle-light-icon" class="hidden w-5 h-5 text-neutral-300" fill="currentColor" viewBox="0 0 20 20"><path d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z" fill-rule="evenodd" clip-rule="evenodd"></path></svg>
        </button>
      </div>
    </div>
  </div>
</nav>
```

## BLOCO C — footer + scripts de fim de página. Copiar inteiro.

```html
<footer class="border-t border-dark-600 py-8 mt-16">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 text-center text-neutral-500 text-sm space-y-2">
    <p>Auditoria de Ablação — INEMA · 2026</p>
    <p>
      <a href="https://inema.club" target="_blank" class="text-sky-400 hover:text-sky-300">INEMA.CLUB</a>
      <span class="text-neutral-600">-</span>
      <a href="https://inema.pro" target="_blank" class="text-amber-700 hover:text-amber-600 dark:text-slate-300 dark:hover:text-slate-200">PRO</a>
    </p>
    <p class="text-xs">Método e visão: Boris Cherny (Anthropic) — palestra na Y Combinator sobre o corte de 80% do prompt de sistema do Claude Code.</p>
  </div>
</footer>

<script>
  function toggleTopic(button) {
    const topicItem = button.closest('.topic-item');
    const explanation = topicItem.querySelector('.topic-explanation');
    const moduleCard = button.closest('.bg-dark-800') || document;
    moduleCard.querySelectorAll('.topic-explanation.active').forEach(exp => { if (exp !== explanation) { exp.classList.remove('active'); const b = exp.closest('.topic-item').querySelector('button'); if (b) b.setAttribute('aria-expanded','false'); } });
    explanation.classList.toggle('active');
    button.setAttribute('aria-expanded', explanation.classList.contains('active') ? 'true' : 'false');
  }
  function openModal(modalId) { const m = document.getElementById(modalId); if (m) { m.classList.remove('hidden'); document.body.style.overflow = 'hidden'; } }
  function closeModal() { document.querySelectorAll('.modal').forEach(m => m.classList.add('hidden')); document.body.style.overflow = 'auto'; }
  document.addEventListener('keydown', e => { if (e.key === 'Escape') closeModal(); });
  const themeToggle = document.getElementById('theme-toggle');
  const themeToggleDarkIcon = document.getElementById('theme-toggle-dark-icon');
  const themeToggleLightIcon = document.getElementById('theme-toggle-light-icon');
  const htmlEl = document.documentElement;
  if (htmlEl.classList.contains('dark')) themeToggleLightIcon.classList.remove('hidden'); else themeToggleDarkIcon.classList.remove('hidden');
  themeToggle.addEventListener('click', () => {
    themeToggleDarkIcon.classList.toggle('hidden'); themeToggleLightIcon.classList.toggle('hidden');
    htmlEl.classList.toggle('dark');
    const dark = htmlEl.classList.contains('dark');
    localStorage.setItem('theme', dark ? 'dark' : 'light');
    try { const p = JSON.parse(localStorage.getItem('inema.prefs') || '{}'); p.theme = dark ? 'inema-dark' : 'claro'; localStorage.setItem('inema.prefs', JSON.stringify(p)); } catch (e) {}
  });
</script>
<script src="<<REL>>/assets/learn.js"></script>
<script>
  if (window.INEMA && typeof window.INEMA.init === 'function') { window.INEMA.init(); }
</script>
</body>
</html>
```

## Regras não-negociáveis (resumo dos Erros Críticos)

1. Botões `justify-start` (nunca `justify-center`).
2. Tópico = número em círculo, nunca seta `▶`.
3. Todo tópico expansível do index tem 3 seções: **O que é / Por que aprender / Conceitos-chave**.
4. INEMA.CLUB (`text-sky-400`) + PRO em toda página — já está no BLOCO B e C.
5. Light mode vem do `assets/curso.css` — não repetir inline.
6. Título de módulo no card do index: `text-2xl font-bold`.
7. Index de trilha: seção `Mapa da trilha` (nome exato) + `Conteúdo detalhado` como `<h2 class="text-2xl font-bold mb-6">` simples.
8. Cada card de módulo no index tem `id="modulo-X-Y"`, botão "Ver em Modal" e link "Ver Completo".
9. Módulo: 6 seções `<section id="topico-N" data-inema-topic="modulo-X-Y#topico-N">`, 500-800 linhas, variedade de componentes (≥2 grids ✓/✗, ≥1 timeline, ≥2 tip boxes, ≥1 code box).
10. ≥1 SVG inline `role="img"` + `aria-label` por módulo (hero SVG no index da trilha), cor da trilha + ciano `#38bdf8`, `class="w-full h-auto"`, glow `stdDeviation="1.8"` só na caixa-foco.
11. Botão "marcar como lido" ao fim de cada `<section>` (`data-inema-read-toggle`, `aria-pressed="false"`, `justify-start`).
12. `data-inema-block` nos parágrafos de prosa principais.
13. Sem emoji-decoração vazia: toda ilustração com legenda que ensina.
