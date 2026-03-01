<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Sachin Yadav — HUD Preview</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&display=swap');

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: #000408;
    color: #00FFFF;
    font-family: 'Share Tech Mono', monospace;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* Scanline overlay */
  body::before {
    content: '';
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,255,255,0.015) 2px,
      rgba(0,255,255,0.015) 4px
    );
    pointer-events: none;
    z-index: 1000;
  }

  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
  }

  /* ═══ BANNER ═══ */
  .banner {
    position: relative;
    background: linear-gradient(135deg, #000408 0%, #000D1A 50%, #000408 100%);
    border: 1px solid #003355;
    border-radius: 4px;
    padding: 24px;
    margin-bottom: 20px;
    overflow: hidden;
    display: flex;
    align-items: center;
    gap: 30px;
  }

  .banner::before {
    content: '';
    position: absolute;
    inset: 0;
    background: 
      linear-gradient(90deg, transparent 89px, #001830 89px, #001830 90px, transparent 90px),
      linear-gradient(0deg, transparent 55px, #001830 55px, #001830 56px, transparent 56px),
      linear-gradient(0deg, transparent 111px, #001830 111px, #001830 112px, transparent 112px),
      linear-gradient(0deg, transparent 167px, #001830 167px, #001830 168px, transparent 168px);
    opacity: 0.4;
  }

  /* Corner brackets */
  .banner::after {
    content: '';
    position: absolute;
    top: 8px; left: 8px;
    width: 30px; height: 30px;
    border-top: 2px solid #00BFFF;
    border-left: 2px solid #00BFFF;
  }

  .corner-tr, .corner-bl, .corner-br {
    position: absolute;
    width: 30px; height: 30px;
  }
  .corner-tr { top: 8px; right: 8px; border-top: 2px solid #00BFFF; border-right: 2px solid #00BFFF; }
  .corner-bl { bottom: 8px; left: 8px; border-bottom: 2px solid #00BFFF; border-left: 2px solid #00BFFF; }
  .corner-br { bottom: 8px; right: 8px; border-bottom: 2px solid #00BFFF; border-right: 2px solid #00BFFF; }

  /* ═══ ARC REACTOR ═══ */
  .arc-reactor {
    position: relative;
    width: 160px;
    height: 160px;
    flex-shrink: 0;
  }

  .arc-reactor svg {
    width: 100%;
    height: 100%;
  }

  .ring-outer { animation: rotateClockwise 8s linear infinite; transform-origin: 80px 80px; }
  .ring-mid { animation: rotateCounter 5s linear infinite; transform-origin: 80px 80px; }
  .ring-fast { animation: rotateClockwise 2.5s linear infinite; transform-origin: 80px 80px; }
  .core-pulse { animation: corePulse 2s ease-in-out infinite; }
  .glow-pulse { animation: glowPulse 2s ease-in-out infinite; }

  @keyframes rotateClockwise { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
  @keyframes rotateCounter { from { transform: rotate(0deg); } to { transform: rotate(-360deg); } }
  @keyframes corePulse {
    0%, 100% { opacity: 1; filter: drop-shadow(0 0 8px #00FFFF); }
    50% { opacity: 0.6; filter: drop-shadow(0 0 20px #00FFFF); }
  }
  @keyframes glowPulse {
    0%, 100% { opacity: 0.6; }
    50% { opacity: 1; }
  }

  /* ═══ BANNER TEXT ═══ */
  .banner-text {
    flex: 1;
    position: relative;
    z-index: 1;
  }

  .system-label {
    font-size: 9px;
    color: #004466;
    letter-spacing: 2px;
    margin-bottom: 6px;
  }

  .jarvis-status {
    font-size: 9px;
    color: #005577;
    margin-bottom: 12px;
  }

  .jarvis-status span { color: #00FF41; }

  .main-name {
    font-family: 'Orbitron', monospace;
    font-size: 38px;
    font-weight: 900;
    background: linear-gradient(90deg, #00FFFF, #0099FF, #FF4500);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    filter: drop-shadow(0 0 10px rgba(0,255,255,0.5));
    margin-bottom: 10px;
    line-height: 1;
    animation: nameFlicker 6s ease-in-out infinite;
  }

  @keyframes nameFlicker {
    0%, 94%, 100% { opacity: 1; }
    95%, 97% { opacity: 0.7; }
    96%, 98% { opacity: 0.3; }
  }

  .divider {
    height: 1px;
    background: linear-gradient(90deg, #004466, #00BFFF, #004466);
    margin: 8px 0;
    position: relative;
  }
  .divider::before, .divider::after {
    content: '';
    position: absolute;
    top: -2px;
    width: 5px; height: 5px;
    border-radius: 50%;
    background: #00FFFF;
    box-shadow: 0 0 6px #00FFFF;
  }
  .divider::before { left: 0; }
  .divider::after { right: 0; }

  .badges {
    display: flex;
    gap: 8px;
    margin: 8px 0;
    flex-wrap: wrap;
  }

  .badge {
    font-size: 10px;
    padding: 3px 10px;
    border-radius: 2px;
    border: 1px solid;
    letter-spacing: 1px;
  }
  .badge-red { border-color: #FF4500; color: #FF4500; background: rgba(255,69,0,0.08); }
  .badge-cyan { border-color: #00BFFF; color: #00BFFF; background: rgba(0,191,255,0.08); }
  .badge-teal { border-color: #00FFFF; color: #00FFFF; background: rgba(0,255,255,0.08); }

  .stats-row {
    display: flex;
    gap: 0;
    margin-top: 10px;
    border-top: 1px solid #002233;
    padding-top: 10px;
  }

  .stat {
    flex: 1;
    padding-right: 16px;
    border-right: 1px solid #002233;
    margin-right: 16px;
  }
  .stat:last-child { border-right: none; margin-right: 0; }

  .stat-label { font-size: 8px; color: #004466; letter-spacing: 2px; margin-bottom: 2px; }
  .stat-value { font-size: 20px; font-weight: bold; color: #00FFFF; text-shadow: 0 0 10px #00FFFF; font-family: 'Orbitron', monospace; }
  .stat-value.blue { color: #0099FF; text-shadow: 0 0 10px #0099FF; font-size: 14px; }

  .bottom-status {
    font-size: 8px;
    color: #003344;
    margin-top: 10px;
    display: flex;
    gap: 12px;
    align-items: center;
    border-top: 1px solid #002233;
    padding-top: 8px;
  }

  .online-badge {
    background: #001500;
    border: 1px solid #00FF41;
    color: #00FF41;
    font-size: 8px;
    padding: 1px 6px;
    border-radius: 2px;
  }

  .max-badge {
    color: #FF4500;
    text-shadow: 0 0 8px #FF4500;
    font-weight: bold;
  }

  /* ═══ TYPING ANIMATION ═══ */
  .typing-section {
    text-align: center;
    padding: 12px;
    background: rgba(0,20,40,0.5);
    border: 1px solid #002233;
    border-radius: 4px;
    margin-bottom: 16px;
    font-size: 14px;
    color: #00FFFF;
    min-height: 36px;
  }

  .cursor { animation: blink 1s step-end infinite; }
  @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }

  /* ═══ SECTIONS ═══ */
  .section {
    background: rgba(0,10,20,0.7);
    border: 1px solid #002233;
    border-radius: 4px;
    padding: 20px;
    margin-bottom: 16px;
    position: relative;
  }

  .section-title {
    font-family: 'Orbitron', monospace;
    font-size: 13px;
    color: #00FFFF;
    letter-spacing: 3px;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, #004466, transparent);
  }

  /* Terminal block */
  .terminal {
    background: #000208;
    border: 1px solid #003355;
    border-radius: 4px;
    padding: 16px;
    font-size: 12px;
    line-height: 2;
    color: #0088AA;
    position: relative;
  }

  .terminal::before {
    content: '● ● ●';
    position: absolute;
    top: 8px; left: 12px;
    font-size: 8px;
    color: #003355;
  }

  .terminal .key { color: #005577; }
  .terminal .val { color: #00BFFF; }
  .terminal .val.hot { color: #FF4500; }
  .terminal .val.green { color: #00FF41; }
  .terminal .border-line { color: #003355; }

  /* Mission table */
  table { width: 100%; border-collapse: collapse; font-size: 12px; }
  th { color: #004466; font-size: 9px; letter-spacing: 2px; text-align: left; padding: 6px 8px; border-bottom: 1px solid #002233; }
  td { padding: 8px; border-bottom: 1px solid #001525; color: #0088AA; }
  tr:hover td { background: rgba(0,100,150,0.1); color: #00BFFF; }
  .status-critical { color: #FF4500; }
  .status-queued { color: #FF8C00; }
  .status-planned { color: #0066FF; }
  .status-lifetime { color: #00FFFF; text-shadow: 0 0 8px #00FFFF; }

  /* Stats cards */
  .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .stat-card {
    background: #000208;
    border: 1px solid #003355;
    border-radius: 4px;
    padding: 16px;
    text-align: center;
  }
  .stat-card-title { font-size: 9px; color: #004466; letter-spacing: 2px; margin-bottom: 8px; }
  .stat-card-value { font-family: 'Orbitron', monospace; font-size: 28px; color: #00FFFF; text-shadow: 0 0 15px #00FFFF; }
  .stat-card-sub { font-size: 9px; color: #003355; margin-top: 4px; }

  /* Dialogue */
  .dialogue { font-size: 13px; line-height: 2.2; }
  .dialogue .speaker { color: #FF4500; font-weight: bold; }
  .dialogue .speaker.blue { color: #00BFFF; }
  .dialogue .line { color: #00BFFF; }

  /* Footer quote */
  .footer-quote {
    background: #000208;
    border: 1px solid #003355;
    border-top: 2px solid #FF4500;
    padding: 20px;
    font-size: 13px;
    line-height: 2;
    text-align: center;
    color: #0088AA;
    border-radius: 4px;
    margin-bottom: 16px;
  }
  .footer-quote .quote-text { color: #00BFFF; font-style: italic; font-size: 15px; }
  .footer-quote .sig { color: #FF4500; margin-top: 8px; font-family: 'Orbitron', monospace; font-size: 11px; }

  /* Footer wave simulation */
  .footer-wave {
    text-align: center;
    padding: 12px;
    background: linear-gradient(90deg, #FF4500, #0066FF, #00FFFF);
    height: 6px;
    border-radius: 3px;
    margin-bottom: 8px;
  }

  /* Activity bar simulation */
  .activity-bar {
    display: flex;
    align-items: flex-end;
    gap: 3px;
    height: 60px;
    padding: 0 4px;
  }
  .bar {
    flex: 1;
    background: linear-gradient(0deg, #FF4500, #00FFFF);
    border-radius: 2px 2px 0 0;
    opacity: 0.7;
    animation: barLoad 0.5s ease-out both;
  }
  @keyframes barLoad { from { height: 0; } }
</style>
</head>
<body>
<div class="container">

  <!-- BANNER -->
  <div class="banner">
    <div class="corner-tr"></div>
    <div class="corner-bl"></div>
    <div class="corner-br"></div>

    <!-- Arc Reactor SVG -->
    <div class="arc-reactor">
      <svg viewBox="0 0 160 160" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <filter id="glow"><feGaussianBlur stdDeviation="3" result="b"/><feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
          <filter id="glow2"><feGaussianBlur stdDeviation="7" result="b"/><feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge></filter>
          <radialGradient id="bg"><stop offset="0%" style="stop-color:#00FFFF;stop-opacity:0.2"/><stop offset="100%" style="stop-color:#000408;stop-opacity:0"/></radialGradient>
        </defs>
        <!-- Glow base -->
        <circle cx="80" cy="80" r="75" fill="url(#bg)"/>
        <!-- Outer rings -->
        <circle cx="80" cy="80" r="70" fill="none" stroke="#001830" stroke-width="1"/>
        <g class="ring-outer">
          <circle cx="80" cy="80" r="64" fill="none" stroke="#003366" stroke-width="1" stroke-dasharray="8 4"/>
          <circle cx="80" cy="16" r="4" fill="#00BFFF" filter="url(#glow)"/>
          <circle cx="80" cy="144" r="4" fill="#00BFFF" filter="url(#glow)"/>
          <circle cx="16" cy="80" r="4" fill="#00FFFF" filter="url(#glow)"/>
          <circle cx="144" cy="80" r="4" fill="#00FFFF" filter="url(#glow)"/>
        </g>
        <!-- Mid ring -->
        <g class="ring-mid">
          <circle cx="80" cy="80" r="52" fill="none" stroke="#004488" stroke-width="1.5" stroke-dasharray="15 6 3 6"/>
          <line x1="80" y1="28" x2="80" y2="20" stroke="#00BFFF" stroke-width="2"/>
          <line x1="80" y1="132" x2="80" y2="140" stroke="#00BFFF" stroke-width="2"/>
        </g>
        <!-- Inner area -->
        <circle cx="80" cy="80" r="42" fill="#000510" stroke="#0066AA" stroke-width="1.5"/>
        <!-- Hex -->
        <polygon points="80,60 95,70 95,90 80,100 65,90 65,70" fill="none" stroke="#00AADD" stroke-width="1.2" opacity="0.7" class="glow-pulse"/>
        <polygon points="80,65 91,72 91,88 80,95 69,88 69,72" fill="none" stroke="#00CCFF" stroke-width="0.6" opacity="0.4"/>
        <!-- Fast ring -->
        <g class="ring-fast">
          <circle cx="80" cy="80" r="36" fill="none" stroke="#0088CC" stroke-width="2" stroke-dasharray="20 5 3 5"/>
        </g>
        <!-- Core -->
        <circle cx="80" cy="80" r="26" fill="#001830" stroke="#00EEFF" stroke-width="2" filter="url(#glow2)" class="core-pulse"/>
        <circle cx="80" cy="80" r="18" fill="#003366"/>
        <circle cx="80" cy="80" r="11" fill="#0066FF" opacity="0.9" filter="url(#glow)"/>
        <circle cx="80" cy="80" r="6" fill="#00FFFF" filter="url(#glow2)" class="core-pulse"/>
        <!-- Tick marks -->
        <g stroke="#00BFFF" stroke-width="1.5" opacity="0.6">
          <line x1="80" y1="10" x2="80" y2="18"/>
          <line x1="80" y1="142" x2="80" y2="150"/>
          <line x1="10" y1="80" x2="18" y2="80"/>
          <line x1="142" y1="80" x2="150" y2="80"/>
        </g>
      </svg>
    </div>

    <!-- Text -->
    <div class="banner-text">
      <div class="system-label">// STARK INDUSTRIES · AUTHORIZED ACCESS ONLY //</div>
      <div class="jarvis-status">JARVIS v4.2 · USER: bunnybot1121 · STATUS: <span>● ONLINE</span></div>
      <div class="main-name">SACHIN YADAV</div>
      <div class="divider"></div>
      <div class="badges">
        <span class="badge badge-red">⚙ DEVELOPER</span>
        <span class="badge badge-cyan">✦ STUDENT</span>
        <span class="badge badge-teal">💡 INNOVATOR</span>
      </div>
      <div class="stats-row">
        <div class="stat">
          <div class="stat-label">COMMITS</div>
          <div class="stat-value">207+</div>
        </div>
        <div class="stat">
          <div class="stat-label">STACK</div>
          <div class="stat-value blue">HTML · CSS</div>
        </div>
        <div class="stat">
          <div class="stat-label">DIRECTIVE</div>
          <div class="stat-value blue" style="font-size:11px">BUILD EVERYTHING</div>
        </div>
      </div>
      <div class="bottom-status">
        ARC REACTOR: <span class="online-badge">ONLINE</span>
        THREAT: ZERO &nbsp;|&nbsp; BUILD LEVEL: <span class="max-badge">MAXIMUM 🔥</span>
      </div>
    </div>
  </div>

  <!-- TYPING -->
  <div class="typing-section" id="typing"></div>

  <!-- TERMINAL -->
  <div class="section">
    <div class="section-title">⚙ SYSTEM IDENTIFICATION</div>
    <div class="terminal">
      <br/>
      <span class="border-line">╔══════════════════════════════════════════════════╗</span><br/>
      <span class="border-line">║</span>       <span style="color:#FF4500;font-family:Orbitron,monospace">⚡ STARK INDUSTRIES TERMINAL ⚡</span>       <span class="border-line">║</span><br/>
      <span class="border-line">╠══════════════════════════════════════════════════╣</span><br/>
      <span class="border-line">║</span>  <span class="key">AGENT      »</span>  <span class="val">Sachin Yadav</span>                      <span class="border-line">║</span><br/>
      <span class="border-line">║</span>  <span class="key">ALIAS      »</span>  <span class="val">bunnybot1121</span>                     <span class="border-line">║</span><br/>
      <span class="border-line">║</span>  <span class="key">CLEARANCE  »</span>  <span class="val hot">Level 7 — DEVELOPER</span>              <span class="border-line">║</span><br/>
      <span class="border-line">║</span>  <span class="key">LOCATION   »</span>  <span class="val">India 🇮🇳</span>                         <span class="border-line">║</span><br/>
      <span class="border-line">║</span>  <span class="key">ARC CORE   »</span>  <span class="val hot">100 Ideas [OVERCLOCKED 🔥]</span>       <span class="border-line">║</span><br/>
      <span class="border-line">║</span>  <span class="key">DIRECTIVE  »</span>  <span class="val">Build. Break. Improve. Repeat.</span>   <span class="border-line">║</span><br/>
      <span class="border-line">║</span>  <span class="key">STATUS     »</span>  <span class="val green">██████████████░░  BUILDING</span>      <span class="border-line">║</span><br/>
      <span class="border-line">╚══════════════════════════════════════════════════╝</span>
    </div>
  </div>

  <!-- MISSION LOG -->
  <div class="section">
    <div class="section-title">⚡ JARVIS — MISSION LOG</div>
    <table>
      <tr><th>PROTOCOL</th><th>STATUS</th><th>PRIORITY</th></tr>
      <tr><td>🔨 Mastering HTML &amp; CSS</td><td class="status-critical">IN PROGRESS</td><td class="status-critical">🔴 CRITICAL</td></tr>
      <tr><td>🧠 Learning JavaScript</td><td class="status-queued">QUEUED</td><td class="status-queued">🟠 HIGH</td></tr>
      <tr><td>🚀 Build a full website</td><td class="status-critical">IN PROGRESS</td><td class="status-critical">🔴 CRITICAL</td></tr>
      <tr><td>🌍 Make something the world uses</td><td class="status-planned">PLANNED</td><td class="status-planned">🟡 MEDIUM</td></tr>
      <tr><td>🦾 Become the Tony Stark of code</td><td class="status-lifetime">LIFETIME GOAL</td><td class="status-lifetime">⚡ INFINITE</td></tr>
    </table>
  </div>

  <!-- AVENGERS DIALOGUE -->
  <div class="section">
    <div class="section-title">🎬 THE AVENGERS TEST</div>
    <div class="dialogue">
      <div><span class="speaker">STARK :</span> <span class="line">"Kid, you write HTML?"</span></div>
      <div><span class="speaker blue">SACHIN:</span> <span class="line">"Yes sir."</span></div>
      <div><span class="speaker">STARK :</span> <span class="line">"CSS too?"</span></div>
      <div><span class="speaker blue">SACHIN:</span> <span class="line">"Yes sir."</span></div>
      <div><span class="speaker">STARK :</span> <span class="line">"Ideas?"</span></div>
      <div><span class="speaker blue">SACHIN:</span> <span class="line">"A hundred of them."</span></div>
      <div><span class="speaker">STARK :</span> <span class="line">*nods* "You're in."</span></div>
      <div><span class="speaker blue">PARKER:</span> <span class="line">"That's literally how I got recruited too."</span></div>
    </div>
  </div>

  <!-- QUOTE -->
  <div class="footer-quote">
    <div class="quote-text">"Tony built his first suit in a cave.<br/>Peter stopped a bus with his bare hands.<br/>I'm just getting started —<br/>but I've got the same fire."</div>
    <div class="sig">— SACHIN YADAV 🦾🕷️</div>
  </div>

  <div class="footer-wave"></div>

</div>

<script>
// Typing animation
const lines = [
  "JARVIS ONLINE. Welcome back, Mr. Yadav. 🦾",
  "Arc Reactor Status: FULLY CHARGED ⚡",
  "100 active ideas detected in neural core 💡",
  "Spider-Sense tingling — new project incoming 🕷️",
  "I am Iron Man. And I build things. 🚀",
  "Not all heroes wear capes — some write code. 💻"
];
let lineIdx = 0, charIdx = 0, deleting = false;
const el = document.getElementById('typing');

function type() {
  const line = lines[lineIdx];
  if (!deleting) {
    charIdx++;
    el.innerHTML = line.slice(0, charIdx) + '<span class="cursor">▮</span>';
    if (charIdx === line.length) { deleting = true; setTimeout(type, 2000); return; }
    setTimeout(type, 50);
  } else {
    charIdx--;
    el.innerHTML = line.slice(0, charIdx) + '<span class="cursor">▮</span>';
    if (charIdx === 0) {
      deleting = false;
      lineIdx = (lineIdx + 1) % lines.length;
      setTimeout(type, 300);
      return;
    }
    setTimeout(type, 25);
  }
}
type();
</script>
</body>
</html>
