<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Swati Choudhary — Indian Treks</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,300;0,400;1,300;1,400&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet" />

  <style>
    /* ─── Reset & Base ─────────────────────────────────────────── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:        #f4f1e8;
      --dark:      #2d3a2e;
      --text:      #2c3225;
      --accent:    #c0652a;
      --taupe:     #c8b99a;
      --muted:     #8faa7e;
      --forest:    #4a6741;
      --font-display: 'Playfair Display', Georgia, serif;
      --font-body:    'DM Sans', system-ui, sans-serif;
    }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--font-body);
      font-size: 15px;
      line-height: 1.6;
      overflow-x: hidden;
      cursor: none;
    }

    a { color: inherit; text-decoration: none; }
    img { display: block; max-width: 100%; }

    /* ─── Custom Cursor ─────────────────────────────────────────── */
    #cursor {
      position: fixed;
      width: 10px; height: 10px;
      background: var(--accent);
      border-radius: 50%;
      pointer-events: none;
      z-index: 9999;
      transform: translate(-50%, -50%);
      transition: transform .15s ease, width .3s ease, height .3s ease, background .3s ease;
    }
    #cursor-ring {
      position: fixed;
      width: 36px; height: 36px;
      border: 1px solid var(--text);
      border-radius: 50%;
      pointer-events: none;
      z-index: 9998;
      transform: translate(-50%, -50%);
      transition: transform .4s ease, width .3s ease, height .3s ease, opacity .3s ease;
      opacity: .5;
    }
    body:has(a:hover) #cursor { transform: translate(-50%,-50%) scale(1.8); background: var(--text); }
    body:has(a:hover) #cursor-ring { width: 50px; height: 50px; }

    /* ─── Loading Screen ─────────────────────────────────────────── */
    #loader {
      position: fixed; inset: 0;
      background: var(--dark);
      z-index: 10000;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 24px;
      transition: opacity .8s ease, visibility .8s ease;
    }
    #loader.hidden { opacity: 0; visibility: hidden; }
    #loader-title {
      font-family: var(--font-display);
      font-size: clamp(3rem, 10vw, 8rem);
      color: var(--bg);
      font-weight: 300;
      letter-spacing: .04em;
    }
    #loader-title span { color: var(--accent); }
    #loader-subtitle {
      font-size: .72rem;
      letter-spacing: .28em;
      text-transform: uppercase;
      color: var(--muted);
      margin-top: -16px;
    }
    #loader-bar-wrap {
      width: 200px; height: 1px;
      background: rgba(255,255,255,.15);
      position: relative; overflow: hidden;
    }
    #loader-bar {
      height: 100%; width: 0;
      background: var(--muted);
      animation: loadProgress 2s ease forwards;
    }
    @keyframes loadProgress { to { width: 100%; } }

    /* ─── Navigation ─────────────────────────────────────────────── */
    nav {
      position: fixed; top: 0; left: 0; right: 0;
      z-index: 1000;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 24px 40px;
      mix-blend-mode: multiply;
    }
    .nav-logo {
      font-family: var(--font-display);
      font-size: 1.1rem;
      font-weight: 300;
      letter-spacing: .1em;
      text-transform: uppercase;
    }
    .nav-logo span { color: var(--accent); font-style: italic; }
    .nav-links {
      display: flex;
      align-items: center;
      gap: 0;
      font-size: .75rem;
      letter-spacing: .12em;
      text-transform: uppercase;
      font-weight: 500;
    }
    .nav-links a {
      padding: 4px 10px;
      opacity: .6;
      transition: opacity .2s;
      position: relative;
    }
    .nav-links a:hover, .nav-links a.active { opacity: 1; }
    .nav-links a.active::before { content: '('; margin-right: -4px; }
    .nav-links a.active::after  { content: ')'; margin-left: -4px; }
    .nav-divider { opacity: .3; font-size: .7rem; }
    .hamburger {
      display: none;
      flex-direction: column;
      gap: 5px;
      cursor: none;
      background: none;
      border: none;
    }
    .hamburger span {
      display: block;
      width: 24px; height: 1px;
      background: var(--text);
      transition: transform .3s, opacity .3s;
    }

    /* ─── Mobile Menu ─────────────────────────────────────────────── */
    #mobile-menu {
      position: fixed; inset: 0;
      background: var(--dark);
      z-index: 999;
      display: flex;
      flex-direction: column;
      justify-content: center;
      padding: 60px 40px;
      transform: translateX(100%);
      transition: transform .5s cubic-bezier(.77,0,.175,1);
    }
    #mobile-menu.open { transform: none; }
    #mobile-menu a {
      font-family: var(--font-display);
      font-size: 11.2vw;
      color: var(--bg);
      font-weight: 300;
      line-height: 1.1;
      opacity: .8;
      transition: opacity .2s;
    }
    #mobile-menu a:hover { opacity: 1; color: var(--muted); }
    #mobile-menu .menu-footer {
      position: absolute; bottom: 40px; left: 40px;
      font-size: .75rem;
      color: var(--muted);
      letter-spacing: .1em;
    }

    /* ─── Hero ───────────────────────────────────────────────────── */
    #hero {
      height: 100vh;
      position: relative;
      display: grid;
      place-items: center;
      overflow: hidden;
    }
    .hero-corner {
      position: absolute;
      font-family: var(--font-display);
      font-size: clamp(2.5rem, 5vw, 4.5rem);
      font-weight: 300;
      font-style: italic;
      line-height: 1;
      pointer-events: none;
    }
    .hero-corner.tl { top: 100px; left: 40px; }
    .hero-corner.tr { top: 100px; right: 40px; text-align: right; }
    .hero-corner.bl { bottom: 60px; left: 40px; }
    .hero-corner.br { bottom: 60px; right: 40px; text-align: right; }
    .hero-corner span { display: block; color: var(--accent); font-style: normal; font-size: .6em; letter-spacing: .15em; font-family: var(--font-body); }

    .hero-carousel {
      position: relative;
      width: min(360px, 80vw);
      aspect-ratio: 9/12;
      overflow: hidden;
    }
    .hero-slide {
      position: absolute; inset: 0;
      opacity: 0;
      transition: opacity .8s ease;
    }
    .hero-slide.active { opacity: 1; }
    .hero-slide img {
      width: 100%; height: 100%;
      object-fit: cover;
      filter: brightness(.88);
    }
    .hero-slide-placeholder {
      width: 100%; height: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-end;
      padding: 28px;
      position: relative;
    }
    .hero-slide-placeholder::after {
      content: '';
      position: absolute; inset: 0;
      background: linear-gradient(to top, rgba(0,0,0,.5) 0%, transparent 60%);
    }
    .hero-slide-placeholder .ph-label {
      font-family: var(--font-display);
      font-size: 1.3rem;
      color: rgba(255,255,255,.9);
      font-style: italic;
      position: relative; z-index: 1;
      text-align: center;
    }
    .hero-slide-placeholder .ph-elev {
      font-size: .62rem;
      letter-spacing: .18em;
      text-transform: uppercase;
      color: rgba(255,255,255,.55);
      margin-top: 4px;
      position: relative; z-index: 1;
    }
    .hero-dots {
      position: absolute;
      bottom: -32px; left: 50%;
      transform: translateX(-50%);
      display: flex; gap: 8px;
    }
    .hero-dot {
      width: 5px; height: 5px;
      border-radius: 50%;
      background: var(--muted);
      cursor: none;
      transition: background .3s, transform .3s;
    }
    .hero-dot.active { background: var(--text); transform: scale(1.4); }

    .hero-tagline {
      position: absolute;
      bottom: 32px; left: 50%;
      transform: translateX(-50%);
      font-size: .72rem;
      letter-spacing: .18em;
      text-transform: uppercase;
      opacity: .5;
      white-space: nowrap;
    }

    /* ─── Section Base ───────────────────────────────────────────── */
    section { padding: 100px 40px; }
    .section-label {
      font-size: .7rem;
      letter-spacing: .2em;
      text-transform: uppercase;
      opacity: .45;
      margin-bottom: 20px;
    }
    .section-title {
      font-family: var(--font-display);
      font-size: clamp(2rem, 4vw, 3.5rem);
      font-weight: 300;
      line-height: 1.15;
      margin-bottom: 40px;
    }
    .section-title em { color: var(--accent); }

    /* ─── Treks / Gallery ─────────────────────────────────────────── */
    #work { background: var(--bg); }
    .view-toggle {
      display: flex; gap: 12px;
      margin-bottom: 40px;
    }
    .toggle-btn {
      font-size: .7rem;
      letter-spacing: .15em;
      text-transform: uppercase;
      padding: 6px 14px;
      border: 1px solid var(--taupe);
      background: none;
      cursor: none;
      font-family: var(--font-body);
      color: var(--text);
      transition: background .25s, color .25s;
    }
    .toggle-btn.active { background: var(--text); color: var(--bg); border-color: var(--text); }

    /* Grid view */
    .gallery-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
      gap: 2px;
    }
    .gallery-item {
      position: relative;
      overflow: hidden;
      aspect-ratio: 3/4;
      cursor: none;
    }
    .gallery-item:nth-child(3n+2) { aspect-ratio: 3/3.5; }
    .gallery-item .g-info {
      position: absolute; inset: 0;
      background: linear-gradient(to top, rgba(20,30,18,.8) 0%, transparent 55%);
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
      padding: 24px;
      opacity: 0;
      transition: opacity .35s;
    }
    .gallery-item:hover .g-info { opacity: 1; }
    .g-name {
      font-family: var(--font-display);
      font-size: 1.6rem;
      color: var(--bg);
      font-weight: 300;
      font-style: italic;
    }
    .g-meta {
      font-size: .68rem;
      letter-spacing: .12em;
      color: var(--muted);
      text-transform: uppercase;
      margin-top: 4px;
    }
    .g-badge {
      display: inline-block;
      font-size: .58rem;
      letter-spacing: .12em;
      text-transform: uppercase;
      padding: 3px 8px;
      border: 1px solid rgba(143,170,126,.4);
      color: var(--muted);
      margin-bottom: 8px;
      align-self: flex-start;
    }

    /* Placeholder colored blocks for gallery */
    .g-placeholder {
      width: 100%; height: 100%;
      display: flex; align-items: flex-end;
      padding: 20px;
      transition: filter .4s, transform .6s cubic-bezier(.25,.46,.45,.94);
    }
    .gallery-item:hover .g-placeholder { filter: brightness(1.08); transform: scale(1.04); }

    /* List view */
    .gallery-list { display: none; }
    .gallery-list.show { display: block; }
    .gallery-grid.list-mode { display: none; }
    .list-item {
      display: grid;
      grid-template-columns: 80px 1fr auto;
      align-items: center;
      gap: 24px;
      padding: 20px 0;
      border-top: 1px solid rgba(44,50,37,.12);
      cursor: none;
      transition: background .2s;
    }
    .list-item:last-child { border-bottom: 1px solid rgba(44,50,37,.12); }
    .list-item:hover { background: rgba(44,50,37,.03); }
    .list-thumb-placeholder { width: 80px; height: 60px; }
    .list-title {
      font-family: var(--font-display);
      font-size: 1.3rem;
      font-weight: 300;
      font-style: italic;
    }
    .list-num {
      font-size: .7rem;
      letter-spacing: .1em;
      opacity: .35;
    }

    /* ─── About ──────────────────────────────────────────────────── */
    #about {
      background: var(--dark);
      color: var(--bg);
      position: relative;
      overflow: hidden;
    }
    #about .section-label { color: var(--muted); }
    #about .section-title { color: var(--bg); }
    .about-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 60px;
      align-items: start;
    }
    .about-body {
      font-size: 1.05rem;
      line-height: 1.8;
      color: rgba(244,241,232,.7);
      font-weight: 300;
    }
    .about-body strong { color: var(--bg); font-weight: 500; }
    .about-body p + p { margin-top: 1.4em; }
    .about-stats {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 24px;
      margin-top: 40px;
    }
    .stat { border-top: 1px solid rgba(244,241,232,.15); padding-top: 16px; }
    .stat-num {
      font-family: var(--font-display);
      font-size: 2.4rem;
      font-weight: 300;
      color: var(--muted);
      line-height: 1;
    }
    .stat-label {
      font-size: .7rem;
      letter-spacing: .12em;
      text-transform: uppercase;
      color: rgba(244,241,232,.4);
      margin-top: 6px;
    }
    .about-photo-stack {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;
    }
    .about-photo-stack .ph-block {
      aspect-ratio: 2/3;
      overflow: hidden;
    }
    .about-photo-stack .ph-block:first-child {
      grid-column: span 2;
      aspect-ratio: 16/9;
    }
    .ph-block-inner {
      width: 100%; height: 100%;
      background: rgba(74,103,65,.2);
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: var(--font-display);
      font-style: italic;
      color: var(--muted);
      font-size: .9rem;
      border: 1px solid rgba(143,170,126,.12);
    }

    /* Gear list */
    .tools-list {
      margin-top: 40px;
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }
    .tool-tag {
      font-size: .68rem;
      letter-spacing: .14em;
      text-transform: uppercase;
      padding: 6px 14px;
      border: 1px solid rgba(244,241,232,.2);
      color: rgba(244,241,232,.5);
    }

    /* ─── Quote / Manifesto ──────────────────────────────────────── */
    #manifesto {
      background: var(--bg);
      text-align: center;
      padding: 120px 40px;
    }
    .manifesto-text {
      font-family: var(--font-display);
      font-size: clamp(1.5rem, 3.5vw, 3rem);
      font-weight: 300;
      font-style: italic;
      line-height: 1.4;
      max-width: 800px;
      margin: 0 auto;
      color: var(--text);
    }
    .manifesto-text em { color: var(--accent); font-style: normal; }
    .manifesto-attr {
      margin-top: 32px;
      font-size: .7rem;
      letter-spacing: .2em;
      text-transform: uppercase;
      opacity: .4;
    }

    /* ─── Contact ────────────────────────────────────────────────── */
    #contact { background: var(--bg); }
    .contact-inner {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 80px;
      align-items: center;
    }
    .contact-cta {
      font-family: var(--font-display);
      font-size: clamp(2rem, 4.5vw, 4rem);
      font-weight: 300;
      line-height: 1.15;
    }
    .contact-cta em { color: var(--accent); font-style: italic; }
    .contact-form { display: flex; flex-direction: column; gap: 20px; }
    .form-field {
      display: flex;
      flex-direction: column;
      gap: 6px;
    }
    .form-field label {
      font-size: .68rem;
      letter-spacing: .15em;
      text-transform: uppercase;
      opacity: .5;
    }
    .form-field input,
    .form-field textarea {
      background: none;
      border: none;
      border-bottom: 1px solid rgba(44,50,37,.25);
      padding: 10px 0;
      font-family: var(--font-body);
      font-size: .95rem;
      color: var(--text);
      outline: none;
      resize: none;
      transition: border-color .25s;
    }
    .form-field input:focus,
    .form-field textarea:focus { border-color: var(--text); }
    .form-field textarea { min-height: 80px; }
    .btn-send {
      align-self: flex-start;
      padding: 14px 32px;
      background: var(--text);
      color: var(--bg);
      font-family: var(--font-body);
      font-size: .72rem;
      letter-spacing: .2em;
      text-transform: uppercase;
      border: none;
      cursor: none;
      position: relative;
      overflow: hidden;
      transition: background .3s;
    }
    .btn-send::after {
      content: '';
      position: absolute; inset: 0;
      background: var(--forest);
      transform: scaleX(0);
      transform-origin: left;
      transition: transform .4s cubic-bezier(.77,0,.175,1);
    }
    .btn-send:hover::after { transform: scaleX(1); }
    .btn-send span { position: relative; z-index: 1; }

    /* ─── Footer ─────────────────────────────────────────────────── */
    footer {
      background: var(--taupe);
      padding: 60px 40px 36px;
    }
    .footer-title {
      font-family: var(--font-display);
      font-size: clamp(3rem, 8vw, 8rem);
      font-weight: 300;
      line-height: 1;
      color: var(--text);
      margin-bottom: 48px;
    }
    .footer-title span { color: rgba(44,50,37,.25); }
    .footer-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 40px;
      margin-bottom: 40px;
    }
    .footer-col-title {
      font-size: .68rem;
      letter-spacing: .2em;
      text-transform: uppercase;
      opacity: .45;
      margin-bottom: 16px;
    }
    .footer-col a {
      display: block;
      font-size: .85rem;
      opacity: .65;
      margin-bottom: 8px;
      transition: opacity .2s;
    }
    .footer-col a:hover { opacity: 1; }
    .footer-bottom {
      border-top: 1px solid rgba(44,50,37,.15);
      padding-top: 24px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      font-size: .7rem;
      opacity: .45;
    }
    .footer-accent { color: var(--accent); }

    /* ─── Scroll Indicator ───────────────────────────────────────── */
    .scroll-line {
      position: fixed;
      right: 32px;
      top: 50%;
      transform: translateY(-50%);
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
      z-index: 100;
    }
    .scroll-line-bar {
      width: 1px;
      height: 60px;
      background: linear-gradient(to bottom, var(--text), transparent);
      opacity: .25;
    }
    .scroll-line-text {
      writing-mode: vertical-rl;
      font-size: .6rem;
      letter-spacing: .18em;
      text-transform: uppercase;
      opacity: .3;
    }

    /* ─── Reveal animations ──────────────────────────────────────── */
    .reveal {
      opacity: 0;
      transform: translateY(30px);
      transition: opacity .8s ease, transform .8s ease;
    }
    .reveal.visible { opacity: 1; transform: none; }
    .reveal-delay-1 { transition-delay: .1s; }
    .reveal-delay-2 { transition-delay: .2s; }
    .reveal-delay-3 { transition-delay: .3s; }

    /* ─── Responsive ─────────────────────────────────────────────── */
    @media (max-width: 768px) {
      nav { padding: 20px 24px; }
      .nav-links { display: none; }
      .hamburger { display: flex; }
      section { padding: 80px 24px; }
      .hero-corner.tr, .hero-corner.bl, .hero-corner.br { display: none; }
      .hero-corner.tl { top: 80px; }
      .about-grid { grid-template-columns: 1fr; gap: 40px; }
      .contact-inner { grid-template-columns: 1fr; gap: 40px; }
      .footer-grid { grid-template-columns: 1fr 1fr; }
      .scroll-line { display: none; }
      .gallery-grid { grid-template-columns: repeat(2, 1fr); }
    }

    @media (max-width: 480px) {
      .gallery-grid { grid-template-columns: 1fr; }
      .footer-grid { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>

<!-- Custom Cursor -->
<div id="cursor"></div>
<div id="cursor-ring"></div>

<!-- Loader -->
<div id="loader">
  <div id="loader-title">Sw<span>a</span>ti</div>
  <div id="loader-subtitle">Indian Treks</div>
  <div id="loader-bar-wrap"><div id="loader-bar"></div></div>
</div>

<!-- Navigation -->
<nav id="navbar">
  <div class="nav-logo">Sw<span>a</span>ti <small style="font-style:normal; font-size:.6em; letter-spacing:.05em; opacity:.5;">— Indian Treks</small></div>
  <div class="nav-links">
    <a href="#work" class="active">Treks</a>
    <span class="nav-divider">/</span>
    <a href="#about">About</a>
    <span class="nav-divider">/</span>
    <a href="#contact">Contact</a>
  </div>
  <button class="hamburger" id="hamburgerBtn" aria-label="Menu">
    <span></span><span></span>
  </button>
</nav>

<!-- Mobile Menu -->
<div id="mobile-menu">
  <a href="#work" onclick="closeMenu()">Treks</a>
  <a href="#about" onclick="closeMenu()">About</a>
  <a href="#contact" onclick="closeMenu()">Contact</a>
  <div class="menu-footer">swati.choudhary@gmail.com</div>
</div>

<!-- Hero -->
<section id="hero">
  <div class="hero-corner tl reveal">
    Into the<br/>mountains<span>India's Trails</span>
  </div>
  <div class="hero-corner tr reveal reveal-delay-1">
    Peaks &amp;<br/>valleys<span>Himalayan Life</span>
  </div>
  <div class="hero-corner bl reveal reveal-delay-2">
    Swati<br/>Choudhary<span>Est. 2018</span>
  </div>
  <div class="hero-corner br reveal reveal-delay-3">
    Based in<br/>India<span>Roaming the Himalayas</span>
  </div>

  <div class="hero-carousel" id="heroCarousel">
    <div class="hero-slide active">
      <div class="hero-slide-placeholder" style="background:linear-gradient(160deg,#3d5e35,#1e3319)">
        <span class="ph-label">Roopkund Trek</span>
        <span class="ph-elev">5,029 m · Uttarakhand</span>
      </div>
    </div>
    <div class="hero-slide">
      <div class="hero-slide-placeholder" style="background:linear-gradient(160deg,#4a6e6e,#1f3a3a)">
        <span class="ph-label">Valley of Flowers</span>
        <span class="ph-elev">3,658 m · Uttarakhand</span>
      </div>
    </div>
    <div class="hero-slide">
      <div class="hero-slide-placeholder" style="background:linear-gradient(160deg,#7a5c3a,#3d2a18)">
        <span class="ph-label">Hampta Pass</span>
        <span class="ph-elev">4,270 m · Himachal Pradesh</span>
      </div>
    </div>
    <div class="hero-slide">
      <div class="hero-slide-placeholder" style="background:linear-gradient(160deg,#5e4a6a,#2a1e38)">
        <span class="ph-label">Kedarkantha</span>
        <span class="ph-elev">3,810 m · Uttarakhand</span>
      </div>
    </div>
    <div class="hero-slide">
      <div class="hero-slide-placeholder" style="background:linear-gradient(160deg,#3a5e5a,#1a2e2c)">
        <span class="ph-label">Sandakphu</span>
        <span class="ph-elev">3,636 m · West Bengal</span>
      </div>
    </div>
    <div class="hero-dots" id="heroDots"></div>
  </div>

  <div class="hero-tagline">One trail at a time — Exploring India's wildest paths</div>
</section>

<!-- Scroll Indicator -->
<div class="scroll-line">
  <div class="scroll-line-bar"></div>
  <div class="scroll-line-text">Scroll</div>
</div>

<!-- Treks -->
<section id="work">
  <div class="section-label reveal">All Expeditions</div>
  <div style="display:flex; align-items:baseline; justify-content:space-between; flex-wrap:wrap; gap:16px;">
    <h2 class="section-title reveal reveal-delay-1">Indian<br/><em>Treks</em></h2>
    <div class="view-toggle reveal reveal-delay-2">
      <button class="toggle-btn active" onclick="setView('grid')">Grid</button>
      <button class="toggle-btn" onclick="setView('list')">List</button>
    </div>
  </div>

  <!-- Grid View -->
  <div class="gallery-grid" id="galleryGrid"></div>

  <!-- List View -->
  <div class="gallery-list" id="galleryList"></div>
</section>

<!-- Manifesto -->
<section id="manifesto">
  <p class="manifesto-text reveal">
    "The mountains are not stadiums where I satisfy my ambition to achieve.
    They are cathedrals where I practice my <em>religion</em> —
    and every trail through India is a verse of that <em>devotion</em>."
  </p>
  <p class="manifesto-attr reveal reveal-delay-1">— Swati Choudhary, Trekker</p>
</section>

<!-- About -->
<section id="about">
  <div class="section-label reveal">About Swati</div>
  <div class="about-grid">
    <div>
      <h2 class="section-title reveal">A life lived<br/>on <em>high trails</em></h2>
      <div class="about-body reveal reveal-delay-1">
        <p>
          I'm Swati Choudhary — a passionate trekker, storyteller, and nature lover
          who has spent years exploring the <strong>diverse mountain landscapes of India</strong>.
          From the rhododendron forests of Uttarakhand to the stark high-altitude deserts
          of Ladakh, every trail has been a chapter of my life.
        </p>
        <p>
          Trekking, for me, is not just about reaching summits. It is about the
          <strong>slow unfolding of landscape</strong>, the rhythm of breath at altitude,
          the warmth of a chai at a dhaba, and the silence that only mountains can offer.
        </p>
        <p>
          Through this journal, I share the routes I've walked, the camps I've slept in,
          and the people I've met along the way — so you can plan your own journey into
          the wild heart of India.
        </p>
      </div>
      <div class="about-stats reveal reveal-delay-2">
        <div class="stat">
          <div class="stat-num">6+</div>
          <div class="stat-label">Years Trekking</div>
        </div>
        <div class="stat">
          <div class="stat-num">40+</div>
          <div class="stat-label">Treks Completed</div>
        </div>
        <div class="stat">
          <div class="stat-num">12</div>
          <div class="stat-label">Indian States</div>
        </div>
        <div class="stat">
          <div class="stat-num">5K+</div>
          <div class="stat-label">Km on Trail</div>
        </div>
      </div>
      <div class="tools-list reveal reveal-delay-3">
        <span class="tool-tag">Himachal Pradesh</span>
        <span class="tool-tag">Uttarakhand</span>
        <span class="tool-tag">Ladakh</span>
        <span class="tool-tag">Sikkim</span>
        <span class="tool-tag">West Bengal</span>
        <span class="tool-tag">Kashmir</span>
        <span class="tool-tag">Arunachal</span>
      </div>
    </div>
    <div class="about-photo-stack reveal reveal-delay-1">
      <div class="ph-block">
        <div class="ph-block-inner" style="background:linear-gradient(160deg,rgba(74,103,65,.35),rgba(30,51,25,.5));">At basecamp, Roopkund</div>
      </div>
      <div class="ph-block">
        <div class="ph-block-inner" style="background:linear-gradient(160deg,rgba(74,110,110,.3),rgba(30,58,58,.4));">Valley of Flowers</div>
      </div>
      <div class="ph-block">
        <div class="ph-block-inner" style="background:linear-gradient(160deg,rgba(122,92,58,.3),rgba(61,42,24,.4));">Hampta crossings</div>
      </div>
    </div>
  </div>
</section>

<!-- Contact -->
<section id="contact">
  <div class="section-label reveal">Get in touch</div>
  <div class="contact-inner">
    <div>
      <h2 class="contact-cta reveal">
        Let's explore<br/>the trails<br/><em>together</em>.
      </h2>
      <p style="margin-top:24px; opacity:.55; font-size:.9rem; line-height:1.7;" class="reveal reveal-delay-1">
        Open to trek partnerships, collaborations,<br/>
        trail guides, and sharing route knowledge.
      </p>
      <p style="margin-top:20px; font-size:.8rem; letter-spacing:.12em; opacity:.4; text-transform:uppercase;" class="reveal reveal-delay-2">
        swati.choudhary@gmail.com
      </p>
    </div>
    <form class="contact-form reveal reveal-delay-1" onsubmit="handleSubmit(event)">
      <div class="form-field">
        <label>Your Name</label>
        <input type="text" placeholder="Riya Sharma" required />
      </div>
      <div class="form-field">
        <label>Email</label>
        <input type="email" placeholder="riya@example.com" required />
      </div>
      <div class="form-field">
        <label>Message</label>
        <textarea placeholder="Tell me which trek you're dreaming of..." rows="4"></textarea>
      </div>
      <button class="btn-send" type="submit"><span>Send Message</span></button>
    </form>
  </div>
</section>

<!-- Footer -->
<footer>
  <div class="footer-title">Sw<span>a</span>ti</div>
  <div class="footer-grid">
    <div class="footer-col">
      <div class="footer-col-title">Navigate</div>
      <a href="#work">Treks</a>
      <a href="#about">About</a>
      <a href="#contact">Contact</a>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">Connect</div>
      <a href="mailto:swati.choudhary@gmail.com">swati.choudhary@gmail.com</a>
      <a href="#">Instagram</a>
      <a href="#">YouTube</a>
    </div>
    <div class="footer-col">
      <div class="footer-col-title">Regions</div>
      <a href="#">Himachal Pradesh</a>
      <a href="#">Uttarakhand</a>
      <a href="#">Ladakh & Kashmir</a>
    </div>
  </div>
  <div class="footer-bottom">
    <span>© 2024 Swati Choudhary. All rights reserved.</span>
    <span>Made with <span class="footer-accent">♥</span> for the mountains</span>
  </div>
</footer>

<script>
  /* ── Trek Data ── */
  const projects = [
    { name: 'Roopkund',          loc: 'Uttarakhand',        elev: '5,029 m', diff: 'Difficult',  color: 'linear-gradient(160deg,#3d5e35,#1e3319)' },
    { name: 'Valley of Flowers', loc: 'Uttarakhand',        elev: '3,658 m', diff: 'Moderate',   color: 'linear-gradient(160deg,#4a7a4a,#24432a)' },
    { name: 'Hampta Pass',       loc: 'Himachal Pradesh',   elev: '4,270 m', diff: 'Moderate',   color: 'linear-gradient(160deg,#7a5c3a,#3d2a18)' },
    { name: 'Kedarkantha',       loc: 'Uttarakhand',        elev: '3,810 m', diff: 'Easy',       color: 'linear-gradient(160deg,#5e4a6a,#2a1e38)' },
    { name: 'Sandakphu',         loc: 'West Bengal',        elev: '3,636 m', diff: 'Moderate',   color: 'linear-gradient(160deg,#3a5e5a,#1a2e2c)' },
    { name: 'Chadar Trek',       loc: 'Ladakh',             elev: '3,200 m', diff: 'Difficult',  color: 'linear-gradient(160deg,#4a607a,#1e2c3a)' },
    { name: 'Goecha La',         loc: 'Sikkim',             elev: '4,940 m', diff: 'Difficult',  color: 'linear-gradient(160deg,#3e6e56,#1a3228)' },
    { name: 'Pin Parvati Pass',  loc: 'Himachal Pradesh',   elev: '5,319 m', diff: 'Very Hard',  color: 'linear-gradient(160deg,#6a4a3a,#30201a)' },
    { name: 'Tarsar Marsar',     loc: 'Kashmir',            elev: '4,100 m', diff: 'Moderate',   color: 'linear-gradient(160deg,#4a6a7a,#1e3040)' },
    { name: 'Har Ki Dun',        loc: 'Uttarakhand',        elev: '3,566 m', diff: 'Easy',       color: 'linear-gradient(160deg,#5a6a3a,#283018)' },
    { name: 'Stok Kangri',       loc: 'Ladakh',             elev: '6,153 m', diff: 'Extreme',    color: 'linear-gradient(160deg,#3a4a6a,#141c30)' },
  ];

  /* ── Build Gallery ── */
  const grid = document.getElementById('galleryGrid');
  const list = document.getElementById('galleryList');

  projects.forEach((p, i) => {
    // Grid item
    const item = document.createElement('div');
    item.className = 'gallery-item reveal';
    item.style.transitionDelay = (i * 0.05) + 's';
    item.innerHTML = `
      <div class="g-placeholder" style="background:${p.color}"></div>
      <div class="g-info">
        <div class="g-badge">${p.diff} · ${p.elev}</div>
        <div class="g-name">${p.name}</div>
        <div class="g-meta">${p.loc}</div>
      </div>`;
    grid.appendChild(item);

    // List item
    const li = document.createElement('div');
    li.className = 'list-item reveal';
    li.innerHTML = `
      <div class="list-thumb-placeholder" style="background:${p.color}; width:80px; height:60px;"></div>
      <div class="list-title">${p.name}<br/><small style="font-size:.7rem; font-family:var(--font-body); font-style:normal; opacity:.45; letter-spacing:.08em;">${p.loc} · ${p.elev} · ${p.diff}</small></div>
      <div class="list-num">0${i + 1}</div>`;
    list.appendChild(li);
  });

  /* ── View Toggle ── */
  function setView(mode) {
    const btns = document.querySelectorAll('.toggle-btn');
    btns.forEach(b => b.classList.remove('active'));
    btns[mode === 'grid' ? 0 : 1].classList.add('active');
    if (mode === 'grid') {
      grid.classList.remove('list-mode');
      list.classList.remove('show');
    } else {
      grid.classList.add('list-mode');
      list.classList.add('show');
    }
  }

  /* ── Hero Carousel ── */
  const slides = document.querySelectorAll('.hero-slide');
  const dotsContainer = document.getElementById('heroDots');
  let current = 0, timer;

  slides.forEach((_, i) => {
    const d = document.createElement('div');
    d.className = 'hero-dot' + (i === 0 ? ' active' : '');
    d.onclick = () => goTo(i);
    dotsContainer.appendChild(d);
  });

  function goTo(n) {
    slides[current].classList.remove('active');
    document.querySelectorAll('.hero-dot')[current].classList.remove('active');
    current = n;
    slides[current].classList.add('active');
    document.querySelectorAll('.hero-dot')[current].classList.add('active');
    resetTimer();
  }
  function resetTimer() {
    clearInterval(timer);
    timer = setInterval(() => goTo((current + 1) % slides.length), 4000);
  }
  resetTimer();

  /* ── Loader ── */
  window.addEventListener('load', () => {
    setTimeout(() => {
      document.getElementById('loader').classList.add('hidden');
      triggerReveal();
    }, 2200);
  });

  /* ── Scroll Reveal ── */
  const observer = new IntersectionObserver(entries => {
    entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); });
  }, { threshold: 0.12 });

  function triggerReveal() {
    document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
  }

  /* ── Custom Cursor ── */
  const cursor = document.getElementById('cursor');
  const ring   = document.getElementById('cursor-ring');
  let mx = 0, my = 0, rx = 0, ry = 0;

  document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; });

  function animCursor() {
    cursor.style.left = mx + 'px';
    cursor.style.top  = my + 'px';
    rx += (mx - rx) * .12;
    ry += (my - ry) * .12;
    ring.style.left = rx + 'px';
    ring.style.top  = ry + 'px';
    requestAnimationFrame(animCursor);
  }
  animCursor();

  /* ── Hamburger Menu ── */
  const menu   = document.getElementById('mobile-menu');
  const burger = document.getElementById('hamburgerBtn');
  let menuOpen = false;

  burger.onclick = () => {
    menuOpen = !menuOpen;
    menu.classList.toggle('open', menuOpen);
    const spans = burger.querySelectorAll('span');
    if (menuOpen) {
      spans[0].style.transform = 'rotate(45deg) translate(4px, 4px)';
      spans[1].style.transform = 'rotate(-45deg) translate(4px, -4px)';
    } else {
      spans[0].style.transform = '';
      spans[1].style.transform = '';
    }
  };

  function closeMenu() {
    menuOpen = false;
    menu.classList.remove('open');
    burger.querySelectorAll('span').forEach(s => s.style.transform = '');
  }

  /* ── Nav Active State on Scroll ── */
  const sections = ['work','about','contact'];
  window.addEventListener('scroll', () => {
    const y = window.scrollY + 200;
    let active = 'work';
    sections.forEach(id => {
      const el = document.getElementById(id);
      if (el && el.offsetTop <= y) active = id;
    });
    document.querySelectorAll('.nav-links a').forEach(a => {
      a.classList.toggle('active', a.getAttribute('href') === '#' + active);
    });
  });

  /* ── Contact Form ── */
  function handleSubmit(e) {
    e.preventDefault();
    const btn = e.target.querySelector('.btn-send span');
    btn.textContent = 'Sent ✓';
    setTimeout(() => btn.textContent = 'Send Message', 3000);
  }
</script>
</body>
</html>
