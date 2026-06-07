<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>The Journey of PyTorch — Osama Shabih</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Syne:wght@400;600;700;800&family=JetBrains+Mono:wght@300;400;500;600&display=swap" rel="stylesheet"/>
<style>
  :root {
    --fire: #EE4C2C;
    --fire-dim: #c73d22;
    --fire-glow: rgba(238,76,44,0.25);
    --gold: #f5a623;
    --bg: #0a0a0b;
    --bg2: #111114;
    --bg3: #18181d;
    --surface: #1e1e25;
    --surface2: #252530;
    --border: rgba(238,76,44,0.2);
    --border-dim: rgba(255,255,255,0.06);
    --text: #f0ede8;
    --text-dim: #9994a0;
    --text-muted: #524f5a;
    --green: #4ade80;
    --blue: #60a5fa;
    --purple: #a78bfa;
    --cyan: #22d3ee;
    --yellow: #fbbf24;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    line-height: 1.7;
    overflow-x: hidden;
  }

  /* ── NOISE TEXTURE ── */
  body::before {
    content: '';
    position: fixed; inset: 0; z-index: 0; pointer-events: none;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    opacity: 0.5;
  }

  /* ── SCROLLBAR ── */
  ::-webkit-scrollbar { width: 4px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--fire); border-radius: 2px; }

  /* ── HERO ── */
  .hero {
    position: relative; min-height: 100vh;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    text-align: center; padding: 80px 24px 60px;
    overflow: hidden;
  }

  .hero-bg {
    position: absolute; inset: 0; z-index: 0;
    background:
      radial-gradient(ellipse 80% 60% at 50% 0%, rgba(238,76,44,0.12) 0%, transparent 60%),
      radial-gradient(ellipse 40% 40% at 80% 80%, rgba(245,166,35,0.06) 0%, transparent 50%),
      radial-gradient(ellipse 30% 30% at 20% 60%, rgba(96,165,250,0.05) 0%, transparent 50%);
  }

  .hero-grid {
    position: absolute; inset: 0; z-index: 0;
    background-image:
      linear-gradient(rgba(238,76,44,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(238,76,44,0.04) 1px, transparent 1px);
    background-size: 60px 60px;
    mask-image: radial-gradient(ellipse 70% 70% at 50% 50%, black 30%, transparent 100%);
  }

  .hero-content { position: relative; z-index: 1; max-width: 900px; }

  .flame-icon {
    font-size: 4rem; line-height: 1;
    animation: flicker 2s ease-in-out infinite alternate;
    display: block; margin-bottom: 24px;
  }

  @keyframes flicker {
    0%   { text-shadow: 0 0 20px #f97316, 0 0 60px #ef4444, 0 0 100px #dc2626; transform: scale(1); }
    100% { text-shadow: 0 0 30px #fb923c, 0 0 80px #f97316, 0 0 120px #ef4444; transform: scale(1.05); }
  }

  .hero-eyebrow {
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem; letter-spacing: 0.3em;
    color: var(--fire); text-transform: uppercase;
    margin-bottom: 16px; opacity: 0.9;
  }

  .hero-title {
    font-family: 'Syne', sans-serif;
    font-size: clamp(3rem, 8vw, 6.5rem);
    font-weight: 800; line-height: 0.95;
    letter-spacing: -0.02em;
    background: linear-gradient(135deg, #fff 30%, var(--fire) 70%, var(--gold) 100%);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 12px;
  }

  .hero-sub {
    font-family: 'Space Mono', monospace;
    font-size: 0.85rem; color: var(--text-dim);
    letter-spacing: 0.12em; text-transform: uppercase;
    margin-bottom: 32px;
  }

  .hero-quote {
    font-family: 'Syne', sans-serif;
    font-size: 1.15rem; color: var(--text-dim);
    font-weight: 400; font-style: italic;
    max-width: 580px; margin: 0 auto 40px;
    border-left: 3px solid var(--fire);
    padding-left: 20px; text-align: left;
  }

  /* BADGES */
  .badges { display: flex; flex-wrap: wrap; gap: 10px; justify-content: center; margin-bottom: 48px; }
  .badge {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.7rem; font-weight: 500;
    padding: 6px 14px; border-radius: 4px;
    border: 1px solid; letter-spacing: 0.05em;
    transition: all 0.2s;
  }
  .badge:hover { transform: translateY(-2px); }
  .badge-fire { background: rgba(238,76,44,0.1); border-color: rgba(238,76,44,0.4); color: #f97316; }
  .badge-blue { background: rgba(96,165,250,0.08); border-color: rgba(96,165,250,0.3); color: #60a5fa; }
  .badge-green { background: rgba(74,222,128,0.08); border-color: rgba(74,222,128,0.3); color: #4ade80; }
  .badge-yellow { background: rgba(251,191,36,0.08); border-color: rgba(251,191,36,0.3); color: #fbbf24; }

  /* AUTHOR CARD */
  .author-card {
    display: inline-flex; align-items: center; gap: 16px;
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 16px; padding: 16px 24px;
    backdrop-filter: blur(10px);
  }
  .author-avatar {
    width: 56px; height: 56px; border-radius: 50%;
    background: linear-gradient(135deg, var(--fire), var(--gold));
    display: flex; align-items: center; justify-content: center;
    font-size: 1.5rem; flex-shrink: 0;
    box-shadow: 0 0 20px var(--fire-glow);
  }
  .author-name { font-weight: 700; font-size: 1rem; color: var(--text); }
  .author-meta { font-family: 'Space Mono', monospace; font-size: 0.65rem; color: var(--text-dim); margin-top: 2px; }

  /* ── NAV ── */
  .sticky-nav {
    position: sticky; top: 0; z-index: 100;
    background: rgba(10,10,11,0.92); backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border-dim);
    padding: 0 24px;
    display: flex; align-items: center; gap: 0;
    overflow-x: auto; scrollbar-width: none;
  }
  .sticky-nav::-webkit-scrollbar { display: none; }
  .nav-brand {
    font-family: 'Space Mono', monospace;
    font-size: 0.75rem; color: var(--fire);
    font-weight: 700; letter-spacing: 0.1em;
    white-space: nowrap; padding: 14px 20px 14px 0;
    border-right: 1px solid var(--border-dim); margin-right: 16px;
    flex-shrink: 0;
  }
  .nav-link {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem; color: var(--text-muted);
    text-decoration: none; padding: 14px 14px;
    white-space: nowrap; transition: color 0.2s;
    letter-spacing: 0.05em; flex-shrink: 0;
  }
  .nav-link:hover { color: var(--fire); }

  /* ── TIMELINE EVOLUTION ── */
  .evolution {
    display: flex; align-items: center; justify-content: center;
    gap: 0; flex-wrap: wrap; margin: 48px auto;
    max-width: 700px; padding: 0 24px;
  }
  .evo-item {
    display: flex; flex-direction: column; align-items: center;
    text-align: center;
  }
  .evo-dot {
    width: 10px; height: 10px; border-radius: 50%;
    background: var(--fire);
    box-shadow: 0 0 12px var(--fire-glow);
    margin-bottom: 8px;
  }
  .evo-year {
    font-family: 'Space Mono', monospace;
    font-size: 0.6rem; color: var(--fire);
    margin-bottom: 4px;
  }
  .evo-name {
    font-size: 0.75rem; font-weight: 600; color: var(--text);
  }
  .evo-lang {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.6rem; color: var(--text-muted);
  }
  .evo-arrow {
    font-size: 1rem; color: var(--fire); padding: 0 16px;
    margin-bottom: 20px; opacity: 0.6;
    align-self: center;
  }

  /* ── MAIN LAYOUT ── */
  .container { max-width: 980px; margin: 0 auto; padding: 0 24px; position: relative; z-index: 1; }

  /* ── SECTION ── */
  .section { padding: 80px 0; border-top: 1px solid var(--border-dim); }

  .section-header { margin-bottom: 48px; }

  .section-num {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem; color: var(--fire);
    letter-spacing: 0.2em; text-transform: uppercase;
    margin-bottom: 8px;
  }

  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: clamp(1.8rem, 4vw, 2.8rem);
    font-weight: 800; line-height: 1.1;
    letter-spacing: -0.02em;
    color: var(--text);
  }

  .section-title span { color: var(--fire); }

  .section-desc {
    font-size: 1rem; color: var(--text-dim);
    margin-top: 12px; max-width: 600px;
  }

  /* ── CODE BLOCKS ── */
  .code-wrap {
    position: relative; margin: 24px 0;
    border-radius: 12px; overflow: hidden;
    border: 1px solid var(--border-dim);
    box-shadow: 0 8px 32px rgba(0,0,0,0.4);
  }

  .code-header {
    display: flex; align-items: center; justify-content: space-between;
    background: var(--surface); padding: 10px 16px;
    border-bottom: 1px solid var(--border-dim);
  }

  .code-dots { display: flex; gap: 6px; }
  .code-dot {
    width: 10px; height: 10px; border-radius: 50%;
  }
  .dot-red { background: #ff5f57; }
  .dot-yellow { background: #febc2e; }
  .dot-green { background: #28c840; }

  .code-label {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.65rem; color: var(--text-muted);
    letter-spacing: 0.08em;
  }

  .code-copy {
    font-family: 'Space Mono', monospace;
    font-size: 0.6rem; color: var(--text-muted);
    background: none; border: 1px solid var(--border-dim);
    padding: 3px 10px; border-radius: 4px; cursor: pointer;
    transition: all 0.2s;
  }
  .code-copy:hover { color: var(--fire); border-color: var(--fire); }

  pre {
    background: #0d0d10;
    padding: 24px;
    overflow-x: auto;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.8rem; line-height: 1.8;
    color: #cdd6f4;
  }

  .k { color: #cba6f7; }  /* keyword */
  .f { color: #89b4fa; }  /* function */
  .s { color: #a6e3a1; }  /* string */
  .n { color: #fab387; }  /* number */
  .c { color: #6c7086; font-style: italic; } /* comment */
  .p { color: #94e2d5; }  /* punctuation */
  .m { color: #f38ba8; }  /* method */
  .o { color: #89dceb; }  /* class/object */

  /* ── CARDS ── */
  .cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 20px; margin: 32px 0; }

  .card {
    background: var(--surface);
    border: 1px solid var(--border-dim);
    border-radius: 16px; padding: 28px;
    transition: all 0.3s; position: relative; overflow: hidden;
  }
  .card::before {
    content: ''; position: absolute;
    top: 0; left: 0; right: 0; height: 2px;
    background: linear-gradient(90deg, var(--fire), var(--gold));
    opacity: 0; transition: opacity 0.3s;
  }
  .card:hover { border-color: var(--border); transform: translateY(-4px); box-shadow: 0 16px 48px rgba(0,0,0,0.4); }
  .card:hover::before { opacity: 1; }

  .card-icon { font-size: 1.8rem; margin-bottom: 16px; }
  .card-title { font-size: 1rem; font-weight: 700; margin-bottom: 8px; color: var(--text); }
  .card-body { font-size: 0.85rem; color: var(--text-dim); line-height: 1.7; }

  /* ── TABLE ── */
  .table-wrap { overflow-x: auto; margin: 24px 0; border-radius: 12px; border: 1px solid var(--border-dim); }

  table { width: 100%; border-collapse: collapse; }
  thead { background: var(--surface); }
  th {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem; letter-spacing: 0.1em; text-transform: uppercase;
    color: var(--fire); padding: 14px 20px; text-align: left;
    border-bottom: 1px solid var(--border-dim);
  }
  td {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.78rem; color: var(--text-dim);
    padding: 12px 20px;
    border-bottom: 1px solid var(--border-dim);
  }
  tr:last-child td { border-bottom: none; }
  tr:hover td { background: rgba(238,76,44,0.04); color: var(--text); }
  td.center { text-align: center; }
  td .check { color: var(--green); }
  td .warn { color: var(--yellow); }
  td .star { color: var(--gold); }

  /* ── TOC ── */
  .toc {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px; padding: 32px;
    margin: 48px 0;
  }
  .toc-title {
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem; color: var(--fire);
    letter-spacing: 0.2em; text-transform: uppercase;
    margin-bottom: 24px;
  }
  .toc-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 8px; }
  .toc-item {
    display: flex; align-items: center; gap: 12px;
    padding: 10px 14px; border-radius: 8px;
    text-decoration: none;
    transition: all 0.2s;
    border: 1px solid transparent;
  }
  .toc-item:hover {
    background: rgba(238,76,44,0.08);
    border-color: var(--border);
  }
  .toc-num {
    font-family: 'Space Mono', monospace;
    font-size: 0.6rem; color: var(--fire);
    font-weight: 700; flex-shrink: 0;
  }
  .toc-text { font-size: 0.82rem; color: var(--text-dim); transition: color 0.2s; }
  .toc-item:hover .toc-text { color: var(--text); }
  .toc-icon { font-size: 0.9rem; flex-shrink: 0; }

  /* ── TIPS / CALLOUTS ── */
  .callout {
    border-radius: 10px; padding: 18px 20px;
    margin: 24px 0; display: flex; gap: 14px;
    align-items: flex-start;
    font-size: 0.85rem; line-height: 1.7;
  }
  .callout-tip { background: rgba(74,222,128,0.07); border: 1px solid rgba(74,222,128,0.2); }
  .callout-warn { background: rgba(251,191,36,0.07); border: 1px solid rgba(251,191,36,0.2); }
  .callout-fire { background: rgba(238,76,44,0.07); border: 1px solid rgba(238,76,44,0.25); }
  .callout-icon { font-size: 1.1rem; flex-shrink: 0; margin-top: 1px; }
  .callout-text { color: var(--text-dim); }
  .callout-text strong { color: var(--text); }

  /* ── ACCORDION / DETAILS ── */
  details { margin: 16px 0; }
  details > summary {
    font-family: 'Space Mono', monospace;
    font-size: 0.78rem; cursor: pointer;
    padding: 14px 18px;
    background: var(--surface); border: 1px solid var(--border-dim);
    border-radius: 10px; color: var(--text);
    list-style: none; display: flex; align-items: center; gap: 10px;
    transition: all 0.2s; letter-spacing: 0.04em;
  }
  details > summary::-webkit-details-marker { display: none; }
  details > summary::before { content: '▶'; color: var(--fire); font-size: 0.6rem; transition: transform 0.2s; }
  details[open] > summary::before { transform: rotate(90deg); }
  details > summary:hover { border-color: var(--fire); color: var(--fire); }
  details[open] > summary { border-radius: 10px 10px 0 0; border-color: var(--border); }
  details > .detail-body {
    border: 1px solid var(--border-dim); border-top: none;
    border-radius: 0 0 10px 10px; overflow: hidden;
  }
  details > .detail-body pre { border-radius: 0; }

  /* ── CHECKLIST ── */
  .checklist { list-style: none; margin: 24px 0; display: grid; grid-template-columns: repeat(auto-fill, minmax(340px, 1fr)); gap: 10px; }
  .checklist li {
    display: flex; align-items: flex-start; gap: 12px;
    padding: 12px 16px;
    background: var(--surface); border: 1px solid var(--border-dim);
    border-radius: 8px; font-size: 0.82rem;
    font-family: 'JetBrains Mono', monospace; color: var(--text-dim);
    transition: all 0.2s;
  }
  .checklist li:hover { border-color: var(--border); color: var(--text); }
  .checklist li::before { content: '□'; color: var(--fire); flex-shrink: 0; }

  /* ── LAYER TABLE ── */
  .layer-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 12px; margin: 24px 0; }
  .layer-card {
    background: var(--surface); border: 1px solid var(--border-dim);
    border-radius: 10px; padding: 16px;
    transition: all 0.2s;
  }
  .layer-card:hover { border-color: var(--border); transform: translateY(-2px); }
  .layer-name {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.75rem; color: var(--fire);
    margin-bottom: 6px; font-weight: 600;
  }
  .layer-desc { font-size: 0.78rem; color: var(--text-dim); }

  /* ── RESOURCES ── */
  .resources { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 16px; margin: 32px 0; }
  .resource-link {
    display: flex; align-items: center; gap: 12px;
    padding: 16px 20px;
    background: var(--surface); border: 1px solid var(--border-dim);
    border-radius: 12px; text-decoration: none;
    transition: all 0.3s; color: var(--text-dim);
  }
  .resource-link:hover { border-color: var(--fire); color: var(--text); transform: translateY(-3px); box-shadow: 0 8px 24px rgba(238,76,44,0.15); }
  .resource-icon { font-size: 1.3rem; flex-shrink: 0; }
  .resource-text { font-size: 0.82rem; font-weight: 600; }
  .resource-url { font-family: 'Space Mono', monospace; font-size: 0.6rem; color: var(--text-muted); margin-top: 2px; }

  /* ── FOOTER ── */
  footer {
    text-align: center; padding: 80px 24px;
    border-top: 1px solid var(--border-dim);
    position: relative;
  }
  .footer-ascii {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.55rem; line-height: 1.4;
    color: var(--fire); opacity: 0.4;
    margin-bottom: 32px; letter-spacing: 0.05em;
  }
  .footer-text { font-size: 0.85rem; color: var(--text-dim); }
  .footer-text a { color: var(--fire); text-decoration: none; }
  .footer-heart { color: var(--fire); animation: pulse 1.5s ease-in-out infinite; display: inline-block; }
  @keyframes pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.2); } }

  /* ── FADE-IN ANIMATION ── */
  .fade-in { opacity: 0; transform: translateY(24px); transition: opacity 0.6s ease, transform 0.6s ease; }
  .fade-in.visible { opacity: 1; transform: translateY(0); }

  /* ── INLINE CODE ── */
  code {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.78em;
    background: rgba(238,76,44,0.12);
    color: #f97316;
    padding: 2px 7px; border-radius: 4px;
  }

  /* ── DIVIDER ── */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--fire) 40%, var(--gold) 60%, transparent);
    opacity: 0.3; margin: 0;
  }

  /* ── PERFORMANCE CHIPS ── */
  .chips { display: flex; flex-wrap: wrap; gap: 10px; margin: 20px 0; }
  .chip {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem; padding: 6px 14px;
    border-radius: 100px; border: 1px solid;
    letter-spacing: 0.05em; font-weight: 700;
  }
  .chip-fire { border-color: rgba(238,76,44,0.5); color: #f97316; background: rgba(238,76,44,0.08); }
  .chip-gold { border-color: rgba(245,166,35,0.5); color: #f5a623; background: rgba(245,166,35,0.08); }
  .chip-blue { border-color: rgba(96,165,250,0.5); color: #60a5fa; background: rgba(96,165,250,0.08); }
  .chip-green { border-color: rgba(74,222,128,0.5); color: #4ade80; background: rgba(74,222,128,0.08); }

  @media (max-width: 600px) {
    .hero-title { font-size: 2.8rem; }
    .checklist { grid-template-columns: 1fr; }
    .toc-grid { grid-template-columns: 1fr; }
    .evolution { gap: 4px; }
    .evo-arrow { padding: 0 8px; font-size: 0.8rem; }
  }
</style>
</head>
<body>

<!-- ══════════════════════════════════
  HERO
══════════════════════════════════ -->
<section class="hero">
  <div class="hero-bg"></div>
  <div class="hero-grid"></div>
  <div class="hero-content">
    <span class="flame-icon">🔥</span>
    <p class="hero-eyebrow">Meta AI · Deep Learning Framework · 2016 – Present</p>
    <h1 class="hero-title">The Journey<br/>of PyTorch</h1>
    <p class="hero-sub">From Tensors to Production — An In-Depth Overview</p>

    <blockquote class="hero-quote">
      "The best way to understand deep learning is to build it — one tensor at a time."
    </blockquote>

    <div class="badges">
      <span class="badge badge-fire">PyTorch 2.x</span>
      <span class="badge badge-blue">Python 3.8+</span>
      <span class="badge badge-green">CUDA 11.8+</span>
      <span class="badge badge-yellow">MIT License</span>
      <span class="badge badge-fire">Status: Active</span>
    </div>

    <div class="author-card">
      <div class="author-avatar">👨‍💻</div>
      <div>
        <div class="author-name">Osama Shabih</div>
        <div class="author-meta">🎓 Jamia Hamdard University &nbsp;·&nbsp; 🔬 Deep Learning Researcher &nbsp;·&nbsp; 🔥 PyTorch Enthusiast</div>
      </div>
    </div>
  </div>
</section>

<!-- ══════════════════════════════════
  NAV
══════════════════════════════════ -->
<nav class="sticky-nav">
  <div class="nav-brand">🔥 PyTorch</div>
  <a class="nav-link" href="#intro">Intro</a>
  <a class="nav-link" href="#install">Install</a>
  <a class="nav-link" href="#tensors">Tensors</a>
  <a class="nav-link" href="#autograd">Autograd</a>
  <a class="nav-link" href="#nn">Neural Nets</a>
  <a class="nav-link" href="#loss">Loss</a>
  <a class="nav-link" href="#optimizers">Optimizers</a>
  <a class="nav-link" href="#loop">Train Loop</a>
  <a class="nav-link" href="#data">Datasets</a>
  <a class="nav-link" href="#arch">Architectures</a>
  <a class="nav-link" href="#transfer">Transfer</a>
  <a class="nav-link" href="#save">Save/Load</a>
  <a class="nav-link" href="#gpu">GPU</a>
  <a class="nav-link" href="#deploy">Deploy</a>
  <a class="nav-link" href="#tips">Tips</a>
</nav>

<!-- ══════════════════════════════════
  MAIN
══════════════════════════════════ -->
<main>
<div class="container">

<!-- TOC -->
<div class="toc fade-in">
  <p class="toc-title">// Table of Contents</p>
  <div class="toc-grid">
    <a class="toc-item" href="#intro"><span class="toc-num">01</span><span class="toc-icon">🌟</span><span class="toc-text">Introduction</span></a>
    <a class="toc-item" href="#install"><span class="toc-num">02</span><span class="toc-icon">⚙️</span><span class="toc-text">Installation & Setup</span></a>
    <a class="toc-item" href="#tensors"><span class="toc-num">03</span><span class="toc-icon">🧱</span><span class="toc-text">Tensors — The Core</span></a>
    <a class="toc-item" href="#autograd"><span class="toc-num">04</span><span class="toc-icon">🔁</span><span class="toc-text">Autograd & Graphs</span></a>
    <a class="toc-item" href="#nn"><span class="toc-num">05</span><span class="toc-icon">🏗️</span><span class="toc-text">Neural Networks</span></a>
    <a class="toc-item" href="#loss"><span class="toc-num">06</span><span class="toc-icon">📉</span><span class="toc-text">Loss Functions</span></a>
    <a class="toc-item" href="#optimizers"><span class="toc-num">07</span><span class="toc-icon">⚡</span><span class="toc-text">Optimizers</span></a>
    <a class="toc-item" href="#loop"><span class="toc-num">08</span><span class="toc-icon">🔄</span><span class="toc-text">Training Loop</span></a>
    <a class="toc-item" href="#data"><span class="toc-num">09</span><span class="toc-icon">📦</span><span class="toc-text">Datasets & DataLoaders</span></a>
    <a class="toc-item" href="#arch"><span class="toc-num">10</span><span class="toc-icon">🧠</span><span class="toc-text">CNN · RNN · Transformer</span></a>
    <a class="toc-item" href="#transfer"><span class="toc-num">11</span><span class="toc-icon">🔀</span><span class="toc-text">Transfer Learning</span></a>
    <a class="toc-item" href="#save"><span class="toc-num">12</span><span class="toc-icon">💾</span><span class="toc-text">Save & Load</span></a>
    <a class="toc-item" href="#gpu"><span class="toc-num">13</span><span class="toc-icon">🚀</span><span class="toc-text">GPU Acceleration</span></a>
    <a class="toc-item" href="#deploy"><span class="toc-num">14</span><span class="toc-icon">📦</span><span class="toc-text">Deployment</span></a>
    <a class="toc-item" href="#tips"><span class="toc-num">15</span><span class="toc-icon">✅</span><span class="toc-text">Best Practices</span></a>
  </div>
</div>

<!-- ── 01 INTRO ── -->
<section id="intro" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 01</p>
    <h2 class="section-title">Introduction to <span>PyTorch</span></h2>
    <p class="section-desc">An open-source deep learning framework by Meta AI (FAIR), powering everything from cutting-edge research to production AI at massive scale.</p>
  </div>

  <div class="evolution">
    <div class="evo-item">
      <div class="evo-dot"></div>
      <div class="evo-year">2002</div>
      <div class="evo-name">Torch</div>
      <div class="evo-lang">C</div>
    </div>
    <div class="evo-arrow">→</div>
    <div class="evo-item">
      <div class="evo-dot"></div>
      <div class="evo-year">2011</div>
      <div class="evo-name">Torch7</div>
      <div class="evo-lang">Lua</div>
    </div>
    <div class="evo-arrow">→</div>
    <div class="evo-item">
      <div class="evo-dot"></div>
      <div class="evo-year">2016</div>
      <div class="evo-name">PyTorch</div>
      <div class="evo-lang">Python</div>
    </div>
    <div class="evo-arrow">→</div>
    <div class="evo-item">
      <div class="evo-dot" style="background:var(--gold);box-shadow:0 0 12px rgba(245,166,35,0.5)"></div>
      <div class="evo-year" style="color:var(--gold)">2023</div>
      <div class="evo-name">PyTorch 2.x</div>
      <div class="evo-lang">torch.compile</div>
    </div>
  </div>

  <div class="cards">
    <div class="card">
      <div class="card-icon">✅</div>
      <div class="card-title">Why PyTorch?</div>
      <div class="card-body">Dynamic computation graphs (define-by-run). Fully Pythonic — debug with <code>print()</code> and <code>pdb</code>. Dominant in academic research (~75% of papers). Production-ready with TorchScript + ONNX + <code>torch.compile</code>.</div>
    </div>
    <div class="card">
      <div class="card-icon">⚡</div>
      <div class="card-title">PyTorch 2.x Highlights</div>
      <div class="card-body"><code>torch.compile()</code> delivers up to 2× speedup. Kernel fusion via TorchInductor. FlexAttention for custom attention patterns. Fully backward-compatible with all 1.x code.</div>
    </div>
  </div>

  <div class="table-wrap">
    <table>
      <thead><tr><th>Feature</th><th>PyTorch</th><th>TensorFlow</th></tr></thead>
      <tbody>
        <tr><td>Graph Type</td><td>Dynamic (eager)</td><td>Static + Eager</td></tr>
        <tr><td>Debugging</td><td class="center"><span class="check">✅ Pythonic</span></td><td class="center"><span class="warn">⚠️ Complex</span></td></tr>
        <tr><td>Research Adoption</td><td class="center"><span class="star">⭐ Dominant</span></td><td class="center">Popular</td></tr>
        <tr><td>Mobile/Edge</td><td>TorchMobile</td><td>TFLite</td></tr>
        <tr><td>torch.compile</td><td class="center"><span class="check">✅ 2.x</span></td><td class="center">✅ XLA</td></tr>
      </tbody>
    </table>
  </div>
</section>

<!-- ── 02 INSTALL ── -->
<section id="install" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 02</p>
    <h2 class="section-title">Installation & <span>Setup</span></h2>
  </div>

  <details open>
    <summary>📦 pip (recommended)</summary>
    <div class="detail-body">
<pre><span class="c"># CPU only</span>
pip install torch torchvision torchaudio

<span class="c"># CUDA 11.8</span>
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

<span class="c"># CUDA 12.1</span>
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121</pre>
    </div>
  </details>

  <details>
    <summary>🐍 conda</summary>
    <div class="detail-body">
<pre><span class="c"># CPU</span>
conda install pytorch torchvision torchaudio cpuonly -c pytorch

<span class="c"># CUDA 12.1</span>
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia</pre>
    </div>
  </details>

  <details>
    <summary>☁️ Google Colab</summary>
    <div class="detail-body">
<pre><span class="c"># PyTorch is pre-installed — just verify:</span>
<span class="k">import</span> torch
<span class="f">print</span>(torch.__version__)           <span class="c"># e.g. 2.3.0+cu121</span>
<span class="f">print</span>(torch.cuda.<span class="m">is_available</span>())   <span class="c"># True if GPU runtime</span></pre>
    </div>
  </details>
</section>

<!-- ── 03 TENSORS ── -->
<section id="tensors" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 03</p>
    <h2 class="section-title">Tensors — <span>The Core</span></h2>
    <p class="section-desc">Multi-dimensional arrays that can live on CPU or GPU. Everything in PyTorch is a tensor.</p>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">tensors.py</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">import</span> torch

<span class="c"># ── Creation ──────────────────────────────────────────────────────</span>
x   = torch.<span class="f">tensor</span>([[<span class="n">1.0</span>, <span class="n">2.0</span>], [<span class="n">3.0</span>, <span class="n">4.0</span>]])   <span class="c"># from list</span>
z   = torch.<span class="f">zeros</span>(<span class="n">3</span>, <span class="n">4</span>)                          <span class="c"># all zeros</span>
o   = torch.<span class="f">ones</span>(<span class="n">3</span>, <span class="n">4</span>)                           <span class="c"># all ones</span>
r   = torch.<span class="f">rand</span>(<span class="n">3</span>, <span class="n">4</span>)                           <span class="c"># uniform [0,1)</span>
rn  = torch.<span class="f">randn</span>(<span class="n">3</span>, <span class="n">4</span>)                          <span class="c"># N(0,1) normal</span>
ar  = torch.<span class="f">arange</span>(<span class="n">0</span>, <span class="n">10</span>, <span class="n">2</span>)                    <span class="c"># [0, 2, 4, 6, 8]</span>
ls  = torch.<span class="f">linspace</span>(<span class="n">0</span>, <span class="n">1</span>, <span class="n">5</span>)                   <span class="c"># [0.0, 0.25 ... 1.0]</span>
eye = torch.<span class="f">eye</span>(<span class="n">3</span>)                               <span class="c"># 3×3 identity</span>

<span class="c"># ── Properties ────────────────────────────────────────────────────</span>
<span class="f">print</span>(x.shape)     <span class="c"># torch.Size([2, 2])</span>
<span class="f">print</span>(x.dtype)     <span class="c"># torch.float32</span>
<span class="f">print</span>(x.device)    <span class="c"># cpu</span>
<span class="f">print</span>(x.ndim)      <span class="c"># 2</span>

<span class="c"># ── Operations ────────────────────────────────────────────────────</span>
a = torch.<span class="f">randn</span>(<span class="n">2</span>, <span class="n">3</span>)
b = torch.<span class="f">randn</span>(<span class="n">3</span>, <span class="n">4</span>)

c = a @ b                        <span class="c"># matmul → (2, 4)</span>
d = a + <span class="n">1</span>                        <span class="c"># broadcast scalar</span>
e = torch.<span class="f">cat</span>([a, a], dim=<span class="n">0</span>)     <span class="c"># concat rows → (4, 3)</span>
f = torch.<span class="f">stack</span>([a, a], dim=<span class="n">0</span>)   <span class="c"># new dim    → (2, 2, 3)</span>

<span class="c"># ── Reshape & View ────────────────────────────────────────────────</span>
t  = torch.<span class="f">arange</span>(<span class="n">12</span>)
t2 = t.<span class="m">view</span>(<span class="n">3</span>, <span class="n">4</span>)           <span class="c"># same memory, new shape</span>
t3 = t.<span class="m">reshape</span>(<span class="n">2</span>, <span class="n">6</span>)        <span class="c"># may copy if non-contiguous</span>
t4 = t2.<span class="m">permute</span>(<span class="n">1</span>, <span class="n">0</span>)       <span class="c"># transpose → (4, 3)</span>
t5 = t2.<span class="m">unsqueeze</span>(<span class="n">0</span>)        <span class="c"># add batch dim → (1, 3, 4)</span></pre>
  </div>

  <div class="callout callout-tip">
    <span class="callout-icon">💡</span>
    <span class="callout-text"><strong>Tip by Osama:</strong> Always check <code>.shape</code> after every operation. 90% of PyTorch bugs are shape mismatches — especially the batch dimension!</span>
  </div>
</section>

<!-- ── 04 AUTOGRAD ── -->
<section id="autograd" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 04</p>
    <h2 class="section-title">Autograd & <span>Computational Graphs</span></h2>
    <p class="section-desc">PyTorch builds a dynamic computation graph on every forward pass and computes gradients via reverse-mode autodiff.</p>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">autograd.py</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">import</span> torch

<span class="c"># ── Basic gradient ────────────────────────────────────────────────</span>
x = torch.<span class="f">tensor</span>([<span class="n">2.0</span>], requires_grad=<span class="k">True</span>)
y = x ** <span class="n">3</span> + <span class="n">2</span> * x          <span class="c"># y = x³ + 2x</span>

y.<span class="m">backward</span>()                 <span class="c"># compute dy/dx</span>
<span class="f">print</span>(x.grad)                <span class="c"># tensor([14.])  →  dy/dx = 3x² + 2 = 14</span>

<span class="c"># ── No-grad context (inference / eval) ───────────────────────────</span>
<span class="k">with</span> torch.<span class="f">no_grad</span>():
    y_eval = x ** <span class="n">2</span>          <span class="c"># graph NOT built → faster + less VRAM</span>

<span class="c"># ── CRITICAL — zero grads every iteration ─────────────────────────</span>
optimizer.<span class="m">zero_grad</span>()        <span class="c"># without this, gradients ACCUMULATE!</span></pre>
  </div>

  <div class="callout callout-warn">
    <span class="callout-icon">⚠️</span>
    <span class="callout-text"><strong>Critical:</strong> Always call <code>optimizer.zero_grad()</code> at the start of every training iteration. Without this, gradients accumulate across batches and your model will diverge.</span>
  </div>
</section>

<!-- ── 05 NEURAL NETWORKS ── -->
<section id="nn" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 05</p>
    <h2 class="section-title">Building <span>Neural Networks</span></h2>
  </div>

  <div class="chips">
    <span class="chip chip-fire">nn.Sequential</span>
    <span class="chip chip-gold">nn.Module</span>
    <span class="chip chip-blue">BatchNorm</span>
    <span class="chip chip-green">Dropout</span>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">model.py — nn.Module (recommended)</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">import</span> torch.nn <span class="k">as</span> nn
<span class="k">import</span> torch.nn.functional <span class="k">as</span> F

<span class="k">class</span> <span class="o">DeepNet</span>(nn.Module):
    <span class="k">def</span> <span class="f">__init__</span>(self, input_dim, hidden_dim, output_dim, dropout=<span class="n">0.4</span>):
        <span class="f">super</span>().__init__()
        self.fc1  = nn.<span class="f">Linear</span>(input_dim, hidden_dim)
        self.bn1  = nn.<span class="f">BatchNorm1d</span>(hidden_dim)
        self.fc2  = nn.<span class="f">Linear</span>(hidden_dim, hidden_dim // <span class="n">2</span>)
        self.bn2  = nn.<span class="f">BatchNorm1d</span>(hidden_dim // <span class="n">2</span>)
        self.fc3  = nn.<span class="f">Linear</span>(hidden_dim // <span class="n">2</span>, output_dim)
        self.drop = nn.<span class="f">Dropout</span>(p=dropout)

    <span class="k">def</span> <span class="f">forward</span>(self, x):
        x = self.drop(F.<span class="f">relu</span>(self.bn1(self.fc1(x))))
        x = self.drop(F.<span class="f">relu</span>(self.bn2(self.fc2(x))))
        <span class="k">return</span> self.fc3(x)    <span class="c"># raw logits — no softmax here</span>

model = <span class="o">DeepNet</span>(<span class="n">784</span>, <span class="n">256</span>, <span class="n">10</span>)
trainable = <span class="f">sum</span>(p.<span class="m">numel</span>() <span class="k">for</span> p <span class="k">in</span> model.<span class="m">parameters</span>() <span class="k">if</span> p.requires_grad)
<span class="f">print</span>(<span class="s">f"Trainable params: {trainable:,}"</span>)</pre>
  </div>

  <p style="font-size:0.9rem;color:var(--text-dim);margin:24px 0 16px;font-weight:600;">📌 Common Layers Reference</p>
  <div class="layer-grid">
    <div class="layer-card"><div class="layer-name">nn.Linear(in, out)</div><div class="layer-desc">Fully connected / dense layer</div></div>
    <div class="layer-card"><div class="layer-name">nn.Conv2d(in, out, k)</div><div class="layer-desc">2D spatial convolution</div></div>
    <div class="layer-card"><div class="layer-name">nn.BatchNorm1d/2d</div><div class="layer-desc">Batch normalisation</div></div>
    <div class="layer-card"><div class="layer-name">nn.LayerNorm(dim)</div><div class="layer-desc">Layer normalisation</div></div>
    <div class="layer-card"><div class="layer-name">nn.Dropout(p)</div><div class="layer-desc">Regularisation via random zeroing</div></div>
    <div class="layer-card"><div class="layer-name">nn.Embedding(vocab, dim)</div><div class="layer-desc">Token / word embeddings</div></div>
    <div class="layer-card"><div class="layer-name">nn.LSTM(in, hidden)</div><div class="layer-desc">Recurrent sequence layer</div></div>
    <div class="layer-card"><div class="layer-name">nn.MultiheadAttention</div><div class="layer-desc">Self-attention mechanism</div></div>
    <div class="layer-card"><div class="layer-name">nn.TransformerEncoderLayer</div><div class="layer-desc">Full transformer block</div></div>
  </div>
</section>

<!-- ── 06 LOSS ── -->
<section id="loss" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 06</p>
    <h2 class="section-title">Loss <span>Functions</span></h2>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">losses.py</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">import</span> torch.nn <span class="k">as</span> nn

<span class="c"># Multi-class classification  (input = raw logits, NOT softmax)</span>
criterion = nn.<span class="f">CrossEntropyLoss</span>(label_smoothing=<span class="n">0.1</span>)

<span class="c"># Binary classification  (sigmoid applied internally)</span>
criterion = nn.<span class="f">BCEWithLogitsLoss</span>(pos_weight=torch.<span class="f">tensor</span>([<span class="n">2.0</span>]))

<span class="c"># Regression</span>
criterion = nn.<span class="f">MSELoss</span>()        <span class="c"># L2 — sensitive to outliers</span>
criterion = nn.<span class="f">L1Loss</span>()         <span class="c"># MAE — robust to outliers</span>
criterion = nn.<span class="f">SmoothL1Loss</span>()   <span class="c"># Huber — best of both</span>

<span class="c"># Metric learning</span>
criterion = nn.<span class="f">TripletMarginLoss</span>(margin=<span class="n">1.0</span>)

<span class="c"># Usage</span>
logits = model(x_batch)                <span class="c"># (B, num_classes)</span>
loss   = criterion(logits, y_batch)    <span class="c"># y_batch = class indices (Long)</span>
<span class="f">print</span>(<span class="s">f"Loss: {loss.item():.4f}"</span>)</pre>
  </div>
</section>

<!-- ── 07 OPTIMIZERS ── -->
<section id="optimizers" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 07</p>
    <h2 class="section-title"><span>Optimizers</span> & Schedulers</h2>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">optimizers.py</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">import</span> torch.optim <span class="k">as</span> optim

<span class="c"># SGD + Nesterov momentum — great for CNNs</span>
opt = optim.<span class="f">SGD</span>(model.<span class="m">parameters</span>(), lr=<span class="n">0.01</span>,
                momentum=<span class="n">0.9</span>, weight_decay=<span class="n">1e-4</span>, nesterov=<span class="k">True</span>)

<span class="c"># Adam — robust default for most tasks</span>
opt = optim.<span class="f">Adam</span>(model.<span class="m">parameters</span>(), lr=<span class="n">1e-3</span>, betas=(<span class="n">0.9</span>, <span class="n">0.999</span>))

<span class="c"># AdamW — decoupled weight decay (recommended for Transformers) ⭐</span>
opt = optim.<span class="f">AdamW</span>(model.<span class="m">parameters</span>(), lr=<span class="n">1e-3</span>, weight_decay=<span class="n">0.01</span>)

<span class="c"># ── Schedulers ────────────────────────────────────────────────────</span>

<span class="c"># Cosine annealing — smooth decay (most popular)</span>
scheduler = optim.lr_scheduler.<span class="f">CosineAnnealingLR</span>(opt, T_max=<span class="n">50</span>)

<span class="c"># OneCycle — warmup + cosine decay in one shot (fastest convergence)</span>
scheduler = optim.lr_scheduler.<span class="f">OneCycleLR</span>(
    opt, max_lr=<span class="n">1e-2</span>,
    steps_per_epoch=<span class="f">len</span>(train_loader), epochs=<span class="n">30</span>
)

<span class="c"># Reduce on plateau — adaptive decay when val loss stagnates</span>
scheduler = optim.lr_scheduler.<span class="f">ReduceLROnPlateau</span>(
    opt, mode=<span class="s">'min'</span>, factor=<span class="n">0.5</span>, patience=<span class="n">5</span>, verbose=<span class="k">True</span>
)</pre>
  </div>
</section>

<!-- ── 08 TRAINING LOOP ── -->
<section id="loop" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 08</p>
    <h2 class="section-title">The Complete <span>Training Loop</span></h2>
  </div>

  <div class="callout callout-warn">
    <span class="callout-icon">⚠️</span>
    <span class="callout-text"><strong>Critical:</strong> Call <code>model.train()</code> before training and <code>model.eval()</code> before validation — this controls BatchNorm and Dropout behaviour.</span>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">training_loop.py</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">def</span> <span class="f">train_one_epoch</span>(model, loader, criterion, optimizer, device, scaler=<span class="k">None</span>):
    model.<span class="m">train</span>()
    total_loss, correct, total = <span class="n">0.0</span>, <span class="n">0</span>, <span class="n">0</span>

    <span class="k">for</span> X, y <span class="k">in</span> loader:
        X, y = X.<span class="m">to</span>(device), y.<span class="m">to</span>(device)

        optimizer.<span class="m">zero_grad</span>()                              <span class="c"># 1️⃣  Clear gradients</span>

        <span class="k">with</span> autocast():
            logits = model(X)                               <span class="c"># 2️⃣  Forward pass</span>
            loss   = criterion(logits, y)                   <span class="c"># 3️⃣  Compute loss</span>
        scaler.<span class="m">scale</span>(loss).<span class="m">backward</span>()                      <span class="c"># 4️⃣  Backprop (scaled)</span>
        scaler.<span class="m">unscale_</span>(optimizer)
        torch.nn.utils.<span class="f">clip_grad_norm_</span>(model.<span class="m">parameters</span>(), <span class="n">1.0</span>)
        scaler.<span class="m">step</span>(optimizer)                             <span class="c"># 5️⃣  Update weights</span>
        scaler.<span class="m">update</span>()

        total_loss += loss.<span class="m">item</span>() * X.<span class="m">size</span>(<span class="n">0</span>)
        correct    += (logits.<span class="m">argmax</span>(<span class="n">1</span>) == y).<span class="m">sum</span>().<span class="m">item</span>()
        total      += X.<span class="m">size</span>(<span class="n">0</span>)

    <span class="k">return</span> total_loss / total, correct / total


<span class="c"># ── Main Loop ─────────────────────────────────────────────────────</span>
<span class="k">for</span> epoch <span class="k">in</span> <span class="f">range</span>(<span class="n">1</span>, <span class="n">51</span>):
    tr_loss, tr_acc = <span class="f">train_one_epoch</span>(model, train_loader, criterion, optimizer, device, scaler)
    va_loss, va_acc = <span class="f">evaluate</span>(model, val_loader, criterion, device)
    scheduler.<span class="m">step</span>()

    <span class="f">print</span>(<span class="s">f"Epoch [{epoch:02d}/50]  Train → loss: {tr_loss:.4f}  acc: {tr_acc:.3f}"</span>)

    <span class="k">if</span> va_acc > best_val_acc:
        best_val_acc = va_acc
        torch.<span class="f">save</span>(model.<span class="m">state_dict</span>(), <span class="s">"best_model.pth"</span>)
        <span class="f">print</span>(<span class="s">f"  ✅ New best saved  (val_acc = {best_val_acc:.4f})"</span>)</pre>
  </div>
</section>

<!-- ── 09 DATASETS ── -->
<section id="data" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 09</p>
    <h2 class="section-title">Datasets & <span>DataLoaders</span></h2>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">dataset.py</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">from</span> torch.utils.data <span class="k">import</span> Dataset, DataLoader

<span class="k">class</span> <span class="o">TabularDataset</span>(Dataset):
    <span class="k">def</span> <span class="f">__init__</span>(self, X, y):
        self.X = torch.<span class="f">tensor</span>(X, dtype=torch.float32)
        self.y = torch.<span class="f">tensor</span>(y, dtype=torch.long)

    <span class="k">def</span> <span class="f">__len__</span>(self):           <span class="k">return</span> <span class="f">len</span>(self.X)
    <span class="k">def</span> <span class="f">__getitem__</span>(self, idx):  <span class="k">return</span> self.X[idx], self.y[idx]

<span class="c"># ── DataLoader (Optimal Settings) ─────────────────────────────────</span>
train_loader = <span class="f">DataLoader</span>(
    train_ds,
    batch_size=<span class="n">64</span>,
    shuffle=<span class="k">True</span>,
    num_workers=<span class="n">4</span>,          <span class="c"># parallel CPU loading</span>
    pin_memory=<span class="k">True</span>,        <span class="c"># faster CPU→GPU transfer</span>
    persistent_workers=<span class="k">True</span> <span class="c"># keep workers alive between epochs</span>
)</pre>
  </div>
</section>

<!-- ── 10 ARCHITECTURES ── -->
<section id="arch" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 10</p>
    <h2 class="section-title">CNN · RNN · <span>Transformers</span></h2>
  </div>

  <details open>
    <summary>🖼️ CNN — Image Classification</summary>
    <div class="detail-body">
<pre><span class="k">class</span> <span class="o">ConvNet</span>(nn.Module):
    <span class="k">def</span> <span class="f">__init__</span>(self, num_classes=<span class="n">10</span>):
        <span class="f">super</span>().__init__()
        self.features = nn.<span class="f">Sequential</span>(
            nn.<span class="f">Conv2d</span>(<span class="n">3</span>, <span class="n">32</span>, <span class="n">3</span>, padding=<span class="n">1</span>), nn.<span class="f">BatchNorm2d</span>(<span class="n">32</span>), nn.<span class="f">ReLU</span>(<span class="k">True</span>), nn.<span class="f">MaxPool2d</span>(<span class="n">2</span>),
            nn.<span class="f">Conv2d</span>(<span class="n">32</span>, <span class="n">64</span>, <span class="n">3</span>, padding=<span class="n">1</span>), nn.<span class="f">BatchNorm2d</span>(<span class="n">64</span>), nn.<span class="f">ReLU</span>(<span class="k">True</span>), nn.<span class="f">MaxPool2d</span>(<span class="n">2</span>),
            nn.<span class="f">AdaptiveAvgPool2d</span>((<span class="n">4</span>, <span class="n">4</span>)),
        )
        self.classifier = nn.<span class="f">Sequential</span>(
            nn.<span class="f">Flatten</span>(),
            nn.<span class="f">Linear</span>(<span class="n">128</span>*<span class="n">4</span>*<span class="n">4</span>, <span class="n">512</span>), nn.<span class="f">ReLU</span>(<span class="k">True</span>), nn.<span class="f">Dropout</span>(<span class="n">0.5</span>),
            nn.<span class="f">Linear</span>(<span class="n">512</span>, num_classes),
        )</pre>
    </div>
  </details>

  <details>
    <summary>📝 LSTM — Sequence Classification</summary>
    <div class="detail-body">
<pre><span class="k">class</span> <span class="o">LSTMClassifier</span>(nn.Module):
    <span class="k">def</span> <span class="f">__init__</span>(self, vocab_size, embed_dim, hidden, n_layers, n_class):
        <span class="f">super</span>().__init__()
        self.emb  = nn.<span class="f">Embedding</span>(vocab_size, embed_dim, padding_idx=<span class="n">0</span>)
        self.lstm = nn.<span class="f">LSTM</span>(embed_dim, hidden, n_layers,
                            batch_first=<span class="k">True</span>, dropout=<span class="n">0.3</span>, bidirectional=<span class="k">True</span>)
        self.fc   = nn.<span class="f">Linear</span>(hidden * <span class="n">2</span>, n_class)

    <span class="k">def</span> <span class="f">forward</span>(self, x):
        out, _ = self.lstm(self.emb(x))
        <span class="k">return</span> self.fc(out.<span class="m">mean</span>(dim=<span class="n">1</span>))</pre>
    </div>
  </details>

  <details>
    <summary>🤖 Transformer — Text Classification</summary>
    <div class="detail-body">
<pre><span class="k">class</span> <span class="o">TransformerClassifier</span>(nn.Module):
    <span class="k">def</span> <span class="f">__init__</span>(self, vocab_size, d_model=<span class="n">512</span>, nhead=<span class="n">8</span>,
                 num_layers=<span class="n">6</span>, num_classes=<span class="n">10</span>, max_len=<span class="n">512</span>):
        <span class="f">super</span>().__init__()
        self.emb     = nn.<span class="f">Embedding</span>(vocab_size, d_model)
        self.pos     = nn.<span class="f">Embedding</span>(max_len, d_model)
        enc_layer    = nn.<span class="f">TransformerEncoderLayer</span>(
            d_model, nhead, dim_feedforward=<span class="n">2048</span>, dropout=<span class="n">0.1</span>, batch_first=<span class="k">True</span>
        )
        self.encoder = nn.<span class="f">TransformerEncoder</span>(enc_layer, num_layers)
        self.head    = nn.<span class="f">Linear</span>(d_model, num_classes)

    <span class="k">def</span> <span class="f">forward</span>(self, x):
        pos = torch.<span class="f">arange</span>(x.<span class="m">size</span>(<span class="n">1</span>), device=x.device).<span class="m">unsqueeze</span>(<span class="n">0</span>)
        x   = self.emb(x) + self.pos(pos)
        <span class="k">return</span> self.head(self.encoder(x)[:, <span class="n">0</span>])   <span class="c"># CLS token</span></pre>
    </div>
  </details>
</section>

<!-- ── 11 TRANSFER ── -->
<section id="transfer" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 11</p>
    <h2 class="section-title">Transfer <span>Learning</span></h2>
    <p class="section-desc">Fine-tuning pretrained ResNet-50 with differential learning rates.</p>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">transfer_learning.py</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">import</span> torchvision.models <span class="k">as</span> models

<span class="c"># Load pretrained ResNet-50</span>
backbone = models.<span class="f">resnet50</span>(weights=models.ResNet50_Weights.DEFAULT)

<span class="c"># Step 1 — Freeze ALL pretrained weights</span>
<span class="k">for</span> p <span class="k">in</span> backbone.<span class="m">parameters</span>(): p.requires_grad = <span class="k">False</span>

<span class="c"># Step 2 — Replace classifier head</span>
backbone.fc = nn.<span class="f">Sequential</span>(
    nn.<span class="f">Dropout</span>(<span class="n">0.5</span>),
    nn.<span class="f">Linear</span>(backbone.fc.in_features, <span class="n">256</span>), nn.<span class="f">ReLU</span>(),
    nn.<span class="f">Linear</span>(<span class="n">256</span>, NUM_CLASSES),
)

<span class="c"># Step 3 — Unfreeze top layers for fine-tuning</span>
<span class="k">for</span> p <span class="k">in</span> backbone.layer4.<span class="m">parameters</span>(): p.requires_grad = <span class="k">True</span>

<span class="c"># Step 4 — Differential learning rates</span>
optimizer = torch.optim.<span class="f">AdamW</span>([
    {<span class="s">"params"</span>: backbone.layer3.<span class="m">parameters</span>(), <span class="s">"lr"</span>: <span class="n">1e-5</span>},
    {<span class="s">"params"</span>: backbone.layer4.<span class="m">parameters</span>(), <span class="s">"lr"</span>: <span class="n">5e-5</span>},
    {<span class="s">"params"</span>: backbone.fc.<span class="m">parameters</span>(),     <span class="s">"lr"</span>: <span class="n">1e-3</span>},
], weight_decay=<span class="n">0.01</span>)</pre>
  </div>

  <div class="callout callout-fire">
    <span class="callout-icon">🔥</span>
    <span class="callout-text"><strong>Osama's Rule:</strong> Always use differential learning rates — lower LR for pretrained layers, higher LR for the new head. This is the #1 trick for fast fine-tuning!</span>
  </div>
</section>

<!-- ── 12 SAVE/LOAD ── -->
<section id="save" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 12</p>
    <h2 class="section-title">Save & Load <span>Models</span></h2>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">checkpointing.py</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="c"># ── Recommended: weights only ─────────────────────────────────────</span>
torch.<span class="f">save</span>(model.<span class="m">state_dict</span>(), <span class="s">"weights.pth"</span>)
model.<span class="m">load_state_dict</span>(torch.<span class="f">load</span>(<span class="s">"weights.pth"</span>, map_location=device))

<span class="c"># ── Full checkpoint (for resuming training) ───────────────────────</span>
torch.<span class="f">save</span>({
    <span class="s">"epoch"</span>     : epoch,
    <span class="s">"model"</span>     : model.<span class="m">state_dict</span>(),
    <span class="s">"optimizer"</span> : optimizer.<span class="m">state_dict</span>(),
    <span class="s">"scheduler"</span> : scheduler.<span class="m">state_dict</span>(),
    <span class="s">"scaler"</span>    : scaler.<span class="m">state_dict</span>(),
    <span class="s">"best_acc"</span>  : best_val_acc,
}, <span class="s">"checkpoint.pt"</span>)

<span class="c"># ── Restore ────────────────────────────────────────────────────────</span>
ckpt = torch.<span class="f">load</span>(<span class="s">"checkpoint.pt"</span>, map_location=device)
model.<span class="m">load_state_dict</span>(ckpt[<span class="s">"model"</span>])
start_epoch = ckpt[<span class="s">"epoch"</span>] + <span class="n">1</span></pre>
  </div>
</section>

<!-- ── 13 GPU ── -->
<section id="gpu" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 13</p>
    <h2 class="section-title">GPU <span>Acceleration</span></h2>
  </div>

  <div class="chips">
    <span class="chip chip-green">CUDA</span>
    <span class="chip chip-fire">AMP ~2× speedup</span>
    <span class="chip chip-blue">Multi-GPU</span>
    <span class="chip chip-gold">torch.compile</span>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">gpu.py</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="c"># ── Device setup ──────────────────────────────────────────────────</span>
device = torch.<span class="f">device</span>(<span class="s">"cuda"</span> <span class="k">if</span> torch.cuda.<span class="m">is_available</span>() <span class="k">else</span> <span class="s">"cpu"</span>)
model  = model.<span class="m">to</span>(device)

<span class="c"># ── Automatic Mixed Precision (AMP) — ~2× speedup ─────────────────</span>
<span class="k">from</span> torch.cuda.amp <span class="k">import</span> autocast, GradScaler
scaler = <span class="f">GradScaler</span>()

<span class="k">with</span> <span class="f">autocast</span>():
    logits = model(X)
    loss   = criterion(logits, y)

scaler.<span class="m">scale</span>(loss).<span class="m">backward</span>()
scaler.<span class="m">step</span>(optimizer); scaler.<span class="m">update</span>()

<span class="c"># ── Multi-GPU ─────────────────────────────────────────────────────</span>
<span class="k">if</span> torch.cuda.<span class="f">device_count</span>() > <span class="n">1</span>:
    model = nn.<span class="f">DataParallel</span>(model)

<span class="c"># ── torch.compile — PyTorch 2.x (up to 2× faster) ────────────────</span>
model = torch.<span class="f">compile</span>(model)    <span class="c"># ← single line, massive gains</span></pre>
  </div>
</section>

<!-- ── 14 DEPLOY ── -->
<section id="deploy" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 14</p>
    <h2 class="section-title">TorchScript & <span>Deployment</span></h2>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">deploy.py — FastAPI Inference Server</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">from</span> fastapi <span class="k">import</span> FastAPI
<span class="k">from</span> pydantic <span class="k">import</span> BaseModel

app   = <span class="f">FastAPI</span>(title=<span class="s">"PyTorch Model API"</span>)
model = torch.jit.<span class="f">load</span>(<span class="s">"model_scripted.pt"</span>).<span class="m">to</span>(device).<span class="m">eval</span>()

<span class="k">class</span> <span class="o">Request</span>(BaseModel):
    features: list[float]

@app.<span class="f">post</span>(<span class="s">"/predict"</span>)
<span class="k">async def</span> <span class="f">predict</span>(data: <span class="o">Request</span>):
    x = torch.<span class="f">tensor</span>(data.features).<span class="m">unsqueeze</span>(<span class="n">0</span>).<span class="m">to</span>(device)
    <span class="k">with</span> torch.<span class="f">no_grad</span>():
        logits = model(x)
        probs  = torch.<span class="f">softmax</span>(logits, dim=<span class="n">1</span>)
    <span class="k">return</span> {<span class="s">"class"</span>: logits.<span class="m">argmax</span>().<span class="m">item</span>(),
            <span class="s">"confidence"</span>: <span class="f">round</span>(probs.<span class="m">max</span>().<span class="m">item</span>(), <span class="n">4</span>)}

<span class="c"># Run: uvicorn app:app --host 0.0.0.0 --port 8000</span></pre>
  </div>
</section>

<!-- ── 15 BEST PRACTICES ── -->
<section id="tips" class="section fade-in">
  <div class="section-header">
    <p class="section-num">// 15</p>
    <h2 class="section-title">Best Practices & <span>Checklist</span></h2>
  </div>

  <div class="code-wrap">
    <div class="code-header">
      <div class="code-dots"><span class="code-dot dot-red"></span><span class="code-dot dot-yellow"></span><span class="code-dot dot-green"></span></div>
      <span class="code-label">reproducibility.py — Paste at the TOP of every script</span>
      <button class="code-copy" onclick="copyCode(this)">copy</button>
    </div>
<pre><span class="k">import</span> torch, random, numpy <span class="k">as</span> np

<span class="k">def</span> <span class="f">set_seed</span>(seed: int = <span class="n">42</span>):
    torch.<span class="f">manual_seed</span>(seed)
    torch.cuda.<span class="f">manual_seed_all</span>(seed)
    np.random.<span class="f">seed</span>(seed)
    random.<span class="f">seed</span>(seed)
    torch.backends.cudnn.deterministic = <span class="k">True</span>
    torch.backends.cudnn.benchmark     = <span class="k">False</span>

<span class="f">set_seed</span>(<span class="n">42</span>)</pre>
  </div>

  <p style="font-size:0.9rem;color:var(--text-dim);margin:32px 0 16px;font-weight:700;font-family:'Space Mono',monospace;letter-spacing:0.1em;text-transform:uppercase;font-size:0.7rem;color:var(--fire);">✅ Pre-Training Checklist</p>
  <ul class="checklist">
    <li>set_seed(42) called at top of script</li>
    <li>model.train() / model.eval() used correctly</li>
    <li>optimizer.zero_grad() at start of every iteration</li>
    <li>Gradient clipping (clip_grad_norm_, max=1.0)</li>
    <li>Mixed precision (GradScaler + autocast) enabled</li>
    <li>pin_memory=True + num_workers > 0 in DataLoader</li>
    <li>Checkpoint saved on best validation metric</li>
    <li>LR scheduler stepped at end of each epoch</li>
    <li>torch.no_grad() used during inference</li>
  </ul>
</section>

<!-- ── RESOURCES ── -->
<section class="section fade-in">
  <div class="section-header">
    <p class="section-num">// Resources</p>
    <h2 class="section-title">Learn <span>More</span></h2>
  </div>
  <div class="resources">
    <a class="resource-link" href="https://pytorch.org/docs" target="_blank">
      <span class="resource-icon">📖</span>
      <div><div class="resource-text">Official Docs</div><div class="resource-url">pytorch.org/docs</div></div>
    </a>
    <a class="resource-link" href="https://pytorch.org/tutorials" target="_blank">
      <span class="resource-icon">🎓</span>
      <div><div class="resource-text">Tutorials</div><div class="resource-url">pytorch.org/tutorials</div></div>
    </a>
    <a class="resource-link" href="https://huggingface.co" target="_blank">
      <span class="resource-icon">🤗</span>
      <div><div class="resource-text">HuggingFace</div><div class="resource-url">huggingface.co</div></div>
    </a>
    <a class="resource-link" href="https://paperswithcode.com" target="_blank">
      <span class="resource-icon">📄</span>
      <div><div class="resource-text">Papers With Code</div><div class="resource-url">paperswithcode.com</div></div>
    </a>
    <a class="resource-link" href="https://discuss.pytorch.org" target="_blank">
      <span class="resource-icon">💬</span>
      <div><div class="resource-text">PyTorch Forum</div><div class="resource-url">discuss.pytorch.org</div></div>
    </a>
  </div>
</section>

</div><!-- /container -->
</main>

<!-- ── FOOTER ── -->
<footer>
  <div class="footer-ascii">
███████╗██╗██████╗ ███████╗
██╔════╝██║██╔══██╗██╔════╝
█████╗  ██║██████╔╝█████╗
██╔══╝  ██║██╔══██╗██╔══╝
██║     ██║██║  ██║███████╗
╚═╝     ╚═╝╚═╝  ╚═╝╚══════╝</div>
  <p class="footer-text">
    Made with <span class="footer-heart">❤️</span> and 🔥 by
    <a href="https://github.com/osamashabih6960">Osama Shabih</a>
    <br/><br/>
    <span style="opacity:0.5;font-size:0.75rem;">Jamia Hamdard University — Deep Learning Research · MIT License © 2024</span>
  </p>
</footer>

<script>
// ── Intersection Observer for fade-in ─────────────────────────────
const observer = new IntersectionObserver((entries) => {
  entries.forEach((e, i) => {
    if (e.isIntersecting) {
      setTimeout(() => e.target.classList.add('visible'), i * 80);
      observer.unobserve(e.target);
    }
  });
}, { threshold: 0.08 });

document.querySelectorAll('.fade-in').forEach(el => observer.observe(el));

// ── Copy button ────────────────────────────────────────────────────
function copyCode(btn) {
  const pre = btn.closest('.code-wrap').querySelector('pre');
  navigator.clipboard.writeText(pre.innerText).then(() => {
    btn.textContent = 'copied!';
    btn.style.color = 'var(--green)';
    btn.style.borderColor = 'var(--green)';
    setTimeout(() => {
      btn.textContent = 'copy';
      btn.style.color = '';
      btn.style.borderColor = '';
    }, 1500);
  });
}

// ── Active nav highlight ───────────────────────────────────────────
const sections = document.querySelectorAll('section[id]');
const navLinks = document.querySelectorAll('.nav-link');

const navObserver = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      navLinks.forEach(l => l.style.color = '');
      const active = document.querySelector(`.nav-link[href="#${e.target.id}"]`);
      if (active) active.style.color = 'var(--fire)';
    }
  });
}, { threshold: 0.3 });

sections.forEach(s => navObserver.observe(s));
</script>
</body>
</html>