<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>HMI — Control de Cortina Industrial</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Rajdhani:wght@300;400;500;600;700&family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&display=swap');

:root {
  --bg:       #080a0d;
  --panel:    #0c1018;
  --panel2:   #101520;
  --glass:    rgba(16,21,32,0.92);
  --border:   #1c2535;
  --border2:  #243040;
  --steel:    #2a3a50;
  --accent:   #e8a020;
  --accent2:  #c07010;
  --cyan:     #00d4ff;
  --green:    #00ff88;
  --green2:   #00cc66;
  --red:      #ff2244;
  --red2:     #cc1133;
  --yellow:   #ffdd00;
  --orange:   #ff6600;
  --estop:    #ff1a00;
  --text:     #d0e0f0;
  --text2:    #5a7090;
  --text3:    #8aa0bc;
  --mono:     'Share Tech Mono', monospace;
  --disp:     'Orbitron', sans-serif;
  --ui:       'Rajdhani', sans-serif;
}

* { margin:0; padding:0; box-sizing:border-box; user-select:none; }
html, body { width:100%; height:100%; overflow:hidden; background:var(--bg); }

body {
  font-family: var(--ui);
  color: var(--text);
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  overflow: auto;
}

/* scanline effect */
body::after {
  content:''; position:fixed; inset:0; pointer-events:none; z-index:1000;
  background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px);
}

/* noise overlay */
body::before {
  content:''; position:fixed; inset:0; pointer-events:none; z-index:999; opacity:.018;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  background-size: 128px 128px;
}

/* ── TOPBAR ── */
.topbar {
  display:flex; align-items:center; justify-content:space-between;
  background: linear-gradient(90deg, #0c1018 0%, #101828 50%, #0c1018 100%);
  border-bottom: 2px solid var(--accent2);
  padding: 0 20px; height: 52px; flex-shrink:0;
  position:relative; z-index:10;
}
.topbar::before {
  content:''; position:absolute; bottom:0; left:0; right:0; height:1px;
  background: linear-gradient(90deg, transparent, var(--accent), transparent);
}
.tb-left { display:flex; align-items:center; gap:16px; }
.tb-logo {
  font-family:var(--disp); font-weight:900; font-size:14px; letter-spacing:3px;
  color:var(--accent); text-transform:uppercase;
}
.tb-logo span { color:var(--text2); font-weight:400; }
.tb-sep { width:1px; height:30px; background:var(--border2); }
.tb-title { font-family:var(--ui); font-weight:600; font-size:13px; letter-spacing:2px; color:var(--text3); text-transform:uppercase; }
.tb-right { display:flex; align-items:center; gap:16px; }
.tb-clock { font-family:var(--mono); font-size:13px; color:var(--accent); }
.tb-status { display:flex; align-items:center; gap:6px; font-family:var(--mono); font-size:11px; }
.tb-dot { width:7px; height:7px; border-radius:50%; }

/* ── ESTOP BANNER ── */
.estop-banner {
  background: repeating-linear-gradient(45deg, #1a0000, #1a0000 10px, #220000 10px, #220000 20px);
  border-top: 2px solid var(--estop); border-bottom: 2px solid var(--estop);
  padding: 8px 20px; display:flex; align-items:center; justify-content:space-between;
  flex-shrink:0; display:none;
  animation: estop-flash 0.6s infinite;
}
.estop-banner.active { display:flex; }
@keyframes estop-flash { 0%,100%{opacity:1} 50%{opacity:.7} }
.estop-text { font-family:var(--disp); font-weight:700; font-size:16px; letter-spacing:4px; color:var(--estop); text-shadow:0 0 20px var(--estop); }
.estop-sub { font-family:var(--mono); font-size:11px; color:#ff6655; }

/* ── MAIN LAYOUT ── */
.main {
  display: grid;
  grid-template-columns: 260px 1fr 260px;
  grid-template-rows: 1fr auto;
  gap: 0;
  flex:1;
  min-height: 0;
  padding: 12px;
  gap: 10px;
}

/* ── PANELS ── */
.panel {
  background: var(--glass);
  border: 1px solid var(--border2);
  backdrop-filter: blur(4px);
  position:relative;
  overflow: hidden;
}
.panel::before {
  content:''; position:absolute; top:0; left:0; right:0; height:2px;
  background: linear-gradient(90deg, transparent, var(--accent2), transparent);
}
.panel-header {
  padding: 8px 14px;
  border-bottom: 1px solid var(--border);
  display:flex; align-items:center; gap:8px;
  background: rgba(0,0,0,0.3);
}
.panel-header-icon { font-size:12px; color:var(--accent); }
.panel-header-title { font-family:var(--ui); font-weight:600; font-size:11px; letter-spacing:2px; text-transform:uppercase; color:var(--accent); }
.panel-body { padding:12px; }

/* LEFT PANEL */
.left-panel { grid-column:1; grid-row:1; display:flex; flex-direction:column; gap:8px; }

/* CENTER PANEL */
.center-panel { grid-column:2; grid-row:1; position:relative; }

/* RIGHT PANEL */
.right-panel { grid-column:3; grid-row:1; display:flex; flex-direction:column; gap:8px; }

/* BOTTOM BAR */
.bottom-bar { grid-column:1/-1; grid-row:2; }

/* ── SENSOR ROWS ── */
.sensor-item {
  display:flex; align-items:center; gap:8px;
  padding: 6px 10px; margin-bottom:3px;
  border: 1px solid var(--border); background:rgba(0,0,0,.2);
  position:relative;
}
.sensor-item.active { border-color: var(--cyan); background:rgba(0,212,255,.04); }
.sensor-item.active::before { content:''; position:absolute; left:0; top:0; bottom:0; width:2px; background:var(--cyan); }
.si-led { width:9px; height:9px; border-radius:50%; background:#1a2535; flex-shrink:0; transition:.2s; }
.si-led.on { background:var(--green); box-shadow:0 0 8px var(--green); }
.si-led.on-red { background:var(--red); box-shadow:0 0 8px var(--red); }
.si-led.on-amber { background:var(--accent); box-shadow:0 0 8px var(--accent); }
.si-name { font-family:var(--mono); font-size:11px; color:var(--text3); width:22px; flex-shrink:0; }
.si-desc { font-family:var(--ui); font-size:11px; color:var(--text2); flex:1; line-height:1.2; }
.si-val { font-family:var(--mono); font-size:12px; font-weight:bold; color:var(--text2); width:14px; text-align:right; }
.si-val.v1 { color:var(--cyan); }

/* ── OUTPUT CARDS ── */
.output-cards { display:grid; grid-template-columns:1fr 1fr; gap:6px; }
.out-card {
  padding:10px 8px; border:1px solid var(--border); background:rgba(0,0,0,.3);
  display:flex; flex-direction:column; align-items:center; gap:3px;
  position:relative; overflow:hidden; transition:.3s;
}
.out-card::after { content:''; position:absolute; inset:0; background:radial-gradient(ellipse at 50% 50%, transparent 60%, rgba(0,0,0,.4) 100%); }
.out-card.on { border-color:var(--green); background:rgba(0,255,136,.06); }
.out-card.on-red { border-color:var(--red); background:rgba(255,34,68,.08); }
.oc-name { font-family:var(--disp); font-size:11px; font-weight:700; color:var(--text2); letter-spacing:2px; }
.oc-val { font-family:var(--disp); font-size:22px; font-weight:900; color:var(--text2); transition:.2s; }
.out-card.on .oc-val { color:var(--green); text-shadow:0 0 12px var(--green); }
.out-card.on-red .oc-val { color:var(--red); text-shadow:0 0 12px var(--red); }
.oc-desc { font-family:var(--ui); font-size:9px; color:var(--text2); text-align:center; letter-spacing:.5px; }

/* ── TIMER RING ── */
.timer-section { padding:10px; display:flex; flex-direction:column; align-items:center; gap:8px; }
.timer-ring-wrap { position:relative; width:80px; height:80px; }
.timer-ring-wrap svg { transform:rotate(-90deg); }
.timer-circle-bg { fill:none; stroke:var(--border); stroke-width:5; }
.timer-circle-fill { fill:none; stroke:var(--cyan); stroke-width:5; stroke-linecap:round; transition:stroke-dashoffset .2s linear; }
.timer-ring-label { position:absolute; inset:0; display:flex; flex-direction:column; align-items:center; justify-content:center; }
.timer-ring-val { font-family:var(--disp); font-size:16px; font-weight:700; color:var(--cyan); }
.timer-ring-sub { font-family:var(--mono); font-size:8px; color:var(--text2); }
.timer-status { font-family:var(--mono); font-size:10px; color:var(--text2); }

/* ── CENTER: PROCESS VISUALIZATION ── */
.process-view {
  width:100%; height:100%;
  display:flex; align-items:center; justify-content:center;
  background: radial-gradient(ellipse at 50% 30%, rgba(0,100,150,.06) 0%, transparent 70%);
  position:relative;
}
#process-svg { width:100%; height:100%; max-height:480px; }

/* ── STATE MACHINE INDICATOR ── */
.state-machine { display:flex; flex-direction:column; gap:4px; padding:10px; }
.sm-step {
  display:flex; align-items:center; gap:8px; padding:6px 8px;
  border:1px solid var(--border); background:rgba(0,0,0,.2);
  font-family:var(--ui); font-size:11px; color:var(--text2);
  position:relative; transition:.3s;
}
.sm-step.current { border-color:var(--accent); background:rgba(232,160,32,.08); color:var(--text); }
.sm-step.current::before { content:'▶'; position:absolute; right:8px; color:var(--accent); font-size:8px; }
.sm-step.done { border-color:var(--border); opacity:.4; }
.sm-num { font-family:var(--mono); font-size:10px; width:16px; color:var(--text2); flex-shrink:0; }
.sm-step.current .sm-num { color:var(--accent); }
.sm-dot { width:7px; height:7px; border-radius:50%; background:var(--border2); flex-shrink:0; }
.sm-step.current .sm-dot { background:var(--accent); box-shadow:0 0 6px var(--accent); }

/* ── CONTROLS ── */
.ctrl-btn {
  display:flex; align-items:center; justify-content:center; gap:8px;
  padding: 11px; border:1px solid; cursor:pointer;
  font-family:var(--ui); font-weight:600; font-size:12px; letter-spacing:2px; text-transform:uppercase;
  transition:.15s; position:relative; overflow:hidden;
  clip-path: polygon(0 0, calc(100% - 8px) 0, 100% 8px, 100% 100%, 8px 100%, 0 calc(100% - 8px));
}
.ctrl-btn::before { content:''; position:absolute; inset:0; background:currentColor; opacity:0; transition:.15s; }
.ctrl-btn:hover::before { opacity:.08; }
.ctrl-btn:active::before { opacity:.18; }
.btn-start { background:rgba(0,200,100,.08); border-color:var(--green2); color:var(--green); }
.btn-start:hover { background:rgba(0,255,136,.12); }
.btn-reset { background:rgba(0,150,200,.06); border-color:var(--cyan); color:var(--cyan); }
.btn-auto { background:rgba(232,160,32,.08); border-color:var(--accent2); color:var(--accent); }

/* ── E-STOP ── */
.estop-section {
  padding:12px; display:flex; flex-direction:column; align-items:center; gap:8px;
}
.estop-btn {
  width:90px; height:90px; border-radius:50%;
  background: radial-gradient(circle at 40% 35%, #ff4422, #cc1100 50%, #880800);
  border: 4px solid #ff3311;
  box-shadow: 0 0 0 3px #330500, 0 0 0 5px #ff2200, 0 6px 20px rgba(255,30,0,.4);
  cursor:pointer; display:flex; flex-direction:column; align-items:center; justify-content:center; gap:2px;
  transition:.1s; position:relative; overflow:hidden;
}
.estop-btn::before {
  content:''; position:absolute; top:10%; left:20%; width:35%; height:25%;
  background:rgba(255,255,255,.15); border-radius:50%; transform:rotate(-20deg);
}
.estop-btn:hover { transform:scale(1.04); box-shadow:0 0 0 3px #330500, 0 0 0 5px #ff2200, 0 8px 30px rgba(255,30,0,.6); }
.estop-btn:active { transform:scale(.96); }
.estop-btn.pressed {
  background: radial-gradient(circle at 40% 35%, #cc2200, #880800 50%, #440400);
  box-shadow: 0 0 0 3px #330500, 0 0 0 5px #ff4400, 0 0 30px rgba(255,60,0,.8), 0 0 60px rgba(255,0,0,.3);
  transform:scale(.95) translateY(2px);
  animation: estop-pulse 0.5s infinite;
}
@keyframes estop-pulse { 0%,100%{box-shadow:0 0 0 3px #330500,0 0 0 5px #ff4400,0 0 30px rgba(255,60,0,.8),0 0 60px rgba(255,0,0,.3)} 50%{box-shadow:0 0 0 3px #550000,0 0 0 8px #ff6600,0 0 50px rgba(255,100,0,1),0 0 100px rgba(255,50,0,.5)} }
.estop-icon { font-size:22px; color:#fff; text-shadow:0 2px 4px rgba(0,0,0,.5); }
.estop-label { font-family:var(--disp); font-size:8px; font-weight:700; color:rgba(255,255,255,.9); letter-spacing:1px; text-align:center; }
.estop-state-label { font-family:var(--mono); font-size:10px; text-align:center; }
.estop-desc { font-family:var(--ui); font-size:10px; color:var(--text2); text-align:center; line-height:1.5; }
.sc5-badge {
  background:rgba(255,100,0,.1); border:1px solid var(--orange);
  padding:4px 8px; font-family:var(--mono); font-size:9px; color:var(--orange);
  letter-spacing:1px; text-align:center;
}

/* ── EVENT LOG ── */
.log-area {
  height:90px; overflow-y:auto; padding:6px 10px;
  font-family:var(--mono); font-size:10px;
  background:rgba(0,0,0,.3); border-top:1px solid var(--border);
}
.log-entry { padding:1px 0; border-bottom:1px solid rgba(28,37,53,.5); display:flex; gap:6px; }
.log-time { color:var(--text2); flex-shrink:0; }
.log-msg { }
.log-msg.info { color:var(--cyan); }
.log-msg.ok   { color:var(--green); }
.log-msg.warn { color:var(--yellow); }
.log-msg.err  { color:var(--red); animation:flash-red .3s ease; }
@keyframes flash-red { 0%{background:rgba(255,30,0,.2)} 100%{background:transparent} }

/* ── BOTTOM STATUS BAR ── */
.status-bar {
  background:rgba(0,0,0,.5); border:1px solid var(--border);
  padding:6px 14px; display:flex; align-items:center; gap:20px;
  flex-wrap:wrap;
}
.sb-item { display:flex; align-items:center; gap:6px; font-family:var(--mono); font-size:10px; color:var(--text2); }
.sb-val { color:var(--text3); }
.sb-sep { width:1px; height:14px; background:var(--border); }
.sb-state { font-family:var(--ui); font-size:11px; font-weight:600; letter-spacing:1px; color:var(--accent); flex:1; text-align:right; }

/* ── TREND MINI ── */
.trend-area { padding:8px 10px; }
.trend-label { font-family:var(--mono); font-size:9px; color:var(--text2); margin-bottom:4px; letter-spacing:1px; }
.trend-canvas { width:100%; height:36px; background:rgba(0,0,0,.3); border:1px solid var(--border); display:block; }

/* ── SCROLLBAR ── */
::-webkit-scrollbar { width:4px; } ::-webkit-scrollbar-track { background:transparent; } ::-webkit-scrollbar-thumb { background:var(--border2); }

/* ── RESPONSIVE ── */
@media(max-width:900px) {
  .main { grid-template-columns:1fr; grid-template-rows:auto auto auto auto; }
  .left-panel,.right-panel,.center-panel { grid-column:1; }
  .center-panel { grid-row:1; }
  .left-panel { grid-row:2; }
  .right-panel { grid-row:3; }
  .bottom-bar { grid-row:4; }
  html,body { overflow:auto; }
  #process-svg { max-height:300px; }
}

/* ── FLICKER ANIM ── */
@keyframes flicker { 0%,100%{opacity:1} 92%{opacity:.97} 94%{opacity:.85} 96%{opacity:.97} }

/* ── PULSE ── */
@keyframes glow-pulse { 0%,100%{filter:drop-shadow(0 0 3px currentColor)} 50%{filter:drop-shadow(0 0 8px currentColor)} }
.glow-anim { animation:glow-pulse 2s infinite; }
</style>
</head>
<body>

<!-- TOP BAR -->
<div class="topbar">
  <div class="tb-left">
    <div class="tb-logo">IND<span>·</span>SYS</div>
    <div class="tb-sep"></div>
    <div class="tb-title">HMI — Control de Cortina Industrial v2.0</div>
  </div>
  <div class="tb-right">
    <div class="tb-status">
      <div class="tb-dot" id="conn-dot" style="background:var(--green);box-shadow:0 0 6px var(--green)"></div>
      <span id="conn-label" style="color:var(--green)">EN LÍNEA</span>
    </div>
    <div class="tb-sep"></div>
    <div class="tb-clock" id="clock">00:00:00</div>
  </div>
</div>

<!-- E-STOP BANNER -->
<div class="estop-banner" id="estop-banner">
  <div>
    <div class="estop-text">⚠ PARO DE EMERGENCIA ACTIVADO</div>
    <div class="estop-sub">SC5 — Sensor conductivo activo · Todos los actuadores detenidos</div>
  </div>
  <div style="font-family:var(--mono);font-size:11px;color:#ff6655">REQUIERE RESET MANUAL</div>
</div>

<!-- MAIN GRID -->
<div class="main">

  <!-- ══════════════ LEFT ══════════════ -->
  <div class="left-panel">

    <!-- ENTRADAS -->
    <div class="panel" style="flex:0 0 auto">
      <div class="panel-header">
        <span class="panel-header-icon">⬡</span>
        <span class="panel-header-title">Entradas — Sensores</span>
      </div>
      <div class="panel-body" style="padding:8px 10px">
        <div class="sensor-item" id="si-S1">
          <div class="si-led on" id="led-S1"></div>
          <span class="si-name">S1</span>
          <span class="si-desc">Proximidad IR<br><span style="font-size:9px;color:var(--text2)">Zona libre</span></span>
          <span class="si-val v1" id="val-S1">1</span>
        </div>
        <div class="sensor-item" id="si-S2">
          <div class="si-led on" id="led-S2"></div>
          <span class="si-name">S2</span>
          <span class="si-desc">Cortina arriba<br><span style="font-size:9px;color:var(--text2)">Posición alta</span></span>
          <span class="si-val v1" id="val-S2">1</span>
        </div>
        <div class="sensor-item" id="si-S3">
          <div class="si-led" id="led-S3"></div>
          <span class="si-name">S3</span>
          <span class="si-desc">Cortina media<br><span style="font-size:9px;color:var(--text2)">Posición intermedia</span></span>
          <span class="si-val" id="val-S3">0</span>
        </div>
        <div class="sensor-item" id="si-S4">
          <div class="si-led" id="led-S4"></div>
          <span class="si-name">S4</span>
          <span class="si-desc">Cortina abajo<br><span style="font-size:9px;color:var(--text2)">Posición baja</span></span>
          <span class="si-val" id="val-S4">0</span>
        </div>
        <div class="sensor-item" id="si-P">
          <div class="si-led" id="led-P"></div>
          <span class="si-name">P</span>
          <span class="si-desc">Pulsador inicio<br><span style="font-size:9px;color:var(--text2)">Momentáneo</span></span>
          <span class="si-val" id="val-P">0</span>
        </div>
        <div class="sensor-item" id="si-T">
          <div class="si-led" id="led-T"></div>
          <span class="si-name">T</span>
          <span class="si-desc">Temporizador 10s<br><span style="font-size:9px;color:var(--text2)">1 = completado</span></span>
          <span class="si-val" id="val-T">0</span>
        </div>
        <!-- E-STOP SENSOR -->
        <div class="sensor-item" id="si-SC5" style="border-color:var(--orange);background:rgba(255,102,0,.04)">
          <div class="si-led" id="led-SC5" style="background:#ff6600;box-shadow:none;opacity:.3"></div>
          <span class="si-name" style="color:var(--orange)">SC5</span>
          <span class="si-desc" style="color:var(--text3)">Sensor conductivo<br><span style="font-size:9px;color:var(--orange)">PARO EMERGENCIA</span></span>
          <span class="si-val" id="val-SC5">0</span>
        </div>
      </div>
    </div>

    <!-- CONTROLES -->
    <div class="panel" style="flex:0 0 auto">
      <div class="panel-header">
        <span class="panel-header-icon">◈</span>
        <span class="panel-header-title">Controles Manuales</span>
      </div>
      <div class="panel-body" style="display:flex;flex-direction:column;gap:6px">
        <button class="ctrl-btn btn-start"
          onmousedown="pressP(true)" onmouseup="pressP(false)"
          onmouseleave="pressP(false)" ontouchstart="pressP(true)" ontouchend="pressP(false)">
          ▶ INICIAR (P)
        </button>
        <button class="ctrl-btn btn-auto" onclick="runAutoDemo()">⟳ AUTO DEMO</button>
        <button class="ctrl-btn btn-reset" onclick="fullReset()">↺ RESET SISTEMA</button>
      </div>
    </div>

    <!-- E-STOP PANEL -->
    <div class="panel" style="flex:0 0 auto">
      <div class="panel-header" style="border-color:rgba(255,60,0,.3)">
        <span class="panel-header-icon" style="color:var(--estop)">⬡</span>
        <span class="panel-header-title" style="color:var(--estop)">Paro de Emergencia</span>
      </div>
      <div class="estop-section">
        <div class="sc5-badge">SC5 — SENSOR CONDUCTIVO NC</div>
        <div id="estop-btn" class="estop-btn" onclick="toggleEstop()">
          <div class="estop-icon">⊘</div>
          <div class="estop-label">PARO<br>EMERG.</div>
        </div>
        <div class="estop-state-label" id="estop-state-text" style="color:var(--green)">● NORMAL — SC5 = 0</div>
        <div class="estop-desc">
          Sensor conductivo de contacto.<br>
          <strong style="color:var(--orange)">Activo (1)</strong> = circuito cerrado = PARO.<br>
          <strong style="color:var(--text2)">Inactivo (0)</strong> = circuito abierto = OK.
        </div>
      </div>
    </div>

  </div><!-- /left -->

  <!-- ══════════════ CENTER ══════════════ -->
  <div class="center-panel panel">
    <div class="panel-header">
      <span class="panel-header-icon">⬡</span>
      <span class="panel-header-title">Visualización del Proceso</span>
      <div style="margin-left:auto;font-family:var(--mono);font-size:10px;color:var(--text2)" id="process-state-label">ESTADO: INICIAL</div>
    </div>
    <div class="process-view">
      <svg id="process-svg" viewBox="0 0 640 500" preserveAspectRatio="xMidYMid meet">
        <defs>
          <radialGradient id="lampGrad-green" cx="40%" cy="35%">
            <stop offset="0%" stop-color="#80ffcc"/>
            <stop offset="50%" stop-color="#00cc66"/>
            <stop offset="100%" stop-color="#004420"/>
          </radialGradient>
          <radialGradient id="lampGrad-red" cx="40%" cy="35%">
            <stop offset="0%" stop-color="#ff8080"/>
            <stop offset="50%" stop-color="#cc1133"/>
            <stop offset="100%" stop-color="#440010"/>
          </radialGradient>
          <radialGradient id="lampGrad-estop" cx="40%" cy="35%">
            <stop offset="0%" stop-color="#ffaa44"/>
            <stop offset="50%" stop-color="#ff4400"/>
            <stop offset="100%" stop-color="#441000"/>
          </radialGradient>
          <filter id="glow-svg" x="-50%" y="-50%" width="200%" height="200%">
            <feGaussianBlur stdDeviation="4" result="blur"/>
            <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
          </filter>
          <filter id="glow-strong" x="-100%" y="-100%" width="300%" height="300%">
            <feGaussianBlur stdDeviation="8" result="blur"/>
            <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
          </filter>
          <linearGradient id="curtain-grad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="#1a3a60"/>
            <stop offset="100%" stop-color="#0a1e40"/>
          </linearGradient>
          <pattern id="slat-pattern" x="0" y="0" width="1" height="12" patternUnits="userSpaceOnUse">
            <rect width="300" height="11" fill="url(#curtain-grad)"/>
            <rect y="11" width="300" height="1" fill="#0a1428"/>
          </pattern>
          <linearGradient id="robot-grad" x1="0" y1="0" x2="0" y2="1">
            <stop offset="0%" stop-color="#1e2a3a"/>
            <stop offset="100%" stop-color="#0f1520"/>
          </linearGradient>
          <linearGradient id="wall-grad" x1="0" y1="0" x2="1" y2="0">
            <stop offset="0%" stop-color="#151f2e"/>
            <stop offset="100%" stop-color="#0f1520"/>
          </linearGradient>
        </defs>

        <!-- ── BACKGROUND ── -->
        <rect width="640" height="500" fill="#080a0d"/>
        <!-- floor -->
        <rect x="0" y="430" width="640" height="70" fill="#0c1018" opacity=".8"/>
        <line x1="0" y1="430" x2="640" y2="430" stroke="#1c2535" stroke-width="1"/>
        <!-- ceiling -->
        <rect x="0" y="0" width="640" height="30" fill="#0c1018" opacity=".6"/>
        <line x1="0" y1="30" x2="640" y2="30" stroke="#1c2535" stroke-width="1"/>

        <!-- ── MEASUREMENT RULES (S2/S3/S4 lines) ── -->
        <line x1="170" y1="30" x2="170" y2="430" stroke="#1a2535" stroke-width="1" stroke-dasharray="3,8"/>
        <line x1="170" y1="30" x2="470" y2="30" stroke="#1a2535" stroke-width="1" stroke-dasharray="3,8"/>
        <!-- S2 top -->
        <line x1="155" y1="60" x2="175" y2="60" stroke="#253050" stroke-width="1"/>
        <circle id="ind-S2" cx="162" cy="60" r="4" fill="#253050" stroke="#253050"/>
        <text x="142" y="64" fill="#253050" font-family="'Share Tech Mono',monospace" font-size="9" text-anchor="end">S2</text>
        <!-- S3 mid -->
        <line x1="155" y1="240" x2="175" y2="240" stroke="#253050" stroke-width="1"/>
        <circle id="ind-S3" cx="162" cy="240" r="4" fill="#253050" stroke="#253050"/>
        <text x="142" y="244" fill="#253050" font-family="'Share Tech Mono',monospace" font-size="9" text-anchor="end">S3</text>
        <!-- S4 bottom -->
        <line x1="155" y1="400" x2="175" y2="400" stroke="#253050" stroke-width="1"/>
        <circle id="ind-S4" cx="162" cy="400" r="4" fill="#253050" stroke="#253050"/>
        <text x="142" y="404" fill="#253050" font-family="'Share Tech Mono',monospace" font-size="9" text-anchor="end">S4</text>

        <!-- ── CURTAIN RAIL ── -->
        <rect x="200" y="20" width="14" height="415" rx="3" fill="#0f1825" stroke="#1c2838" stroke-width="1"/>
        <rect x="446" y="20" width="14" height="415" rx="3" fill="#0f1825" stroke="#1c2838" stroke-width="1"/>
        <!-- rail bolts -->
        <circle cx="207" cy="40" r="3" fill="#1c2838"/><circle cx="207" cy="80" r="3" fill="#1c2838"/>
        <circle cx="207" cy="380" r="3" fill="#1c2838"/><circle cx="207" cy="420" r="3" fill="#1c2838"/>
        <circle cx="453" cy="40" r="3" fill="#1c2838"/><circle cx="453" cy="80" r="3" fill="#1c2838"/>
        <circle cx="453" cy="380" r="3" fill="#1c2838"/><circle cx="453" cy="420" r="3" fill="#1c2838"/>

        <!-- ── MOTOR BOX (top) ── -->
        <g id="motor-grp">
          <rect x="270" y="6" width="100" height="28" rx="3" fill="#101828" stroke="#1c2838" stroke-width="1.5"/>
          <text x="320" y="21" fill="#253050" font-family="'Share Tech Mono',monospace" font-size="9" text-anchor="middle">MOTOR</text>
          <!-- M1/M2 indicators -->
          <circle id="m1-ind" cx="290" cy="21" r="4" fill="#1c2838"/>
          <text x="290" y="40" fill="#253050" font-family="'Share Tech Mono',monospace" font-size="7" text-anchor="middle">M1↓</text>
          <circle id="m2-ind" cx="350" cy="21" r="4" fill="#1c2838"/>
          <text x="350" y="40" fill="#253050" font-family="'Share Tech Mono',monospace" font-size="7" text-anchor="middle">M2↑</text>
        </g>

        <!-- ── CURTAIN (animated) ── -->
        <g id="curtain-grp">
          <!-- slats body -->
          <rect id="curtain-body" x="214" y="50" width="232" height="16" fill="url(#slat-pattern)" rx="1"/>
          <!-- top bar (header) -->
          <rect id="curtain-top-bar" x="210" y="46" width="240" height="10" rx="2" fill="#1e3a5a" stroke="#00d4ff" stroke-width="1.5"/>
          <!-- curtain fill between bars (grows) -->
          <rect id="curtain-fill" x="214" y="55" width="232" height="0" fill="url(#slat-pattern)"/>
          <!-- bottom bar -->
          <rect id="curtain-bot-bar" x="210" y="55" width="240" height="8" rx="2" fill="#152a45" stroke="#0077aa" stroke-width="1"/>
          <!-- direction arrows -->
          <g id="arrow-down-grp" opacity="0">
            <polygon points="318,72 322,72 320,88" fill="#00d4ff" filter="url(#glow-svg)"/>
            <line x1="320" y1="58" x2="320" y2="74" stroke="#00d4ff" stroke-width="1.5"/>
            <text x="332" y="82" fill="#00d4ff" font-family="'Share Tech Mono',monospace" font-size="8">BAJA</text>
          </g>
          <g id="arrow-up-grp" opacity="0">
            <polygon points="318,42 322,42 320,26" fill="#00ff88" filter="url(#glow-svg)"/>
            <line x1="320" y1="56" x2="320" y2="40" stroke="#00ff88" stroke-width="1.5"/>
            <text x="332" y="38" fill="#00ff88" font-family="'Share Tech Mono',monospace" font-size="8">SUBE</text>
          </g>
        </g>

        <!-- ── PROXIMITY SENSOR ── -->
        <g id="prox-grp">
          <rect x="545" y="42" width="60" height="26" rx="3" fill="#101828" stroke="#253050" stroke-width="1.5"/>
          <text x="575" y="57" fill="#6080a0" font-family="'Share Tech Mono',monospace" font-size="9" text-anchor="middle">S1 — IR</text>
          <line id="prox-beam" x1="545" y1="55" x2="475" y2="55" stroke="#253050" stroke-width="1.5" stroke-dasharray="5,4" opacity=".4"/>
          <circle id="prox-dot" cx="545" cy="55" r="3" fill="#253050"/>
        </g>

        <!-- ── ROBOT ZONE ── -->
        <rect x="310" y="340" width="200" height="92" rx="4" fill="#0c1018" stroke="#1c2535" stroke-width="1"/>
        <text x="410" y="358" fill="#253050" font-family="'Share Tech Mono',monospace" font-size="8" text-anchor="middle" letter-spacing="1">ZONA DE TRABAJO — ROBOT</text>

        <!-- Robot body -->
        <g id="robot-grp">
          <!-- chassis -->
          <rect id="robot-chassis" x="355" y="365" width="110" height="58" rx="4" fill="url(#robot-grad)" stroke="#1c2838" stroke-width="2"/>
          <!-- head/sensors -->
          <rect x="368" y="372" width="22" height="16" rx="2" fill="#0f1825" stroke="#1c2838"/>
          <rect x="430" y="372" width="22" height="16" rx="2" fill="#0f1825" stroke="#1c2838"/>
          <!-- body panel -->
          <rect x="362" y="393" width="96" height="24" rx="2" fill="#0f1825" stroke="#1c2838"/>
          <!-- eye/status light -->
          <circle id="robot-status" cx="410" cy="405" r="9" fill="#1a1020" stroke="#253050" stroke-width="2"/>
          <circle id="robot-eye-inner" cx="410" cy="405" r="5" fill="#ff2244"/>
          <!-- wheels -->
          <ellipse cx="375" cy="423" rx="12" ry="6" fill="#0c1018" stroke="#1c2838"/>
          <ellipse cx="445" cy="423" rx="12" ry="6" fill="#0c1018" stroke="#1c2838"/>
          <!-- arm indicator -->
          <rect id="robot-arm" x="456" y="375" width="20" height="6" rx="2" fill="#1a2535" stroke="#1c2838" transform-origin="456 378"/>
        </g>

        <!-- ── LAMP ASSEMBLY ── -->
        <g id="lamp-grp">
          <!-- pole -->
          <rect x="88" y="80" width="6" height="350" rx="2" fill="#0f1825" stroke="#1a2535"/>
          <!-- arm -->
          <rect x="88" y="80" width="40" height="5" rx="2" fill="#0f1825" stroke="#1a2535"/>
          <!-- housing -->
          <rect id="lamp-housing" x="118" y="56" width="36" height="48" rx="4" fill="#1a1020" stroke="#ff2244" stroke-width="2"/>
          <!-- lens glow -->
          <ellipse id="lamp-glow" cx="136" cy="68" rx="14" ry="10" fill="#3a1020" opacity=".8"/>
          <!-- bulb -->
          <ellipse id="lamp-bulb-svg" cx="136" cy="68" rx="10" ry="8" fill="#5a1828"/>
          <!-- lens highlight -->
          <ellipse cx="131" cy="64" rx="3" ry="2" fill="rgba(255,255,255,.08)" transform="rotate(-15,131,64)"/>
          <!-- base ribs -->
          <rect x="126" y="92" width="20" height="5" rx="1" fill="#0f1020" stroke="#ff2244" stroke-width="1"/>
          <rect x="124" y="97" width="24" height="4" rx="1" fill="#0f1020" stroke="#ff2244" stroke-width="1"/>
          <!-- label -->
          <text id="lamp-text-svg" x="136" y="114" fill="#ff2244" font-family="'Share Tech Mono',monospace" font-size="8" text-anchor="middle">● ROJA</text>
          <text x="136" y="124" fill="#445566" font-family="'Share Tech Mono',monospace" font-size="7" text-anchor="middle">LÁMPARA</text>
          <!-- halo -->
          <ellipse id="lamp-halo-svg" cx="136" cy="68" rx="22" ry="18" fill="#3a1020" opacity=".3"/>
        </g>

        <!-- ── SC5 ESTOP SENSOR visual ── -->
        <g id="sc5-grp">
          <rect x="32" y="380" width="50" height="48" rx="3" fill="#101828" stroke="#ff4400" stroke-width="1.5"/>
          <rect x="38" y="386" width="38" height="26" rx="2" fill="#0a0e14" stroke="#1c2838"/>
          <!-- contact plates -->
          <rect id="sc5-plate1" x="42" y="390" width="12" height="18" rx="1" fill="#1a2a40" stroke="#2a4060"/>
          <rect id="sc5-plate2" x="60" y="390" width="12" height="18" rx="1" fill="#1a2a40" stroke="#2a4060"/>
          <!-- gap indicator -->
          <line id="sc5-gap" x1="54" y1="396" x2="60" y2="396" stroke="#253050" stroke-width="1.5"/>
          <line id="sc5-gap2" x1="54" y1="402" x2="60" y2="402" stroke="#253050" stroke-width="1.5"/>
          <text x="57" y="436" fill="#ff6600" font-family="'Share Tech Mono',monospace" font-size="7" text-anchor="middle">SC5</text>
          <text x="57" y="444" fill="#445566" font-family="'Share Tech Mono',monospace" font-size="6" text-anchor="middle">COND.</text>
        </g>

        <!-- ── ESTOP OVERLAY (shown when active) ── -->
        <g id="estop-overlay" opacity="0" pointer-events="none">
          <rect width="640" height="500" fill="rgba(180,0,0,.07)"/>
          <rect width="640" height="500" fill="none" stroke="#ff2200" stroke-width="6" opacity=".5"/>
          <text x="320" y="200" fill="#ff2200" font-family="'Orbitron',sans-serif" font-size="28" font-weight="900" text-anchor="middle" opacity=".15" letter-spacing="4">EMERGENCIA</text>
        </g>

        <!-- ── POSITION LABEL ── -->
        <text id="pos-label" x="320" y="22" fill="#253050" font-family="'Share Tech Mono',monospace" font-size="9" text-anchor="middle">POS: ALTA</text>

      </svg>
    </div>
    <!-- Event log inside center -->
    <div class="log-area" id="event-log"></div>
  </div><!-- /center -->

  <!-- ══════════════ RIGHT ══════════════ -->
  <div class="right-panel">

    <!-- SALIDAS -->
    <div class="panel" style="flex:0 0 auto">
      <div class="panel-header">
        <span class="panel-header-icon">⬡</span>
        <span class="panel-header-title">Salidas — Actuadores</span>
      </div>
      <div class="panel-body">
        <div class="output-cards">
          <div class="out-card" id="card-M1">
            <div class="oc-name">M1</div>
            <div class="oc-val" id="ov-M1">0</div>
            <div class="oc-desc">Motor<br>Bajar</div>
          </div>
          <div class="out-card" id="card-M2">
            <div class="oc-name">M2</div>
            <div class="oc-val" id="ov-M2">0</div>
            <div class="oc-desc">Motor<br>Subir</div>
          </div>
          <div class="out-card" id="card-L">
            <div class="oc-name">L</div>
            <div class="oc-val" id="ov-L">0</div>
            <div class="oc-desc" id="oc-L-desc">Lámpara<br>ROJA</div>
          </div>
          <div class="out-card" id="card-R">
            <div class="oc-name">R</div>
            <div class="oc-val" id="ov-R">0</div>
            <div class="oc-desc">Permisivo<br>Robot</div>
          </div>
        </div>
      </div>
    </div>

    <!-- TIMER -->
    <div class="panel" style="flex:0 0 auto">
      <div class="panel-header">
        <span class="panel-header-icon">◷</span>
        <span class="panel-header-title">Temporizador T — 10s</span>
      </div>
      <div class="timer-section">
        <div class="timer-ring-wrap">
          <svg width="80" height="80" viewBox="0 0 80 80">
            <circle class="timer-circle-bg" cx="40" cy="40" r="34"/>
            <circle class="timer-circle-fill" id="timer-arc" cx="40" cy="40" r="34"
              stroke-dasharray="213.6" stroke-dashoffset="213.6"/>
          </svg>
          <div class="timer-ring-label">
            <div class="timer-ring-val" id="timer-val">0.0</div>
            <div class="timer-ring-sub">SEG</div>
          </div>
        </div>
        <div class="timer-status" id="timer-status">INACTIVO</div>
      </div>
    </div>

    <!-- STATE MACHINE -->
    <div class="panel" style="flex:1">
      <div class="panel-header">
        <span class="panel-header-icon">◫</span>
        <span class="panel-header-title">Máquina de Estado</span>
      </div>
      <div class="state-machine">
        <div class="sm-step current" id="sm0">
          <span class="sm-num">00</span>
          <div class="sm-dot"></div>
          <span>INICIAL — En espera</span>
        </div>
        <div class="sm-step" id="sm1">
          <span class="sm-num">01</span>
          <div class="sm-dot"></div>
          <span>BAJANDO — M1 activo</span>
        </div>
        <div class="sm-step" id="sm2">
          <span class="sm-num">02</span>
          <div class="sm-dot"></div>
          <span>ROBOT — L·R activos</span>
        </div>
        <div class="sm-step" id="sm3">
          <span class="sm-num">03</span>
          <div class="sm-dot"></div>
          <span>SUBIENDO — M2 activo</span>
        </div>
        <div class="sm-step" id="sm4" style="border-color:rgba(255,60,0,.3)">
          <span class="sm-num" style="color:var(--estop)">E!</span>
          <div class="sm-dot" style="background:var(--estop);box-shadow:none;opacity:.3" id="sm4-dot"></div>
          <span style="color:var(--text2)">E-STOP — SC5 activo</span>
        </div>
      </div>

      <!-- Trend mini -->
      <div class="trend-area">
        <div class="trend-label">TENDENCIA — Activaciones M1/M2</div>
        <canvas class="trend-canvas" id="trend-canvas" width="230" height="36"></canvas>
      </div>
    </div>

  </div><!-- /right -->

  <!-- ══════════════ BOTTOM ══════════════ -->
  <div class="bottom-bar">
    <div class="status-bar">
      <div class="sb-item">PLC: <span class="sb-val" style="margin-left:4px">SIM-001</span></div>
      <div class="sb-sep"></div>
      <div class="sb-item">CICLO: <span class="sb-val" id="sb-cycle">0</span></div>
      <div class="sb-sep"></div>
      <div class="sb-item">S1=<span class="sb-val" id="sb-S1">1</span></div>
      <div class="sb-item">S2=<span class="sb-val" id="sb-S2">1</span></div>
      <div class="sb-item">S3=<span class="sb-val" id="sb-S3">0</span></div>
      <div class="sb-item">S4=<span class="sb-val" id="sb-S4">0</span></div>
      <div class="sb-item">P=<span class="sb-val" id="sb-P">0</span></div>
      <div class="sb-item">T=<span class="sb-val" id="sb-T">0</span></div>
      <div class="sb-item" style="color:var(--orange)">SC5=<span class="sb-val" id="sb-SC5" style="color:var(--green)">0</span></div>
      <div class="sb-sep"></div>
      <div class="sb-item">M1=<span class="sb-val" id="sb-M1">0</span></div>
      <div class="sb-item">M2=<span class="sb-val" id="sb-M2">0</span></div>
      <div class="sb-item">L=<span class="sb-val" id="sb-L">0</span></div>
      <div class="sb-item">R=<span class="sb-val" id="sb-R">0</span></div>
      <div class="sb-state" id="sb-state-text">INICIAL — EN ESPERA</div>
    </div>
  </div>

</div><!-- /main -->

<script>
// ═══════════════════════════════════════════════════
//  STATE
// ═══════════════════════════════════════════════════
const inp = { S1:1, S2:1, S3:0, S4:0, P:0, T:0, SC5:0 };
const out = { M1:0, M2:0, L:0, R:0 };

let sysState = 0; // 0=IDLE 1=DOWN 2=ROBOT 3=UP 4=ESTOP
let prevState = -1;
let timerInt = null, timerVal = 0;
const TDUR = 10;
let cycleCount = 0;
let autoTO = null;
let trendData = []; // for mini trend
let curtainYAnim = 50; // animated curtain Y top position (svg units)
let rafId = null;

const stateLabels = [
  'INICIAL — EN ESPERA',
  'BAJANDO CORTINA',
  'ROBOT ACTIVO',
  'SUBIENDO CORTINA',
  'E-STOP — EMERGENCIA'
];

// ═══════════════════════════════════════════════════
//  LOGIC ENGINE
// ═══════════════════════════════════════════════════
function compute() {
  const { S1, S2, S3, S4, P, T, SC5 } = inp;

  // E-STOP overrides everything
  if(SC5) {
    if(sysState !== 4) {
      stopTimer();
      out.M1 = 0; out.M2 = 0; out.L = 0; out.R = 0;
      sysState = 4;
      log('⊘ PARO DE EMERGENCIA — SC5 activo. Todos los actuadores detenidos.', 'err');
    }
    return;
  }

  // If recovering from E-STOP, go back to IDLE
  if(sysState === 4) {
    sysState = 0;
    log('✔ E-STOP liberado. Sistema retorna a estado inicial.', 'ok');
  }

  switch(sysState) {
    case 0: // IDLE
      out.M1=0; out.M2=0; out.L=0; out.R=0;
      if(P && S1 && S2 && !S3 && !S4) {
        sysState = 1;
        cycleCount++;
        log('▶ Pulsador P activo. Iniciando descenso de cortina.', 'info');
      }
      break;

    case 1: // GOING DOWN
      out.M1=1; out.M2=0; out.L=0; out.R=0;
      if(!S1) {
        sysState = 0; out.M1=0;
        log('⚠ Persona detectada (S1=0) — Motor M1 detenido.', 'warn');
      }
      if(S4) {
        sysState = 2;
        out.M1=0; out.L=1; out.R=1;
        startTimer();
        log('▣ Cortina en posición baja. Robot habilitado. Timer 10s iniciado.', 'ok');
      }
      break;

    case 2: // ROBOT ACTIVE
      out.M1=0; out.M2=0; out.L=1; out.R=1;
      if(!S1) { out.R=0; log('⚠ Persona detectada — Permisivo R revocado.','warn'); }
      if(T) {
        sysState = 3;
        out.L=0; out.R=0; out.M2=1;
        stopTimer();
        log('◷ Timer completado. Subiendo cortina. Robot deshabilitado.','info');
      }
      break;

    case 3: // GOING UP
      out.M1=0; out.M2=1; out.L=0; out.R=0;
      if(S2) {
        sysState = 0; out.M2=0;
        log('▲ Cortina arriba. Sistema retorna a estado inicial.','ok');
      }
      break;
  }
}

// ═══════════════════════════════════════════════════
//  TIMER
// ═══════════════════════════════════════════════════
function startTimer() {
  timerVal = 0; inp.T = 0;
  clearInterval(timerInt);
  timerInt = setInterval(()=>{
    timerVal += 0.1;
    updateTimerUI();
    if(timerVal >= TDUR) {
      inp.T = 1;
      clearInterval(timerInt);
      compute(); updateUI();
    }
  }, 100);
}
function stopTimer() {
  clearInterval(timerInt);
  timerVal = 0; inp.T = 0;
  updateTimerUI();
}
function updateTimerUI() {
  const pct = Math.min(timerVal / TDUR, 1);
  const circ = 213.6;
  document.getElementById('timer-arc').setAttribute('stroke-dashoffset', circ*(1-pct));
  document.getElementById('timer-val').textContent = timerVal.toFixed(1);
  const ts = document.getElementById('timer-status');
  if(timerVal > 0 && timerVal < TDUR) { ts.textContent='EN CURSO'; ts.style.color='var(--cyan)'; }
  else if(inp.T) { ts.textContent='COMPLETADO'; ts.style.color='var(--green)'; }
  else { ts.textContent='INACTIVO'; ts.style.color='var(--text2)'; }
  document.getElementById('val-T').textContent = inp.T;
  document.getElementById('sb-T').textContent = inp.T;
  setSensorRow('T', inp.T);
}

// ═══════════════════════════════════════════════════
//  UI UPDATE
// ═══════════════════════════════════════════════════
function updateUI() {
  // Sensor rows
  ['S1','S2','S3','S4','P'].forEach(k => {
    const v = inp[k];
    document.getElementById('val-'+k).textContent = v;
    document.getElementById('sb-'+k).textContent = v;
    setSensorRow(k, v);
  });

  // SC5
  setSc5UI(inp.SC5);

  // Outputs
  setOutCard('M1', out.M1, false);
  setOutCard('M2', out.M2, false);
  setOutCard('R',  out.R,  false);
  setLampCard(out.L, sysState === 4);

  // Status bar
  ['M1','M2','L','R'].forEach(k => {
    document.getElementById('sb-'+k).textContent = out[k];
  });

  // State machine steps
  for(let i=0;i<=4;i++) {
    const el = document.getElementById('sm'+i);
    if(!el) continue;
    el.classList.remove('current','done');
    if(i === sysState) el.classList.add('current');
  }

  // E-stop banner
  const banner = document.getElementById('estop-banner');
  if(sysState === 4) banner.classList.add('active'); else banner.classList.remove('active');

  // State label
  document.getElementById('process-state-label').textContent = 'ESTADO: '+stateLabels[sysState];
  document.getElementById('sb-state-text').textContent = stateLabels[sysState];

  // Cycle count
  document.getElementById('sb-cycle').textContent = cycleCount;

  // Topbar status
  if(sysState === 4) {
    document.getElementById('conn-dot').style.background = 'var(--red)';
    document.getElementById('conn-dot').style.boxShadow = '0 0 6px var(--red)';
    document.getElementById('conn-label').style.color = 'var(--red)';
    document.getElementById('conn-label').textContent = 'E-STOP';
  } else {
    document.getElementById('conn-dot').style.background = 'var(--green)';
    document.getElementById('conn-dot').style.boxShadow = '0 0 6px var(--green)';
    document.getElementById('conn-label').style.color = 'var(--green)';
    document.getElementById('conn-label').textContent = 'EN LÍNEA';
  }

  // Update trend
  trendData.push({ m1: out.M1, m2: out.M2 });
  if(trendData.length > 100) trendData.shift();
  drawTrend();
}

function setSensorRow(key, val) {
  const row = document.getElementById('si-'+key);
  const led = document.getElementById('led-'+key);
  if(!row||!led) return;
  if(val) {
    row.classList.add('active');
    led.classList.add('on'); led.classList.remove('on-red','on-amber');
  } else {
    row.classList.remove('active');
    led.classList.remove('on','on-red','on-amber');
  }
}

function setOutCard(id, val, danger) {
  const card = document.getElementById('card-'+id);
  const valEl = document.getElementById('ov-'+id);
  if(!card||!valEl) return;
  card.className = 'out-card' + (val ? (danger?' on-red':' on') : '');
  valEl.textContent = val;
}

function setLampCard(val, estop) {
  const card = document.getElementById('card-L');
  const valEl = document.getElementById('ov-L');
  const desc = document.getElementById('oc-L-desc');
  if(estop) {
    card.className = 'out-card on-red';
    valEl.textContent = '!';
    if(desc) { desc.textContent = 'Lámpara\nE-STOP'; }
  } else if(val) {
    card.className = 'out-card on';
    valEl.textContent = '1';
    if(desc) desc.textContent = 'Lámpara\nVERDE';
  } else {
    card.className = 'out-card on-red';
    valEl.textContent = '0';
    if(desc) desc.textContent = 'Lámpara\nROJA';
  }
}

function setSc5UI(val) {
  document.getElementById('val-SC5').textContent = val;
  document.getElementById('sb-SC5').textContent = val;
  const row = document.getElementById('si-SC5');
  const led = document.getElementById('led-SC5');
  const estopBtn = document.getElementById('estop-btn');
  const stateText = document.getElementById('estop-state-text');
  if(val) {
    led.style.background = 'var(--estop)';
    led.style.boxShadow = '0 0 10px var(--estop)';
    led.style.opacity = '1';
    row.style.borderColor = 'var(--estop)';
    row.style.background = 'rgba(255,26,0,.08)';
    estopBtn.classList.add('pressed');
    stateText.textContent = '⊘ ACTIVO — SC5 = 1';
    stateText.style.color = 'var(--estop)';
  } else {
    led.style.background = '#ff6600';
    led.style.boxShadow = 'none';
    led.style.opacity = '.3';
    row.style.borderColor = 'var(--orange)';
    row.style.background = 'rgba(255,102,0,.04)';
    estopBtn.classList.remove('pressed');
    stateText.textContent = '● NORMAL — SC5 = 0';
    stateText.style.color = 'var(--green)';
  }
}

// ═══════════════════════════════════════════════════
//  SVG ANIMATION (rAF loop)
// ═══════════════════════════════════════════════════
function animLoop() {
  updateProcessSVG();
  rafId = requestAnimationFrame(animLoop);
}

function updateProcessSVG() {
  // ── Curtain position target (SVG y of top-bar)
  let targetY = 50;
  if(sysState === 1) targetY = 200; // going down (mid point in animation)
  if(sysState === 2) targetY = 390; // at bottom
  if(sysState === 3) targetY = 200; // going up
  if(inp.S4) targetY = 392;
  if(inp.S2 && sysState === 0) targetY = 50;
  if(sysState === 4) { /* freeze */ }

  curtainYAnim += (targetY - curtainYAnim) * 0.08;

  const topBarY = curtainYAnim;
  const fillH = Math.max(0, topBarY - 55);
  const botBarY = topBarY + 6;

  const topBar = document.getElementById('curtain-top-bar');
  const fill   = document.getElementById('curtain-fill');
  const botBar = document.getElementById('curtain-bot-bar');
  const body   = document.getElementById('curtain-body');

  topBar.setAttribute('y', topBarY);
  fill.setAttribute('y', 55);
  fill.setAttribute('height', Math.max(0, topBarY - 55));
  botBar.setAttribute('y', topBarY + 8);
  body.setAttribute('y', topBarY + 2);
  body.setAttribute('height', Math.max(0, topBarY - 50));

  // ── Position text
  const posLabel = document.getElementById('pos-label');
  let posText = 'POS: ALTA';
  if(inp.S3) posText = 'POS: MEDIA';
  if(inp.S4) posText = 'POS: BAJA';
  if(sysState === 1) posText = 'BAJANDO...';
  if(sysState === 3) posText = 'SUBIENDO...';
  posLabel.textContent = posText;

  // ── Motor indicators
  const m1 = document.getElementById('m1-ind');
  const m2 = document.getElementById('m2-ind');
  m1.setAttribute('fill', out.M1 ? '#00d4ff' : '#1c2838');
  m1.setAttribute('filter', out.M1 ? 'url(#glow-svg)' : '');
  m2.setAttribute('fill', out.M2 ? '#00ff88' : '#1c2838');
  m2.setAttribute('filter', out.M2 ? 'url(#glow-svg)' : '');

  // ── Arrows
  document.getElementById('arrow-down-grp').setAttribute('opacity', out.M1 ? '1':'0');
  document.getElementById('arrow-up-grp').setAttribute('opacity', out.M2 ? '1':'0');

  // ── Position sensor indicators
  const setInd = (id, on) => {
    const el = document.getElementById(id);
    if(!el) return;
    el.setAttribute('fill', on ? '#00ff88':'#253050');
    el.setAttribute('stroke', on ? '#00ff88':'#253050');
    el.setAttribute('filter', on ? 'url(#glow-svg)':'');
  };
  setInd('ind-S2', inp.S2);
  setInd('ind-S3', inp.S3);
  setInd('ind-S4', inp.S4);

  // ── Proximity beam
  const beam = document.getElementById('prox-beam');
  const dot  = document.getElementById('prox-dot');
  if(!inp.S1) {
    beam.setAttribute('stroke','#ff6600');
    beam.setAttribute('opacity','0.9');
    dot.setAttribute('fill','#ff6600');
    dot.setAttribute('filter','url(#glow-svg)');
  } else {
    beam.setAttribute('stroke','#253050');
    beam.setAttribute('opacity','0.4');
    dot.setAttribute('fill','#253050');
    dot.setAttribute('filter','');
  }

  // ── Lamp
  const lhalo  = document.getElementById('lamp-halo-svg');
  const lbulb  = document.getElementById('lamp-bulb-svg');
  const lglow  = document.getElementById('lamp-glow');
  const lhousing = document.getElementById('lamp-housing');
  const ltxt   = document.getElementById('lamp-text-svg');

  if(sysState === 4) { // E-STOP → flashing amber
    const t = Date.now();
    const blink = Math.sin(t/200) > 0;
    const col = blink ? '#ff6600' : '#ff4400';
    lhalo.setAttribute('fill','#3a2000'); lhalo.setAttribute('opacity', blink ? '0.6':'0.2');
    lglow.setAttribute('fill','#ff6600'); lglow.setAttribute('opacity','0.4');
    lbulb.setAttribute('fill',col);
    lhousing.setAttribute('stroke',col);
    ltxt.setAttribute('fill',col); ltxt.textContent='● ÁMBAR';
  } else if(out.L) { // GREEN
    lhalo.setAttribute('fill','#003a1a'); lhalo.setAttribute('opacity','0.7');
    lglow.setAttribute('fill','#00ff88'); lglow.setAttribute('opacity','0.5');
    lbulb.setAttribute('fill','#00cc66');
    lhousing.setAttribute('stroke','#00ff88');
    ltxt.setAttribute('fill','#00ff88'); ltxt.textContent='● VERDE';
  } else { // RED
    lhalo.setAttribute('fill','#3a1020'); lhalo.setAttribute('opacity','0.5');
    lglow.setAttribute('fill','#ff2244'); lglow.setAttribute('opacity','0.3');
    lbulb.setAttribute('fill','#5a1828');
    lhousing.setAttribute('stroke','#ff2244');
    ltxt.setAttribute('fill','#ff2244'); ltxt.textContent='● ROJA';
  }

  // ── Robot
  const rEye  = document.getElementById('robot-eye-inner');
  const rStat = document.getElementById('robot-status');
  const rChas = document.getElementById('robot-chassis');
  if(sysState === 4) {
    rEye.setAttribute('fill','#ff6600');
    rStat.setAttribute('stroke','#ff6600');
    rChas.setAttribute('stroke','#ff2200');
  } else if(out.R) {
    rEye.setAttribute('fill','#00ff88');
    rStat.setAttribute('stroke','#00ff88');
    rChas.setAttribute('stroke','#00aa55');
  } else {
    rEye.setAttribute('fill','#ff2244');
    rStat.setAttribute('stroke','#253050');
    rChas.setAttribute('stroke','#1c2838');
  }

  // ── SC5 sensor plates
  const p1 = document.getElementById('sc5-plate1');
  const p2 = document.getElementById('sc5-plate2');
  const g1 = document.getElementById('sc5-gap');
  const g2 = document.getElementById('sc5-gap2');
  if(inp.SC5) {
    // plates touching (conductive contact)
    p1.setAttribute('x','42'); p1.setAttribute('fill','#ff3300'); p1.setAttribute('stroke','#ff5500');
    p2.setAttribute('x','56'); p2.setAttribute('fill','#ff3300'); p2.setAttribute('stroke','#ff5500');
    g1.setAttribute('stroke','#ff4400'); g1.setAttribute('opacity','0.9');
    g2.setAttribute('stroke','#ff4400'); g2.setAttribute('opacity','0.9');
  } else {
    p1.setAttribute('x','42'); p1.setAttribute('fill','#1a2a40'); p1.setAttribute('stroke','#2a4060');
    p2.setAttribute('x','60'); p2.setAttribute('fill','#1a2a40'); p2.setAttribute('stroke','#2a4060');
    g1.setAttribute('stroke','#253050'); g1.setAttribute('opacity','0.6');
    g2.setAttribute('stroke','#253050'); g2.setAttribute('opacity','0.6');
  }

  // ── E-stop overlay
  const overlay = document.getElementById('estop-overlay');
  if(sysState === 4) {
    const t = Date.now();
    const blink = Math.sin(t/400) * 0.5 + 0.5;
    overlay.setAttribute('opacity', (blink * 0.5).toFixed(2));
  } else {
    overlay.setAttribute('opacity','0');
  }

  // ── SM4 dot pulse
  const sm4dot = document.getElementById('sm4-dot');
  if(sysState === 4) {
    sm4dot.style.background = 'var(--estop)';
    sm4dot.style.boxShadow = '0 0 8px var(--estop)';
    sm4dot.style.opacity = '1';
  } else {
    sm4dot.style.background = 'var(--estop)';
    sm4dot.style.boxShadow = 'none';
    sm4dot.style.opacity = '.3';
  }
}

// ═══════════════════════════════════════════════════
//  TREND CANVAS
// ═══════════════════════════════════════════════════
function drawTrend() {
  const cv = document.getElementById('trend-canvas');
  if(!cv) return;
  const ctx = cv.getContext('2d');
  const W = cv.width, H = cv.height;
  ctx.clearRect(0,0,W,H);
  ctx.fillStyle = 'rgba(0,0,0,.3)';
  ctx.fillRect(0,0,W,H);

  // Grid
  ctx.strokeStyle = '#1c2535'; ctx.lineWidth = 1;
  for(let x=0;x<W;x+=20) { ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,H); ctx.stroke(); }

  if(trendData.length < 2) return;
  const step = W / (trendData.length-1);

  // M1 - cyan
  ctx.strokeStyle = '#00d4ff'; ctx.lineWidth = 1.5;
  ctx.beginPath();
  trendData.forEach((d,i)=>{ const x=i*step; const y=d.m1 ? 6 : H-6; i===0?ctx.moveTo(x,y):ctx.lineTo(x,y); });
  ctx.stroke();

  // M2 - green
  ctx.strokeStyle = '#00ff88'; ctx.lineWidth = 1.5;
  ctx.beginPath();
  trendData.forEach((d,i)=>{ const x=i*step; const y=d.m2 ? 14 : H-2; i===0?ctx.moveTo(x,y):ctx.lineTo(x,y); });
  ctx.stroke();

  // Labels
  ctx.fillStyle = '#00d4ff'; ctx.font = '8px Share Tech Mono'; ctx.fillText('M1', 2, 12);
  ctx.fillStyle = '#00ff88'; ctx.fillText('M2', 2, 24);
}

// ═══════════════════════════════════════════════════
//  LOG
// ═══════════════════════════════════════════════════
function log(msg, type='info') {
  const area = document.getElementById('event-log');
  const el = document.createElement('div');
  el.className = 'log-entry';
  const t = new Date().toLocaleTimeString('es',{hour12:false});
  el.innerHTML = `<span class="log-time">[${t}]</span><span class="log-msg ${type}">${msg}</span>`;
  area.prepend(el);
  if(area.children.length > 60) area.removeChild(area.lastChild);
}

// ═══════════════════════════════════════════════════
//  CONTROLS
// ═══════════════════════════════════════════════════
function pressP(down) {
  if(sysState === 4) { log('⊘ Pulsador ignorado — E-STOP activo.','err'); return; }
  inp.P = down ? 1 : 0;
  setSensorRow('P', inp.P);
  document.getElementById('val-P').textContent = inp.P;
  document.getElementById('sb-P').textContent = inp.P;
  if(down) { compute(); updateUI(); }
  else { inp.P=0; compute(); updateUI(); }
}

function toggleEstop() {
  inp.SC5 = inp.SC5 ? 0 : 1;
  compute();
  updateUI();
  if(inp.SC5) log('⊘ SC5 activado por operador — Paro de emergencia.','err');
  else log('✔ SC5 liberado por operador — Listo para reset.','warn');
}

function fullReset() {
  clearInterval(timerInt); clearTimeout(autoTO);
  timerVal=0; inp.T=0;
  Object.assign(inp,{S1:1,S2:1,S3:0,S4:0,P:0,T:0,SC5:0});
  Object.assign(out,{M1:0,M2:0,L:0,R:0});
  sysState=0; curtainYAnim=50;
  updateTimerUI(); updateUI();
  log('↺ Sistema reiniciado al estado inicial.','warn');
}

function runAutoDemo() {
  if(autoTO) { clearTimeout(autoTO); autoTO=null; }
  fullReset();
  log('▷ AUTO-DEMO iniciado.','info');
  const seq = [
    [400,  ()=>{ pressP(true); }],
    [700,  ()=>{ pressP(false); }],
    [1600, ()=>{ inp.S2=0;inp.S3=1; compute();updateUI(); log('→ Cortina: posición media.','info'); }],
    [2800, ()=>{ inp.S3=0;inp.S4=1; compute();updateUI(); log('→ Cortina: posición baja.','ok'); }],
    [13500,()=>{ /* timer fires naturally */ }],
    [14200,()=>{ inp.S4=0;inp.S3=1; compute();updateUI(); }],
    [15400,()=>{ inp.S3=0;inp.S2=1; compute();updateUI(); }],
  ];
  let cum=0;
  seq.forEach(([d,fn])=>{ cum+=d; setTimeout(fn,cum); });
}

// ═══════════════════════════════════════════════════
//  CLOCK
// ═══════════════════════════════════════════════════
function tickClock() {
  const now = new Date();
  const h = String(now.getHours()).padStart(2,'0');
  const m = String(now.getMinutes()).padStart(2,'0');
  const s = String(now.getSeconds()).padStart(2,'0');
  document.getElementById('clock').textContent = `${h}:${m}:${s}`;
}
setInterval(tickClock,1000); tickClock();

// ═══════════════════════════════════════════════════
//  INIT
// ═══════════════════════════════════════════════════
updateUI();
animLoop();
log('Sistema HMI iniciado. Estado inicial: Cortina arriba.','ok');
log('SC5 — Sensor conductivo en modo NC. Haga click en ⊘ para simular paro.','info');
</script>
</body>
</html>
