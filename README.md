<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Crystalized Hub</title>
<style>
  :root {
    --neon: #00ff88;
    --neon-dim: rgba(0,255,136,0.7);
    --neon-glow: rgba(0,255,136,0.25);
    --bg: #050805;
    --bg-2: #0a140a;
    --bg-3: #0f1f12;
    --surface: rgba(12,22,14,0.72);
    --surface-border: rgba(0,255,136,0.14);
    --text: #e2f0e6;
    --text-dim: #94a99a;
    --red: #ff5252;
    --yellow: #ffd740;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html, body {
    width: 100%;
    min-height: 100vh;
    overflow-x: hidden;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    color: var(--text);
    background: radial-gradient(ellipse 85% 60% at 50% -10%, #0f2b18 0%, var(--bg) 60%, #020302 100%);
    background-attachment: fixed;
  }

  body, a, button, .game-header, .tab-btn, .copy-btn, .discord-btn { cursor: none !important; }

  /* Custom cursor */
  .cursor-ring {
    position: fixed;
    top: 0; left: 0;
    width: 36px; height: 36px;
    border: 2.5px solid var(--neon-dim);
    border-radius: 50%;
    transform: translate(-50%, -50%);
    pointer-events: none;
    z-index: 10000;
    box-shadow: 0 0 18px var(--neon-glow), inset 0 0 10px var(--neon-glow);
    transition: width 0.18s, height 0.18s, border-color 0.18s;
    will-change: transform;
  }
  .cursor-ring.hover {
    width: 54px; height: 54px;
    border-color: rgba(0,255,200,0.95);
    box-shadow: 0 0 30px rgba(0,255,170,0.45), inset 0 0 14px rgba(0,255,170,0.25);
  }
  .cursor-point {
    position: fixed;
    top: 0; left: 0;
    width: 6px; height: 6px;
    background: #fff;
    border-radius: 50%;
    transform: translate(-50%, -50%);
    pointer-events: none;
    z-index: 10001;
    box-shadow: 0 0 8px rgba(255,255,255,0.9);
  }

  /* Background boxes layer */
  #bg-layer {
    position: fixed;
    inset: 0;
    z-index: 0;
    pointer-events: none;
    overflow: hidden;
  }
  .bg-box {
    position: absolute;
    background: rgba(0,255,136,0.035);
    border: 1px solid rgba(0,255,136,0.12);
    border-radius: 16px;
    box-shadow: 0 0 28px rgba(0,255,136,0.06);
    pointer-events: none;
    animation-name: boxFloat;
    animation-timing-function: cubic-bezier(0.25, 0.46, 0.45, 0.94);
    animation-fill-mode: forwards;
    will-change: transform, opacity;
  }
  @keyframes boxFloat {
    0%   { transform: translate(0, 0) scale(0.35); opacity: 0; }
    10%  { opacity: 0.85; }
    85%  { opacity: 0.12; }
    100% { transform: translate(28vw, -85vh) scale(1.25); opacity: 0; }
  }

  /* Canvas overlay */
  #fx-canvas {
    position: fixed;
    inset: 0;
    z-index: 1;
    pointer-events: none;
  }

  /* Content wrapper */
  .content-wrapper {
    position: relative;
    z-index: 2;
    padding: 0 18px 80px;
    max-width: 920px;
    margin: 0 auto;
  }

  /* Tabs */
  .tabs {
    position: relative;
    z-index: 3;
    display: flex;
    justify-content: center;
    gap: 14px;
    padding-top: 28px;
    padding-bottom: 8px;
  }
  .tab-btn {
    padding: 12px 28px;
    border-radius: 50px;
    border: 1px solid var(--surface-border);
    background: var(--surface);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    color: var(--neon);
    font-weight: 700;
    font-size: 0.95rem;
    transition: all 0.3s cubic-bezier(0.4,0,0.2,1);
    box-shadow: 0 4px 12px rgba(0,0,0,0.25);
    letter-spacing: 0.3px;
  }
  .tab-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 24px rgba(0,255,136,0.12);
    background: rgba(16,32,20,0.85);
    color: #fff;
  }
  .tab-btn.active {
    background: linear-gradient(135deg, #00c853, #00e676);
    color: #000;
    border-color: transparent;
    box-shadow: 0 8px 28px rgba(0,255,100,0.25);
  }

  /* Hub header */
  .hub-header {
    position: relative;
    z-index: 3;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 14px;
    margin: 18px 0 28px;
  }
  .crystal-icon {
    width: 48px; height: 48px;
    filter: drop-shadow(0 0 10px rgba(0,255,136,0.4));
    animation: crystalPulse 3.2s ease-in-out infinite;
  }
  @keyframes crystalPulse {
    0%, 100% { transform: scale(1) rotate(0deg); filter: drop-shadow(0 0 10px rgba(0,255,136,0.35)); }
    50% { transform: scale(1.08) rotate(4deg); filter: drop-shadow(0 0 20px rgba(0,255,136,0.6)); }
  }
  .hub-title {
    font-size: 2rem;
    font-weight: 900;
    letter-spacing: -0.5px;
    color: #fff;
    text-shadow: 0 0 18px rgba(0,255,136,0.35), 0 2px 8px rgba(0,0,0,0.4);
    background: linear-gradient(90deg, #ffffff, #a5ffce, #00ff88);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .hub-sub {
    display: block;
    font-size: 0.85rem;
    color: var(--neon-dim);
    font-weight: 600;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    margin-top: 2px;
  }

  /* Tab content */
  .tab-content {
    display: none;
    animation: fadeIn 0.5s ease both;
  }
  .tab-content.active { display: block; }
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(14px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* Glass card */
  .glass-card {
    background: var(--surface);
    backdrop-filter: blur(24px);
    -webkit-backdrop-filter: blur(24px);
    border: 1px solid var(--surface-border);
    border-radius: 24px;
    box-shadow: 0 20px 60px rgba(0,0,0,0.35);
    padding: 36px 32px;
  }
  @media (max-width: 640px) {
    .glass-card { padding: 26px 22px; }
    .hub-title { font-size: 1.5rem; }
  }

  h2 {
    font-size: 1.5rem;
    margin-bottom: 14px;
    color: #fff;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  /* Standout text */
  .standout {
    margin: 14px 0 28px;
    font-size: 1.05rem;
    font-weight: 800;
    letter-spacing: 0.5px;
    text-transform: uppercase;
    color: var(--neon);
    text-shadow: 0 0 12px rgba(0,255,136,0.35);
    animation: textGlow 2.5s ease-in-out infinite;
  }
  @keyframes textGlow {
    0%, 100% { opacity: 1; text-shadow: 0 0 12px rgba(0,255,136,0.35); }
    50% { opacity: 0.85; text-shadow: 0 0 24px rgba(0,255,136,0.55); }
  }

  /* Code block */
  .code-wrap {
    position: relative;
    margin: 14px 0 22px;
    border-radius: 16px;
    overflow: hidden;
    background: #070a08;
    border: 1px solid rgba(0,255,136,0.18);
    box-shadow: 0 10px 30px rgba(0,0,0,0.4);
  }
  .code-wrap pre {
    padding: 22px 80px 22px 24px;
    overflow-x: auto;
    font-family: 'SFMono-Regular', Consolas, 'Liberation Mono', Menlo, monospace;
    font-size: 0.82rem;
    line-height: 1.6;
    color: #a5ffce;
    white-space: pre-wrap;
    word-break: break-all;
  }
  .copy-btn {
    position: absolute;
    top: 12px; right: 12px;
    padding: 8px 16px;
    border-radius: 10px;
    border: 1px solid rgba(0,255,136,0.25);
    background: rgba(0,255,136,0.1);
    color: var(--neon);
    font-size: 0.8rem;
    font-weight: 700;
    transition: all 0.25s ease;
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .copy-btn:hover {
    background: rgba(0,255,136,0.2);
    transform: scale(1.05);
    box-shadow: 0 0 16px rgba(0,255,136,0.2);
  }
  .copy-btn.copied {
    background: var(--neon);
    color: #000;
    border-color: var(--neon);
    transform: scale(1.05);
  }

  /* Live status bars */
  .live-status {
    margin: 18px 0 6px;
  }
  .live-status-title {
    font-size: 0.9rem;
    font-weight: 700;
    color: var(--neon-dim);
    letter-spacing: 0.8px;
    text-transform: uppercase;
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .status-row {
    margin-bottom: 14px;
  }
  .status-meta {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 6px;
  }
  .status-name { font-weight: 700; color: #fff; font-size: 0.95rem; }
  .status-tag {
    font-size: 0.72rem;
    font-weight: 800;
    padding: 3px 10px;
    border-radius: 50px;
    text-transform: uppercase;
    letter-spacing: 0.6px;
  }
  .status-tag.up { background: rgba(0,255,136,0.15); color: var(--neon); border: 1px solid rgba(0,255,136,0.3); }
  .status-tag.warn { background: rgba(255,215,64,0.12); color: var(--yellow); border: 1px solid rgba(255,215,64,0.35); }
  .status-tag.down { background: rgba(255,82,82,0.12); color: var(--red); border: 1px solid rgba(255,82,82,0.35); }

  .status-track {
    height: 8px;
    border-radius: 50px;
    background: rgba(255,255,255,0.06);
    overflow: hidden;
    border: 1px solid rgba(255,255,255,0.05);
  }
  .status-fill {
    height: 100%;
    border-radius: 50px;
    width: 0;
    transition: width 1.2s cubic-bezier(0.4,0,0.2,1);
    position: relative;
  }
  .status-fill.up { background: linear-gradient(90deg, #00c853, #00ff88); box-shadow: 0 0 10px rgba(0,255,136,0.3); }
  .status-fill.warn { background: linear-gradient(90deg, #ffab00, #ffd740); box-shadow: 0 0 10px rgba(255,215,64,0.25); }
  .status-fill.down { background: linear-gradient(90deg, #d50000, #ff5252); box-shadow: 0 0 10px rgba(255,82,82,0.25); }
  .status-pct {
    text-align: right;
    font-size: 0.78rem;
    color: var(--text-dim);
    margin-top: 4px;
    font-weight: 600;
  }
  .status-time {
    font-size: 0.75rem;
    color: var(--text-dim);
    margin-top: 10px;
    text-align: right;
    opacity: 0.8;
  }

  /* Games list */
  .games-title {
    font-size: 1.05rem;
    color: var(--neon);
    font-weight: 800;
    margin: 26px 0 12px;
    display: flex;
    align-items: center;
    gap: 8px;
    text-transform: uppercase;
    letter-spacing: 0.8px;
  }
  .game-item {
    border-bottom: 1px solid rgba(0,255,136,0.1);
    overflow: hidden;
  }
  .game-item:last-child { border-bottom: none; }
  .game-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 16px 6px;
    user-select: none;
    transition: color 0.2s;
    font-weight: 700;
    color: #fff;
  }
  .game-header:hover { color: var(--neon); }
  .game-header-left { display: flex; align-items: center; gap: 10px; }
  .game-dot {
    width: 8px; height: 8px;
    background: var(--neon);
    border-radius: 50%;
    box-shadow: 0 0 10px rgba(0,255,136,0.7);
  }
  .toggle-icon {
    font-size: 1.1rem;
    width: 24px; height: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 6px;
    background: rgba(0,255,136,0.1);
    color: var(--neon);
    transition: transform 0.3s ease;
  }
  .game-item.open .toggle-icon { transform: rotate(180deg); }
  .game-body {
    max-height: 0;
    opacity: 0;
    overflow: hidden;
    transition: max-height 0.5s cubic-bezier(0.4,0,0.2,1), opacity 0.35s ease, padding 0.3s ease;
    padding-left: 26px;
    padding-right: 6px;
  }
  .game-item.open .game-body {
    max-height: 420px;
    opacity: 1;
    padding-bottom: 18px;
    padding-top: 4px;
  }

  /* Image placeholder */
  .game-preview {
    width: 100%;
    height: 150px;
    border-radius: 14px;
    background: linear-gradient(135deg, rgba(255,255,255,0.04), rgba(255,255,255,0.02));
    border: 1px dashed rgba(0,255,136,0.2);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-bottom: 14px;
    color: var(--text-dim);
    font-size: 0.85rem;
    font-weight: 600;
    transition: border-color 0.3s, background 0.3s;
  }
  .game-item.open .game-preview:hover {
    border-color: rgba(0,255,136,0.45);
    background: linear-gradient(135deg, rgba(0,255,136,0.06), rgba(0,255,136,0.03));
  }
  .placeholder-icon {
    width: 40px; height: 40px;
    stroke: var(--neon-dim);
    opacity: 0.5;
  }

  .feature-list {
    list-style: none;
    display: grid;
    grid-template-columns: 1fr;
    gap: 8px;
  }
  .feature-list li {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 0.92rem;
    color: var(--text-dim);
    padding: 5px 0;
  }
  .feature-check { color: var(--neon); font-weight: 800; }

  /* Buy tab */
  .buy-wrap { text-align: center; padding: 36px 24px; }
  .buy-heading {
    font-size: 1.7rem;
    color: #fff;
    margin-bottom: 8px;
    font-weight: 900;
    letter-spacing: -0.3px;
    text-shadow: 0 0 18px rgba(0,255,136,0.2);
  }
  .buy-sub {
    color: var(--text-dim);
    margin-bottom: 28px;
    font-size: 0.95rem;
    line-height: 1.5;
  }
  .invite-box {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    padding: 10px 18px;
    background: rgba(255,255,255,0.04);
    border: 1px solid rgba(0,255,136,0.2);
    border-radius: 12px;
    margin-bottom: 10px;
  }
  .invite-code {
    font-family: monospace;
    color: var(--neon);
    font-weight: 700;
    font-size: 1rem;
    letter-spacing: 0.5px;
  }
  .copy-invite-btn {
    padding: 8px 14px;
    border-radius: 8px;
    border: 1px solid rgba(0,255,136,0.25);
    background: rgba(0,255,136,0.1);
    color: var(--neon);
    font-weight: 700;
    font-size: 0.78rem;
    transition: all 0.2s;
    cursor: none;
  }
  .copy-invite-btn:hover { background: rgba(0,255,136,0.2); transform: scale(1.05); }
  .discord-btn {
    display: inline-flex;
    align-items: center;
    gap: 12px;
    padding: 16px 36px;
    background: #5865F2;
    color: #fff;
    border-radius: 14px;
    text-decoration: none;
    font-weight: 700;
    font-size: 1.05rem;
    transition: all 0.3s cubic-bezier(0.4,0,0.2,1);
    box-shadow: 0 8px 24px rgba(88,101,242,0.25);
    border: 1px solid rgba(255,255,255,0.08);
    margin-top: 16px;
  }
  .discord-btn:hover {
    transform: translateY(-4px) scale(1.03);
    box-shadow: 0 16px 40px rgba(88,101,242,0.4);
    background: #4752c4;
  }
  .discord-btn svg { width: 24px; height: 24px; fill: currentColor; }

  /* Info tab */
  .info-text {
    color: var(--text-dim);
    line-height: 1.75;
    font-size: 0.98rem;
  }
  .info-text p + p { margin-top: 18px; }
  .info-badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 6px 14px;
    border-radius: 50px;
    background: rgba(0,255,136,0.08);
    color: var(--neon);
    font-size: 0.78rem;
    font-weight: 800;
    margin-bottom: 16px;
    border: 1px solid rgba(0,255,136,0.15);
    text-transform: uppercase;
    letter-spacing: 0.6px;
  }
  .owner-msg {
    color: #fff;
    font-style: italic;
    font-size: 1.02rem;
    line-height: 1.75;
    margin-bottom: 10px;
  }
  .owner-sig {
    color: var(--neon);
    font-weight: 800;
    font-size: 0.95rem;
    margin-top: 4px;
  }
</style>
</head>
<body>

<!-- Custom cursor -->
<div id="cursor-ring" class="cursor-ring"></div>
<div id="cursor-point" class="cursor-point"></div>

<!-- Floating boxes layer -->
<div id="bg-layer"></div>

<!-- Interactive dots / click FX canvas -->
<canvas id="fx-canvas"></canvas>

<!-- Tabs -->
<div class="tabs">
  <button class="tab-btn active" onclick="showTab('main')">Main</button>
  <button class="tab-btn" onclick="showTab('buy')">Buy</button>
  <button class="tab-btn" onclick="showTab('info')">Information</button>
</div>

<!-- Hub Header -->
<div class="hub-header">
  <svg class="crystal-icon" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
    <defs>
      <linearGradient id="cg1" x1="0" y1="0" x2="1" y2="1">
        <stop offset="0%" stop-color="#b4ffda"/>
        <stop offset="100%" stop-color="#00e676"/>
      </linearGradient>
      <linearGradient id="cg2" x1="1" y1="0" x2="0" y2="1">
        <stop offset="0%" stop-color="#00c853"/>
        <stop offset="100%" stop-color="#00e676"/>
      </linearGradient>
    </defs>
    <path d="M24 3 L40 15 L24 27 L8 15 Z" fill="url(#cg1)" opacity="0.95"/>
    <path d="M24 27 L40 15 L36 39 L24 45 L12 39 L8 15 Z" fill="url(#cg2)" opacity="0.65"/>
    <path d="M24 3 L24 45 M8 15 L24 27 L40 15 M12 39 L24 27 L36 39" stroke="rgba(255,255,255,0.25)" stroke-width="0.8" fill="none"/>
    <path d="M24 27 L24 45" stroke="rgba(255,255,255,0.15)" stroke-width="0.6"/>
  </svg>
  <div>
    <div class="hub-title">Crystalized Hub</div>
    <span class="hub-sub">Premium Roblox Scripting</span>
  </div>
</div>

<div class="content-wrapper">

  <!-- MAIN TAB -->
  <div id="main" class="tab-content active">
    <div class="glass-card">
      <h2>Script Loader</h2>
      <p style="color:var(--text-dim); font-size:0.92rem;">Paste this into your executor to load the latest version.</p>

      <div class="code-wrap">
        <pre id="scriptText">loadstring(game:HttpGet("https://api.jnkie.com/api/v1/luascripts/public/8d2b935df413d020109fe8b10ff37672ba937f791a668940a730149a8467c68f/download"))()</pre>
        <button class="copy-btn" id="copyBtn" onclick="copyScript()">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><rect x="9" y="9" width="13" height="13" rx="2" ry="2"></rect><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"></path></svg>
          Copy
        </button>
      </div>

      <div class="standout">Best free / keyless / paid Hub out there!</div>

      <div class="games-title">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="var(--neon)" stroke-width="2.5"><path d="M21 6H3M21 12H3M21 18H3"/></svg>
        Games we support
      </div>

      <div id="gamesList">
        <!-- Rivals -->
        <div class="game-item">
          <div class="game-header" onclick="toggleGame(this)">
            <div class="game-header-left">
              <div class="game-dot"></div>
              <span>Rivals</span>
            </div>
            <div class="toggle-icon">+</div>
          </div>
          <div class="game-body">
            <div class="game-preview">
              <svg class="placeholder-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
              <span>Preview</span>
            </div>
            <ul class="feature-list">
              <li><span class="feature-check">✦</span> Silent Aimbot with FOV Circle</li>
              <li><span class="feature-check">✦</span> Full Player / Item ESP</li>
              <li><span class="feature-check">✦</span> Wallbang & Bullet Redirect</li>
              <li><span class="feature-check">✦</span> Auto Weapon Swap & Rapid Fire</li>
              <li><span class="feature-check">✦</span> Stream-Proof Overlay Mode</li>
            </ul>
          </div>
        </div>

        <!-- Booga Booga -->
        <div class="game-item">
          <div class="game-header" onclick="toggleGame(this)">
            <div class="game-header-left">
              <div class="game-dot"></div>
              <span>Booga Booga</span>
            </div>
            <div class="toggle-icon">+</div>
          </div>
          <div class="game-body">
            <div class="game-preview">
              <svg class="placeholder-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
              <span>Preview</span>
            </div>
            <ul class="feature-list">
              <li><span class="feature-check">✦</span> Auto Farm Resources</li>
              <li><span class="feature-check">✦</span> Kill Aura & God Swing</li>
              <li><span class="feature-check">✦</span> Speed & Fly Toggle</li>
              <li><span class="feature-check">✦</span> Auto Craft & Base Builder</li>
              <li><span class="feature-check">✦</span> Player ESP & Totem Finder</li>
            </ul>
          </div>
        </div>

        <!-- Survive Zombie Arena -->
        <div class="game-item">
          <div class="game-header" onclick="toggleGame(this)">
            <div class="game-header-left">
              <div class="game-dot"></div>
              <span>Survive Zombie Arena</span>
            </div>
            <div class="toggle-icon">+</div>
          </div>
          <div class="game-body">
            <div class="game-preview">
              <svg class="placeholder-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/></svg>
              <span>Preview</span>
            </div>
            <ul class="feature-list">
              <li><span class="feature-check">✦</span> Auto Shoot with Trigger-Bot</li>
              <li><span class="feature-check">✦</span> God Mode & Infinite HP</li>
              <li><span class="feature-check">✦</span> Infinite Ammo / No Reload</li>
              <li><span class="feature-check">✦</span> Zombie ESP & Radar</li>
              <li><span class="feature-check">✦</span> Auto Cash Farm Loop</li>
            </ul>
          </div>
        </div>
      </div>

      <!-- Live status -->
      <div class="live-status">
        <div class="live-status-title">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M22 12h-4l-3 9L9 3l-3 9H2"/></svg>
          Live Script Status
        </div>
        <div class="status-row">
          <div class="status-meta">
            <span class="status-name">Rivals</span>
            <span class="status-tag up">Operational</span>
          </div>
          <div class="status-track"><div class="status-fill up" id="bar1" data-w="100"></div></div>
          <div class="status-pct">100% Uptime</div>
        </div>
        <div class="status-row">
          <div class="status-meta">
            <span class="status-name">Booga Booga</span>
            <span class="status-tag warn">Degraded</span>
          </div>
          <div class="status-track"><div class="status-fill warn" id="bar2" data-w="94"></div></div>
          <div class="status-pct">94.2% Uptime — reports from some users</div>
        </div>
        <div class="status-row">
          <div class="status-meta">
            <span class="status-name">Survive Zombie Arena</span>
            <span class="status-tag up">Operational</span>
          </div>
          <div class="status-track"><div class="status-fill up" id="bar3" data-w="100"></div></div>
          <div class="status-pct">100% Uptime</div>
        </div>
        <div class="status-time" id="status-time">Last checked: just now</div>
      </div>
    </div>
  </div>

  <!-- BUY TAB -->
  <div id="buy" class="tab-content">
    <div class="glass-card buy-wrap">
      <div class="buy-heading">Ready to dominate?</div>
      <div class="buy-sub">
        Discord links can be blocked on some networks.<br>
        If the button doesn't work, copy the invite below and paste it into Discord.
      </div>

      <div class="invite-box">
        <span class="invite-code">discord.gg/crystalized</span>
        <button class="copy-invite-btn" onclick="copyInvite()" id="copyInviteBtn">Copy Invite</button>
      </div>

      <a href="https://discord.gg/crystalized" target="_blank" class="discord-btn" rel="noopener noreferrer" onclick="return tryDiscord()">
        <svg viewBox="0 0 24 24"><path d="M20.317 4.37a19.791 19.791 0 0 0-4.885-1.515.074.074 0 0 0-.079.037c-.21.375-.444.864-.608 1.25a18.27 18.27 0 0 0-5.487 0 12.64 12.64 0 0 0-.617-1.25.077.077 0 0 0-.079-.037A19.736 19.736 0 0 0 3.677 4.37a.07.07 0 0 0-.032.027C.533 9.046-.32 13.58.099 18.057a.082.082 0 0 0 .031.057 19.9 19.9 0 0 0 5.993 3.03.078.078 0 0 0 .084-.028c.462-.63.874-1.295 1.226-1.994a.076.076 0 0 0-.041-.106 13.107 13.107 0 0 1-1.872-.892.077.077 0 0 1-.008-.128 10.2 10.2 0 0 0 .372-.292.074.074 0 0 1 .077-.01c3.928 1.793 8.18 1.793 12.062 0a.074.074 0 0 1 .078.01c.12.098.246.198.373.292a.077.077 0 0 1-.006.127 12.299 12.299 0 0 1-1.873.892.077.077 0 0 0-.041.107c.36.698.772 1.363 1.225 1.993a.076.076 0 0 0 .084.028 19.839 19.839 0 0 0 6.002-3.03.077.077 0 0 0 .032-.054c.5-5.177-.838-9.674-3.549-13.66a.061.061 0 0 0-.031-.03zM8.02 15.33c-1.183 0-2.157-1.085-2.157-2.419 0-1.333.956-2.419 2.157-2.419 1.21 0 2.176 1.096 2.157 2.42 0 1.333-.956 2.418-2.157 2.418zm7.975 0c-1.183 0-2.157-1.085-2.157-2.419 0-1.333.955-2.419 2.157-2.419 1.21 0 2.176 1.096 2.157 2.42 0 1.333-.946 2.418-2.157 2.418z"/></svg>
        Join Discord to Buy
      </a>
    </div>
  </div>

  <!-- INFORMATION TAB -->
  <div id="info" class="tab-content">
    <div class="glass-card">
      <div class="info-badge">A message from the owner</div>
      <p class="owner-msg">
        I am a owner thats been coding hubs for about 2 years now ive made tons of money from this i allways have the best support and im really careing about my members i love everyone and my friends that joined me on this amazing journey. ive made tons of friends from this and paid so much people for helping 40% of my earnings went to my support and my members. i usally find dupes and release them to free version depends. earning members and executions takes time. i really love everyone that supports me :D
      </p>
      <div class="owner-sig">- Crystalized Owner</div>

      <div style="margin-top:26px;">
        <div class="info-badge">
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg>
          Privacy First
        </div>
        <div class="info-text">
          <p>
            Let's get one thing straight — we don't log your shit. No IP hoarding, no session tracking, no sketchy analytics buried in the fine print. Crystalized was built by people who actually use exploits, not corporate robots trying to monetize your data. We host everything on bare-metal infrastructure with zero retention policies, meaning once you disconnect, there's nothing left to scrape. Your privacy isn't a marketing bullet point here; it's the foundation.
          </p>
          <p>
            What you get is pure, undiluted performance. Our devs push updates daily, not weekly, and every feature is battle-tested before it hits production. Silent aim feels crispy, ESP renders clean without FPS tanking, and our detection evasion isn't just a promise — it's a track record. Free users get the same robust core as paid members because gatekeeping stability is lame. We keep it lean, we keep it mean, and we keep it running. If you're tired of bloated hubs that break every Tuesday, you're home.
          </p>
        </div>
      </div>
    </div>
  </div>

</div>

<script>
/* ================= ANTI-INSPECT ================= */
(function(){
  document.addEventListener('contextmenu', function(e){ e.preventDefault(); });
  document.addEventListener('keydown', function(e){
    if(e.key === 'F12' || (e.ctrlKey && e.shiftKey && (e.key === 'I' || e.key === 'J' || e.key === 'C')) || (e.ctrlKey && (e.key === 'u' || e.key === 'U' || e.key === 'i' || e.key === 'I' || e.key === 's' || e.key === 'S'))){
      e.preventDefault();
      e.stopPropagation();
    }
  });
  setInterval(function(){
    try { (function(){}).constructor('debugger')(); } catch(e){}
  }, 1200);
})();

/* ================= CUSTOM CURSOR ================= */
const ring = document.getElementById('cursor-ring');
const point = document.getElementById('cursor-point');
let ringX = 0, ringY = 0, ptX = 0, ptY = 0;
document.addEventListener('mousemove', function(e){
  ptX = e.clientX; ptY = e.clientY;
});
function curLoop(){
  ringX += (ptX - ringX) * 0.18;
  ringY += (ptY - ringY) * 0.18;
  ring.style.left = ringX + 'px'; ring.style.top = ringY + 'px';
  point.style.left = ptX + 'px'; point.style.top = ptY + 'px';
  requestAnimationFrame(curLoop);
}
curLoop();
document.querySelectorAll('a, button, .game-header, .tab-btn, .copy-btn, .copy-invite-btn, .discord-btn').forEach(function(el){
  el.addEventListener('mouseenter', function(){ ring.classList.add('hover'); });
  el.addEventListener('mouseleave', function(){ ring.classList.remove('hover'); });
});

/* ================= TABS ================= */
function showTab(id){
  document.querySelectorAll('.tab-content').forEach(function(el){ el.classList.remove('active'); });
  document.getElementById(id).classList.add('active');
  document.querySelectorAll('.tab-btn').forEach(function(el){ el.classList.remove('active'); });
  document.querySelector('[onclick="showTab(\''+id+'\')"]').classList.add('active');
}

/* ================= COPY SCRIPT ================= */
function copyScript(){
  var text = 'loadstring(game:HttpGet("https://api.jnkie.com/api/v1/luascripts/public/8d2b935df413d020109fe8b10ff37672ba937f791a668940a730149a8467c68f/download"))()';
  var btn = document.getElementById('copyBtn');
  var originalHTML = btn.innerHTML;

  function finish(){
    btn.classList.add('copied');
    btn.innerHTML = '<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg> Coppied!';
    setTimeout(function(){
      btn.innerHTML = originalHTML;
      btn.classList.remove('copied');
    }, 2000);
  }

  if(navigator.clipboard && window.isSecureContext){
    navigator.clipboard.writeText(text).then(finish);
  } else {
    var ta = document.createElement('textarea');
    ta.value = text;
    ta.style.position = 'fixed';
    ta.style.opacity = '0';
    document.body.appendChild(ta);
    ta.select();
    try{ document.execCommand('copy'); }catch(e){}
    document.body.removeChild(ta);
    finish();
  }
}

/* ================= COPY INVITE ================= */
function copyInvite(){
  var text = 'discord.gg/crystalized';
  var btn = document.getElementById('copyInviteBtn');
  var orig = btn.textContent;
  function done(){
    btn.textContent = 'Copied!';
    btn.style.background = 'rgba(0,255,136,0.25)';
    setTimeout(function(){ btn.textContent = orig; btn.style.background = ''; }, 2000);
  }
  if(navigator.clipboard && window.isSecureContext){
    navigator.clipboard.writeText(text).then(done);
  } else {
    var ta = document.createElement('textarea');
    ta.value = text; ta.style.position='fixed'; ta.style.opacity='0';
    document.body.appendChild(ta); ta.select();
    try{ document.execCommand('copy'); }catch(e){}
    document.body.removeChild(ta);
    done();
  }
}
function tryDiscord(){
  return true;
}

/* ================= ACCORDION ================= */
function toggleGame(header){
  var item = header.parentElement;
  var icon = header.querySelector('.toggle-icon');
  var isOpen = item.classList.contains('open');
  document.querySelectorAll('.game-item').forEach(function(el){
    el.classList.remove('open');
    el.querySelector('.toggle-icon').textContent = '+';
  });
  if(!isOpen){
    item.classList.add('open');
    icon.textContent = '-';
  }
}

/* ================= BACKGROUND BOXES ================= */
function spawnBox(){
  var box = document.createElement('div');
  box.className = 'bg-box';
  var size = Math.random() * 90 + 30;
  box.style.width = size + 'px';
  box.style.height = size + 'px';
  box.style.left = (Math.random() * 90 + 5) + 'vw';
  box.style.top = (Math.random() * 50 + 50) + 'vh';
  box.style.animationDuration = (Math.random() * 10 + 12) + 's';
  document.getElementById('bg-layer').appendChild(box);
  setTimeout(function(){
    if(box.parentElement) box.parentElement.removeChild(box);
  }, 22000);
}
for(var i=0;i<6;i++) spawnBox();
setInterval(spawnBox, 1800);

/* ================= LIVE STATUS BARS ================= */
window.addEventListener('load', function(){
  setTimeout(function(){
    document.getElementById('bar1').style.width = document.getElementById('bar1').getAttribute('data-w') + '%';
    document.getElementById('bar2').style.width = document.getElementById('bar2').getAttribute('data-w') + '%';
    document.getElementById('bar3').style.width = document.getElementById('bar3').getAttribute('data-w') + '%';
  }, 300);
});
function updateStatusTime(){
  var el = document.getElementById('status-time');
  if(el) el.textContent = 'Last checked: ' + new Date().toLocaleTimeString();
}
setInterval(updateStatusTime, 5000);
updateStatusTime();

/* ================= CANVAS DOTS + CLICK FX ================= */
const canvas = document.getElementById('fx-canvas');
const ctx = canvas.getContext('2d');
let W, H, mx = -1000, my = -1000;
function resize(){
  W = canvas.width = window.innerWidth;
  H = canvas.height = window.innerHeight;
}
window.addEventListener('resize', resize);
resize();

const dots = [];
const DOT_COUNT = 85;
for(let i=0;i<DOT_COUNT;i++){
  // ensure minimum velocity so nothing stands still
  let vx = (Math.random()-0.5)*1.2;
  let vy = (Math.random()-0.5)*1.2;
  if(Math.abs(vx)<0.25) vx = vx<0 ? -0.3 : 0.3;
  if(Math.abs(vy)<0.25) vy = vy<0 ? -0.3 : 0.3;
  dots.push({
    x: Math.random()*W, y: Math.random()*H,
    vx: vx, vy: vy,
    r: Math.random()*2 + 1.2,
    alpha: Math.random()*0.6 + 0.35
  });
}

class Burst{
  constructor(x,y){
    this.x=x; this.y=y;
    this.particles=[]; this.ring=null; this.life=1.0;
    // single clean pulse ring
    this.ring = {r: 0, alpha: 0.9, speed: 4};
    // tiny particles only
    for(let i=0;i<18;i++){
      const a=Math.random()*Math.PI*2;
      const sp=Math.random()*2.8+1.2;
      this.particles.push({
        x,y, vx:Math.cos(a)*sp, vy:Math.sin(a)*sp,
        life:1, decay:Math.random()*0.03+0.018,
        size:Math.random()*2+0.6,
        color: Math.random()>0.5 ? '0,255,170' : '255,255,255'
      });
    }
  }
  update(){
    this.particles.forEach(function(p){
      p.x+=p.vx; p.y+=p.vy;
      p.vx*=0.95; p.vy*=0.95;
      p.life-=p.decay;
    });
    this.particles=this.particles.filter(function(p){ return p.life>0; });
    if(this.ring){ this.ring.r+=this.ring.speed; this.ring.alpha-=0.045; if(this.ring.alpha<=0) this.ring=null; }
    this.life-=0.02;
  }
  draw(ctx){
    if(this.ring){
      ctx.beginPath();
      ctx.arc(this.x,this.y,this.ring.r,0,Math.PI*2);
      ctx.strokeStyle='rgba(0,255,170,'+(this.ring.alpha*0.6)+')';
      ctx.lineWidth=1.8;
      ctx.stroke();
      // inner ring echo
      ctx.beginPath();
      ctx.arc(this.x,this.y,this.ring.r*0.6,0,Math.PI*2);
      ctx.strokeStyle='rgba(0,255,170,'+(this.ring.alpha*0.25)+')';
      ctx.lineWidth=1.0;
      ctx.stroke();
    }
    this.particles.forEach(function(p){
      ctx.beginPath();
      ctx.arc(p.x,p.y,p.size*p.life,0,Math.PI*2);
      ctx.fillStyle='rgba('+p.color+','+(p.life*0.9)+')';
      ctx.fill();
    });
  }
  isDead(){ return this.life<=0 && this.particles.length===0 && !this.ring; }
}

let bursts=[];

function animate(){
  ctx.clearRect(0,0,W,H);

  dots.forEach(function(d){
    d.x+=d.vx; d.y+=d.vy;
    if(d.x<-20) d.x=W+20; if(d.x>W+20) d.x=-20;
    if(d.y<-20) d.y=H+20; if(d.y>H+20) d.y=-20;

    ctx.beginPath();
    ctx.arc(d.x,d.y,d.r*3.5,0,Math.PI*2);
    ctx.fillStyle='rgba(0,255,136,'+(d.alpha*0.15)+')';
    ctx.fill();

    ctx.beginPath();
    ctx.arc(d.x,d.y,d.r,0,Math.PI*2);
    ctx.fillStyle='rgba(255,255,255,'+(d.alpha)+')';
    ctx.fill();

    const dx=mx-d.x, dy=my-d.y, dist=Math.sqrt(dx*dx+dy*dy);
    if(dist<190){
      ctx.beginPath(); ctx.moveTo(d.x,d.y); ctx.lineTo(mx,my);
      const a=1-dist/190;
      ctx.strokeStyle='rgba(0,255,170,'+(a*0.75)+')';
      ctx.lineWidth=0.9;
      ctx.shadowBlur=10;
      ctx.shadowColor='rgba(0,255,136,0.4)';
      ctx.stroke();
      ctx.shadowBlur=0;
    }
  });

  bursts.forEach(function(b){ b.update(); b.draw(ctx); });
  bursts=bursts.filter(function(b){ return !b.isDead(); });

  requestAnimationFrame(animate);
}
animate();

window.addEventListener('mousemove', function(e){ mx=e.clientX; my=e.clientY; });
window.addEventListener('click', function(e){ bursts.push(new Burst(e.clientX,e.clientY)); });
</script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'a01d7c659c7871b1',t:'MTc3OTgwNTkxMQ=='};var a=document.createElement('script');a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
