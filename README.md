<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>The Movement Club</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&family=Bebas+Neue&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  :root { --bg: #0d0d0d; --text: #ffffff; --muted: #888; --accent: #E8FF47; }
  html { background: var(--bg); height: 100%; }
  body {
    background: var(--bg); color: var(--text);
    font-family: 'DM Mono', monospace;
    touch-action: manipulation; min-height: 100%;
    overflow-y: auto; -webkit-overflow-scrolling: touch;
  }

  /* ── SCREENS ── */
  .screen { display: none; flex-direction: column; align-items: center; min-height: 100vh; }
  .screen.active { display: flex; }

  /* ── GLOW ── */
  .glow {
    position: fixed; width: 320px; height: 320px; border-radius: 50%;
    background: #ffffff; opacity: 0.05; filter: blur(90px);
    top: 40%; left: 50%; transform: translate(-50%,-50%);
    transition: background 0.5s; pointer-events: none; z-index: 0;
  }

  /* ════════════════════════════
     HOME SCREEN
  ════════════════════════════ */
  #homeScreen {
    padding: 0 24px 60px;
    justify-content: flex-start;
  }

  .home-header {
    width: 100%; max-width: 480px;
    padding: 36px 0 24px;
    text-align: center;
  }

  .brand {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 38px; letter-spacing: 0.08em;
    color: var(--text); line-height: 1;
  }

  .tagline {
    font-size: 10px; letter-spacing: 0.3em;
    text-transform: uppercase; color: var(--muted); margin-top: 8px;
  }

  .home-section-label {
    width: 100%; max-width: 480px;
    font-size: 10px; letter-spacing: 0.3em;
    text-transform: uppercase; color: var(--muted);
    margin-bottom: 12px;
  }

  .timer-choices {
    width: 100%; max-width: 480px;
    display: flex; gap: 12px;
    margin-bottom: 40px;
  }

  .choice-btn {
    flex: 1; height: 100px; border-radius: 16px;
    border: 1px solid #1e1e1e; background: #111;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center; gap: 8px;
    cursor: pointer; transition: border-color 0.2s, background 0.2s;
    -webkit-tap-highlight-color: transparent;
  }

  .choice-btn:active { background: #1a1a1a; border-color: #333; }

  .choice-icon {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 22px; color: var(--accent); line-height: 1;
  }

  .choice-label {
    font-size: 10px; letter-spacing: 0.2em;
    text-transform: uppercase; color: var(--muted);
  }

  /* saved presets */
  .presets-list {
    width: 100%; max-width: 480px;
    display: flex; flex-direction: column; gap: 8px;
  }

  .preset-card {
    background: #111; border: 1px solid #1e1e1e;
    border-radius: 12px; padding: 16px 20px;
    display: flex; align-items: center; justify-content: space-between;
    cursor: pointer; transition: border-color 0.2s;
    -webkit-tap-highlight-color: transparent;
  }

  .preset-card:active { border-color: #333; }

  .preset-info { display: flex; flex-direction: column; gap: 4px; }

  .preset-name {
    font-size: 14px; font-weight: 500; color: var(--text);
    letter-spacing: 0.05em;
  }

  .preset-meta {
    font-size: 10px; letter-spacing: 0.15em;
    text-transform: uppercase; color: var(--muted);
  }

  .preset-type {
    font-size: 9px; letter-spacing: 0.2em;
    text-transform: uppercase;
    padding: 3px 8px; border-radius: 4px;
    background: #1a1a1a; color: var(--muted);
  }

  .preset-actions { display: flex; align-items: center; gap: 12px; }

  .preset-del {
    font-size: 18px; color: #333; cursor: pointer;
    padding: 4px; line-height: 1;
    -webkit-tap-highlight-color: transparent;
    transition: color 0.2s;
  }
  .preset-del:active { color: #ff4444; }

  .no-presets {
    font-size: 11px; letter-spacing: 0.15em;
    text-transform: uppercase; color: #2a2a2a;
    text-align: center; padding: 24px 0;
  }

  /* ════════════════════════════
     SETUP SCREENS
  ════════════════════════════ */
  .setup-screen {
    padding: 0 24px 80px;
    justify-content: flex-start;
    width: 100%;
  }

  .nav-bar {
    width: 100%; max-width: 480px;
    display: flex; align-items: center;
    padding: 52px 0 24px;
    gap: 16px;
  }

  .nav-back {
    font-size: 11px; letter-spacing: 0.2em;
    text-transform: uppercase; color: var(--muted);
    cursor: pointer; -webkit-tap-highlight-color: transparent;
    padding: 4px 0;
  }

  .nav-back:active { color: var(--text); }

  .nav-title {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 24px; letter-spacing: 0.06em; color: var(--text);
  }

  .setup-inner { width: 100%; max-width: 480px; padding: 0 24px; }

  .settings { width: 100%; display: flex; flex-direction: column; margin-bottom: 16px; }

  .setting-row {
    display: flex; align-items: center; justify-content: space-between;
    padding: 13px 0; border-bottom: 1px solid #1a1a1a;
  }
  .setting-row:last-child { border-bottom: none; }

  .setting-label {
    font-size: 11px; letter-spacing: 0.15em; text-transform: uppercase;
    color: var(--muted); display: flex; align-items: center; gap: 6px;
  }

  .phase-dot { display: inline-block; width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }

  .setting-value-tap {
    font-size: 15px; font-weight: 500; color: var(--text);
    padding: 4px 12px; border-radius: 8px;
    border: 1px solid #2a2a2a; background: #1a1a1a;
    cursor: pointer; -webkit-tap-highlight-color: transparent;
    min-width: 72px; text-align: center; transition: border-color 0.2s;
  }
  .setting-value-tap:active { border-color: #777; }

  .setting-controls { display: flex; align-items: center; gap: 14px; }
  .setting-value { font-size: 15px; font-weight: 500; min-width: 48px; text-align: center; color: var(--text); }

  .btn-adj {
    width: 32px; height: 32px; border-radius: 50%;
    border: 1px solid #2a2a2a; background: #1a1a1a; color: var(--text);
    font-size: 18px; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    transition: border-color 0.2s, background 0.2s;
    -webkit-tap-highlight-color: transparent; user-select: none;
  }
  .btn-adj:active { background: #2a2a2a; border-color: #777; }

  .phase-selector { display: flex; gap: 8px; margin-bottom: 18px; }
  .phase-btn {
    flex: 1; height: 34px; border-radius: 8px; border: 1px solid #2a2a2a;
    background: #1a1a1a; color: var(--muted);
    font-family: 'DM Mono', monospace; font-size: 10px; letter-spacing: 0.08em;
    cursor: pointer; transition: all 0.2s;
    -webkit-tap-highlight-color: transparent; user-select: none;
  }
  .phase-btn.active { border-color: var(--accent); color: var(--accent); background: rgba(232,255,71,0.05); }

  .toggle { position: relative; width: 44px; height: 24px; cursor: pointer; -webkit-tap-highlight-color: transparent; flex-shrink: 0; }
  .toggle input { opacity: 0; width: 0; height: 0; position: absolute; }
  .toggle-track { position: absolute; inset: 0; background: transparent; border: 1px solid #666; border-radius: 24px; transition: background 0.3s, border-color 0.3s; }
  .toggle input:checked + .toggle-track { background: #E8FF47; border-color: #E8FF47; }
  .toggle-thumb { position: absolute; top: 3px; left: 3px; width: 18px; height: 18px; border-radius: 50%; background: #fff; transition: transform 0.3s; pointer-events: none; }
  .toggle input:checked ~ .toggle-thumb { transform: translateX(20px); }

  .btn-row { display: flex; gap: 12px; margin-top: 8px; }

  .btn-main {
    flex: 1; height: 58px; border-radius: 12px; border: none;
    background: var(--accent); color: #000;
    font-family: 'DM Mono', monospace; font-size: 12px; font-weight: 500;
    letter-spacing: 0.2em; text-transform: uppercase; cursor: pointer;
    transition: opacity 0.15s; -webkit-tap-highlight-color: transparent; user-select: none;
  }
  .btn-main:active { opacity: 0.8; }

  .btn-save-preset {
    height: 58px; padding: 0 20px; border-radius: 12px;
    border: 1px solid #2a2a2a; background: transparent; color: var(--muted);
    font-family: 'DM Mono', monospace; font-size: 11px; letter-spacing: 0.15em;
    text-transform: uppercase; cursor: pointer;
    -webkit-tap-highlight-color: transparent;
    white-space: nowrap;
  }
  .btn-save-preset:active { border-color: #777; color: var(--text); }

  /* ── SAVE MODAL ── */
  .modal-overlay {
    display: none; position: fixed; inset: 0; z-index: 60;
    background: rgba(0,0,0,0.8);
    align-items: flex-end; justify-content: center;
  }
  .modal-overlay.active { display: flex; }

  .modal-sheet {
    background: #141414; width: 100%; max-width: 480px;
    border-radius: 20px 20px 0 0; padding: 28px 24px 48px;
  }

  .modal-title {
    font-size: 10px; letter-spacing: 0.3em;
    text-transform: uppercase; color: var(--muted); margin-bottom: 16px;
  }

  .modal-input {
    width: 100%; height: 52px; border-radius: 10px;
    border: 1px solid #2a2a2a; background: #1a1a1a;
    color: var(--text); font-family: 'DM Mono', monospace;
    font-size: 16px; padding: 0 16px; outline: none;
    margin-bottom: 16px;
  }
  .modal-input:focus { border-color: var(--accent); }

  .modal-actions { display: flex; gap: 12px; }

  .modal-btn {
    flex: 1; height: 52px; border-radius: 10px; border: none;
    font-family: 'DM Mono', monospace; font-size: 12px;
    font-weight: 500; letter-spacing: 0.15em; text-transform: uppercase;
    cursor: pointer; -webkit-tap-highlight-color: transparent;
  }
  .modal-confirm { background: var(--accent); color: #000; }
  .modal-cancel  { background: #1a1a1a; color: var(--muted); }

  /* ════════════════════════════
     PICKER MODAL
  ════════════════════════════ */
  .picker-overlay {
    display: none; position: fixed; inset: 0; z-index: 50;
    background: rgba(0,0,0,0.7); align-items: flex-end; justify-content: center;
  }
  .picker-overlay.active { display: flex; }
  .picker-sheet {
    background: #141414; width: 100%; max-width: 480px;
    border-radius: 20px 20px 0 0; padding: 0 0 40px; overflow: hidden;
  }
  .picker-header {
    display: flex; align-items: center; justify-content: space-between;
    padding: 20px 24px 12px; border-bottom: 1px solid #1e1e1e;
  }
  .picker-title { font-size: 10px; letter-spacing: 0.3em; text-transform: uppercase; color: var(--muted); }
  .picker-hint { font-size: 10px; color: #333; letter-spacing: 0.1em; text-transform: uppercase; text-align: center; padding: 10px 0 4px; }
  .picker-actions { display: flex; gap: 10px; }
  .picker-btn {
    height: 34px; padding: 0 18px; border-radius: 8px; border: none;
    font-family: 'DM Mono', monospace; font-size: 11px; letter-spacing: 0.15em;
    text-transform: uppercase; cursor: pointer; -webkit-tap-highlight-color: transparent;
  }
  .picker-save { background: var(--accent); color: #000; }
  .picker-back { background: #1e1e1e; color: var(--muted); }

  .drum-labels { display: flex; padding: 12px 24px 4px; }
  .drum-label { flex: 1; font-size: 11px; letter-spacing: 0.2em; text-transform: uppercase; color: var(--muted); text-align: center; }

  .picker-drums { display: flex; justify-content: center; align-items: center; padding: 0 24px; position: relative; }

  .drum-col { flex: 1; height: 220px; overflow: hidden; position: relative; cursor: grab; }
  .drum-col:active { cursor: grabbing; }
  .drum-inner {
    height: 100%; overflow-y: scroll; scroll-snap-type: y mandatory;
    -webkit-overflow-scrolling: touch; scrollbar-width: none;
  }
  .drum-inner::-webkit-scrollbar { display: none; }
  .drum-item {
    height: 44px; display: flex; align-items: center; justify-content: center;
    font-family: 'Bebas Neue', sans-serif; font-size: 32px; color: #2a2a2a;
    scroll-snap-align: center; user-select: none;
  }
  .drum-pad { height: 88px; scroll-snap-align: none; }

  /* ════════════════════════════
     RUN SCREEN
  ════════════════════════════ */
  .run-screen {
    display: none; position: fixed; inset: 0; z-index: 10;
    background: var(--bg);
    flex-direction: column; align-items: center; justify-content: center;
    padding: 40px 32px 140px;
  }
  .run-screen.active { display: flex; }
  .run-phase { font-size: 12px; font-weight: 500; letter-spacing: 0.3em; text-transform: uppercase; margin-bottom: 8px; transition: color 0.3s; }
  .run-time { font-family: 'Bebas Neue', sans-serif; font-size: clamp(140px,38vw,220px); line-height: 0.85; text-align: center; transition: color 0.3s; }
  .run-counter { font-size: 13px; letter-spacing: 0.22em; color: var(--muted); text-transform: uppercase; margin-top: 16px; }
  .run-counter span { color: var(--text); }
  .run-progress { width: 100%; max-width: 320px; height: 2px; background: #1e1e1e; border-radius: 2px; overflow: hidden; margin-top: 20px; }
  .run-progress-bar { height: 100%; width: 100%; transition: background 0.3s; }
  .run-actions { position: absolute; bottom: 40px; left: 24px; right: 24px; display: flex; gap: 12px; }
  .run-btn { flex: 1; height: 58px; border-radius: 12px; border: none; font-family: 'DM Mono', monospace; font-size: 12px; font-weight: 500; letter-spacing: 0.2em; text-transform: uppercase; cursor: pointer; transition: opacity 0.15s; -webkit-tap-highlight-color: transparent; user-select: none; }
  .run-btn:active { opacity: 0.75; }
  .rbtn-primary { background: #ffffff; color: #000; transition: background 0.4s; }
  .rbtn-ghost { background: transparent; color: var(--muted); border: 1px solid #2a2a2a; }

  .pulse { animation: pulse 0.13s ease-out; }
  @keyframes pulse { 0%{transform:scale(1)} 50%{transform:scale(1.06)} 100%{transform:scale(1)} }

  .skip-btn {
    position: absolute;
    top: 50%; transform: translateY(-120%);
    background: none; border: none;
    cursor: pointer; padding: 8px 20px;
    -webkit-tap-highlight-color: transparent;
    user-select: none; transition: opacity 0.2s;
    line-height: 1;
  }
  .skip-btn:active { opacity: 0.5; }
  .skip-prev { left: 24px; }
  .skip-next { right: 24px; }
</style>
</head>
<body>

<div class="glow" id="glow"></div>

<!-- ═══ PICKER ═══ -->
<div class="picker-overlay" id="pickerOverlay">
  <div class="picker-sheet">
    <div class="picker-header">
      <span class="picker-title" id="pickerTitle">Duration</span>
      <div class="picker-actions">
        <button class="picker-btn picker-back" onclick="pickerBack()">Back</button>
        <button class="picker-btn picker-save" onclick="pickerSave()">Save</button>
      </div>
    </div>
    <div class="picker-hint">Scroll to adjust</div>
    <div class="drum-labels">
      <div class="drum-label" id="drumMinLabel">min</div>
      <div class="drum-label">sec</div>
    </div>
    <div class="picker-drums">
      <div class="drum-col" id="drumMinCol"><div class="drum-inner" id="drumMin"></div></div>
      <div class="drum-col"><div class="drum-inner" id="drumSec"></div></div>
    </div>
  </div>
</div>

<!-- ═══ SAVE PRESET MODAL ═══ -->
<div class="modal-overlay" id="saveModal">
  <div class="modal-sheet">
    <div class="modal-title">Name this preset</div>
    <input class="modal-input" id="presetNameInput" type="text" placeholder="e.g. Ankle Rehab" maxlength="30">
    <div class="modal-actions">
      <button class="modal-btn modal-cancel" onclick="closeSaveModal()">Cancel</button>
      <button class="modal-btn modal-confirm" onclick="confirmSavePreset()">Save</button>
    </div>
  </div>
</div>

<!-- ═══ INTERVAL RUN SCREEN ═══ -->
<div class="run-screen" id="iRunScreen">
  <button class="skip-btn skip-prev" onclick="iSkipBack()"><svg width="44" height="80" viewBox="0 0 44 80" fill="none" xmlns="http://www.w3.org/2000/svg"><polyline points="36,8 8,40 36,72" stroke="#333" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"/></svg></button>
  <button class="skip-btn skip-next" onclick="iSkipNext()"><svg width="44" height="80" viewBox="0 0 44 80" fill="none" xmlns="http://www.w3.org/2000/svg"><polyline points="8,8 36,40 8,72" stroke="#333" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"/></svg></button>
  <div class="run-phase" id="iRunPhase">GET READY</div>
  <div class="run-time" id="iRunTime">05</div>
  <div class="run-counter">
    SET <span id="iRunSet">0</span>/<span id="iRunTotSets">5</span>
    &nbsp;·&nbsp;
    CYCLE <span id="iRunCycle">1</span>/<span id="iRunTotCycles">1</span>
  </div>
  <div class="run-progress"><div class="run-progress-bar" id="iRunBar"></div></div>
  <div class="run-actions"><button class="run-btn rbtn-primary" onclick="iStop()">STOP</button></div>
</div>

<!-- ═══ TEMPO RUN SCREEN ═══ -->
<div class="run-screen" id="tRunScreen">
  <button class="skip-btn skip-prev" onclick="tSkipBack()"><svg width="44" height="80" viewBox="0 0 44 80" fill="none" xmlns="http://www.w3.org/2000/svg"><polyline points="36,8 8,40 36,72" stroke="#333" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"/></svg></button>
  <button class="skip-btn skip-next" onclick="tSkipNext()"><svg width="44" height="80" viewBox="0 0 44 80" fill="none" xmlns="http://www.w3.org/2000/svg"><polyline points="8,8 36,40 8,72" stroke="#333" stroke-width="4" stroke-linecap="round" stroke-linejoin="round"/></svg></button>
  <div class="run-phase" id="tRunPhase">GET READY</div>
  <div class="run-time" id="tRunTime">05</div>
  <div class="run-counter">
    REP <span id="tRunRep">1</span>/<span id="tRunTotReps">5</span>
    &nbsp;·&nbsp;
    SET <span id="tRunSet">1</span>/<span id="tRunTotSets">5</span>
  </div>
  <div class="run-progress"><div class="run-progress-bar" id="tRunBar"></div></div>
  <div class="run-actions"><button class="run-btn rbtn-primary" onclick="tStop()">STOP</button></div>
</div>

<!-- ═══ HOME SCREEN ═══ -->
<div class="screen active" id="homeScreen">
  <div class="home-header">
    <div class="brand">Every Second Counts</div>
    <div class="tagline">Move better, live better</div>
  </div>

  <div style="width:100%;max-width:480px;padding:0 24px 20px;display:flex;flex-direction:column;gap:10px;margin-top:32px;">
    <div style="font-family:'Bebas Neue',sans-serif;font-size:16px;letter-spacing:0.08em;color:#ffffff;">Let's get moving</div>
    <div style="font-size:11px;line-height:1.7;color:#777;letter-spacing:0.05em;">
      <span style="color:#888;font-weight:700;">Intervals</span> — Set your work and rest periods, choose your sets, and go.
    </div>
    <div style="font-size:11px;line-height:1.7;color:#777;letter-spacing:0.05em;">
      <span style="color:#888;font-weight:700;">Tempo</span> — Control the speed of each rep — perfect for strength, rehab and body awareness.
    </div>
  </div>

  <div style="width:100%;max-width:480px;border-top:1px solid #1a1a1a;padding:8px 24px 0px;border-bottom:1px solid #1a1a1a;margin-bottom:24px;">
    <div class="timer-choices" style="margin-top:40px;">
      <div class="choice-btn" onclick="goTo('intervalScreen')">
        <div class="choice-icon">Intervals</div>
      </div>
      <div class="choice-btn" onclick="goTo('tempoScreen')">
        <div class="choice-icon">Tempo</div>
      </div>
    </div>
  </div>

  <div style="width:100%;max-width:480px;padding:0 24px 12px;">
    <div class="home-section-label" style="margin-bottom:12px;">Saved timers</div>
    <div class="presets-list" id="homePresetsList"></div>
  </div>

  <div style="width:100%;max-width:480px;padding:40px 24px 48px;text-align:center;">
    <div style="font-size:10px;letter-spacing:0.2em;text-transform:uppercase;color:#333;">Created by Senpen Designs</div>
  </div>
</div>

<!-- ═══ INTERVAL SETUP SCREEN ═══ -->
<div class="screen" id="intervalScreen">
  <div class="setup-inner">
    <div class="nav-bar">
      <span class="nav-back" onclick="goTo('homeScreen')">← Home</span>
      <span class="nav-title">Interval Timer</span>
    </div>
    <div class="settings">
      <div class="setting-row">
        <span class="setting-label">Countdown</span>
        <span class="setting-value-tap" id="iCdVal" onclick="openPicker('iCd',120)">5s</span>
      </div>
      <div class="setting-row">
        <span class="setting-label">Work</span>
        <span class="setting-value-tap" id="iWorkVal" onclick="openPicker('iWork',3600)">30s</span>
      </div>
      <div class="setting-row">
        <span class="setting-label">Rest</span>
        <span class="setting-value-tap" id="iRestVal" onclick="openPicker('iRest',300)">15s</span>
      </div>
      <div class="setting-row">
        <span class="setting-label">Sets</span>
        <div class="setting-controls">
          <button class="btn-adj" onclick="iAdj('sets',-1)">−</button>
          <span class="setting-value" id="iSetsVal">5</span>
          <button class="btn-adj" onclick="iAdj('sets',1)">+</button>
        </div>
      </div>
    </div>

    <!-- Advanced toggle -->
    <div style="display:flex;align-items:center;justify-content:space-between;padding:16px 0 0;">
      <span style="font-size:10px;letter-spacing:0.3em;text-transform:uppercase;color:#E8FF47;">Advanced</span>
      <label class="toggle">
        <input type="checkbox" id="advancedToggle" onchange="toggleAdvanced()">
        <div class="toggle-track"></div>
        <div class="toggle-thumb"></div>
      </label>
    </div>

    <div id="advancedSettings" style="display:none;">
      <div class="settings" style="margin-top:4px;">
        <div class="setting-row">
          <span class="setting-label">Cycles</span>
          <div class="setting-controls">
            <button class="btn-adj" onclick="iAdj('cycles',-1)">−</button>
            <span class="setting-value" id="iCyclesVal">1</span>
            <button class="btn-adj" onclick="iAdj('cycles',1)">+</button>
          </div>
        </div>
        <div class="setting-row">
          <span class="setting-label">Recovery</span>
          <span class="setting-value-tap" id="iRecoveryVal" onclick="openPicker('iRecovery',300)">60s</span>
        </div>
        <div class="setting-row">
          <span class="setting-label">Cooldown</span>
          <span class="setting-value-tap" id="iCooldownVal" onclick="openPicker('iCooldown',600)">60s</span>
        </div>
      </div>
    </div>

    <div class="btn-row" style="margin-top:20px;">
      <button class="btn-main" onclick="iStart()">START</button>
    </div>

    <!-- Presets section -->
    <div style="margin-top:28px;border-top:1px solid #1a1a1a;padding-top:20px;">
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;">
        <span style="font-size:10px;letter-spacing:0.3em;text-transform:uppercase;color:#888;">Presets</span>
        <button class="btn-save-preset" onclick="openSaveModal('interval')" style="height:30px;font-size:10px;padding:0 14px;">Save current</button>
      </div>
      <div id="iPresetsList" style="display:flex;flex-direction:column;gap:8px;"></div>
    </div>

  </div>
</div>

<!-- ═══ TEMPO SETUP SCREEN ═══ -->
<div class="screen" id="tempoScreen">
  <div class="setup-inner">
    <div class="nav-bar">
      <span class="nav-back" onclick="goTo('homeScreen')">← Home</span>
      <span class="nav-title">Tempo Timer</span>
    </div>
    <div class="phase-selector">
      <button class="phase-btn" id="pc2" onclick="setPC(2)">2 phases</button>
      <button class="phase-btn active" id="pc3" onclick="setPC(3)">3 phases</button>
      <button class="phase-btn" id="pc4" onclick="setPC(4)">4 phases</button>
    </div>
    <div class="settings" id="tPhaseRows"></div>
    <div style="height:8px"></div>
    <div class="settings">
      <div class="setting-row">
        <span class="setting-label">Countdown</span>
        <span class="setting-value-tap" id="tCdVal" onclick="openPicker('tCd',120)">5s</span>
      </div>
      <div class="setting-row">
        <span class="setting-label">Reps per set</span>
        <div class="setting-controls">
          <button class="btn-adj" onclick="tAdj('reps',-1)">−</button>
          <span class="setting-value" id="tRepsVal">5</span>
          <button class="btn-adj" onclick="tAdj('reps',1)">+</button>
        </div>
      </div>
      <div class="setting-row">
        <span class="setting-label">Sets</span>
        <div class="setting-controls">
          <button class="btn-adj" onclick="tAdj('sets',-1)">−</button>
          <span class="setting-value" id="tSetsVal">5</span>
          <button class="btn-adj" onclick="tAdj('sets',1)">+</button>
        </div>
      </div>
      <div class="setting-row">
        <span class="setting-label">Rest between sets</span>
        <span class="setting-value-tap" id="tRestVal" onclick="openPicker('tRest',300)">1:00</span>
      </div>
    </div>
    <div class="btn-row">
      <button class="btn-save-preset" onclick="openSaveModal('tempo')">Save</button>
      <button class="btn-main" onclick="tStart()">START</button>
    </div>
  </div>
</div>

<script>
// ── NAV ───────────────────────────────────
function goTo(id){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0,0);
  if(id==='homeScreen') renderHomePresets();
  if(id==='intervalScreen') renderIPresets();
}

// ── AUDIO ─────────────────────────────────
const AudioCtx=window.AudioContext||window.webkitAudioContext;
let actx=null;
function audio(){if(!actx)actx=new AudioCtx();return actx;}
function beep(f,d,v=0.3){
  try{const c=audio(),o=c.createOscillator(),g=c.createGain();
  o.connect(g);g.connect(c.destination);o.type='sine';o.frequency.value=f;
  g.gain.setValueAtTime(v,c.currentTime);g.gain.exponentialRampToValueAtTime(0.001,c.currentTime+d);
  o.start(c.currentTime);o.stop(c.currentTime+d);}catch(e){}
}
const bWork=()=>{beep(880,.1);setTimeout(()=>beep(1100,.14),120);};
const bRest=()=>{beep(600,.14);setTimeout(()=>beep(420,.18),120);};
const bCd=()=>beep(800,.08,.25);
const bCdGo=()=>{beep(1100,.08);setTimeout(()=>beep(1400,.18),100);};
const bTick=()=>beep(1200,.06,.18);
const bDone=()=>{beep(660,.09);setTimeout(()=>beep(880,.09),110);setTimeout(()=>beep(1100,.28),220);};

function fmt(s){
  if(s>=60){const m=Math.floor(s/60),r=s%60;return r===0?`${m}:00`:`${m}:${String(r).padStart(2,'0')}`;}
  return String(s).padStart(2,'0');
}
function pulse(id){const el=document.getElementById(id);el.classList.remove('pulse');void el.offsetWidth;el.classList.add('pulse');}
function setRunColor(sid,color){
  const s=document.getElementById(sid);
  s.querySelectorAll('.run-phase,.run-time').forEach(el=>el.style.color=color);
  s.querySelector('.run-progress-bar').style.background=color;
  const pb=s.querySelector('.rbtn-primary');if(pb)pb.style.background=color;
  document.getElementById('glow').style.background=color;
}
function showStopped(sid,rFn,bFn){
  document.querySelector('#'+sid+' .run-actions').innerHTML=`
    <button class="run-btn rbtn-primary" onclick="${rFn}()">RESUME</button>
    <button class="run-btn rbtn-ghost" onclick="${bFn}()">BACK</button>`;
}
function showRunning(sid,sFn){
  document.querySelector('#'+sid+' .run-actions').innerHTML=`
    <button class="run-btn rbtn-primary" onclick="${sFn}()">STOP</button>`;
}
function showDone(sid,bFn){
  document.querySelector('#'+sid+' .run-actions').innerHTML=`
    <button class="run-btn rbtn-primary" onclick="${bFn}()">BACK</button>`;
}

// ── PICKER ────────────────────────────────
let pickerKey=null,pickerMax=3600;
const ITEM_H=44;

function buildDrum(innerId,count){
  const el=document.getElementById(innerId);el.innerHTML='';
  const t=document.createElement('div');t.className='drum-pad';el.appendChild(t);
  for(let i=0;i<count;i++){
    const d=document.createElement('div');d.className='drum-item';d.textContent=String(i).padStart(2,'0');el.appendChild(d);
  }
  const b=document.createElement('div');b.className='drum-pad';el.appendChild(b);
}
function drumScrollTo(id,val){document.getElementById(id).scrollTop=val*ITEM_H;}
function drumGetVal(id){return Math.round(document.getElementById(id).scrollTop/ITEM_H);}
function highlightDrum(id){
  const el=document.getElementById(id),val=drumGetVal(id);
  el.querySelectorAll('.drum-item').forEach((item,i)=>{
    item.style.color=i===val?'#E8FF47':'#2a2a2a';
    item.style.fontSize=i===val?'40px':'32px';
  });
}
function setupDrumListeners(id){
  const el=document.getElementById(id);let raf=null;
  el.addEventListener('scroll',()=>{cancelAnimationFrame(raf);raf=requestAnimationFrame(()=>highlightDrum(id));});
}

function openPicker(key,maxSec){
  pickerKey=key;pickerMax=maxSec;
  const vals={iCd:I.cd,iWork:I.work,iRest:I.rest,iRecovery:I.recovery,iCooldown:I.cooldown,tCd:T.cd,tRest:T.rest};
  let cur=vals[key]!==undefined?vals[key]:0;
  if(key.startsWith('tPh')){const i=parseInt(key.replace('tPh',''));cur=T.dur[i];}
  const titles={iCd:'Countdown',iWork:'Work',iRest:'Rest',iRecovery:'Recovery',iCooldown:'Cooldown',tCd:'Countdown',tRest:'Rest between sets'};
  let title=titles[key]||'Duration';
  if(key.startsWith('tPh')){const i=parseInt(key.replace('tPh',''));title=activePhases()[i]?.label||'Phase';}
  document.getElementById('pickerTitle').textContent=title;
  const showMin=maxSec>=60;
  document.getElementById('drumMinCol').style.display=showMin?'block':'none';
  document.getElementById('drumMinLabel').style.display=showMin?'block':'none';
  if(showMin){
    buildDrum('drumMin',Math.floor(maxSec/60)+1);
    setupDrumListeners('drumMin');
    setTimeout(()=>{drumScrollTo('drumMin',Math.floor(cur/60));highlightDrum('drumMin');},30);
  }
  buildDrum('drumSec',60);
  setupDrumListeners('drumSec');
  setTimeout(()=>{drumScrollTo('drumSec',cur%60);highlightDrum('drumSec');},30);
  document.getElementById('pickerOverlay').classList.add('active');
}

function pickerBack(){document.getElementById('pickerOverlay').classList.remove('active');}

function pickerSave(){
  const showMin=pickerMax>=60;
  const mins=showMin?drumGetVal('drumMin'):0;
  const secs=drumGetVal('drumSec');
  let total=mins*60+secs;if(total<1)total=1;
  if(pickerKey==='iCd'){I.cd=total;document.getElementById('iCdVal').textContent=total<60?total+'s':fmt(total);}
  else if(pickerKey==='iWork'){I.work=total;document.getElementById('iWorkVal').textContent=total<60?total+'s':fmt(total);}
  else if(pickerKey==='iRest'){I.rest=total;document.getElementById('iRestVal').textContent=total<60?total+'s':fmt(total);}
  else if(pickerKey==='iRecovery'){I.recovery=total;document.getElementById('iRecoveryVal').textContent=total<60?total+'s':fmt(total);}
  else if(pickerKey==='iCooldown'){I.cooldown=total;document.getElementById('iCooldownVal').textContent=total<60?total+'s':fmt(total);}
  else if(pickerKey==='tCd'){T.cd=total;document.getElementById('tCdVal').textContent=total<60?total+'s':fmt(total);}
  else if(pickerKey==='tRest'){T.rest=total;document.getElementById('tRestVal').textContent=total<60?total+'s':fmt(total);}
  else if(pickerKey.startsWith('tPh')){const i=parseInt(pickerKey.replace('tPh',''));T.dur[i]=total;document.getElementById('tD'+i).textContent=total<60?total+'s':fmt(total);}
  document.getElementById('pickerOverlay').classList.remove('active');
}

// ── PRESETS ───────────────────────────────
function loadPresets(){try{return JSON.parse(localStorage.getItem('tmc_presets')||'[]');}catch(e){return[];}}
function savePresets(p){localStorage.setItem('tmc_presets',JSON.stringify(p));}

let pendingPresetType=null;

function openSaveModal(type){
  pendingPresetType=type;
  document.getElementById('presetNameInput').value='';
  document.getElementById('saveModal').classList.add('active');
  setTimeout(()=>document.getElementById('presetNameInput').focus(),100);
}
function closeSaveModal(){document.getElementById('saveModal').classList.remove('active');}

function confirmSavePreset(){
  const name=document.getElementById('presetNameInput').value.trim();
  if(!name)return;
  const presets=loadPresets();
  let preset={name,type:pendingPresetType,created:Date.now()};
  if(pendingPresetType==='interval'){
    preset.data={cd:I.cd,work:I.work,rest:I.rest,sets:I.sets,recovery:I.recovery,cycles:I.cycles,cooldown:I.cooldown};
  } else {
    preset.data={cd:T.cd,pc:T.pc,dur:[...T.dur],reps:T.reps,sets:T.sets,rest:T.rest};
  }
  presets.unshift(preset);
  savePresets(presets);
  closeSaveModal();
}

function loadPreset(index){
  const presets=loadPresets();
  const p=presets[index];
  if(!p)return;
  if(p.type==='interval'){
    I.cd=p.data.cd;I.work=p.data.work;I.rest=p.data.rest;I.sets=p.data.sets;
    if(p.data.recovery)I.recovery=p.data.recovery;
    if(p.data.cycles)I.cycles=p.data.cycles;
    if(p.data.cooldown)I.cooldown=p.data.cooldown;
    document.getElementById('iCdVal').textContent=I.cd<60?I.cd+'s':fmt(I.cd);
    document.getElementById('iWorkVal').textContent=I.work<60?I.work+'s':fmt(I.work);
    document.getElementById('iRestVal').textContent=I.rest<60?I.rest+'s':fmt(I.rest);
    document.getElementById('iSetsVal').textContent=I.sets;
    document.getElementById('iRecoveryVal').textContent=I.recovery<60?I.recovery+'s':fmt(I.recovery);
    document.getElementById('iCyclesVal').textContent=I.cycles;
    document.getElementById('iCooldownVal').textContent=I.cooldown<60?I.cooldown+'s':fmt(I.cooldown);
    goTo('intervalScreen');
  } else {
    T.cd=p.data.cd;T.pc=p.data.pc;T.dur=[...p.data.dur];T.reps=p.data.reps;T.sets=p.data.sets;T.rest=p.data.rest;
    ['pc2','pc3','pc4'].forEach(id=>document.getElementById(id).classList.remove('active'));
    document.getElementById('pc'+T.pc).classList.add('active');
    document.getElementById('tCdVal').textContent=fmt(T.cd);
    document.getElementById('tRepsVal').textContent=T.reps;
    document.getElementById('tSetsVal').textContent=T.sets;
    document.getElementById('tRestVal').textContent=fmt(T.rest);
    buildPhaseRows();
    goTo('tempoScreen');
  }
}

function deletePreset(index,e){
  e.stopPropagation();
  const presets=loadPresets();
  presets.splice(index,1);
  savePresets(presets);
  renderHomePresets();
  renderIPresets();
}

function presetMeta(p){
  if(p.type==='interval'){
    return `Work ${fmt(p.data.work)} · Rest ${fmt(p.data.rest)} · ${p.data.sets} sets`;
  } else {
    return `${p.data.pc} phases · ${p.data.reps} reps · ${p.data.sets} sets`;
  }
}

function renderHomePresets(){
  const list=document.getElementById('homePresetsList');
  const presets=loadPresets();
  if(presets.length===0){list.innerHTML='<div class="no-presets">Add your first timer</div>';return;}
  list.innerHTML=presets.map((p,i)=>`
    <div class="preset-card" onclick="loadPreset(${i})">
      <div class="preset-info">
        <div class="preset-name">${p.name}</div>
        <div class="preset-meta">${presetMeta(p)}</div>
      </div>
      <div class="preset-actions">
        <span class="preset-type">${p.type==='interval'?'HIIT':'TEMPO'}</span>
        <span class="preset-del" onclick="deletePreset(${i},event)">×</span>
      </div>
    </div>`).join('');
}

function toggleAdvanced(){
  const on=document.getElementById('advancedToggle').checked;
  document.getElementById('advancedSettings').style.display=on?'block':'none';
}

function renderIPresets(){
  const list=document.getElementById('iPresetsList');
  const presets=loadPresets().filter(p=>p.type==='interval');
  if(presets.length===0){list.innerHTML='<div class="no-presets" style="padding:8px 0;">No saved interval presets</div>';return;}
  const all=loadPresets();
  list.innerHTML=presets.map(p=>{
    const idx=all.indexOf(p);
    return `<div class="preset-card" onclick="loadPreset(${idx})" style="padding:12px 16px;">
      <div class="preset-info">
        <div class="preset-name">${p.name}</div>
        <div class="preset-meta">${presetMeta(p)}</div>
      </div>
      <span class="preset-del" onclick="deletePreset(${idx},event)">×</span>
    </div>`;
  }).join('');
}

// ── INTERVAL ──────────────────────────────
const I={cd:5,work:30,rest:15,sets:5,recovery:60,cycles:1,cooldown:60,curSet:0,curCycle:0,phase:'idle',left:30,total:30,iv:null,running:false};
const IC={countdown:'#aaaaaa',work:'#E8FF47',rest:'#47FFDB',recovery:'#FF9F47',cooldown:'#B47FFF',done:'#ffffff'};

function iDrawRun(){
  const lbl={countdown:'GET READY',work:'WORK',rest:'REST',recovery:'RECOVERY',cooldown:'COOLDOWN',done:'DONE'};
  document.getElementById('iRunPhase').textContent=lbl[I.phase]||'';
  document.getElementById('iRunTime').textContent=fmt(I.left);
  document.getElementById('iRunSet').textContent=I.curSet;
  document.getElementById('iRunTotSets').textContent=I.sets;
  document.getElementById('iRunCycle').textContent=I.curCycle;
  document.getElementById('iRunTotCycles').textContent=I.cycles;
  document.getElementById('iRunBar').style.width=(I.total>0?(I.left/I.total*100):100)+'%';
  setRunColor('iRunScreen',IC[I.phase]||'#fff');
}

function iGo(phase){
  I.phase=phase;
  if(phase==='countdown'){I.total=I.left=I.cd;bCd();}
  else if(phase==='work'){I.curSet++;I.total=I.left=I.work;bWork();}
  else if(phase==='rest'){I.total=I.left=I.rest;bRest();}
  else if(phase==='recovery'){I.total=I.left=I.recovery;bRest();}
  else if(phase==='cooldown'){I.total=I.left=I.cooldown;bRest();}
  else if(phase==='done'){
    I.running=false;clearInterval(I.iv);bDone();iDrawRun();
    showDone('iRunScreen','iBack');return;
  }
  pulse('iRunTime');iDrawRun();
}

const advOn=()=>document.getElementById('advancedToggle')&&document.getElementById('advancedToggle').checked;

function iTick(){
  I.left--;
  if(I.left<=0){
    if(I.phase==='countdown'){bCdGo();iGo('work');}
    else if(I.phase==='work'){
      if(I.curSet<I.sets) iGo('rest');
      else {
        // all sets done for this cycle
        if(advOn()&&I.curCycle<I.cycles) iGo('recovery');
        else if(advOn()&&I.cooldown>0) iGo('cooldown');
        else iGo('done');
      }
    }
    else if(I.phase==='rest') iGo('work');
    else if(I.phase==='recovery'){
      // start next cycle
      I.curCycle++;I.curSet=0;iGo('work');
    }
    else if(I.phase==='cooldown') iGo('done');
  } else {
    if(I.phase==='countdown'&&I.left<=3)bCd();
    iDrawRun();
  }
}

function iStart(){
  document.getElementById('iRunScreen').classList.add('active');
  showRunning('iRunScreen','iStop');
  I.curSet=0;I.curCycle=1;I.running=true;
  iGo(I.cd>0?'countdown':'work');
  I.iv=setInterval(iTick,1000);
}
function iSkipNext(){
  if(!I.running)return;
  clearInterval(I.iv);
  I.left=0;iTick();
  if(I.running)I.iv=setInterval(iTick,1000);
}
function iSkipBack(){
  if(!I.running)return;
  // restart current phase
  if(I.phase==='work'){I.left=I.total=I.work;}
  else if(I.phase==='rest'){I.left=I.total=I.rest;}
  else if(I.phase==='recovery'){I.left=I.total=I.recovery;}
  else if(I.phase==='cooldown'){I.left=I.total=I.cooldown;}
  else if(I.phase==='countdown'){I.left=I.total=I.cd;}
  pulse('iRunTime');iDrawRun();
}
function iStop(){clearInterval(I.iv);I.running=false;showStopped('iRunScreen','iResume','iBack');}
function iResume(){I.running=true;showRunning('iRunScreen','iStop');I.iv=setInterval(iTick,1000);}
function iBack(){
  clearInterval(I.iv);I.running=false;I.phase='idle';I.curSet=0;I.curCycle=0;
  document.getElementById('iRunScreen').classList.remove('active');
}
function iAdj(k,d){
  if(k==='sets')I.sets=Math.max(1,I.sets+d);
  if(k==='cycles')I.cycles=Math.max(1,I.cycles+d);
  document.getElementById('iSetsVal').textContent=I.sets;
  document.getElementById('iCyclesVal').textContent=I.cycles;
}

// ── TEMPO ─────────────────────────────────
const ALL_PHASES=[
  {key:'down',label:'DOWN',color:'#FF6B6B'},
  {key:'hold',label:'HOLD',color:'#FFB347'},
  {key:'up',  label:'UP',  color:'#69FF84'},
  {key:'hold2',label:'HOLD',color:'#B47FFF'},
];
function activePhases(){
  if(T.pc===2)return[ALL_PHASES[0],ALL_PHASES[2]];
  if(T.pc===3)return[ALL_PHASES[0],ALL_PHASES[1],ALL_PHASES[2]];
  return ALL_PHASES;
}
const T={cd:5,pc:3,dur:[3,3,3,3],reps:5,sets:5,rest:60,curRep:0,curSet:0,curP:0,left:0,total:0,phase:'idle',iv:null,running:false};

function setPC(n){
  T.pc=n;
  ['pc2','pc3','pc4'].forEach(id=>document.getElementById(id).classList.remove('active'));
  document.getElementById('pc'+n).classList.add('active');
  buildPhaseRows();
}
function buildPhaseRows(){
  const c=document.getElementById('tPhaseRows');c.innerHTML='';
  activePhases().forEach((p,i)=>{
    const row=document.createElement('div');row.className='setting-row';
    row.innerHTML=`
      <span class="setting-label"><span class="phase-dot" style="background:${p.color}"></span>${p.label}</span>
      <span class="setting-value-tap" id="tD${i}" onclick="openPicker('tPh${i}',60)">${T.dur[i]}s</span>`;
    c.appendChild(row);
  });
}
function tAdj(k,d){
  if(k==='reps')T.reps=Math.max(1,T.reps+d);
  if(k==='sets')T.sets=Math.max(1,T.sets+d);
  document.getElementById('tRepsVal').textContent=T.reps;
  document.getElementById('tSetsVal').textContent=T.sets;
}
function tDrawRun(){
  let label,color;
  if(T.phase==='countdown'){label='GET READY';color='#aaaaaa';}
  else if(T.phase==='rest'){label='REST';color='#47FFDB';}
  else if(T.phase==='done'){label='DONE';color='#ffffff';}
  else{const p=activePhases()[T.curP];label=p.label;color=p.color;}
  document.getElementById('tRunPhase').textContent=label;
  document.getElementById('tRunTime').textContent=fmt(T.left);
  document.getElementById('tRunRep').textContent=T.curRep;
  document.getElementById('tRunTotReps').textContent=T.reps;
  document.getElementById('tRunSet').textContent=T.curSet;
  document.getElementById('tRunTotSets').textContent=T.sets;
  document.getElementById('tRunBar').style.width=(T.total>0?(T.left/T.total*100):100)+'%';
  setRunColor('tRunScreen',color);
}
function tGoPhase(pi){T.curP=pi;T.phase='rep';T.total=T.left=T.dur[pi];bTick();pulse('tRunTime');tDrawRun();}
function tGoRest(){T.phase='rest';T.total=T.left=T.rest;bRest();pulse('tRunTime');tDrawRun();}
function tTick(){
  T.left--;
  if(T.left<=0){
    if(T.phase==='countdown'){bCdGo();T.curRep=1;T.curSet=1;tGoPhase(0);return;}
    if(T.phase==='rest'){T.curSet++;T.curRep=1;tGoPhase(0);return;}
    const next=T.curP+1;
    if(next<activePhases().length){tGoPhase(next);return;}
    if(T.curRep<T.reps){T.curRep++;tGoPhase(0);return;}
    if(T.curSet<T.sets){tGoRest();return;}
    T.running=false;T.phase='done';clearInterval(T.iv);bDone();tDrawRun();showDone('tRunScreen','tBack');
  } else {if(T.phase==='countdown'&&T.left<=3)bCd();tDrawRun();}
}
function tStart(){
  document.getElementById('tRunScreen').classList.add('active');
  showRunning('tRunScreen','tStop');T.running=true;
  if(T.cd>0){T.phase='countdown';T.total=T.left=T.cd;bCd();pulse('tRunTime');tDrawRun();}
  else{T.curRep=1;T.curSet=1;tGoPhase(0);}
  T.iv=setInterval(tTick,1000);
}
function tSkipNext(){
  if(!T.running)return;
  clearInterval(T.iv);
  T.left=0;tTick();
  if(T.running)T.iv=setInterval(tTick,1000);
}
function tSkipBack(){
  if(!T.running)return;
  if(T.phase==='rep'){T.left=T.total=T.dur[T.curP];}
  else if(T.phase==='rest'){T.left=T.total=T.rest;}
  else if(T.phase==='countdown'){T.left=T.total=T.cd;}
  pulse('tRunTime');tDrawRun();
}
function tStop(){clearInterval(T.iv);T.running=false;showStopped('tRunScreen','tResume','tBack');}
function tResume(){T.running=true;showRunning('tRunScreen','tStop');T.iv=setInterval(tTick,1000);}
function tBack(){
  clearInterval(T.iv);T.running=false;T.phase='idle';T.curRep=0;T.curSet=0;
  document.getElementById('tRunScreen').classList.remove('active');
}

// init
buildPhaseRows();
renderHomePresets();
renderIPresets();
</script>
</body>
</html>
