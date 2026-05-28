<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Chinquest — Science In Action</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Syne:wght@400;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --green-deep: #0f3d1a;
    --green-mid: #1a5c28;
    --green-bright: #2d8a42;
    --green-light: #4db866;
    --green-pale: #a8e4b8;
    --green-mist: #e6f7eb;
    --teal: #0d7377;
    --teal-light: #14a085;
    --amber: #e8a020;
    --amber-light: #f5c842;
    --cream: #f9f6ee;
    --white: #ffffff;
    --text-dark: #0d1f10;
    --text-mid: #2d4a32;
    --text-muted: #5a7a60;
    --border: rgba(45, 138, 66, 0.2);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Space Grotesk', sans-serif;
    background: var(--green-deep);
    color: var(--text-dark);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── FLOATING LEAF ANIMATIONS ── */
  .leaf-canvas {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 0;
    overflow: hidden;
  }
  .leaf {
    position: absolute;
    opacity: 0;
    animation: leafFall linear infinite;
  }
  .leaf svg { display: block; }

  @keyframes leafFall {
    0%   { transform: translateY(-60px) translateX(0) rotate(0deg); opacity: 0; }
    5%   { opacity: 0.55; }
    95%  { opacity: 0.35; }
    100% { transform: translateY(110vh) translateX(60px) rotate(720deg); opacity: 0; }
  }
  @keyframes sway {
    0%,100% { transform: translateX(0); }
    50%      { transform: translateX(30px); }
  }

  /* ── FORMULA DRIFT ── */
  .formula-float {
    position: fixed;
    font-family: 'Space Grotesk', monospace;
    font-size: 13px;
    color: rgba(77,184,102,0.18);
    pointer-events: none;
    z-index: 0;
    animation: formulaDrift linear infinite;
    white-space: nowrap;
  }
  @keyframes formulaDrift {
    0%   { transform: translateY(110vh) rotate(-8deg); opacity: 0; }
    8%   { opacity: 1; }
    92%  { opacity: 1; }
    100% { transform: translateY(-10vh) rotate(8deg); opacity: 0; }
  }

  /* ── VINE DECORATION ── */
  .vine-left, .vine-right {
    position: fixed;
    top: 0;
    width: 90px;
    height: 100%;
    pointer-events: none;
    z-index: 1;
    opacity: 0.6;
  }
  .vine-left { left: 0; }
  .vine-right { right: 0; transform: scaleX(-1); }

  /* ── HEADER ── */
  header {
    position: relative;
    z-index: 10;
    background: linear-gradient(135deg, #0a2e11 0%, #0f3d1a 60%, #0d3320 100%);
    border-bottom: 2px solid var(--green-bright);
    padding: 0 2rem;
    box-shadow: 0 4px 32px rgba(0,0,0,0.5);
  }
  .header-inner {
    max-width: 1100px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1.4rem 0;
  }
  .logo {
    display: flex;
    align-items: center;
    gap: 14px;
  }
  .logo-icon {
    width: 52px;
    height: 52px;
    background: radial-gradient(circle at 40% 40%, var(--green-light), var(--green-mid));
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 26px;
    box-shadow: 0 0 20px rgba(77,184,102,0.4);
    animation: pulse 3s ease-in-out infinite;
  }
  @keyframes pulse {
    0%,100% { box-shadow: 0 0 20px rgba(77,184,102,0.4); }
    50%      { box-shadow: 0 0 36px rgba(77,184,102,0.7); }
  }
  .logo-text h1 {
    font-family: 'Syne', sans-serif;
    font-size: 2rem;
    font-weight: 800;
    color: var(--green-pale);
    letter-spacing: -0.02em;
    line-height: 1;
  }
  .logo-text span {
    font-size: 0.78rem;
    color: var(--green-light);
    letter-spacing: 0.15em;
    text-transform: uppercase;
  }
  .header-nav {
    display: flex;
    gap: 6px;
  }
  .header-nav button {
    background: transparent;
    border: 1px solid rgba(77,184,102,0.3);
    color: var(--green-pale);
    font-family: 'Space Grotesk', sans-serif;
    font-size: 0.85rem;
    padding: 7px 16px;
    border-radius: 20px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .header-nav button:hover, .header-nav button.active {
    background: var(--green-bright);
    border-color: var(--green-bright);
    color: white;
  }

  /* ── MAIN LAYOUT ── */
  main {
    position: relative;
    z-index: 5;
    max-width: 1100px;
    margin: 0 auto;
    padding: 2.5rem 2rem 4rem;
  }

  /* ── VIEWS ── */
  .view { display: none; }
  .view.active { display: block; }

  /* ── GRADE SELECTOR ── */
  .grade-selector {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1.5rem;
    margin-bottom: 2.5rem;
  }
  .grade-card {
    background: linear-gradient(145deg, rgba(15,61,26,0.9), rgba(10,40,15,0.95));
    border: 1px solid rgba(77,184,102,0.25);
    border-radius: 18px;
    padding: 2rem;
    cursor: pointer;
    transition: all 0.25s;
    position: relative;
    overflow: hidden;
    text-align: center;
  }
  .grade-card::before {
    content: '';
    position: absolute;
    top: -30px;
    right: -30px;
    width: 100px;
    height: 100px;
    background: radial-gradient(circle, rgba(77,184,102,0.12), transparent);
    border-radius: 50%;
  }
  .grade-card:hover {
    border-color: var(--green-bright);
    transform: translateY(-4px);
    box-shadow: 0 12px 40px rgba(0,0,0,0.4), 0 0 24px rgba(77,184,102,0.15);
  }
  .grade-card.selected {
    border-color: var(--green-light);
    background: linear-gradient(145deg, rgba(29,94,40,0.9), rgba(15,61,26,0.95));
  }
  .grade-number {
    font-family: 'Syne', sans-serif;
    font-size: 3.5rem;
    font-weight: 800;
    color: var(--green-light);
    line-height: 1;
    margin-bottom: 0.3rem;
  }
  .grade-label {
    font-size: 0.8rem;
    color: var(--green-pale);
    letter-spacing: 0.12em;
    text-transform: uppercase;
  }
  .grade-sub {
    font-size: 0.75rem;
    color: var(--text-muted);
    margin-top: 0.4rem;
  }

  /* ── UNIT GRID ── */
  .section-heading {
    font-family: 'Syne', sans-serif;
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--green-pale);
    margin-bottom: 1.2rem;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .section-heading::after {
    content: '';
    flex: 1;
    height: 1px;
    background: rgba(77,184,102,0.2);
  }
  .breadcrumb {
    font-size: 0.8rem;
    color: var(--text-muted);
    margin-bottom: 1.5rem;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .breadcrumb span { cursor: pointer; }
  .breadcrumb span:hover { color: var(--green-light); }
  .breadcrumb .sep { color: rgba(77,184,102,0.3); }
  .breadcrumb .current { color: var(--green-pale); }

  .unit-grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 1rem;
    margin-bottom: 2rem;
  }
  .unit-card {
    background: rgba(10,40,15,0.85);
    border: 1px solid rgba(77,184,102,0.2);
    border-radius: 14px;
    padding: 1.4rem 1rem;
    cursor: pointer;
    transition: all 0.2s;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .unit-card:hover {
    border-color: var(--green-bright);
    transform: translateY(-3px);
    box-shadow: 0 8px 28px rgba(0,0,0,0.35);
  }
  .unit-letter {
    font-family: 'Syne', sans-serif;
    font-size: 2.4rem;
    font-weight: 800;
    color: var(--green-light);
    line-height: 1;
  }
  .unit-name {
    font-size: 0.72rem;
    color: var(--green-pale);
    margin-top: 4px;
    letter-spacing: 0.06em;
  }
  .unit-test-badge {
    display: inline-block;
    background: rgba(232,160,32,0.15);
    border: 1px solid rgba(232,160,32,0.3);
    color: var(--amber-light);
    font-size: 0.62rem;
    padding: 2px 8px;
    border-radius: 10px;
    margin-top: 6px;
    letter-spacing: 0.05em;
  }

  /* ── SECTIONS VIEW ── */
  .sections-container {
    display: grid;
    grid-template-columns: 1fr;
    gap: 1rem;
  }
  .section-row {
    background: rgba(10,40,15,0.8);
    border: 1px solid rgba(77,184,102,0.18);
    border-radius: 14px;
    padding: 1.2rem 1.5rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    transition: all 0.2s;
  }
  .section-row:hover {
    border-color: rgba(77,184,102,0.4);
    background: rgba(15,60,22,0.9);
  }
  .section-id {
    font-family: 'Syne', sans-serif;
    font-size: 1.5rem;
    font-weight: 700;
    color: var(--green-light);
    min-width: 70px;
  }
  .section-title {
    flex: 1;
    padding: 0 1.5rem;
    color: var(--green-pale);
    font-size: 0.95rem;
  }
  .section-title small {
    display: block;
    color: var(--text-muted);
    font-size: 0.75rem;
    margin-top: 2px;
  }
  .section-actions {
    display: flex;
    gap: 8px;
  }
  .btn-action {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 8px 16px;
    border-radius: 10px;
    font-family: 'Space Grotesk', sans-serif;
    font-size: 0.8rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.18s;
    border: 1px solid;
    text-decoration: none;
  }
  .btn-notes {
    background: rgba(13,115,119,0.15);
    border-color: rgba(13,115,119,0.4);
    color: #5ce0e6;
  }
  .btn-notes:hover {
    background: rgba(13,115,119,0.35);
    transform: translateY(-1px);
  }
  .btn-practice {
    background: rgba(77,184,102,0.12);
    border-color: rgba(77,184,102,0.35);
    color: var(--green-light);
  }
  .btn-practice:hover {
    background: rgba(77,184,102,0.28);
    transform: translateY(-1px);
  }
  .unit-test-row {
    background: rgba(232,160,32,0.06);
    border: 1px solid rgba(232,160,32,0.22);
    border-radius: 14px;
    padding: 1.2rem 1.5rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-top: 0.5rem;
  }
  .unit-test-label {
    display: flex;
    align-items: center;
    gap: 12px;
    color: var(--amber-light);
    font-weight: 500;
    font-size: 0.95rem;
  }
  .unit-test-label .star { font-size: 1.3rem; }
  .btn-unit-test {
    background: rgba(232,160,32,0.15);
    border: 1px solid rgba(232,160,32,0.4);
    color: var(--amber-light);
    padding: 9px 20px;
    border-radius: 10px;
    font-family: 'Space Grotesk', sans-serif;
    font-size: 0.82rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.18s;
  }
  .btn-unit-test:hover {
    background: rgba(232,160,32,0.3);
    transform: translateY(-1px);
  }

  /* ── PDF MODAL ── */
  .modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.75);
    z-index: 100;
    align-items: center;
    justify-content: center;
    backdrop-filter: blur(4px);
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: linear-gradient(145deg, #0e3318, #0a2812);
    border: 1px solid rgba(77,184,102,0.3);
    border-radius: 20px;
    padding: 2rem;
    max-width: 500px;
    width: 90%;
    box-shadow: 0 32px 80px rgba(0,0,0,0.6);
    animation: modalIn 0.25s ease;
    position: relative;
  }
  @keyframes modalIn {
    from { opacity: 0; transform: scale(0.92) translateY(10px); }
    to   { opacity: 1; transform: scale(1) translateY(0); }
  }
  .modal-close {
    position: absolute;
    top: 14px;
    right: 16px;
    background: none;
    border: none;
    color: var(--text-muted);
    font-size: 1.4rem;
    cursor: pointer;
    line-height: 1;
  }
  .modal-close:hover { color: var(--green-light); }
  .modal-icon {
    width: 56px;
    height: 56px;
    border-radius: 14px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.8rem;
    margin-bottom: 1rem;
  }
  .modal-icon.notes { background: rgba(13,115,119,0.2); }
  .modal-icon.practice { background: rgba(77,184,102,0.2); }
  .modal-icon.test { background: rgba(232,160,32,0.15); }
  .modal h2 {
    font-family: 'Syne', sans-serif;
    font-size: 1.3rem;
    color: var(--green-pale);
    margin-bottom: 0.4rem;
  }
  .modal p { color: var(--text-muted); font-size: 0.87rem; line-height: 1.6; margin-bottom: 1.5rem; }
  .modal-path {
    background: rgba(0,0,0,0.3);
    border: 1px solid rgba(77,184,102,0.15);
    border-radius: 8px;
    padding: 10px 14px;
    font-size: 0.78rem;
    color: var(--green-light);
    font-family: monospace;
    margin-bottom: 1.5rem;
    word-break: break-all;
  }
  .modal-actions { display: flex; gap: 10px; }
  .modal-btn {
    flex: 1;
    padding: 11px;
    border-radius: 10px;
    font-family: 'Space Grotesk', sans-serif;
    font-size: 0.85rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.18s;
    border: 1px solid;
    text-align: center;
  }
  .modal-btn.primary {
    background: var(--green-bright);
    border-color: var(--green-bright);
    color: white;
  }
  .modal-btn.primary:hover { background: var(--green-light); }
  .modal-btn.secondary {
    background: transparent;
    border-color: rgba(77,184,102,0.3);
    color: var(--green-pale);
  }
  .modal-btn.secondary:hover { border-color: var(--green-bright); }

  /* ── STUDY TIPS VIEW ── */
  .tips-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 1.2rem;
  }
  .tip-card {
    background: rgba(10,40,15,0.8);
    border: 1px solid rgba(77,184,102,0.18);
    border-radius: 16px;
    padding: 1.6rem;
    transition: border-color 0.2s;
  }
  .tip-card:hover { border-color: rgba(77,184,102,0.4); }
  .tip-icon { font-size: 2rem; margin-bottom: 0.8rem; }
  .tip-card h3 {
    font-family: 'Syne', sans-serif;
    font-size: 1.05rem;
    color: var(--green-pale);
    margin-bottom: 0.6rem;
  }
  .tip-card p { font-size: 0.87rem; color: var(--text-muted); line-height: 1.7; }
  .tip-card ul { padding-left: 1rem; }
  .tip-card li { font-size: 0.84rem; color: var(--text-muted); margin-bottom: 4px; line-height: 1.6; }
  .tip-highlight {
    background: rgba(77,184,102,0.08);
    border-left: 3px solid var(--green-bright);
    border-radius: 0 12px 12px 0;
  }

  /* ── INFO VIEW ── */
  .info-hero {
    background: linear-gradient(135deg, rgba(15,61,26,0.9), rgba(13,115,119,0.2));
    border: 1px solid rgba(77,184,102,0.2);
    border-radius: 18px;
    padding: 2.5rem;
    margin-bottom: 1.5rem;
    text-align: center;
  }
  .info-hero h2 {
    font-family: 'Syne', sans-serif;
    font-size: 1.8rem;
    font-weight: 800;
    color: var(--green-pale);
    margin-bottom: 0.6rem;
  }
  .info-hero p { color: var(--text-muted); font-size: 0.92rem; line-height: 1.7; max-width: 600px; margin: 0 auto; }
  .info-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem;
  }
  .info-stat {
    background: rgba(10,40,15,0.8);
    border: 1px solid rgba(77,184,102,0.18);
    border-radius: 14px;
    padding: 1.5rem;
    text-align: center;
  }
  .info-stat .num {
    font-family: 'Syne', sans-serif;
    font-size: 2.4rem;
    font-weight: 800;
    color: var(--green-light);
    line-height: 1;
  }
  .info-stat .lbl {
    font-size: 0.78rem;
    color: var(--text-muted);
    margin-top: 4px;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }
  .textbook-callout {
    background: rgba(232,160,32,0.06);
    border: 1px solid rgba(232,160,32,0.22);
    border-radius: 14px;
    padding: 1.5rem 2rem;
    margin-top: 1rem;
    display: flex;
    align-items: center;
    gap: 1.5rem;
  }
  .textbook-callout .book-icon { font-size: 2.5rem; }
  .textbook-callout h3 { font-family: 'Syne', sans-serif; color: var(--amber-light); margin-bottom: 3px; }
  .textbook-callout p { font-size: 0.84rem; color: var(--text-muted); line-height: 1.6; }

  /* ── RESPONSIVE ── */
  @media (max-width: 700px) {
    .grade-selector { grid-template-columns: 1fr; }
    .unit-grid { grid-template-columns: repeat(3, 1fr); }
    .tips-grid, .info-grid { grid-template-columns: 1fr; }
    .section-row { flex-direction: column; align-items: flex-start; gap: 10px; }
    .header-nav { display: none; }
  }
</style>
</head>
<body>

<!-- Background leaf canvas -->
<div class="leaf-canvas" id="leafCanvas"></div>

<!-- Floating formulas -->
<div id="formulas"></div>

<!-- Vine decorations -->
<svg class="vine-left" viewBox="0 0 90 800" preserveAspectRatio="xMidYMid slice" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      .vine-path { fill: none; stroke: rgba(45,138,66,0.35); stroke-width: 2; }
      .leaf-shape { fill: rgba(45,138,66,0.28); }
    </style>
  </defs>
  <path class="vine-path" d="M20,0 C30,80 10,160 25,240 C40,320 15,400 30,480 C45,560 20,640 35,720 C50,800 30,880 40,960"/>
  <ellipse class="leaf-shape" cx="35" cy="90" rx="14" ry="8" transform="rotate(-30 35 90)"/>
  <ellipse class="leaf-shape" cx="18" cy="180" rx="12" ry="7" transform="rotate(40 18 180)"/>
  <ellipse class="leaf-shape" cx="32" cy="280" rx="15" ry="9" transform="rotate(-20 32 280)"/>
  <ellipse class="leaf-shape" cx="22" cy="370" rx="11" ry="7" transform="rotate(50 22 370)"/>
  <ellipse class="leaf-shape" cx="38" cy="460" rx="14" ry="8" transform="rotate(-35 38 460)"/>
  <ellipse class="leaf-shape" cx="24" cy="560" rx="13" ry="8" transform="rotate(25 24 560)"/>
  <ellipse class="leaf-shape" cx="36" cy="650" rx="12" ry="7" transform="rotate(-45 36 650)"/>
  <ellipse class="leaf-shape" cx="20" cy="740" rx="14" ry="8" transform="rotate(30 20 740)"/>
</svg>
<svg class="vine-right" viewBox="0 0 90 800" preserveAspectRatio="xMidYMid slice" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      .vine-path2 { fill: none; stroke: rgba(45,138,66,0.35); stroke-width: 2; }
      .leaf-shape2 { fill: rgba(45,138,66,0.28); }
    </style>
  </defs>
  <path class="vine-path2" d="M20,0 C30,80 10,160 25,240 C40,320 15,400 30,480 C45,560 20,640 35,720 C50,800 30,880 40,960"/>
  <ellipse class="leaf-shape2" cx="35" cy="90" rx="14" ry="8" transform="rotate(-30 35 90)"/>
  <ellipse class="leaf-shape2" cx="18" cy="200" rx="12" ry="7" transform="rotate(40 18 200)"/>
  <ellipse class="leaf-shape2" cx="32" cy="300" rx="15" ry="9" transform="rotate(-20 32 300)"/>
  <ellipse class="leaf-shape2" cx="22" cy="400" rx="11" ry="7" transform="rotate(50 22 400)"/>
  <ellipse class="leaf-shape2" cx="38" cy="500" rx="14" ry="8" transform="rotate(-35 38 500)"/>
  <ellipse class="leaf-shape2" cx="24" cy="600" rx="13" ry="8" transform="rotate(25 24 600)"/>
  <ellipse class="leaf-shape2" cx="36" cy="700" rx="12" ry="7" transform="rotate(-45 36 700)"/>
</svg>

<!-- HEADER -->
<header>
  <div class="header-inner">
    <div class="logo">
      <div class="logo-icon">🌿</div>
      <div class="logo-text">
        <h1>Chinquest</h1>
        <span>Science in Action · Student Hub</span>
      </div>
    </div>
    <nav class="header-nav">
      <button class="active" onclick="showView('home')">Grades</button>
      <button onclick="showView('tips')">Study Tips</button>
      <button onclick="showView('info')">Info</button>
    </nav>
  </div>
</header>

<!-- MAIN -->
<main>

  <!-- ── HOME VIEW ── -->
  <div class="view active" id="view-home">
    <h2 class="section-heading">Select Your Grade</h2>
    <div class="grade-selector">
      <div class="grade-card" onclick="selectGrade(7)">
        <div class="grade-number">7</div>
        <div class="grade-label">Grade Seven</div>
        <div class="grade-sub">Science in Action 7</div>
      </div>
      <div class="grade-card" onclick="selectGrade(8)">
        <div class="grade-number">8</div>
        <div class="grade-label">Grade Eight</div>
        <div class="grade-sub">Science in Action 8</div>
      </div>
      <div class="grade-card" onclick="selectGrade(9)">
        <div class="grade-number">9</div>
        <div class="grade-label">Grade Nine</div>
        <div class="grade-sub">Science in Action 9</div>
      </div>
    </div>
    <div id="units-panel" style="display:none">
      <div class="breadcrumb">
        <span onclick="clearGrade()">Home</span>
        <span class="sep">›</span>
        <span class="current" id="bc-grade">Grade 7</span>
      </div>
      <h2 class="section-heading" id="units-heading">Units — Grade 7</h2>
      <div class="unit-grid" id="unitGrid"></div>
    </div>
    <div id="sections-panel" style="display:none">
      <div class="breadcrumb">
        <span onclick="clearGrade()">Home</span>
        <span class="sep">›</span>
        <span onclick="clearUnit()" id="bc-grade2">Grade 7</span>
        <span class="sep">›</span>
        <span class="current" id="bc-unit">Unit A</span>
      </div>
      <h2 class="section-heading" id="sections-heading">Unit A — Sections</h2>
      <div class="sections-container" id="sectionsContainer"></div>
    </div>
  </div>

  <!-- ── STUDY TIPS VIEW ── -->
  <div class="view" id="view-tips">
    <h2 class="section-heading">Study Tips</h2>
    <div class="tips-grid">
      <div class="tip-card tip-highlight">
        <div class="tip-icon">🧠</div>
        <h3>Do Your Notes</h3>
        <p>Doing your notes and handouts is CRUCIAL to doing well in science at H.E. Beriault. Do not dissapoint Mr. Chin!</p>
      </div>
      <div class="tip-card">
        <div class="tip-icon">⏱️</div>
        <h3>NO CRAMMING</h3>
        <p>Review material multiple times spread out over days rather than cramming the night before. Try this schedule:</p>
        <ul>
          <li>Night after class → quick 10-min review</li>
          <li>3 days later → redo practice test</li>
          <li>1 week later → full section review</li>
          <li>Before unit test → full unit review</li>
        </ul>
      </div>
      <div class="tip-card">
        <div class="tip-icon">📝</div>
        <h3>Actually do your flashcards</h3>
        <p>Though it may seem like a nuisance, doing your flashcards will genuinely help you. Studies show that flashcards force your brain to make an answer from scratch rather than just rereading text, moving concepts from short-term to long-term memory.</p>
      </div>
      <div class="tip-card">
        <div class="tip-icon">🔬</div>
        <h3>Science-Specific Tips</h3>
        <ul>
          <li>Draw and label every diagram you encounter</li>
          <li>Write out formulas with units every time</li>
          <li>Know the difference between similar terms (ex. osmosis and diffusion)</li>
          <li>Practice calculations step by step</li>
        </ul>
      </div>
      <div class="tip-card">
        <div class="tip-icon">✅</div>
        <h3>Before a Unit Test</h3>
        <ul>
          <li>Complete all section practice tests first</li>
          <li>Re-read summary sections in your notes</li>
          <li>Write out key vocabulary definitions</li>
          <li>Try explaining concepts out loud</li>
          <li>Get a full night's sleep. If you cram its over bro ngl</li>
        </ul>
      </div>
      <div class="tip-card tip-highlight">
        <div class="tip-icon">🌱</div>
        <h3>Growth Mindset</h3>
        <p>LOWK NO ONE CARES IF YOU FAIL. Education starts with mistakes. Even though I did pretty well in science, I flunked some tests! I also got -5 on a title page for scientific inaccuracy!</p>
      </div>
    </div>
  </div>

  <!-- ── INFO VIEW ── -->
  <div class="view" id="view-info">
    <h2 class="section-heading">About Chinquest</h2>
    <div class="info-hero">
      <h2>Your Science Companion </h2>
      <p>Chinquest is a student-made website made by Lowell Saldivar who formerly attended H.E. Beriault from 2023-2026. The website was made in hopes of restoring a very good learning website edquest.ca, hence the name Chinquest. If anything breaks please tell Mr. Chin or email me at reylo0507@yahoo.com.</p>
    </div>
    <div class="info-grid">
      <div class="info-stat">
        <div class="num">3</div>
        <div class="lbl">Grades (7–9)</div>
      </div>
      <div class="info-stat">
        <div class="num">15</div>
        <div class="lbl">Total Units</div>
      </div>
      <div class="info-stat">
        <div class="num">60</div>
        <div class="lbl">Sections</div>
      </div>
      <div class="info-stat">
        <div class="num">120</div>
        <div class="lbl">Notes + Practice Tests</div>
      </div>
      <div class="info-stat">
        <div class="num">15</div>
        <div class="lbl">Unit Tests</div>
      </div>
      <div class="info-stat">
        <div class="num">135</div>
        <div class="lbl">Total PDF Resources</div>
      </div>
    </div>
    <div class="textbook-callout">
      <div class="book-icon">📗</div>
      <div>
        <h3>Science in Action Series</h3>
        <p>This site is designed as a companion to the Science in Action textbook series used in Alberta schools. THIS DOES NOT MEAN YOU SHOULD ABANDON YOUR CURRENT NOTES AND FLASHCARDS. THE CURRICULUM IS BOUND TO CHANGE</p>
      </div>
    </div>
    <div style="margin-top: 1.2rem; background: rgba(10,40,15,0.8); border: 1px solid rgba(77,184,102,0.18); border-radius: 14px; padding: 1.5rem 2rem;">
      <h3 style="font-family: 'Syne', sans-serif; color: var(--green-pale); margin-bottom: 0.6rem;">How to Use This Site</h3>
      <ol style="padding-left: 1.2rem; color: var(--text-muted); font-size: 0.87rem; line-height: 2;">
        <li>Select your grade from the home screen</li>
        <li>Choose the unit you're currently studying</li>
        <li>Click <em style="color: #5ce0e6;">Notes</em> to study the section material</li>
        <li>Click <em style="color: var(--green-light);">Practice Test</em> to test your knowledge</li>
        <li>Complete all sections, then take the <em style="color: var(--amber-light);">Unit Test</em></li>
      </ol>
    </div>
  </div>

</main>

<!-- PDF Modal -->
<div class="modal-overlay" id="pdfModal" onclick="closeModal(event)">
  <div class="modal">
    <button class="modal-close" onclick="closePdfModal()">✕</button>
    <div class="modal-icon" id="modalIcon">📄</div>
    <h2 id="modalTitle">Resource Title</h2>
    <p id="modalDesc">Description of this resource.</p>
    <div class="modal-path" id="modalPath">/pdfs/grade7/unit-a/section-1.0-notes.pdf</div>
    <div class="modal-actions">
      <button class="modal-btn secondary" onclick="closePdfModal()">Cancel</button>
      <button class="modal-btn primary" id="modalOpenBtn">Open PDF ↗</button>
    </div>
  </div>
</div>

<script>
const unitNames = {
  7: { A:'Interactions & Ecosystems', B:'Plants for Food & Fibre', C:'Heat & Temperature', D:'Structures & Forces', E:'Planet Earth' },
  8: { A:'Cells & Systems', B:'Mix & Flow of Matter', C:'Light & Optical Systems', D:'Mechanical Systems', E:'Fresh & Salt Water' },
  9: { A:'Biological Diversity', B:'Matter & Chemical Change', C:'Environmental Chemistry', D:'Electrical Principles', E:'Space Exploration' }
};
const sectionTitles = {
  7: {
    A: ['Ecosystems & Interactions','Energy Flow in Ecosystems','Nutrient Cycling','Human Impact on Ecosystems'],
    B: ['Plant Structures & Functions','Photosynthesis & Respiration','Plant Reproduction','Agriculture & Biotechnology'],
    C: ['Particle Theory & Heat','Measuring Temperature','Heat Transfer','Effects of Heat'],
    D: ['Properties of Structures','Forces on Structures','Structural Design','Materials & Failure'],
    E: ['The Rock Cycle','Plate Tectonics','Erosion & Deposition','Geological History'],
  },
  8: {
    A: ['Cell Structure & Function','Cell Division','Body Systems Overview','System Interactions'],
    B: ['Properties of Fluids','Density & Buoyancy','Viscosity & Flow','Fluids in Technology'],
    C: ['Properties of Light','Reflection & Mirrors','Refraction & Lenses','Optical Devices'],
    D: ['Simple Machines','Mechanical Advantage','Compound Machines','Machines in Industry'],
    E: ['Properties of Water','Freshwater Systems','Ocean Systems','Water & Climate'],
  },
  9: {
    A: ['Classification Systems','Natural Selection','Adaptation','Biodiversity & Conservation'],
    B: ['Atomic Theory','Chemical Bonding','Chemical Reactions','Acids, Bases & Salts'],
    C: ['Water Quality','Air Pollution','Soil Chemistry','Environmental Monitoring'],
    D: ['Static Electricity','Electric Circuits','Ohm\'s Law','Electrical Energy'],
    E: ['The Solar System','Stars & Galaxies','Space Exploration History','Future of Space Travel'],
  }
};

let currentGrade = null;
let currentUnit = null;

function showView(name) {
  document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
  document.getElementById('view-' + name).classList.add('active');
  document.querySelectorAll('.header-nav button').forEach((b,i) => {
    b.classList.remove('active');
    if (['home','tips','info'][i] === name) b.classList.add('active');
  });
}

function selectGrade(g) {
  currentGrade = g;
  currentUnit = null;
  document.getElementById('units-panel').style.display = 'block';
  document.getElementById('sections-panel').style.display = 'none';
  document.getElementById('bc-grade').textContent = 'Grade ' + g;
  document.getElementById('units-heading').textContent = 'Units — Grade ' + g;
  const grid = document.getElementById('unitGrid');
  grid.innerHTML = '';
  ['A','B','C','D','E'].forEach(u => {
    const card = document.createElement('div');
    card.className = 'unit-card';
    card.innerHTML = `<div class="unit-letter">${u}</div><div class="unit-name">${unitNames[g][u]}</div><div class="unit-test-badge">Unit Test</div>`;
    card.onclick = () => selectUnit(u);
    grid.appendChild(card);
  });
}

function clearGrade() {
  currentGrade = null;
  document.getElementById('units-panel').style.display = 'none';
  document.getElementById('sections-panel').style.display = 'none';
}

function clearUnit() {
  currentUnit = null;
  document.getElementById('units-panel').style.display = 'block';
  document.getElementById('sections-panel').style.display = 'none';
}

function selectUnit(u) {
  currentUnit = u;
  document.getElementById('units-panel').style.display = 'none';
  document.getElementById('sections-panel').style.display = 'block';
  document.getElementById('bc-grade2').textContent = 'Grade ' + currentGrade;
  document.getElementById('bc-unit').textContent = 'Unit ' + u;
  document.getElementById('sections-heading').textContent = `Unit ${u} — ${unitNames[currentGrade][u]}`;
  const container = document.getElementById('sectionsContainer');
  container.innerHTML = '';
  const titles = sectionTitles[currentGrade][u];
  [1,2,3,4].forEach((n,i) => {
    const row = document.createElement('div');
    row.className = 'section-row';
    const secId = n + '.0';
    row.innerHTML = `
      <div class="section-id">${secId}</div>
      <div class="section-title">${titles[i]}<small>Grade ${currentGrade} · Unit ${u} · Section ${secId}</small></div>
      <div class="section-actions">
        <button class="btn-action btn-notes" onclick="openPdf('notes','${currentGrade}','${u}','${secId}','${titles[i]}')">📄 Notes</button>
        <button class="btn-action btn-practice" onclick="openPdf('practice','${currentGrade}','${u}','${secId}','${titles[i]}')">✏️ Practice Test</button>
      </div>`;
    container.appendChild(row);
  });
  const testRow = document.createElement('div');
  testRow.className = 'unit-test-row';
  testRow.innerHTML = `
    <div class="unit-test-label"><span class="star">⭐</span><span>Unit ${u} Test — ${unitNames[currentGrade][u]}</span></div>
    <button class="btn-unit-test" onclick="openPdf('test','${currentGrade}','${u}','','Unit ${u} Test')">📋 Unit Test</button>`;
  container.appendChild(testRow);
}

function openPdf(type, grade, unit, section, title) {
  const modal = document.getElementById('pdfModal');
  const icon = document.getElementById('modalIcon');
  const t = document.getElementById('modalTitle');
  const desc = document.getElementById('modalDesc');
  const path = document.getElementById('modalPath');
  const btn = document.getElementById('modalOpenBtn');

  let iconEl, titleText, descText, pathText;
  if (type === 'notes') {
    iconEl = '📄'; titleText = `Section ${section} Notes`;
    descText = `Notes for Grade ${grade} · Unit ${unit} · Section ${section}: ${title}`;
    pathText = `/pdfs/grade${grade}/unit-${unit.toLowerCase()}/section-${section.replace('.','')}-notes.pdf`;
    icon.className = 'modal-icon notes';
  } else if (type === 'practice') {
    iconEl = '✏️'; titleText = `Section ${section} Practice Test`;
    descText = `Practice test for Grade ${grade} · Unit ${unit} · Section ${section}: ${title}`;
    pathText = `/pdfs/grade${grade}/unit-${unit.toLowerCase()}/section-${section.replace('.','')}-practice.pdf`;
    icon.className = 'modal-icon practice';
  } else {
    iconEl = '📋'; titleText = `Unit ${unit} Test`;
    descText = `Full unit test for Grade ${grade} · Unit ${unit}: ${unitNames[grade][unit]}`;
    pathText = `/pdfs/grade${grade}/unit-${unit.toLowerCase()}/unit-test.pdf`;
    icon.className = 'modal-icon test';
  }
  icon.textContent = iconEl;
  t.textContent = titleText;
  desc.textContent = descText;
  path.textContent = pathText;
  btn.onclick = () => { window.open(pathText, '_blank'); };
  modal.classList.add('open');
}

function closeModal(e) {
  if (e.target === document.getElementById('pdfModal')) closePdfModal();
}
function closePdfModal() {
  document.getElementById('pdfModal').classList.remove('open');
}

// ── Falling leaves ──
const leafCanvas = document.getElementById('leafCanvas');
const leafColors = ['rgba(45,138,66,0.55)','rgba(77,184,102,0.4)','rgba(29,94,40,0.6)','rgba(168,228,184,0.35)','rgba(13,115,119,0.35)'];
for (let i = 0; i < 18; i++) {
  const leaf = document.createElement('div');
  leaf.className = 'leaf';
  const size = 10 + Math.random() * 18;
  const color = leafColors[Math.floor(Math.random() * leafColors.length)];
  const left = Math.random() * 100;
  const dur = 8 + Math.random() * 14;
  const delay = Math.random() * -20;
  leaf.style.cssText = `left:${left}%;width:${size}px;height:${size * 0.6}px;animation-duration:${dur}s;animation-delay:${delay}s;`;
  leaf.innerHTML = `<svg viewBox="0 0 40 25" xmlns="http://www.w3.org/2000/svg"><ellipse cx="20" cy="12" rx="19" ry="11" fill="${color}"/><line x1="3" y1="12" x2="37" y2="12" stroke="rgba(45,138,66,0.4)" stroke-width="1.2"/><line x1="20" y1="3" x2="30" y2="20" stroke="rgba(45,138,66,0.3)" stroke-width="0.8"/><line x1="20" y1="3" x2="10" y2="20" stroke="rgba(45,138,66,0.3)" stroke-width="0.8"/></svg>`;
  leafCanvas.appendChild(leaf);
}

// ── Floating formulas ──
const formulasEl = document.getElementById('formulas');
const formulas = [
  'F = ma','E = mc²','PV = nRT','v = λf','ΔE = hf',
  'a² + b² = c²','pH = -log[H⁺]','KE = ½mv²','W = Fd',
  'P = F/A','d = vt','Q = mcΔT','v² = u² + 2as','ρ = m/v',
  'P = IV','F = kx','T = 2π√(L/g)','n = c/v'
];
for (let i = 0; i < 12; i++) {
  const el = document.createElement('div');
  el.className = 'formula-float';
  const formula = formulas[Math.floor(Math.random() * formulas.length)];
  const left = 5 + Math.random() * 90;
  const dur = 20 + Math.random() * 25;
  const delay = Math.random() * -40;
  const size = 11 + Math.random() * 5;
  el.style.cssText = `left:${left}%;animation-duration:${dur}s;animation-delay:${delay}s;font-size:${size}px;`;
  el.textContent = formula;
  formulasEl.appendChild(el);
}
</script>
</body>
</html>
