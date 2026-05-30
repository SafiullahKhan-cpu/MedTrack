<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MedTrack Pro v3 — Medical Command Center</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,400&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore-compat.js"></script>

<style>
/* ===================================================
   MEDTRACK PRO v3 — DESIGN SYSTEM
=================================================== */
:root {
  --bg: #060C18;
  --bg2: #09111F;
  --surface: #0C1827;
  --surface2: #101E30;
  --surface3: #152438;
  --border: rgba(0,210,190,0.10);
  --border2: rgba(255,255,255,0.05);
  --primary: #00D2BE;
  --primary-dim: rgba(0,210,190,0.12);
  --primary-glow: 0 0 30px rgba(0,210,190,0.22);
  --blue: #3A86FF;
  --blue-dim: rgba(58,134,255,0.13);
  --green: #06FFA5;
  --green-dim: rgba(6,255,165,0.10);
  --yellow: #FFB340;
  --yellow-dim: rgba(255,179,64,0.13);
  --red: #FF4757;
  --red-dim: rgba(255,71,87,0.13);
  --purple: #C77DFF;
  --purple-dim: rgba(199,125,255,0.13);
  --orange: #FF8C42;
  --text: #C8DCED;
  --text-muted: #4D7A9A;
  --text-dim: #243D58;
  --font-head: 'Syne', sans-serif;
  --font-body: 'DM Sans', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
  --r: 14px;
  --r-sm: 8px;
  --shadow: 0 4px 24px rgba(0,0,0,0.4);
}
*,*::before,*::after { box-sizing: border-box; margin: 0; padding: 0; }
html { scroll-behavior: smooth; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--font-body);
  font-size: 14px;
  line-height: 1.6;
  min-height: 100vh;
  overflow-x: hidden;
}
body::before {
  content: '';
  position: fixed; inset: 0;
  background:
    radial-gradient(ellipse 80% 60% at 10% 0%, rgba(0,210,190,0.035) 0%, transparent 60%),
    radial-gradient(ellipse 60% 80% at 90% 100%, rgba(58,134,255,0.035) 0%, transparent 60%);
  pointer-events: none; z-index: 0;
}
::-webkit-scrollbar { width: 4px; height: 4px; }
::-webkit-scrollbar-track { background: var(--bg2); }
::-webkit-scrollbar-thumb { background: var(--surface3); border-radius: 3px; }

/* ===== TOPNAV ===== */
#topnav {
  position: fixed; top: 0; left: 0; right: 0;
  z-index: 1000; height: 56px;
  background: rgba(6,12,24,0.92);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--border);
  display: flex; align-items: center; justify-content: space-between;
  padding: 0 18px; gap: 12px; transition: all 0.2s;
}
.nav-brand {
  font-family: var(--font-head); font-weight: 800; font-size: 16px;
  color: var(--primary); white-space: nowrap; cursor: pointer;
  display: flex; align-items: center; gap: 7px; letter-spacing: -0.5px;
  flex-shrink: 0;
}
.nav-tabs {
  display: flex; gap: 2px;
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--r-sm);
  padding: 3px;
}
.nav-tab {
  background: none; border: none; color: var(--text-muted);
  cursor: pointer; padding: 5px 13px; font-size: 12px;
  font-family: var(--font-body); border-radius: 6px;
  transition: all 0.2s; white-space: nowrap;
  display: flex; align-items: center; gap: 5px;
}
.nav-tab:hover { color: var(--text); background: var(--surface2); }
.nav-tab.active { background: var(--primary-dim); color: var(--primary); }
.nav-search-wrap { flex: 1; max-width: 380px; position: relative; }
.nav-search-wrap input {
  width: 100%; background: var(--surface2);
  border: 1px solid var(--border); border-radius: 30px;
  padding: 6px 14px 6px 34px; color: var(--text);
  font-family: var(--font-body); font-size: 12px; outline: none;
  transition: border-color 0.2s;
}
.nav-search-wrap input::placeholder { color: var(--text-muted); }
.nav-search-wrap input:focus { border-color: var(--primary); }
.search-icon { position: absolute; left: 11px; top: 50%; transform: translateY(-50%); color: var(--text-muted); font-size: 12px; }
#search-dropdown {
  position: absolute; top: calc(100% + 6px); left: 0; right: 0;
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); overflow: hidden; display: none;
  box-shadow: var(--shadow); z-index: 200; max-height: 360px; overflow-y: auto;
}
#search-dropdown.show { display: block; }
.sr-item { display: flex; align-items: center; gap: 10px; padding: 10px 14px; cursor: pointer; transition: background 0.15s; }
.sr-item:hover { background: var(--surface2); }
.sr-item .sr-icon { font-size: 18px; }
.sr-item .sr-name { font-size: 13px; font-weight: 600; }
.sr-item .sr-sub { font-size: 11px; color: var(--text-muted); }
.sr-item .sr-badge { margin-left: auto; font-size: 10px; font-family: var(--font-mono); background: var(--surface3); border-radius: 4px; padding: 1px 6px; color: var(--text-muted); }
.nav-right { display: flex; align-items: center; gap: 6px; flex-shrink: 0; }
.nav-btn {
  background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); color: var(--text); cursor: pointer;
  padding: 5px 9px; font-size: 12px; font-family: var(--font-body);
  transition: all 0.2s; display: flex; align-items: center; gap: 4px;
  white-space: nowrap;
}
.nav-btn:hover { background: var(--surface3); border-color: var(--primary); color: var(--primary); }
#timer-display {
  font-family: var(--font-mono); font-size: 12px; color: var(--green);
  background: var(--green-dim); border: 1px solid rgba(6,255,165,0.18);
  border-radius: var(--r-sm); padding: 5px 10px; min-width: 90px;
  text-align: center; cursor: pointer; margin-left: 6px;
}
#timer-display.running { animation: pulse-timer 2s infinite; }
@keyframes pulse-timer {
  0%,100%{box-shadow:0 0 0 0 rgba(6,255,165,0.3)} 50%{box-shadow:0 0 0 5px rgba(6,255,165,0)}
}

/* ===== VIEWS ===== */
.view { display: none; min-height: 100vh; padding: 72px 22px 40px; max-width: 1320px; margin: 0 auto; position: relative; z-index: 1; }
.view.active { display: block; animation: fadeUp 0.25s ease; }
@keyframes fadeUp { from{opacity:0;transform:translateY(8px)} to{opacity:1;transform:none} }

/* ===== BUTTONS ===== */
.btn { display: inline-flex; align-items: center; gap: 6px; border-radius: var(--r-sm); cursor: pointer; font-family: var(--font-body); font-size: 13px; border: none; transition: all 0.2s; padding: 8px 16px; }
.btn-primary { background: var(--primary); color: var(--bg); font-weight: 600; }
.btn-primary:hover { filter: brightness(1.1); box-shadow: 0 4px 20px rgba(0,210,190,0.3); }
.btn-secondary { background: var(--surface2); border: 1px solid var(--border); color: var(--text); }
.btn-secondary:hover { border-color: var(--primary); color: var(--primary); }
.btn-danger { background: var(--red-dim); border: 1px solid rgba(255,71,87,0.3); color: var(--red); }
.btn-danger:hover { background: rgba(255,71,87,0.25); }
.btn-ghost { background: none; border: 1px solid var(--border); color: var(--text-muted); }
.btn-ghost:hover { color: var(--text); background: var(--surface2); }
.btn-sm { padding: 5px 10px; font-size: 12px; }

.section-title { font-family: var(--font-head); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px; color: var(--text-muted); margin-bottom: 12px; }

/* ===================================================
   HOME / BOOSTER PANEL
=================================================== */
.home-grid { display: grid; grid-template-columns: 1fr 320px; gap: 18px; margin-bottom: 20px; }
@media(max-width:900px){ .home-grid { grid-template-columns: 1fr; } }
.booster-panel {
  background: linear-gradient(135deg, rgba(0,210,190,0.06), rgba(58,134,255,0.04));
  border: 1px solid rgba(0,210,190,0.15);
  border-radius: var(--r); padding: 24px; position: relative; overflow: hidden;
}
.booster-panel::before {
  content: ''; position: absolute; top: -40px; right: -40px;
  width: 200px; height: 200px; border-radius: 50%;
  background: radial-gradient(circle, rgba(0,210,190,0.06), transparent 70%);
  pointer-events: none;
}
.booster-greeting { font-size: 12px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 4px; }
.booster-title { font-family: var(--font-head); font-size: 26px; font-weight: 800; letter-spacing: -0.5px; margin-bottom: 16px; }
.booster-title span { color: var(--primary); }
.quote-box { background: rgba(0,210,190,0.05); border-left: 3px solid var(--primary); border-radius: 0 var(--r-sm) var(--r-sm) 0; padding: 12px 16px; margin-bottom: 16px; }
.quote-text { font-size: 14px; color: var(--text); font-style: italic; line-height: 1.5; }
.quote-author { font-size: 11px; color: var(--text-muted); margin-top: 4px; }
.why-started-box { margin-bottom: 16px; }
.why-started-label { font-size: 11px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 6px; }
.why-started-input { width: 100%; background: var(--surface2); border: 1px solid var(--border2); border-radius: var(--r-sm); padding: 10px 14px; color: var(--text); font-family: var(--font-body); font-size: 13px; outline: none; resize: none; transition: border-color 0.2s; min-height: 60px; }
.why-started-input:focus { border-color: var(--primary); }
.quick-actions { display: flex; gap: 8px; flex-wrap: wrap; }
.islamic-toggle { margin-top: 14px; display: flex; align-items: center; gap: 8px; font-size: 12px; color: var(--text-muted); }
.toggle-switch { width: 36px; height: 20px; background: var(--surface3); border-radius: 10px; position: relative; cursor: pointer; transition: background 0.2s; }
.toggle-switch.on { background: var(--primary); }
.toggle-knob { position: absolute; top: 3px; left: 3px; width: 14px; height: 14px; background: white; border-radius: 50%; transition: left 0.2s; }
.toggle-switch.on .toggle-knob { left: 19px; }
.side-panel { display: flex; flex-direction: column; gap: 14px; }
.streak-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 18px 20px; text-align: center; }
.streak-fire { font-size: 40px; line-height: 1; margin-bottom: 6px; }
.streak-number { font-family: var(--font-mono); font-size: 36px; font-weight: 700; color: var(--orange); line-height: 1; }
.streak-label { font-size: 11px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 1px; margin-top: 4px; }
.streak-best { font-size: 12px; color: var(--text-muted); margin-top: 8px; }
.streak-best span { color: var(--yellow); font-weight: 600; }
.reminders-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 16px; flex: 1; }
.reminders-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
.reminder-item { display: flex; align-items: center; gap: 10px; padding: 8px 10px; background: var(--surface2); border-radius: var(--r-sm); margin-bottom: 6px; font-size: 13px; }
.reminder-item .del-btn { background: none; border: none; color: var(--text-dim); cursor: pointer; font-size: 14px; margin-left: auto; transition: color 0.2s; }
.reminder-item .del-btn:hover { color: var(--red); }
.reminder-input-row { display: flex; gap: 6px; margin-top: 8px; }
.mini-input { flex: 1; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 7px 10px; color: var(--text); font-family: var(--font-body); font-size: 12px; outline: none; }
.mini-input:focus { border-color: var(--primary); }
.mini-input::placeholder { color: var(--text-muted); }

/* ===== STAT CARDS ===== */
.stats-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 12px; margin-bottom: 22px; }
.stat-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 16px 18px; position: relative; overflow: hidden; transition: transform 0.2s, border-color 0.2s; }
.stat-card::before { content: ''; position: absolute; top: 0; left: 0; width: 3px; height: 100%; border-radius: 2px 0 0 2px; }
.stat-card.c-teal::before { background: var(--primary); }
.stat-card.c-blue::before { background: var(--blue); }
.stat-card.c-green::before { background: var(--green); }
.stat-card.c-yellow::before { background: var(--yellow); }
.stat-card.c-red::before { background: var(--red); }
.stat-card.c-purple::before { background: var(--purple); }
.stat-card:hover { transform: translateY(-2px); border-color: var(--primary); }
.stat-label { font-size: 10px; text-transform: uppercase; letter-spacing: 1px; color: var(--text-muted); margin-bottom: 5px; }
.stat-value { font-family: var(--font-mono); font-size: 24px; font-weight: 500; color: var(--text); line-height: 1.1; }
.stat-sub { font-size: 11px; color: var(--text-muted); margin-top: 3px; }
.stat-icon { position: absolute; top: 14px; right: 14px; font-size: 22px; opacity: 0.25; }

/* ===================================================
   DUE TODAY PANEL
=================================================== */
.due-today-section { background: var(--surface); border: 1px solid rgba(255,71,87,0.2); border-radius: var(--r); padding: 18px 20px; margin-bottom: 18px; }
.due-today-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; }
.due-count-badge { background: var(--red-dim); border: 1px solid rgba(255,71,87,0.3); color: var(--red); border-radius: 20px; padding: 2px 10px; font-size: 12px; font-family: var(--font-mono); font-weight: 700; }
.due-topic-list { display: flex; flex-wrap: wrap; gap: 8px; }
.due-topic-pill { background: var(--surface2); border: 1px solid var(--border); border-radius: 20px; padding: 5px 14px; font-size: 12px; cursor: pointer; display: flex; align-items: center; gap: 6px; transition: all 0.2s; }
.due-topic-pill:hover { border-color: var(--primary); color: var(--primary); }
.due-topic-pill.overdue { border-color: rgba(255,71,87,0.3); color: var(--red); }

/* ===================================================
   HEATMAP
=================================================== */
.heatmap-wrap { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 18px 20px; margin-bottom: 18px; overflow-x: auto; }
.heatmap-grid { display: flex; gap: 3px; }
.heatmap-week { display: flex; flex-direction: column; gap: 3px; }
.heatmap-cell { width: 11px; height: 11px; border-radius: 2px; background: var(--surface3); cursor: pointer; transition: transform 0.1s; position: relative; }
.heatmap-cell:hover { transform: scale(1.6); z-index: 10; }
.heatmap-cell[data-level="1"] { background: rgba(0,210,190,0.2); }
.heatmap-cell[data-level="2"] { background: rgba(0,210,190,0.4); }
.heatmap-cell[data-level="3"] { background: rgba(0,210,190,0.65); }
.heatmap-cell[data-level="4"] { background: var(--primary); }
.heatmap-legend { display: flex; align-items: center; gap: 5px; margin-top: 8px; font-size: 11px; color: var(--text-muted); }
.heatmap-legend-cells { display: flex; gap: 3px; }
.heatmap-legend-cell { width: 9px; height: 9px; border-radius: 2px; }

/* ===================================================
   CHARTS
=================================================== */
.charts-row { display: grid; grid-template-columns: 2fr 1fr; gap: 14px; margin-bottom: 14px; }
.charts-row.equal { grid-template-columns: 1fr 1fr; }
@media(max-width:768px) { .charts-row { grid-template-columns: 1fr; } }
.chart-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 18px; }
.chart-card h3 { font-family: var(--font-head); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: var(--text-muted); margin-bottom: 14px; }
.chart-card canvas { max-height: 200px; }

/* ===================================================
   FOLDERS VIEW
=================================================== */
.view-header { display: flex; align-items: center; gap: 14px; margin-bottom: 24px; flex-wrap: wrap; }
.view-title { font-family: var(--font-head); font-size: 24px; font-weight: 800; letter-spacing: -0.5px; }
.view-subtitle { color: var(--text-muted); font-size: 12px; margin-top: 2px; }
.back-btn { background: var(--surface2); border: 1px solid var(--border); color: var(--text-muted); border-radius: var(--r-sm); padding: 7px 12px; cursor: pointer; font-size: 12px; font-family: var(--font-body); transition: all 0.2s; display: flex; align-items: center; gap: 5px; }
.back-btn:hover { color: var(--primary); border-color: var(--primary); background: var(--primary-dim); }
.breadcrumb { display: flex; align-items: center; gap: 6px; font-size: 12px; color: var(--text-muted); margin-bottom: 16px; flex-wrap: wrap; }
.breadcrumb-item { cursor: pointer; transition: color 0.2s; }
.breadcrumb-item:hover { color: var(--primary); }
.breadcrumb-item.active { color: var(--text); }
.breadcrumb-sep { color: var(--text-dim); }
.folders-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 14px; margin-bottom: 20px; }
.folder-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 20px; cursor: pointer; transition: all 0.25s; position: relative; overflow: hidden; }
.folder-card:hover { transform: translateY(-3px); border-color: var(--primary); box-shadow: 0 8px 30px rgba(0,0,0,0.3); }
.folder-card-top { display: flex; align-items: flex-start; justify-content: space-between; margin-bottom: 12px; }
.folder-icon { font-size: 26px; line-height: 1; }
.folder-badge { font-size: 10px; font-family: var(--font-mono); color: var(--text-muted); background: var(--surface2); border: 1px solid var(--border2); border-radius: 4px; padding: 2px 6px; }
.folder-name { font-family: var(--font-head); font-size: 15px; font-weight: 700; margin-bottom: 4px; }
.folder-stats { display: flex; gap: 10px; font-size: 11px; color: var(--text-muted); margin-bottom: 10px; }
.folder-progress-bar { height: 3px; background: var(--surface3); border-radius: 2px; overflow: hidden; }
.folder-progress-fill { height: 100%; border-radius: 2px; transition: width 0.6s ease; }
.folder-progress-text { display: flex; justify-content: space-between; font-size: 10px; color: var(--text-muted); margin-top: 4px; }
.due-badge { margin-top: 8px; font-size: 10px; background: var(--yellow-dim); color: var(--yellow); border-radius: 4px; padding: 2px 8px; display: inline-block; }
.due-badge.none { background: transparent; color: var(--text-dim); }
.folder-actions { display: flex; gap: 4px; opacity: 0; transition: opacity 0.2s; position: absolute; top: 14px; right: 14px; }
.folder-card:hover .folder-actions { opacity: 1; }
.icon-btn { background: var(--surface3); border: none; color: var(--text-muted); cursor: pointer; width: 26px; height: 26px; border-radius: 6px; display: flex; align-items: center; justify-content: center; font-size: 12px; transition: all 0.15s; }
.icon-btn:hover { background: var(--surface2); color: var(--primary); }
.icon-btn.danger:hover { color: var(--red); }
.subfolder-list { margin-bottom: 16px; }
.subfolder-item { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 12px 16px; margin-bottom: 8px; cursor: pointer; display: flex; align-items: center; gap: 12px; transition: all 0.2s; }
.subfolder-item:hover { border-color: var(--primary); background: var(--primary-dim); }
.subfolder-arrow { color: var(--text-dim); font-size: 12px; margin-left: auto; }

/* ===================================================
   TOPICS
=================================================== */
.topic-filters { display: flex; gap: 8px; margin-bottom: 14px; flex-wrap: wrap; align-items: center; }
.topic-search-input { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 7px 12px; color: var(--text); font-family: var(--font-body); font-size: 12px; outline: none; flex: 1; min-width: 200px; transition: border-color 0.2s; }
.topic-search-input:focus { border-color: var(--primary); }
.filter-btn { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 6px 12px; color: var(--text-muted); font-size: 11px; cursor: pointer; transition: all 0.2s; white-space: nowrap; }
.filter-btn.active { background: var(--primary-dim); border-color: var(--primary); color: var(--primary); }
.filter-btn:hover { color: var(--primary); border-color: var(--primary); }
.topic-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); margin-bottom: 8px; overflow: hidden; transition: border-color 0.2s; }
.topic-card.mastered { border-color: rgba(6,255,165,0.15); }
.topic-card.weak { border-left: 3px solid var(--red); }
.topic-card-header { padding: 14px 18px; cursor: pointer; display: flex; align-items: center; gap: 12px; transition: background 0.2s; user-select: none; }
.topic-card-header:hover { background: var(--surface2); }
.topic-completion-ring { width: 34px; height: 34px; position: relative; flex-shrink: 0; }
.topic-completion-ring svg { width: 34px; height: 34px; transform: rotate(-90deg); }
.ring-bg { fill: none; stroke: var(--surface3); stroke-width: 3; }
.ring-fill { fill: none; stroke: var(--primary); stroke-width: 3; stroke-linecap: round; transition: stroke-dashoffset 0.5s ease; }
.topic-ring-pct { position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; font-size: 8px; font-family: var(--font-mono); color: var(--primary); }
.topic-info { flex: 1; min-width: 0; }
.topic-name { font-family: var(--font-head); font-size: 13px; font-weight: 700; color: var(--text); margin-bottom: 3px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.topic-meta { display: flex; gap: 10px; font-size: 10px; color: var(--text-muted); flex-wrap: wrap; }
.topic-right { display: flex; align-items: center; gap: 8px; flex-shrink: 0; }
.star-btn { font-size: 14px; cursor: pointer; opacity: 0.4; transition: all 0.2s; margin-right: 4px; }
.star-btn:hover { opacity: 1; transform: scale(1.2); }
.star-btn.starred { opacity: 1; color: var(--yellow); text-shadow: 0 0 6px rgba(255,179,64,0.5); }
.diff-badge { font-size: 10px; padding: 1px 7px; border-radius: 4px; }
.diff-easy { background: var(--green-dim); color: var(--green); }
.diff-medium { background: var(--yellow-dim); color: var(--yellow); }
.diff-hard { background: var(--red-dim); color: var(--red); }
.next-rev-badge { font-size: 10px; font-family: var(--font-mono); background: var(--yellow-dim); color: var(--yellow); border-radius: 4px; padding: 2px 6px; white-space: nowrap; }
.next-rev-badge.overdue { background: var(--red-dim); color: var(--red); }
.next-rev-badge.today { background: rgba(255,71,87,0.25); color: var(--red); animation: blink-badge 1.5s infinite; }
.next-rev-badge.future { background: var(--blue-dim); color: var(--blue); }
@keyframes blink-badge { 0%,100%{opacity:1} 50%{opacity:0.5} }
.topic-expand-icon { color: var(--text-muted); font-size: 11px; transition: transform 0.3s; }
.topic-expand-icon.open { transform: rotate(90deg); }
.wt-badge { font-size: 10px; font-family: var(--font-mono); background: var(--purple-dim); color: var(--purple); border-radius: 4px; padding: 2px 6px; }
.topic-body { display: none; padding: 0 18px 18px; border-top: 1px solid var(--border2); }
.topic-body.open { display: block; }
.topic-section-title { font-family: var(--font-head); font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.2px; color: var(--text-muted); margin: 16px 0 8px; }

/* ===== STAGES ===== */
.stages-grid { display: flex; flex-wrap: wrap; gap: 7px; }
.stage-item { display: flex; align-items: center; gap: 7px; background: var(--surface2); border: 1px solid var(--border2); border-radius: var(--r-sm); padding: 7px 12px; cursor: pointer; transition: all 0.2s; min-width: 150px; user-select: none; }
.stage-item.done { background: var(--primary-dim); border-color: var(--border); }
.stage-item:hover { border-color: var(--primary); }
.stage-checkbox { width: 15px; height: 15px; border: 2px solid var(--text-dim); border-radius: 3px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; transition: all 0.2s; font-size: 9px; }
.stage-item.done .stage-checkbox { background: var(--primary); border-color: var(--primary); color: var(--bg); }
.stage-label { font-size: 12px; color: var(--text); }
.stage-item.done .stage-label { color: var(--primary); }
.stage-item.stage-mastered.done { background: var(--green-dim); border-color: rgba(6,255,165,0.35); }
.stage-item.stage-mastered.done .stage-checkbox { background: var(--green); border-color: var(--green); }
.stage-item.stage-mastered.done .stage-label { color: var(--green); }

/* ===== SM-2 REVIEW ===== */
.sm2-panel { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 12px 14px; margin-top: 8px; }
.sm2-info { display: flex; gap: 16px; font-size: 11px; color: var(--text-muted); margin-bottom: 10px; flex-wrap: wrap; }
.sm2-info span b { color: var(--text); }
.sm2-buttons { display: flex; gap: 6px; flex-wrap: wrap; }
.sm2-btn { flex: 1; border: none; cursor: pointer; border-radius: var(--r-sm); padding: 10px; font-size: 13px; font-family: var(--font-body); font-weight: 600; transition: all 0.2s; background: var(--blue-dim); color: var(--blue); border: 1px solid rgba(58,134,255,0.3); }
.sm2-btn:hover { filter: brightness(1.2); transform: translateY(-1px); }
.notes-textarea { width: 100%; background: var(--surface2); border: 1px solid var(--border2); border-radius: var(--r-sm); padding: 10px 12px; color: var(--text); font-family: var(--font-body); font-size: 12px; outline: none; resize: vertical; min-height: 80px; transition: border-color 0.2s; }
.notes-textarea:focus { border-color: var(--primary); }
.resources-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
.resource-input-group { display: flex; flex-direction: column; gap: 5px; }
.resource-label { font-size: 10px; color: var(--text-muted); font-weight: 600; text-transform: uppercase; letter-spacing: 0.8px; }
.resource-input { background: var(--surface2); border: 1px solid var(--border2); border-radius: var(--r-sm); padding: 6px 10px; color: var(--text); font-family: var(--font-body); font-size: 12px; outline: none; width: 100%; transition: border-color 0.2s; }
.resource-input:focus { border-color: var(--primary); }
.resource-links-list { display: flex; flex-direction: column; gap: 3px; margin-top: 3px; }
.resource-link-item { display: flex; align-items: center; gap: 7px; background: var(--surface3); border-radius: 5px; padding: 4px 9px; font-size: 11px; overflow: hidden; }
.resource-link-item a { color: var(--blue); text-decoration: none; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; flex: 1; }
.resource-link-item a:hover { color: var(--primary); }
.del-link-btn { background: none; border: none; color: var(--text-dim); cursor: pointer; font-size: 13px; padding: 0 2px; transition: color 0.2s; }
.del-link-btn:hover { color: var(--red); }
.add-resource-row { display: flex; gap: 5px; margin-top: 3px; }
.add-res-btn { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); color: var(--primary); cursor: pointer; padding: 6px 10px; font-size: 11px; transition: all 0.2s; }
.add-res-btn:hover { background: var(--primary-dim); }
.prompts-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(190px, 1fr)); gap: 7px; }
.prompt-btn { background: var(--surface2); border: 1px solid var(--border2); border-radius: var(--r-sm); padding: 10px 12px; color: var(--text); cursor: pointer; text-align: left; transition: all 0.2s; font-family: var(--font-body); font-size: 11px; }
.prompt-btn:hover { border-color: var(--primary); background: var(--primary-dim); color: var(--primary); }
.prompt-btn-title { font-weight: 600; margin-bottom: 2px; font-size: 11px; }
.prompt-btn-sub { color: var(--text-muted); font-size: 10px; }
.prompt-btn:hover .prompt-btn-sub { color: rgba(0,210,190,0.7); }
.copy-indicator { display: inline-block; font-size: 9px; background: var(--green-dim); color: var(--green); border-radius: 3px; padding: 1px 5px; margin-left: 4px; opacity: 0; transition: opacity 0.3s; }
.copy-indicator.show { opacity: 1; }
.prompt-btn.gen-html { grid-column: 1 / -1; background: linear-gradient(135deg, rgba(0,210,190,0.08), rgba(58,134,255,0.08)); border: 1px solid rgba(0,210,190,0.3); padding: 14px 16px; display: flex; align-items: center; gap: 12px; text-align: left; overflow: hidden; }
.prompt-btn.gen-html:hover { border-color: var(--primary); }

/* ===================================================
   REVISION SESSION VIEW
=================================================== */
.revision-queue { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 20px; margin-bottom: 16px; }
.revision-session-card { background: linear-gradient(135deg, rgba(0,210,190,0.06), rgba(58,134,255,0.04)); border: 1px solid rgba(0,210,190,0.15); border-radius: var(--r); padding: 28px; text-align: center; margin-bottom: 20px; }
.rev-topic-name { font-family: var(--font-head); font-size: 22px; font-weight: 800; margin-bottom: 8px; cursor: pointer; transition: color 0.2s; }
.rev-topic-name:hover { color: var(--primary); text-decoration: underline; }
.rev-topic-folder { font-size: 12px; color: var(--text-muted); margin-bottom: 20px; }
.rev-quality-btns { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; }
.rev-q-btn { flex: 1; min-width: 100px; max-width: 250px; border: none; cursor: pointer; border-radius: var(--r-sm); padding: 14px 20px; font-size: 14px; font-weight: 700; font-family: var(--font-head); transition: all 0.2s; background: var(--blue-dim); color: var(--blue); border: 2px solid rgba(58,134,255,0.4); }
.rev-q-btn:hover { transform: translateY(-2px); filter: brightness(1.2); }
.rev-q-sub { font-size: 11px; opacity: 0.7; font-weight: 400; margin-top: 4px; font-family: var(--font-body); }
.rev-queue-list { display: flex; flex-direction: column; gap: 6px; }
.rev-queue-item { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 10px 14px; display: flex; align-items: center; gap: 12px; font-size: 12px; cursor: pointer; transition: border-color 0.2s; }
.rev-queue-item:hover { border-color: var(--primary); }
.rev-queue-item .q-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
.rev-queue-item.current { border-color: var(--primary); }
.rev-queue-item.done { opacity: 0.5; }
.rev-empty { text-align: center; padding: 40px 20px; color: var(--text-muted); }
.rev-empty .rev-empty-icon { font-size: 48px; margin-bottom: 12px; }
.rev-done-banner { text-align: center; background: var(--green-dim); border: 1px solid rgba(6,255,165,0.3); border-radius: var(--r); padding: 24px; }
.rev-done-banner h2 { font-family: var(--font-head); font-size: 22px; color: var(--green); }

/* ===================================================
   ANALYTICS VIEW
=================================================== */
.analytics-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-bottom: 16px; }
@media(max-width:768px) { .analytics-grid { grid-template-columns: 1fr; } }
.ws-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 18px; }
.ws-title { font-family: var(--font-head); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 12px; }
.ws-title.weak { color: var(--red); }
.ws-title.strong { color: var(--green); }
.ws-item { display: flex; align-items: center; gap: 10px; padding: 7px 0; border-bottom: 1px solid var(--border2); font-size: 12px; }
.ws-item:last-child { border-bottom: none; }

/* ===================================================
   PLANNER & TIMER
=================================================== */
.quick-planner { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 18px; margin-bottom: 18px; }
.planner-title { font-family: var(--font-head); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: var(--text-muted); margin-bottom: 12px; }
.planner-item { display: flex; align-items: center; gap: 10px; padding: 8px 0; border-bottom: 1px solid var(--border2); font-size: 12px; }
.planner-item:last-child { border-bottom: none; }
.planner-cb { width: 16px; height: 16px; flex-shrink: 0; cursor: pointer; accent-color: var(--primary); }
.planner-text { flex: 1; }
.planner-text.done { text-decoration: line-through; color: var(--text-muted); }
.planner-del { background: none; border: none; color: var(--text-dim); cursor: pointer; font-size: 14px; transition: color 0.2s; }
.planner-del:hover { color: var(--red); }
.planner-input-row { display: flex; gap: 8px; margin-top: 10px; }
.planner-input { flex: 1; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 7px 12px; color: var(--text); font-family: var(--font-body); font-size: 12px; outline: none; transition: border-color 0.2s; }
.planner-input::placeholder { color: var(--text-muted); }
.planner-input:focus { border-color: var(--primary); }
.timer-panel { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 20px; margin-bottom: 18px; text-align: center; }
.timer-label { font-size: 10px; text-transform: uppercase; letter-spacing: 1.5px; color: var(--text-muted); margin-bottom: 8px; }
.timer-big { font-family: var(--font-mono); font-size: 40px; font-weight: 500; color: var(--green); letter-spacing: 2px; margin-bottom: 14px; }
.timer-controls { display: flex; gap: 8px; justify-content: center; }
.timer-btn { background: var(--surface2); border: 1px solid var(--border); color: var(--text); border-radius: var(--r-sm); padding: 7px 16px; font-size: 12px; cursor: pointer; transition: all 0.2s; font-family: var(--font-body); }
.timer-btn:hover { border-color: var(--primary); color: var(--primary); }
.timer-stats { margin-top: 14px; display: flex; gap: 12px; justify-content: center; flex-wrap: wrap; }
.timer-stat-row { text-align: center; }
.timer-stat-name { font-size: 10px; color: var(--text-muted); }
.timer-stat-val { font-family: var(--font-mono); font-size: 14px; color: var(--text); }

/* ===================================================
   MODALS
=================================================== */
.modal-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.75); backdrop-filter: blur(4px); z-index: 2000; align-items: center; justify-content: center; padding: 20px; }
.modal-overlay.show { display: flex; }
.modal { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 28px; max-width: 540px; width: 100%; position: relative; max-height: 90vh; overflow-y: auto; animation: modalIn 0.2s ease; }
.modal-lg { max-width: 720px; }
.modal-xl { max-width: 920px; }
@keyframes modalIn { from{opacity:0;transform:scale(0.95)} to{opacity:1;transform:none} }
.modal-close { position: absolute; top: 14px; right: 14px; background: var(--surface2); border: 1px solid var(--border); color: var(--text-muted); border-radius: 6px; cursor: pointer; width: 28px; height: 28px; display: flex; align-items: center; justify-content: center; font-size: 13px; transition: all 0.2s; }
.modal-close:hover { color: var(--red); border-color: var(--red); }
.modal-title { font-family: var(--font-head); font-size: 19px; font-weight: 800; margin-bottom: 4px; }
.modal-subtitle { color: var(--text-muted); font-size: 12px; margin-bottom: 20px; }
.form-group { margin-bottom: 14px; }
.form-label { font-size: 11px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.8px; margin-bottom: 5px; display: block; }
.form-input { width: 100%; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 9px 12px; color: var(--text); font-family: var(--font-body); font-size: 13px; outline: none; transition: border-color 0.2s; }
.form-input:focus { border-color: var(--primary); }
.form-select { width: 100%; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 9px 12px; color: var(--text); font-family: var(--font-body); font-size: 13px; outline: none; cursor: pointer; }
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
.modal-footer { display: flex; gap: 8px; justify-content: flex-end; margin-top: 20px; }
.import-log { background: var(--bg2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 12px; font-family: var(--font-mono); font-size: 11px; max-height: 150px; overflow-y: auto; margin-top: 12px; }
.log-success { color: var(--green); }
.log-error { color: var(--red); }
.log-warn { color: var(--yellow); }
.log-info { color: var(--blue); }

/* Upload zone */
.upload-zone { border: 2px dashed var(--border); border-radius: var(--r); padding: 28px; text-align: center; cursor: pointer; transition: all 0.2s; background: var(--surface2); }
.upload-zone:hover, .upload-zone.drag { border-color: var(--primary); background: var(--primary-dim); }
.upload-icon { font-size: 40px; margin-bottom: 10px; }
.upload-text { font-size: 14px; color: var(--text); margin-bottom: 6px; }
.upload-hint { font-size: 12px; color: var(--text-muted); }
.json-schema-hint { background: var(--bg2); border: 1px solid var(--border2); border-radius: var(--r-sm); padding: 12px; font-family: var(--font-mono); font-size: 11px; color: var(--text-muted); margin-top: 12px; overflow-x: auto; }
.merge-options { display: flex; gap: 8px; margin-top: 12px; flex-wrap: wrap; }
.merge-option { flex: 1; min-width: 120px; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 10px; cursor: pointer; text-align: center; font-size: 12px; transition: all 0.2s; }
.merge-option.selected { background: var(--primary-dim); border-color: var(--primary); color: var(--primary); }
.merge-option:hover { border-color: var(--primary); }

/* Pomodoro */
.pomo-display { text-align: center; padding: 10px 0; }
.pomo-ring { position: relative; width: 160px; height: 160px; margin: 0 auto 14px; }
.pomo-ring svg { transform: rotate(-90deg); }
.pomo-ring .ring-fill.break { stroke: var(--green); }
.pomo-ring .ring-fill { stroke: var(--primary); }
.pomo-inner { position: absolute; inset: 0; display: flex; flex-direction: column; align-items: center; justify-content: center; }
.pomo-time { font-family: var(--font-mono); font-size: 32px; font-weight: 500; color: var(--text); }
.pomo-mode { font-size: 11px; text-transform: uppercase; letter-spacing: 1.5px; color: var(--text-muted); margin-top: 4px; }
.pomo-sessions { font-size: 12px; color: var(--text-muted); margin-bottom: 16px; }
.pomo-controls { display: flex; gap: 8px; justify-content: center; }
.pomo-btn { background: var(--surface2); border: 1px solid var(--border); color: var(--text); border-radius: var(--r-sm); padding: 8px 18px; font-size: 13px; cursor: pointer; transition: all 0.2s; font-family: var(--font-body); }
.pomo-btn:hover { border-color: var(--primary); color: var(--primary); }
.pomo-btn.secondary { color: var(--text-muted); }

/* ===================================================
   TOAST
=================================================== */
#toast-container { position: fixed; bottom: 20px; right: 20px; z-index: 9999; display: flex; flex-direction: column; gap: 8px; }
.toast { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r-sm); padding: 10px 16px; font-size: 13px; display: flex; align-items: center; gap: 8px; animation: toastIn 0.25s ease, toastOut 0.25s ease 2.7s forwards; max-width: 340px; box-shadow: var(--shadow); }
.toast.success { border-color: rgba(6,255,165,0.3); }
.toast.error { border-color: rgba(255,71,87,0.3); }
.toast.warn { border-color: rgba(255,179,64,0.3); }
.toast.info { border-color: rgba(58,134,255,0.3); }
@keyframes toastIn { from{opacity:0;transform:translateX(20px)} to{opacity:1;transform:none} }
@keyframes toastOut { from{opacity:1} to{opacity:0;transform:translateX(20px)} }

/* ===================================================
   EMPTY STATES
=================================================== */
.empty-state { text-align: center; padding: 40px 20px; }
.empty-icon { font-size: 48px; margin-bottom: 12px; opacity: 0.5; }
.empty-text { color: var(--text-muted); font-size: 14px; margin-bottom: 16px; }

/* ===================================================
   RESPONSIVE & MOBILE
=================================================== */
@media(max-width:768px) {
  #topnav { height: auto; flex-wrap: wrap; padding: 8px 12px; }
  .nav-tabs { display: none; }
  .nav-brand { flex: 1; }
  .nav-right { flex: none; justify-content: flex-end; }
  .nav-search-wrap { order: 3; flex: 1 1 100%; margin-top: 10px; max-width: none; }
  .hide-mob { display: none; }
  .view { padding-top: 120px; }
  .folders-grid { grid-template-columns: 1fr 1fr; }
  .resources-grid { grid-template-columns: 1fr; }
  .form-row { grid-template-columns: 1fr; }
  .timer-big { font-size: 32px; }
  .btn, .nav-btn, .filter-btn, .back-btn, .icon-btn { min-height: 44px; min-width: 44px; justify-content: center; }
  .topic-card-header { min-height: 48px; }
  .auth-chip-name { display: none; }
}
@media(max-width:480px) {
  .folders-grid { grid-template-columns: 1fr; }
  .stats-grid { grid-template-columns: 1fr 1fr; }
  .dual-input-layout { flex-direction: column !important; }
}

.enter-study-btn { display: block; width: 100%; max-width: 420px; margin: 24px auto 0; background: linear-gradient(135deg, var(--primary), var(--blue)); color: var(--bg); border: none; border-radius: var(--r); padding: 16px 28px; font-family: var(--font-head); font-size: 15px; font-weight: 800; letter-spacing: 0.5px; cursor: pointer; transition: all 0.3s; box-shadow: 0 4px 28px rgba(0,210,190,0.28); }
.enter-study-btn:hover { transform: translateY(-3px); box-shadow: 0 8px 36px rgba(0,210,190,0.4); }

/* ===================================================
   LIGHT / DARK THEME TOGGLE
=================================================== */
body.light {
  --bg: #EEF2F7; --bg2: #E4ECF5; --surface: #FFFFFF; --surface2: #F4F7FB; --surface3: #DDE6F0;
  --border: rgba(0,160,150,0.18); --border2: rgba(0,0,0,0.07);
  --primary-dim: rgba(0,180,165,0.10); --blue-dim: rgba(40,110,230,0.10); --green-dim: rgba(0,180,100,0.10);
  --yellow-dim: rgba(200,130,30,0.10); --red-dim: rgba(210,50,60,0.10); --purple-dim: rgba(140,70,210,0.10);
  --text: #1A2E44; --text-muted: #527090; --text-dim: #96B3C8; --shadow: 0 4px 24px rgba(0,0,0,0.10);
}
body.light::before { background: radial-gradient(ellipse 80% 60% at 10% 0%, rgba(0,210,190,0.05) 0%, transparent 60%), radial-gradient(ellipse 60% 80% at 90% 100%, rgba(58,134,255,0.04) 0%, transparent 60%); }
body.light #topnav { background: rgba(238,242,247,0.96); }
body.light ::-webkit-scrollbar-track { background: var(--bg2); }
body.light ::-webkit-scrollbar-thumb { background: var(--surface3); }
#theme-toggle-btn { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm); color: var(--text); cursor: pointer; padding: 5px 9px; font-size: 14px; font-family: var(--font-body); transition: all 0.2s; display: flex; align-items: center; gap: 4px; white-space: nowrap; line-height:1; }
#theme-toggle-btn:hover { background: var(--surface3); border-color: var(--primary); }

.sync-badge { font-family: var(--font-mono); font-size: 11px; border-radius: var(--r-sm); padding: 4px 8px; display: flex; align-items: center; gap: 4px; white-space: nowrap; transition: all 0.2s; border: 1px solid transparent; }
.sync-badge.synced { color: var(--green); background: var(--green-dim); border-color: rgba(6,255,165,0.2); }
.sync-badge.syncing { color: var(--yellow); background: var(--yellow-dim); border-color: rgba(255,179,64,0.2); animation: pulse-timer 1.5s infinite; }
.sync-badge.error { color: var(--red); background: var(--red-dim); border-color: rgba(255,71,87,0.2); }

.settings-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.75); backdrop-filter: blur(4px); z-index: 2000; align-items: flex-start; justify-content: center; padding: 60px 20px 20px; overflow-y: auto; }
.settings-overlay.show { display: flex; }
.settings-modal { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 28px; max-width: 480px; width: 100%; position: relative; animation: modalIn 0.2s ease; }
.settings-section-title { font-family: var(--font-head); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px; color: var(--text-muted); margin: 20px 0 12px; padding-top: 16px; border-top: 1px solid var(--border2); }
.settings-section-title:first-of-type { margin-top: 0; padding-top: 0; border-top: none; }

/* Weekend */
.weekend-panel { background: linear-gradient(135deg, rgba(199,125,255,0.07), rgba(58,134,255,0.05)); border: 1px solid rgba(199,125,255,0.22); border-radius: var(--r); padding: 20px 22px; margin-bottom: 18px; position: relative; overflow: hidden; }
.weekend-panel::before { content: ''; position: absolute; top: -30px; right: -30px; width: 160px; height: 160px; border-radius: 50%; background: radial-gradient(circle, rgba(199,125,255,0.09), transparent 70%); pointer-events: none; }
.weekend-panel-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; flex-wrap: wrap; gap: 10px; }
.weekend-panel-title { font-family: var(--font-head); font-size: 15px; font-weight: 800; color: var(--purple); display: flex; align-items: center; gap: 8px; }
.weekend-panel-sub { font-size: 12px; color: var(--text-muted); margin-top: 2px; }
.weekend-topic-grid { display: flex; flex-wrap: wrap; gap: 7px; margin-bottom: 14px; }
.weekend-topic-chip { background: var(--surface2); border: 1px solid rgba(199,125,255,0.2); border-radius: 20px; padding: 5px 13px; font-size: 12px; display: flex; align-items: center; gap: 6px; cursor: pointer; transition: all 0.2s; color: var(--text); }
.weekend-topic-chip:hover { border-color: var(--purple); color: var(--purple); }
.weekend-topic-chip.reviewed { border-color: rgba(6,255,165,0.3); color: var(--green); background: var(--green-dim); }
.weekend-stats-row { display: flex; gap: 16px; margin-bottom: 14px; flex-wrap: wrap; }
.weekend-stat { font-size: 11px; color: var(--text-muted); display: flex; align-items: center; gap: 5px; }
.weekend-stat span { font-family: var(--font-mono); font-weight: 700; color: var(--purple); }
.wknd-session-card { background: var(--surface); border: 1px solid rgba(199,125,255,0.2); border-radius: var(--r); padding: 28px; text-align: center; margin-bottom: 14px; }
.wknd-session-topic { font-family: var(--font-head); font-size: 20px; font-weight: 800; margin-bottom: 6px; }
.wknd-session-folder { font-size: 12px; color: var(--text-muted); margin-bottom: 16px; }
.wknd-session-notes { background: var(--surface2); border-radius: var(--r-sm); padding: 12px; font-size: 12px; color: var(--text-muted); text-align: left; margin-bottom: 18px; max-height: 90px; overflow-y: auto; line-height: 1.5; }
.wknd-progress-bar { background: var(--surface3); border-radius: 4px; height: 5px; overflow: hidden; margin-bottom: 8px; }
.wknd-progress-fill { height: 100%; border-radius: 4px; background: linear-gradient(90deg, var(--purple), var(--blue)); transition: width 0.4s ease; }
.wknd-counter { font-size: 11px; color: var(--text-muted); margin-bottom: 20px; text-align: center; }
.wknd-btns { display: flex; gap: 8px; justify-content: center; flex-wrap: wrap; }
.wknd-btn { border-radius: var(--r-sm); padding: 10px 18px; font-size: 13px; font-weight: 600; border: none; cursor: pointer; font-family: var(--font-body); transition: all 0.2s; display: flex; flex-direction: column; align-items: center; gap: 2px; min-width: 80px; }
.wknd-btn small { font-size: 10px; font-weight: 400; opacity: 0.8; }
.wknd-btn.skip { background: var(--surface3); color: var(--text-muted); }
.wknd-btn.skip:hover { background: var(--surface2); color: var(--text); }
.wknd-btn.good { background: var(--blue-dim); color: var(--blue); border: 1px solid rgba(58,134,255,0.3); }
.wknd-done-banner { background: var(--green-dim); border: 1px solid rgba(6,255,165,0.3); border-radius: var(--r); padding: 28px; text-align: center; }
.wknd-done-banner h2 { font-family: var(--font-head); font-size: 22px; color: var(--green); margin-bottom: 8px; }

/* Auth */
.auth-chip { display: flex; align-items: center; gap: 6px; background: var(--surface2); border: 1px solid var(--border); border-radius: 20px; padding: 3px 10px 3px 4px; cursor: pointer; transition: all 0.2s; flex-shrink: 0; }
.auth-chip:hover { border-color: var(--primary); }
.auth-chip img { width: 22px; height: 22px; border-radius: 50%; object-fit: cover; }
.auth-chip-name { font-size: 11px; color: var(--text); max-width: 80px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.auth-chip-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--green); flex-shrink: 0; }
.sign-in-btn { background: linear-gradient(135deg, var(--blue-dim), var(--primary-dim)); border: 1px solid rgba(58,134,255,0.3); border-radius: var(--r-sm); color: var(--blue); cursor: pointer; padding: 5px 10px; font-size: 11px; font-family: var(--font-body); transition: all 0.2s; display: flex; align-items: center; gap: 5px; white-space: nowrap; flex-shrink: 0; }
.sign-in-btn:hover { border-color: var(--blue); background: var(--blue-dim); }
.auth-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.8); backdrop-filter: blur(4px); z-index: 3000; align-items: center; justify-content: center; }
.auth-overlay.show { display: flex; }
.auth-modal { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 32px; max-width: 380px; width: 90%; text-align: center; animation: modalIn 0.2s ease; }
.auth-modal-title { font-family: var(--font-head); font-size: 22px; font-weight: 800; margin-bottom: 6px; }
.auth-modal-sub { font-size: 13px; color: var(--text-muted); margin-bottom: 24px; line-height: 1.5; }
.google-btn { width: 100%; display: flex; align-items: center; justify-content: center; gap: 10px; background: white; color: #333; border: none; border-radius: var(--r-sm); padding: 12px 20px; font-size: 14px; font-weight: 600; cursor: pointer; transition: all 0.2s; font-family: var(--font-body); margin-bottom: 10px; }
.google-btn:hover { background: #f0f0f0; box-shadow: 0 4px 20px rgba(255,255,255,0.15); }
.google-logo { width: 18px; height: 18px; }
.auth-skip-btn { background: none; border: 1px solid var(--border); color: var(--text-muted); border-radius: var(--r-sm); padding: 9px 20px; font-size: 12px; cursor: pointer; transition: all 0.2s; font-family: var(--font-body); width: 100%; }
.auth-skip-btn:hover { border-color: var(--primary); color: var(--primary); }
.auth-note { font-size: 11px; color: var(--text-dim); margin-top: 14px; }

/* ===================================================
   ★ NEW: MEDICAL DIARY SCHEDULE SECTION ★
=================================================== */
.schedule-section {
  background: var(--surface);
  /* Ruled paper effect */
  background-image:
    repeating-linear-gradient(
      transparent,
      transparent 31px,
      rgba(0,210,190,0.035) 31px,
      rgba(0,210,190,0.035) 32px
    );
  border: 1px solid rgba(0,210,190,0.18);
  border-radius: var(--r);
  padding: 20px;
  margin-bottom: 18px;
  position: relative;
}
.schedule-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 16px; flex-wrap: wrap; gap: 8px; }
.schedule-title-wrap { display: flex; flex-direction: column; }
.schedule-title { font-family: var(--font-head); font-size: 15px; font-weight: 800; color: var(--primary); display: flex; align-items: center; gap: 8px; }
.schedule-subtitle { font-size: 11px; color: var(--text-muted); margin-top: 2px; }
.schedule-day-label { font-family: var(--font-mono); font-size: 10px; background: var(--blue-dim); border: 1px solid rgba(58,134,255,0.3); color: var(--blue); border-radius: 4px; padding: 2px 8px; }
.schedule-actions { display: flex; gap: 6px; align-items: center; flex-wrap: wrap; }
.schedule-progress { display: flex; align-items: center; gap: 10px; margin-bottom: 18px; }
.schedule-progress-bar { flex: 1; height: 5px; background: var(--surface3); border-radius: 4px; overflow: hidden; }
.schedule-progress-fill { height: 100%; border-radius: 4px; background: linear-gradient(90deg, var(--primary), var(--blue)); transition: width 0.4s ease; }
.schedule-progress-text { font-size: 11px; color: var(--text-muted); font-family: var(--font-mono); white-space: nowrap; }

/* Diary timeline ribbon */
.diary-timeline {
  position: relative;
  padding-left: 20px;
  display: flex;
  flex-direction: column;
  gap: 0;
}
.diary-timeline::before {
  content: '';
  position: absolute;
  left: 7px; top: 12px; bottom: 12px;
  width: 3px;
  background: linear-gradient(180deg, var(--primary) 0%, rgba(0,210,190,0.2) 100%);
  border-radius: 3px;
}

/* Diary slot entries */
.diary-slot {
  display: flex;
  gap: 14px;
  padding: 12px 14px 12px 10px;
  background: rgba(12,24,39,0.6);
  border: 1px solid var(--border2);
  border-radius: var(--r-sm);
  transition: all 0.2s;
  position: relative;
  margin-bottom: 8px;
  backdrop-filter: blur(4px);
}
.diary-slot::before {
  content: '';
  position: absolute;
  left: -17px;
  top: 50%;
  transform: translateY(-50%);
  width: 9px; height: 9px;
  border-radius: 50%;
  background: var(--surface3);
  border: 2px solid var(--surface3);
  transition: all 0.2s;
  z-index: 2;
}
.diary-slot:hover { border-color: rgba(0,210,190,0.2); background: rgba(16,30,48,0.8); }
.diary-slot:hover::before { background: var(--primary); border-color: var(--primary); }
.diary-slot.completed { opacity: 0.52; }
.diary-slot.completed .slot-topic { text-decoration: line-through; }
.diary-slot.completed::before { background: var(--green); border-color: var(--green); }

/* Active time block — glow effect */
.diary-slot.active-now {
  border-color: var(--primary) !important;
  background: rgba(0,210,190,0.06) !important;
  box-shadow: 0 0 0 1px rgba(0,210,190,0.25), 0 0 24px rgba(0,210,190,0.12);
  animation: diary-slot-glow 2.5s ease-in-out infinite;
}
.diary-slot.active-now::before { background: var(--primary); border-color: var(--primary); box-shadow: 0 0 0 4px rgba(0,210,190,0.2); animation: node-pulse 2s infinite; }
@keyframes diary-slot-glow {
  0%,100% { box-shadow: 0 0 0 1px rgba(0,210,190,0.25), 0 0 20px rgba(0,210,190,0.1); }
  50% { box-shadow: 0 0 0 2px rgba(0,210,190,0.4), 0 0 36px rgba(0,210,190,0.18); }
}
@keyframes node-pulse {
  0%,100% { box-shadow: 0 0 0 3px rgba(0,210,190,0.2); }
  50% { box-shadow: 0 0 0 6px rgba(0,210,190,0.08); }
}

.active-now-pill {
  position: absolute;
  top: -10px; left: 14px;
  background: var(--primary); color: var(--bg);
  font-size: 9px; font-weight: 800; font-family: var(--font-mono);
  border-radius: 4px; padding: 2px 7px; text-transform: uppercase;
  letter-spacing: 0.8px; animation: pulse-timer 1.5s infinite;
}

.diary-slot-time-col {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  min-width: 100px;
  flex-shrink: 0;
  padding-top: 2px;
}
.slot-time { font-family: var(--font-mono); font-size: 11px; color: var(--primary); white-space: nowrap; }
.diary-slot-content { flex: 1; min-width: 0; }
.diary-slot-header { display: flex; align-items: flex-start; gap: 10px; }
.slot-done-cb { width: 17px; height: 17px; flex-shrink: 0; cursor: pointer; accent-color: var(--primary); margin-top: 2px; }
.slot-emoji { font-size: 18px; flex-shrink: 0; }
.slot-body { flex: 1; min-width: 0; }
.slot-topic { font-size: 13px; font-weight: 600; color: var(--text); font-family: var(--font-head); }
.slot-subject { font-size: 11px; color: var(--text-muted); margin-top: 1px; }
.slot-tags { display: flex; flex-wrap: wrap; gap: 4px; margin-top: 5px; }
.slot-tag { background: var(--surface3); border-radius: 3px; padding: 1px 6px; font-size: 10px; color: var(--text-muted); font-family: var(--font-mono); }
.slot-diff { flex-shrink: 0; font-size: 10px; font-family: var(--font-mono); border-radius: 3px; padding: 2px 6px; text-transform: uppercase; }
.slot-diff.easy, .slot-diff.light { background: var(--green-dim); color: var(--green); }
.slot-diff.medium, .slot-diff.moderate { background: var(--yellow-dim); color: var(--yellow); }
.slot-diff.hard, .slot-diff.deep { background: var(--red-dim); color: var(--red); }

/* Micro-steps within diary slot */
.micro-steps-list {
  margin-top: 10px;
  padding-top: 10px;
  border-top: 1px solid var(--border2);
  display: flex;
  flex-direction: column;
  gap: 5px;
}
.micro-step-item {
  display: flex;
  align-items: flex-start;
  gap: 8px;
  cursor: pointer;
  padding: 3px 0;
}
.micro-cb {
  width: 14px; height: 14px;
  flex-shrink: 0;
  cursor: pointer;
  accent-color: var(--green);
  margin-top: 2px;
}
.micro-step-text {
  font-size: 12px;
  color: var(--text-muted);
  line-height: 1.4;
  transition: all 0.2s;
}
.micro-step-text.done { text-decoration: line-through; color: var(--text-dim); }

/* ===================================================
   ★ NEW: DUAL-INPUT SCHEDULE MODAL ★
=================================================== */
.dual-input-layout {
  display: flex;
  gap: 16px;
  margin-bottom: 14px;
}
.dual-input-left, .dual-input-right {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.dual-input-label {
  font-family: var(--font-head);
  font-size: 10px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1.2px;
  color: var(--text-muted);
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 6px;
}
.dual-input-label span.di-badge {
  background: var(--primary-dim);
  color: var(--primary);
  border-radius: 4px;
  padding: 1px 6px;
  font-size: 9px;
}
.dual-input-divider {
  width: 1px;
  background: var(--border);
  align-self: stretch;
  flex-shrink: 0;
  position: relative;
}
.dual-input-divider::after {
  content: 'OR';
  position: absolute; top: 50%; left: 50%;
  transform: translate(-50%,-50%);
  background: var(--surface);
  color: var(--text-muted);
  font-size: 10px; font-family: var(--font-mono);
  padding: 4px 6px;
  border: 1px solid var(--border);
  border-radius: 4px;
}
.sched-paste-area {
  flex: 1;
  width: 100%;
  background: var(--bg2);
  border: 1px solid var(--border);
  border-radius: var(--r-sm);
  padding: 10px 12px;
  color: var(--text);
  font-family: var(--font-mono);
  font-size: 11px;
  line-height: 1.5;
  outline: none;
  resize: none;
  min-height: 180px;
  transition: border-color 0.2s;
  caret-color: var(--primary);
}
.sched-paste-area:focus { border-color: var(--primary); }
.sched-paste-area::placeholder { color: var(--text-dim); font-family: var(--font-mono); font-size: 10px; }
.sched-paste-status {
  margin-top: 6px;
  font-size: 10px;
  font-family: var(--font-mono);
  min-height: 16px;
  transition: color 0.2s;
}
.sched-paste-status.valid { color: var(--green); }
.sched-paste-status.invalid { color: var(--red); }
.sched-paste-status.idle { color: var(--text-dim); }

/* Schedule schema hint */
.schedule-schema-hint { background: var(--bg2); border: 1px solid var(--border2); border-radius: var(--r-sm); padding: 12px; font-family: var(--font-mono); font-size: 10px; color: var(--text-muted); margin-top: 12px; overflow-x: auto; max-height: 160px; overflow-y: auto; white-space: pre; line-height: 1.5; }

/* ===================================================
   ★ NEW: MASTERY CHECK MODAL ★
=================================================== */
.mastery-checklist {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 20px 0;
}
.mastery-item {
  display: flex;
  gap: 12px;
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: var(--r-sm);
  padding: 12px 14px;
  cursor: pointer;
  transition: all 0.2s;
  align-items: flex-start;
}
.mastery-item:has(input:checked) {
  background: var(--primary-dim);
  border-color: rgba(0,210,190,0.3);
}
.mastery-item input[type="checkbox"] {
  width: 18px; height: 18px;
  flex-shrink: 0;
  cursor: pointer;
  accent-color: var(--primary);
  margin-top: 1px;
}
.mastery-item-content {}
.mastery-item-title {
  font-family: var(--font-head);
  font-size: 13px;
  font-weight: 700;
  color: var(--text);
  margin-bottom: 3px;
}
.mastery-item-desc {
  font-size: 12px;
  color: var(--text-muted);
  line-height: 1.4;
}
.mastery-item:has(input:checked) .mastery-item-title { color: var(--primary); }
.mastery-item:has(input:checked) .mastery-item-desc { color: rgba(0,210,190,0.7); }
.mastery-topic-banner {
  background: linear-gradient(135deg, rgba(0,210,190,0.08), rgba(58,134,255,0.06));
  border: 1px solid rgba(0,210,190,0.2);
  border-radius: var(--r-sm);
  padding: 12px 16px;
  margin-bottom: 4px;
  font-family: var(--font-head);
  font-size: 14px;
  font-weight: 700;
  color: var(--primary);
}
.mastery-progress-track {
  display: flex;
  gap: 6px;
  margin-bottom: 16px;
  padding: 0 2px;
}
.mastery-pip {
  flex: 1;
  height: 3px;
  background: var(--surface3);
  border-radius: 2px;
  transition: background 0.3s;
}
.mastery-pip.lit { background: var(--primary); }
#mastery-submit-btn {
  width: 100%;
  padding: 13px;
  font-size: 15px;
  font-family: var(--font-head);
  font-weight: 800;
  margin-top: 6px;
}
#mastery-submit-btn:disabled {
  opacity: 0.4;
  cursor: not-allowed;
  filter: none;
  box-shadow: none;
}

/* ===================================================
   ★ NEW: MEGA PROMPT BUTTON ★
=================================================== */
.mega-prompt-btn {
  display: block;
  width: 100%;
  background: linear-gradient(135deg, rgba(255,179,64,0.12), rgba(0,210,190,0.08));
  border: 1px solid rgba(255,179,64,0.35);
  border-radius: var(--r);
  padding: 16px 20px;
  margin-top: 16px;
  cursor: pointer;
  transition: all 0.25s;
  text-align: left;
  font-family: var(--font-body);
  position: relative;
  overflow: hidden;
}
.mega-prompt-btn::before {
  content: '';
  position: absolute; inset: 0;
  background: linear-gradient(135deg, rgba(255,179,64,0.05), transparent);
  opacity: 0;
  transition: opacity 0.25s;
}
.mega-prompt-btn:hover { border-color: var(--yellow); transform: translateY(-2px); box-shadow: 0 8px 30px rgba(255,179,64,0.12); }
.mega-prompt-btn:hover::before { opacity: 1; }
.mega-prompt-btn:active { transform: translateY(0); }
.mega-prompt-btn-inner { display: flex; align-items: center; gap: 14px; }
.mega-prompt-icon { font-size: 32px; flex-shrink: 0; }
.mega-prompt-text {}
.mega-prompt-title { font-family: var(--font-head); font-size: 15px; font-weight: 800; color: var(--yellow); margin-bottom: 3px; }
.mega-prompt-sub { font-size: 12px; color: var(--text-muted); }
.mega-prompt-badge {
  margin-left: auto;
  background: var(--yellow-dim);
  border: 1px solid rgba(255,179,64,0.3);
  color: var(--yellow);
  border-radius: var(--r-sm);
  padding: 4px 10px;
  font-size: 11px;
  font-family: var(--font-mono);
  white-space: nowrap;
  flex-shrink: 0;
}

/* ============================================================
   MEDTRACK PRO v4 — ADDITIONS
============================================================ */

/* ── CHAPTER CARD SYSTEM ── */
.chapter-cards-wrap{display:flex;flex-direction:column;gap:10px}
.chapter-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);overflow:hidden;transition:box-shadow .2s,border-color .2s}
.chapter-card:hover{border-color:rgba(0,210,190,.22)}
.chapter-card.expanded{border-color:rgba(0,210,190,.28);box-shadow:0 0 0 1px rgba(0,210,190,.08)}
.chapter-card.has-due{border-color:rgba(255,71,87,.2)}
.chapter-card-header{display:flex;align-items:center;gap:10px;padding:13px 16px;cursor:pointer;background:linear-gradient(90deg,rgba(0,210,190,.04),transparent);user-select:none}
.chapter-card-header:hover{background:linear-gradient(90deg,rgba(0,210,190,.08),rgba(0,210,190,.02))}
.chapter-exp-icon{font-size:9px;color:var(--text-muted);transition:transform .2s;flex-shrink:0;width:15px;height:15px;display:flex;align-items:center;justify-content:center;background:var(--surface2);border:1px solid var(--border2);border-radius:3px}
.chapter-card.expanded .chapter-exp-icon{transform:rotate(90deg)}
.chapter-name{font-family:var(--font-head);font-size:13px;font-weight:700;flex:1;min-width:0;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.chapter-meta{display:flex;align-items:center;gap:6px;flex-shrink:0;flex-wrap:wrap}
.chapter-badge{font-family:var(--font-mono);font-size:10px;padding:2px 6px;border-radius:3px;background:var(--surface2);border:1px solid var(--border2);color:var(--text-muted);white-space:nowrap}
.chapter-badge.due{background:var(--red-dim);border-color:rgba(255,71,87,.25);color:var(--red)}
.chapter-badge.mastered{background:var(--green-dim);border-color:rgba(6,255,165,.25);color:var(--green)}
.chapter-ring{width:34px;height:34px;flex-shrink:0;position:relative}
.chapter-ring svg{width:34px;height:34px;transform:rotate(-90deg)}
.chapter-ring .cbg{stroke:var(--surface3);fill:none;stroke-width:3}
.chapter-ring .cfill{fill:none;stroke-width:3;stroke-linecap:round;transition:stroke-dashoffset .5s}
.chapter-pct{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);font-size:8px;font-family:var(--font-mono);font-weight:700}
.chapter-progress{height:3px;background:var(--surface3);overflow:hidden}
.chapter-progress-fill{height:100%;background:linear-gradient(90deg,var(--primary),var(--blue));transition:width .5s}
.chapter-stats-row{display:flex;gap:14px;flex-wrap:wrap;padding:8px 16px;background:var(--surface2);border-top:1px solid var(--border2);font-size:11px;color:var(--text-muted)}
.chapter-stat{display:flex;align-items:center;gap:3px}
.chapter-stat b.g{color:var(--green)}
.chapter-stat b.r{color:var(--red)}
.chapter-stat b.y{color:var(--yellow)}
.chapter-topics-body{display:none;padding:8px 10px 10px;border-top:1px solid var(--border2)}
.chapter-card.expanded .chapter-topics-body{display:block;animation:fadeUp .18s ease}
.chapter-add-row{display:flex;align-items:center;gap:7px;padding:6px 8px;cursor:pointer;color:var(--text-muted);font-size:12px;border-radius:var(--r-sm);transition:all .15s;border:1px dashed var(--border2);margin-top:6px}
.chapter-add-row:hover{color:var(--primary);border-color:var(--primary);background:var(--primary-dim)}

/* ── NOTEBOOK TOPIC ROWS ── */
.nb-row{display:flex;align-items:center;gap:9px;padding:7px 9px;border-radius:var(--r-sm);transition:background .15s;cursor:pointer;border:1px solid transparent;margin-bottom:3px;position:relative}
.nb-row:hover{background:var(--surface2);border-color:var(--border2)}
.nb-row.ms{opacity:.6}
.topic-mini-ring{width:26px;height:26px;flex-shrink:0;position:relative}
.topic-mini-ring svg{width:26px;height:26px;transform:rotate(-90deg)}
.topic-mini-ring .mbg{stroke:var(--surface3);fill:none;stroke-width:3}
.topic-mini-ring .mfill{fill:none;stroke-width:3;stroke-linecap:round}
.topic-mini-pct{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);font-size:7px;font-family:var(--font-mono)}
.nb-name{flex:1;font-size:12px;min-width:0;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.nb-meta{display:flex;align-items:center;gap:4px;flex-shrink:0}

/* ── STUDY STATE PILLS ── */
.state-pill{font-size:10px;padding:2px 5px;border-radius:3px;font-weight:600;white-space:nowrap}
.sp-new{background:var(--surface3);color:var(--text-muted)}
.sp-learning{background:var(--blue-dim);color:var(--blue);border:1px solid rgba(58,134,255,.2)}
.sp-struggling{background:var(--red-dim);color:var(--red);border:1px solid rgba(255,71,87,.2)}
.sp-risk{background:var(--yellow-dim);color:var(--yellow);border:1px solid rgba(255,179,64,.2)}
.sp-stable{background:var(--primary-dim);color:var(--primary);border:1px solid rgba(0,210,190,.2)}
.sp-mastered{background:var(--green-dim);color:var(--green);border:1px solid rgba(6,255,165,.2)}
.sp-examready{background:linear-gradient(90deg,rgba(255,179,64,.15),rgba(6,255,165,.15));color:var(--yellow);border:1px solid rgba(255,179,64,.3)}

/* ── IMPORTANCE BADGES ── */
.imp-badge{font-size:10px;padding:2px 5px;border-radius:3px;font-weight:600;white-space:nowrap;display:inline-flex;align-items:center;gap:2px}
.ib-hy{background:rgba(255,179,64,.15);border:1px solid rgba(255,179,64,.3);color:var(--yellow)}
.ib-viva{background:var(--red-dim);border:1px solid rgba(255,71,87,.25);color:var(--red)}
.ib-concept{background:var(--purple-dim);border:1px solid rgba(199,125,255,.25);color:var(--purple)}
.ib-clinical{background:var(--blue-dim);border:1px solid rgba(58,134,255,.25);color:var(--blue)}
.imp-badges-row{display:flex;gap:3px;flex-wrap:wrap;margin-top:2px}
.imp-toggle-row{display:flex;gap:5px;flex-wrap:wrap;margin:8px 0}
.imp-btn{font-size:11px;padding:4px 9px;border-radius:20px;border:1px solid var(--border2);background:var(--surface2);color:var(--text-muted);cursor:pointer;transition:all .2s;font-family:var(--font-body)}
.imp-btn:hover{border-color:var(--primary);color:var(--primary)}
.imp-btn.on-hy{background:rgba(255,179,64,.15);border-color:var(--yellow);color:var(--yellow)}
.imp-btn.on-viva{background:var(--red-dim);border-color:var(--red);color:var(--red)}
.imp-btn.on-concept{background:var(--purple-dim);border-color:var(--purple);color:var(--purple)}
.imp-btn.on-clinical{background:var(--blue-dim);border-color:var(--blue);color:var(--blue)}

/* ── VIEW MODE TOGGLE ── */
.view-mode-toggle{display:flex;gap:3px;background:var(--surface);border:1px solid var(--border);border-radius:var(--r-sm);padding:3px}
.vm-btn{padding:4px 10px;font-size:11px;border-radius:4px;border:none;cursor:pointer;font-family:var(--font-body);background:none;color:var(--text-muted);transition:all .15s}
.vm-btn.active{background:var(--primary-dim);color:var(--primary)}

/* ── URGENT REVISION PANEL ── */
.urgent-panel{background:linear-gradient(135deg,rgba(255,71,87,.07),rgba(255,179,64,.05));border:1px solid rgba(255,71,87,.2);border-radius:var(--r);padding:14px;margin-bottom:14px}
.urgent-title{font-family:var(--font-head);font-size:11px;font-weight:800;color:var(--red);text-transform:uppercase;letter-spacing:1px;margin-bottom:8px;display:flex;align-items:center;gap:5px}
.urgent-chips{display:flex;flex-wrap:wrap;gap:5px}
.uchip{font-size:11px;padding:3px 9px;border-radius:20px;border:1px solid rgba(255,71,87,.3);background:var(--red-dim);color:var(--red);cursor:pointer;transition:all .15s}
.uchip:hover{background:rgba(255,71,87,.2)}

/* ── DEEP FOCUS OVERLAY ── */
.focus-overlay{display:none;position:fixed;inset:0;background:rgba(4,8,16,.97);backdrop-filter:blur(20px);z-index:5000;flex-direction:column;overflow-y:auto}
.focus-overlay.show{display:flex}
.focus-hdr{position:sticky;top:0;background:rgba(6,12,24,.97);backdrop-filter:blur(20px);border-bottom:1px solid rgba(0,210,190,.15);padding:11px 20px;display:flex;align-items:center;gap:10px;z-index:10;flex-wrap:wrap}
.focus-title{font-family:var(--font-head);font-size:16px;font-weight:800;flex:1;min-width:0;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.focus-clock{font-family:var(--font-mono);font-size:22px;font-weight:700;color:var(--primary)}
.focus-body{max-width:840px;margin:0 auto;padding:22px 18px;width:100%}
.focus-sec{background:var(--surface);border:1px solid var(--border2);border-radius:var(--r);padding:15px;margin-bottom:12px}
.focus-sec-title{font-family:var(--font-head);font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:1.5px;color:var(--text-muted);margin-bottom:10px;display:flex;align-items:center;gap:5px}
.focus-stages{display:grid;grid-template-columns:repeat(5,1fr);gap:5px}
@media(max-width:560px){.focus-stages{grid-template-columns:1fr 1fr}}
.focus-stage{padding:9px 4px;border-radius:var(--r-sm);border:1px solid var(--border2);background:var(--surface2);color:var(--text-muted);font-size:11px;cursor:pointer;transition:all .2s;text-align:center;font-family:var(--font-body);display:flex;flex-direction:column;align-items:center;gap:3px}
.focus-stage.done{background:var(--primary-dim);border-color:var(--primary);color:var(--primary)}
.focus-stage.done.ms-stage{background:var(--green-dim);border-color:var(--green);color:var(--green)}
.focus-stage:hover{border-color:var(--primary)}
.focus-notes{width:100%;background:var(--surface2);border:1px solid var(--border);border-radius:var(--r-sm);padding:11px;color:var(--text);font-family:var(--font-body);font-size:13px;resize:vertical;min-height:100px;outline:none;line-height:1.6}
.focus-notes:focus{border-color:var(--primary)}
.focus-micro-item{display:flex;align-items:center;gap:9px;padding:7px 11px;background:var(--surface2);border:1px solid var(--border2);border-radius:var(--r-sm);cursor:pointer;transition:all .15s;margin-bottom:4px}
.focus-micro-item:hover{border-color:var(--primary)}
.focus-micro-item.done-m{opacity:.5}
.focus-micro-item.done-m span{text-decoration:line-through}
.focus-ai-resp{background:var(--surface2);border:1px solid var(--border2);border-radius:var(--r-sm);padding:13px;margin-top:10px;font-size:13px;line-height:1.7;color:var(--text);max-height:280px;overflow-y:auto;display:none;white-space:pre-wrap}
.focus-ai-btns{display:flex;gap:5px;flex-wrap:wrap;margin-bottom:8px}
.f-ai-btn{font-size:11px;padding:5px 10px;border-radius:var(--r-sm);border:1px solid var(--border2);background:var(--surface2);color:var(--text-muted);cursor:pointer;transition:all .2s;font-family:var(--font-body)}
.f-ai-btn:hover{border-color:var(--primary);color:var(--primary)}

/* ── AI ASSISTANT MODAL ── */
.ai-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.8);backdrop-filter:blur(8px);z-index:4000;align-items:center;justify-content:center;padding:16px}
.ai-overlay.show{display:flex}
.ai-modal{background:var(--surface);border:1px solid rgba(0,210,190,.25);border-radius:var(--r);width:100%;max-width:680px;max-height:88vh;display:flex;flex-direction:column;animation:fadeUp .2s ease}
.ai-modal-hdr{padding:18px 20px 14px;border-bottom:1px solid var(--border);display:flex;align-items:center;gap:9px}
.ai-modal-title{font-family:var(--font-head);font-size:15px;font-weight:800;flex:1}
.ai-quick-btns{display:flex;gap:5px;flex-wrap:wrap;padding:10px 14px 6px}
.ai-qbtn{font-size:11px;padding:4px 9px;border-radius:20px;border:1px solid var(--border2);background:var(--surface2);color:var(--text-muted);cursor:pointer;transition:all .15s;font-family:var(--font-body);white-space:nowrap}
.ai-qbtn:hover{border-color:var(--primary);color:var(--primary)}
.ai-chat{flex:1;overflow-y:auto;padding:12px;min-height:180px;max-height:380px}
.ai-chat::-webkit-scrollbar{width:3px}
.ai-chat::-webkit-scrollbar-thumb{background:var(--surface3);border-radius:3px}
.ai-msg{margin-bottom:12px}
.ai-msg-role{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:1px;margin-bottom:3px}
.ai-msg-role.u{color:var(--blue)}
.ai-msg-role.a{color:var(--primary)}
.ai-msg-body{font-size:13px;line-height:1.65;color:var(--text);background:var(--surface2);border-radius:var(--r-sm);padding:9px 12px;white-space:pre-wrap}
.ai-msg-body.u-msg{background:var(--blue-dim);border:1px solid rgba(58,134,255,.2)}
.ai-input-row{padding:12px 14px;border-top:1px solid var(--border);display:flex;gap:7px}
.ai-input{flex:1;background:var(--surface2);border:1px solid var(--border);border-radius:var(--r-sm);padding:9px 12px;color:var(--text);font-family:var(--font-body);font-size:13px;outline:none;resize:none;min-height:42px;max-height:110px}
.ai-input:focus{border-color:var(--primary)}

/* ── REFLECTION DIARY ── */
.diary-ref-card{background:linear-gradient(135deg,rgba(199,125,255,.05),rgba(58,134,255,.04));border:1px solid rgba(199,125,255,.18);border-radius:var(--r);padding:16px;margin-bottom:16px;background-image:repeating-linear-gradient(transparent,transparent 27px,rgba(199,125,255,.04) 27px,rgba(199,125,255,.04) 28px)}
.diary-ref-title{font-family:var(--font-head);font-size:12px;font-weight:800;color:var(--purple);margin-bottom:12px;display:flex;align-items:center;gap:5px}
.ref-label{font-size:10px;color:var(--text-muted);text-transform:uppercase;letter-spacing:1px;margin-bottom:4px}
.ref-input{width:100%;background:var(--surface2);border:1px solid var(--border2);border-radius:var(--r-sm);padding:8px 11px;color:var(--text);font-family:var(--font-body);font-size:13px;outline:none;resize:none;line-height:1.5}
.ref-input:focus{border-color:var(--purple)}
.ref-field{margin-bottom:11px}
.energy-row{display:flex;align-items:center;gap:10px}
.energy-slider{flex:1;accent-color:var(--purple)}
.energy-val{font-family:var(--font-mono);font-size:13px;font-weight:700;color:var(--purple);min-width:36px}

/* ── RETENTION MAP ── */
.retention-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:18px;margin-top:16px}
.ret-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(130px,1fr));gap:9px;margin-top:14px}
.ret-subject{background:var(--surface2);border:1px solid var(--border2);border-radius:var(--r-sm);padding:11px;text-align:center;transition:all .2s;cursor:pointer}
.ret-subject:hover{border-color:var(--primary);transform:translateY(-2px)}
.ret-name{font-size:11px;font-weight:600;margin-bottom:5px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.ret-pct{font-family:var(--font-mono);font-size:17px;font-weight:800}
.ret-bar{height:5px;background:var(--surface3);border-radius:3px;overflow:hidden;margin:5px 0}
.ret-fill{height:100%;border-radius:3px;transition:width .5s}
.ret-sub{font-size:10px;color:var(--text-muted)}

/* ── INTELLIGENCE BRIEF ── */
.brief-row{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:16px}
.brief-chip{background:var(--surface2);border:1px solid var(--border2);border-radius:var(--r-sm);padding:8px 12px;font-size:12px;display:flex;align-items:center;gap:7px;flex:1;min-width:110px;cursor:pointer;transition:all .2s}
.brief-chip:hover{border-color:var(--primary)}
.bc-val{font-family:var(--font-mono);font-weight:700;font-size:15px}
.bc-lbl{color:var(--text-muted);font-size:11px}

/* ── CONFIDENCE REVISION BUTTONS ── */
.rev-card-v4{background:var(--surface);border:1px solid var(--border);border-left:3px solid var(--primary);border-radius:var(--r);padding:22px;margin-bottom:14px}
.rcv-title{font-family:var(--font-head);font-size:19px;font-weight:800;margin-bottom:4px}
.rcv-folder{font-size:12px;color:var(--text-muted);margin-bottom:14px}
.conf-btns{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:7px}
@media(max-width:500px){.conf-btns{grid-template-columns:1fr 1fr}}
.conf-btn{padding:10px 5px;border-radius:var(--r-sm);border:1px solid var(--border2);background:var(--surface2);color:var(--text);font-size:12px;cursor:pointer;transition:all .2s;font-family:var(--font-body);text-align:center;font-weight:600}
.conf-btn:hover{transform:translateY(-1px)}
.cb-0{border-color:rgba(255,71,87,.4);background:var(--red-dim);color:var(--red)}
.cb-2{border-color:rgba(255,140,66,.4);background:rgba(255,140,66,.12);color:var(--orange)}
.cb-4{border-color:rgba(0,210,190,.4);background:var(--primary-dim);color:var(--primary)}
.cb-5{border-color:rgba(6,255,165,.4);background:var(--green-dim);color:var(--green)}
.rev-prog{height:4px;background:var(--surface3);border-radius:3px;overflow:hidden;margin-bottom:14px}
.rev-prog-fill{height:100%;background:linear-gradient(90deg,var(--primary),var(--blue));transition:width .3s}

/* ── EXAM WAR MODE ── */
.exam-banner{background:linear-gradient(135deg,rgba(255,71,87,.12),rgba(255,179,64,.08));border:1px solid rgba(255,71,87,.35);border-radius:var(--r);padding:11px 16px;display:flex;align-items:center;gap:10px;margin-bottom:14px}
.exam-badge{background:var(--red);color:#fff;font-family:var(--font-head);font-size:10px;font-weight:800;padding:2px 7px;border-radius:3px;text-transform:uppercase;letter-spacing:1px;flex-shrink:0}
.exam-text{flex:1;font-size:12px}
.exam-text strong{color:var(--red)}

/* ── MILESTONE BADGE ── */
.milestone{display:inline-flex;align-items:center;gap:5px;background:linear-gradient(135deg,rgba(255,179,64,.15),rgba(255,140,66,.1));border:1px solid rgba(255,179,64,.35);border-radius:20px;padding:3px 11px;font-size:12px;color:var(--yellow);font-weight:600}

/* ── EXTRA RESPONSIVE ── */
@media(max-width:768px){
  .focus-hdr{padding:9px 12px;gap:6px}
  .focus-title{font-size:13px}
  .focus-clock{font-size:18px}
  .focus-body{padding:14px 10px}
  .ai-modal{max-height:95vh}
  .conf-btns{grid-template-columns:1fr 1fr}
}


/* ═══════════════════════════════════════════════════════
   MEDICAL DIARY VIEW — v4
═══════════════════════════════════════════════════════ */

/* ── DIARY HEADER ── */
.diary-view-hdr{
  display:flex;align-items:center;justify-content:space-between;
  padding:16px 22px 12px;border-bottom:1px solid var(--border);
  flex-wrap:wrap;gap:10px;
}
.diary-view-hdr-left{}
.diary-view-title{font-family:var(--font-head);font-size:20px;font-weight:800}
.diary-view-date{font-size:12px;color:var(--text-muted);margin-top:2px;font-family:var(--font-mono)}
.diary-view-hdr-right{display:flex;align-items:center;gap:7px}
.dv-icon-btn{
  width:30px;height:30px;border-radius:var(--r-sm);border:1px solid var(--border2);
  background:var(--surface2);color:var(--text-muted);cursor:pointer;font-size:16px;
  display:flex;align-items:center;justify-content:center;transition:all .15s;
}
.dv-icon-btn:hover{border-color:var(--primary);color:var(--primary)}
.dv-today-btn{
  padding:5px 12px;border-radius:var(--r-sm);border:1px solid var(--border2);
  background:var(--surface2);color:var(--text-muted);cursor:pointer;font-size:12px;
  transition:all .15s;font-family:var(--font-body);
}
.dv-today-btn:hover{border-color:var(--primary);color:var(--primary)}

/* ── WEEK RIBBON ── */
.diary-week-ribbon{
  display:flex;gap:5px;padding:10px 22px 6px;overflow-x:auto;
  border-bottom:1px solid var(--border);scrollbar-width:none;
}
.diary-week-ribbon::-webkit-scrollbar{display:none}
.dv-day-chip{
  flex-shrink:0;padding:6px 12px;border-radius:var(--r-sm);
  border:1px solid var(--border2);background:var(--surface2);
  color:var(--text-muted);font-size:11px;cursor:pointer;transition:all .15s;
  text-align:center;min-width:58px;font-family:var(--font-body);
}
.dv-day-chip:hover{border-color:var(--primary);color:var(--primary)}
.dv-day-chip.today{border-color:var(--primary);color:var(--primary);background:var(--primary-dim)}
.dv-day-chip.selected{background:var(--primary);color:#000;border-color:var(--primary);font-weight:700}
.dv-day-chip.has-tasks::after{content:'';display:block;width:4px;height:4px;border-radius:50%;background:var(--green);margin:3px auto 0}

/* ── QUOTE BAR ── */
.diary-quote-bar{
  display:flex;align-items:center;gap:8px;padding:8px 22px;
  background:linear-gradient(90deg,rgba(199,125,255,.05),rgba(58,134,255,.03));
  border-bottom:1px solid var(--border);
  font-size:12px;color:var(--text-muted);
}
.diary-quote-icon{font-size:14px;flex-shrink:0}
.diary-quote-text{flex:1;font-style:italic;line-height:1.4}
.diary-quote-author{font-size:10px;font-weight:700;color:var(--purple);flex-shrink:0}

/* ── MAIN GRID ── */
.diary-main-grid{
  display:grid;grid-template-columns:1fr 300px;gap:0;
  min-height:calc(100vh - 200px);
}
@media(max-width:900px){
  .diary-main-grid{grid-template-columns:1fr}
  .diary-sidebar{border-top:1px solid var(--border);border-left:none!important}
}

/* ── NOTEBOOK ── */
.diary-notebook{
  padding:20px 24px;overflow-y:auto;
  /* Ruled paper effect */
  background-image:
    linear-gradient(transparent 0px, transparent 31px, rgba(0,210,190,.07) 31px, rgba(0,210,190,.07) 32px);
  background-size:100% 32px;
  background-attachment:local;
  border-right:1px solid var(--border);
  /* Left margin red line */
  border-left:3px solid rgba(255,71,87,.2);
  position:relative;
}
.diary-notebook::before{
  content:'';position:absolute;top:0;bottom:0;left:48px;
  width:1px;background:rgba(255,71,87,.12);pointer-events:none;z-index:0;
}

/* ── DAY TITLE ROW ── */
.dv-day-title-row{display:flex;align-items:center;gap:12px;margin-bottom:14px;position:relative;z-index:1}
.dv-day-badge{
  font-family:var(--font-head);font-size:11px;font-weight:800;
  background:var(--primary-dim);border:1px solid var(--primary);
  color:var(--primary);padding:4px 10px;border-radius:var(--r-sm);
  white-space:nowrap;flex-shrink:0;text-transform:uppercase;letter-spacing:.5px;
}
.dv-day-title-input{
  flex:1;background:transparent;border:none;outline:none;
  font-family:var(--font-head);font-size:16px;font-weight:700;color:var(--text);
  border-bottom:1px dashed var(--border2);padding-bottom:4px;
}
.dv-day-title-input::placeholder{color:var(--text-dim)}

/* ── PROGRESS ROW ── */
.dv-progress-row{display:flex;align-items:center;gap:10px;margin-bottom:18px;position:relative;z-index:1}
.dv-progress-track{flex:1;height:6px;background:var(--surface3);border-radius:4px;overflow:hidden}
.dv-progress-fill{height:100%;background:linear-gradient(90deg,var(--primary),var(--green));border-radius:4px;transition:width .4s}
.dv-progress-text{font-size:11px;font-family:var(--font-mono);color:var(--text-muted);white-space:nowrap}

/* ── TIMELINE ── */
.dv-timeline{position:relative;z-index:1}
.dv-empty-state{text-align:center;padding:40px 20px;color:var(--text-muted)}
.dv-empty-icon{font-size:36px;margin-bottom:8px}
.dv-empty-title{font-family:var(--font-head);font-size:15px;font-weight:700;margin-bottom:4px}
.dv-empty-sub{font-size:12px}

/* ── TASK SLOT ── */
.dv-slot{
  display:flex;gap:0;margin-bottom:0;position:relative;
}
.dv-slot-time{
  font-family:var(--font-mono);font-size:11px;color:var(--text-muted);
  width:46px;flex-shrink:0;padding-top:14px;text-align:right;
  padding-right:10px;
}
.dv-slot-rail{
  width:28px;flex-shrink:0;display:flex;flex-direction:column;align-items:center;
  padding-top:14px;
}
.dv-slot-dot{
  width:11px;height:11px;border-radius:50%;border:2px solid var(--border2);
  background:var(--surface);flex-shrink:0;transition:all .2s;z-index:1;
}
.dv-slot.done .dv-slot-dot{background:var(--green);border-color:var(--green)}
.dv-slot.active-slot .dv-slot-dot{background:var(--primary);border-color:var(--primary);box-shadow:0 0 0 3px rgba(0,210,190,.15)}
.dv-slot-line{flex:1;width:1px;background:var(--border2);margin-top:4px}
.dv-slot-body{
  flex:1;padding:10px 12px;margin-bottom:4px;
  background:var(--surface);border:1px solid var(--border2);
  border-radius:var(--r);transition:all .2s;position:relative;
  overflow:hidden;
}
.dv-slot-body:hover{border-color:rgba(0,210,190,.25);background:rgba(0,210,190,.02)}
.dv-slot.done .dv-slot-body{opacity:.6;border-color:rgba(6,255,165,.15);background:rgba(6,255,165,.02)}
/* Type color strip */
.dv-slot-body::before{
  content:'';position:absolute;top:0;left:0;bottom:0;width:3px;
  background:var(--slot-color,var(--primary));border-radius:4px 0 0 4px;
}
.dv-slot-top{display:flex;align-items:center;gap:8px}
.dv-slot-check{
  width:20px;height:20px;border-radius:5px;border:2px solid var(--border2);
  background:var(--surface2);cursor:pointer;flex-shrink:0;
  display:flex;align-items:center;justify-content:center;transition:all .2s;font-size:12px;
}
.dv-slot-check:hover{border-color:var(--primary)}
.dv-slot.done .dv-slot-check{background:var(--green);border-color:var(--green);color:#000}
.dv-slot-emoji{font-size:16px;flex-shrink:0}
.dv-slot-title{
  font-family:var(--font-head);font-size:13px;font-weight:700;flex:1;min-width:0;
}
.dv-slot.done .dv-slot-title{text-decoration:line-through;color:var(--text-muted)}
.dv-slot-badges{display:flex;gap:4px;flex-wrap:wrap;margin-top:5px;margin-left:28px}
.dv-slot-badge{
  font-size:10px;padding:2px 6px;border-radius:3px;
  background:var(--surface2);border:1px solid var(--border2);color:var(--text-muted);
}
.dv-slot-badge.linked{background:var(--blue-dim);border-color:rgba(58,134,255,.25);color:var(--blue)}
.dv-slot-badge.stage{background:var(--primary-dim);border-color:rgba(0,210,190,.25);color:var(--primary)}
.dv-slot-badge.sync-done{background:var(--green-dim);border-color:rgba(6,255,165,.25);color:var(--green)}
.dv-slot-actions{display:flex;gap:4px;opacity:0;transition:opacity .15s;flex-shrink:0}
.dv-slot-body:hover .dv-slot-actions{opacity:1}
.dv-slot-act-btn{
  font-size:11px;padding:3px 7px;border-radius:var(--r-sm);
  border:1px solid var(--border2);background:var(--surface2);
  color:var(--text-muted);cursor:pointer;transition:all .15s;font-family:var(--font-body);
}
.dv-slot-act-btn:hover{border-color:var(--primary);color:var(--primary)}
.dv-slot-act-btn.del:hover{border-color:var(--red);color:var(--red)}

/* ── MICRO STEPS ── */
.dv-microsteps{margin-top:8px;margin-left:28px;display:none}
.dv-microsteps.open{display:block;animation:fadeUp .15s ease}
.dv-micro-item{
  display:flex;align-items:center;gap:8px;padding:5px 8px;
  border-radius:var(--r-sm);cursor:pointer;transition:background .1s;
  font-size:12px;border:1px solid transparent;margin-bottom:2px;
}
.dv-micro-item:hover{background:var(--surface2);border-color:var(--border2)}
.dv-micro-item.done-micro{opacity:.5}
.dv-micro-item.done-micro span{text-decoration:line-through}
.dv-micro-cb{
  width:14px;height:14px;border-radius:3px;border:1.5px solid var(--border2);
  background:var(--surface2);flex-shrink:0;display:flex;align-items:center;
  justify-content:center;font-size:9px;transition:all .15s;
}
.dv-micro-item:hover .dv-micro-cb{border-color:var(--primary)}
.dv-micro-item.done-micro .dv-micro-cb{background:var(--primary);border-color:var(--primary);color:#000}
.dv-micro-progress{height:2px;background:var(--surface3);border-radius:2px;overflow:hidden;margin:4px 0 0 28px}
.dv-micro-fill{height:100%;background:var(--primary);border-radius:2px;transition:width .3s}

/* ── SYNC FLASH ── */
@keyframes syncFlash{0%{box-shadow:0 0 0 0 rgba(0,210,190,.6)}100%{box-shadow:0 0 0 12px rgba(0,210,190,0)}}
.synced-flash{animation:syncFlash .6s ease}

/* ── SIDEBAR ── */
.diary-sidebar{
  padding:16px;border-left:1px solid var(--border);
  overflow-y:auto;background:var(--surface);
  display:flex;flex-direction:column;gap:12px;
}
.dsb-card{
  background:var(--surface2);border:1px solid var(--border2);
  border-radius:var(--r);padding:14px;
}
.dsb-card-title{
  font-family:var(--font-head);font-size:11px;font-weight:800;
  text-transform:uppercase;letter-spacing:1px;color:var(--text-muted);
  margin-bottom:10px;
}
.dsb-stats-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.dsb-stat{background:var(--surface);border:1px solid var(--border);border-radius:var(--r-sm);padding:9px;text-align:center}
.dsb-stat-val{font-family:var(--font-mono);font-size:18px;font-weight:800}
.dsb-stat-lbl{font-size:10px;color:var(--text-muted);margin-top:2px}
.dv-synced-chip{
  display:flex;align-items:center;gap:6px;padding:5px 8px;
  background:var(--surface);border:1px solid rgba(6,255,165,.2);
  border-radius:var(--r-sm);font-size:11px;margin-bottom:4px;cursor:pointer;
  transition:all .15s;
}
.dv-synced-chip:hover{border-color:var(--primary)}
.dv-due-chip{
  display:flex;align-items:center;gap:6px;padding:5px 8px;
  background:var(--surface);border:1px solid rgba(255,71,87,.15);
  border-radius:var(--r-sm);font-size:11px;margin-bottom:4px;cursor:pointer;
  transition:all .15s;
}
.dv-due-chip:hover{border-color:var(--red)}
.dv-ref-field{margin-bottom:10px}
.dv-ref-label{font-size:10px;color:var(--text-muted);text-transform:uppercase;letter-spacing:.8px;margin-bottom:3px}

/* ── TYPE COLORS ── */
.dv-type-study{--slot-color:var(--primary)}
.dv-type-revision{--slot-color:var(--blue)}
.dv-type-recall{--slot-color:var(--purple)}
.dv-type-mcq{--slot-color:var(--yellow)}
.dv-type-clinical{--slot-color:rgba(255,71,87,.8)}
.dv-type-video{--slot-color:rgba(255,140,66,.8)}
.dv-type-diagram{--slot-color:rgba(199,125,255,.8)}
.dv-type-notes{--slot-color:var(--green)}
.dv-type-break{--slot-color:var(--surface3)}


/* ── HOME DIARY PREVIEW CARD ── */
.home-diary-preview-card{
  background:var(--surface);border:1px solid var(--border);
  border-radius:var(--r);padding:16px;margin-bottom:16px;
  border-left:3px solid var(--primary);
}
.home-diary-empty{
  text-align:center;padding:24px 16px;color:var(--text-muted);
  background:var(--surface);border:1px solid var(--border);
  border-radius:var(--r);margin-bottom:16px;border-left:3px solid var(--border2);
}
.home-diary-hdr{
  display:flex;align-items:flex-start;justify-content:space-between;
  gap:10px;margin-bottom:10px;flex-wrap:wrap;
}
.home-diary-hdr-title{
  font-family:var(--font-head);font-size:14px;font-weight:800;margin-bottom:2px;
}
.home-diary-hdr-sub{font-size:11px;color:var(--text-muted);font-family:var(--font-mono)}
.home-diary-badge{
  font-size:11px;padding:2px 7px;border-radius:20px;
  background:var(--surface2);border:1px solid var(--border2);font-weight:600;
}
.home-diary-tasks{display:flex;flex-direction:column;gap:4px}
.home-diary-task-row{
  display:flex;align-items:center;gap:8px;padding:7px 9px;
  background:var(--surface2);border:1px solid var(--border2);
  border-radius:var(--r-sm);cursor:pointer;transition:all .15s;
}
.home-diary-task-row:hover{border-color:var(--primary);background:rgba(0,210,190,.04)}
.home-diary-task-check{
  width:18px;height:18px;border-radius:4px;border:1.5px solid var(--border2);
  background:var(--surface);flex-shrink:0;display:flex;align-items:center;
  justify-content:center;font-size:11px;color:var(--green);font-weight:700;
}
.home-diary-task-title{font-size:12px;font-weight:600;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}

/* Update schedule-import modal title to say Diary */


/* ── DIARY STAGE PILLS ── */
.dv-stage-pills {
  display: flex; gap: 5px; flex-wrap: wrap;
  margin: 7px 0 4px 28px; padding: 6px 8px;
  background: var(--surface2); border-radius: var(--r-sm);
  border: 1px solid var(--border2);
}
.dv-stage-pill {
  display: flex; align-items: center; gap: 4px;
  font-size: 11px; padding: 4px 9px; border-radius: 20px;
  border: 1px solid var(--border2); background: var(--surface);
  color: var(--text-muted); cursor: pointer; transition: all .15s;
  font-family: var(--font-body); white-space: nowrap; user-select: none;
}
.dv-stage-pill:hover { border-color: var(--primary); color: var(--primary); background: var(--primary-dim); }
.dv-stage-pill.sp-done {
  background: var(--primary-dim); border-color: var(--primary);
  color: var(--primary); font-weight: 700;
}
.dv-stage-pill.sp-done::before { content: '✓ '; font-size: 10px; }
/* mastered stage special */
.dv-stage-pill[title="Topic Mastered"].sp-done {
  background: var(--green-dim); border-color: var(--green); color: var(--green);
}

/* ── DIARY MEGA PROMPT BAR ── */
.dv-mega-btn {
  display: block; width: 100%;
  background: linear-gradient(135deg, rgba(255,179,64,.1), rgba(0,210,190,.07));
  border: 1px solid rgba(255,179,64,.4); border-radius: var(--r);
  padding: 14px 18px; cursor: pointer; transition: all .2s;
  margin-bottom: 10px; margin-top: 4px; position: relative; z-index: 1;
}
.dv-mega-btn:hover { border-color: var(--yellow); transform: translateY(-1px); box-shadow: 0 6px 24px rgba(255,179,64,.12); }
.dv-mega-inner { display: flex; align-items: center; gap: 12px; }
.dv-mega-icon { font-size: 28px; flex-shrink: 0; }
.dv-mega-text { flex: 1; min-width: 0; }
.dv-mega-title { font-family: var(--font-head); font-size: 14px; font-weight: 800; color: var(--yellow); margin-bottom: 3px; }
.dv-mega-sub { font-size: 11px; color: var(--text-muted); }
.dv-mega-badge {
  background: var(--yellow-dim); border: 1px solid rgba(255,179,64,.3);
  color: var(--yellow); border-radius: var(--r-sm); padding: 3px 10px;
  font-size: 11px; font-weight: 700; flex-shrink: 0; font-family: var(--font-mono);
}
.dv-quick-prompts {
  display: flex; gap: 6px; flex-wrap: wrap; margin-bottom: 14px;
  padding: 0 4px; align-items: center; position: relative; z-index: 1;
}
.dv-qp-btn {
  font-size: 11px; padding: 5px 11px; border-radius: 20px;
  border: 1px solid var(--border2); background: var(--surface2);
  color: var(--text-muted); cursor: pointer; transition: all .15s;
  font-family: var(--font-body); white-space: nowrap;
}
.dv-qp-btn:hover { border-color: var(--primary); color: var(--primary); background: var(--primary-dim); }

</style>
</head>
<body>

<!-- ===================================================
     TOP NAVIGATION
=================================================== -->
<nav id="topnav">
  <div class="nav-brand" onclick="showView('home')">🏥 MedTrack <span style="color:var(--primary)">Pro</span> <span style="font-size:9px;background:var(--primary-dim);border:1px solid var(--primary);color:var(--primary);border-radius:3px;padding:1px 4px;margin-left:2px">v4</span></div>
  <div class="nav-tabs">
    <button class="nav-tab active" id="tab-home" onclick="showView('home')">🏠 Home</button>
    <button class="nav-tab" id="tab-folders" onclick="showView('folders')">📁 Subjects</button>
    <button class="nav-tab" id="tab-revision" onclick="showView('revision')">🔁 Revision</button>
    <button class="nav-tab" id="tab-diary" onclick="showView('diary')">📔 Diary</button>
    <button class="nav-tab" id="tab-analytics" onclick="showView('analytics')">📊 Stats</button>
  </div>
  <div class="nav-search-wrap">
    <span class="search-icon">🔍</span>
    <input type="text" id="global-search" placeholder="Search topics, folders..." autocomplete="off" oninput="doSearch()">
    <div id="search-dropdown"></div>
  </div>
  <div class="nav-right">
    <button class="nav-btn" onclick="openPomodoro()" title="Pomodoro Timer">🍅 <span class="hide-mob">Focus</span></button>
    <button class="nav-btn" onclick="openScheduleImport()" title="Load Daily Schedule">📅 <span class="hide-mob">Schedule</span></button>
    <button class="nav-btn" onclick="openJsonImport()" title="Import Syllabus">⬆ <span class="hide-mob">Import</span></button>
    <div class="sync-badge hide-mob" id="sync-status">☁️ Connecting</div>
    <button class="nav-btn hide-mob" onclick="openSettingsPanel()" title="Settings & Backup">⚙️ Settings</button>
    <button id="theme-toggle-btn" onclick="toggleTheme()" title="Toggle Light/Dark Theme">☀️</button>
    <div id="timer-display" class="hide-mob" onclick="toggleTimer()" title="Click to start/stop">⏱ 00:00:00</div>
    <button class="sign-in-btn" id="nav-signin-btn" onclick="openAuthModal()" title="Sign In for Cloud Sync">
      <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="8" r="4"/><path d="M4 20c0-4 3.6-7 8-7s8 3 8 7"/></svg>
      <span class="hide-mob">Sign In</span>
    </button>
    <div class="auth-chip" id="nav-user-chip" style="display:none" onclick="openAuthModal()">
      <div class="auth-chip-dot"></div>
      <img id="nav-user-avatar" src="" alt="" onerror="this.style.display='none'">
      <span class="auth-chip-name" id="nav-user-name">Signed In</span>
    </div>
  </div>
</nav>

<!-- mobile nav -->
<div style="position:fixed;bottom:0;left:0;right:0;z-index:900;background:rgba(6,12,24,0.95);backdrop-filter:blur(20px);border-top:1px solid var(--border);display:none;padding:6px 4px;" id="mob-nav">
  <div style="display:flex;justify-content:space-around;">
    <button style="background:none;border:none;color:var(--text-muted);cursor:pointer;padding:6px 10px;font-size:11px;display:flex;flex-direction:column;align-items:center;gap:2px;" onclick="showView('home')"><span style="font-size:18px">🏠</span>Home</button>
    <button style="background:none;border:none;color:var(--text-muted);cursor:pointer;padding:6px 10px;font-size:11px;display:flex;flex-direction:column;align-items:center;gap:2px;" onclick="showView('folders')"><span style="font-size:18px">📁</span>Subjects</button>
    <button style="background:none;border:none;color:var(--text-muted);cursor:pointer;padding:6px 10px;font-size:11px;display:flex;flex-direction:column;align-items:center;gap:2px;" onclick="openScheduleImport()"><span style="font-size:18px">📅</span>Schedule</button>
    <button style="background:none;border:none;color:var(--text-muted);cursor:pointer;padding:6px 10px;font-size:11px;display:flex;flex-direction:column;align-items:center;gap:2px;" onclick="showView('revision')"><span style="font-size:18px">🔁</span>Revision</button>
    <button style="background:none;border:none;color:var(--text-muted);cursor:pointer;padding:6px 10px;font-size:11px;display:flex;flex-direction:column;align-items:center;gap:2px;" onclick="showView('analytics')"><span style="font-size:18px">📊</span>Stats</button>
  </div>
</div>

<!-- ===================================================
     VIEW: HOME
=================================================== -->
<div id="view-home" class="view active">
  <div class="home-grid">
    <div class="booster-panel">
      <div class="booster-greeting" id="booster-greeting">Good morning, Doctor in training</div>
      <div class="booster-title">Ready to <span id="booster-action">dominate</span> today?</div>
      <div class="quote-box">
        <div class="quote-text" id="daily-quote">Loading inspiration...</div>
        <div class="quote-author" id="daily-quote-author">— MedTrack</div>
      </div>
      <div class="why-started-box">
        <div class="why-started-label">📌 Why I started medicine</div>
        <textarea class="why-started-input" id="why-started-input" placeholder="Write your motivation here... (it'll be saved)" oninput="saveWhyStarted()"></textarea>
      </div>
      <div class="quick-actions">
        <button class="btn btn-primary" onclick="startRevisionSession()">🔁 Start Revision</button>
        <button class="btn btn-secondary" onclick="showView('folders')">📚 Study Topics</button>
        <button class="btn btn-ghost btn-sm" onclick="showWeakAreas()">⚠️ Weak Areas</button>
      </div>
      <div class="islamic-toggle">
        <div class="toggle-switch" id="islamic-toggle" onclick="toggleIslamic()"><div class="toggle-knob"></div></div>
        <span>Islamic reminder</span>
        <span id="islamic-reminder-text" style="color:var(--primary);display:none;margin-left:8px;font-size:11px;"></span>
      </div>
    </div>
    <div class="side-panel">
      <div class="streak-card">
        <div class="streak-fire">🔥</div>
        <div class="streak-number" id="streak-number">0</div>
        <div class="streak-label">Day Streak</div>
        <div class="streak-best">Best: <span id="streak-best">0</span> days</div>
      </div>
      <div class="reminders-card">
        <div class="reminders-header"><div class="section-title" style="margin-bottom:0">📅 Reminders</div></div>
        <div id="reminders-list"><div style="color:var(--text-muted);font-size:12px;text-align:center;padding:10px 0">No reminders yet</div></div>
        <div class="reminder-input-row">
          <input class="mini-input" id="reminder-input" placeholder="Add reminder..." onkeydown="if(event.key==='Enter')addReminder()">
          <button class="btn btn-primary btn-sm" onclick="addReminder()">+</button>
        </div>
      </div>
    </div>
  </div>

  <div class="stats-grid" id="home-stats-grid"></div>

  <div class="due-today-section" id="due-today-section">
    <div class="due-today-header">
      <div>
        <div class="section-title" style="margin-bottom:2px">📅 Due for Review Today</div>
        <div style="font-size:12px;color:var(--text-muted)">Topics scheduled for automated revision</div>
      </div>
      <span class="due-count-badge" id="due-today-count">0</span>
    </div>
    <div class="due-topic-list" id="due-today-list">
      <div style="color:var(--text-muted);font-size:12px">All caught up! No reviews due today 🎉</div>
    </div>
  </div>

  <div id="weekend-revision-panel" style="display:none">
    <div class="weekend-panel">
      <div class="weekend-panel-header">
        <div>
          <div class="weekend-panel-title">📅 Weekend Revision</div>
          <div class="weekend-panel-sub">Topics you studied this week — review them to lock in your memory</div>
        </div>
        <button class="btn btn-sm" style="background:linear-gradient(135deg,var(--purple),var(--blue));color:#fff;border:none;font-weight:700;" onclick="startWeekendSession()">▶ Start Review Session</button>
      </div>
      <div class="weekend-stats-row" id="weekend-stats-row"></div>
      <div class="weekend-topic-grid" id="weekend-topic-grid"><div style="color:var(--text-muted);font-size:12px">No topics studied this week yet.</div></div>
    </div>
  </div>

  <div class="timer-panel">
    <div class="timer-label">⏱ Today's Study Session</div>
    <div class="timer-big" id="dash-timer-big">00:00:00</div>
    <div class="timer-controls">
      <button class="timer-btn" id="start-stop-btn" onclick="toggleTimer()">▶ Start</button>
      <button class="timer-btn" onclick="resetDayTimer()">↺ Reset</button>
      <button class="timer-btn" onclick="openPomodoro()">🍅 Pomodoro</button>
    </div>
    <div class="timer-stats">
      <div class="timer-stat-row"><div class="timer-stat-name">Today</div><div class="timer-stat-val" id="stat-today-time">0h 0m</div></div>
      <div class="timer-stat-row"><div class="timer-stat-name">This Week</div><div class="timer-stat-val" id="stat-week-time">0h 0m</div></div>
      <div class="timer-stat-row"><div class="timer-stat-name">Total Hours</div><div class="timer-stat-val" id="stat-total-time">0h</div></div>
    </div>
  </div>

  <div class="heatmap-wrap">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:12px">
      <div class="section-title" style="margin-bottom:0">🔥 Study Activity Heatmap</div>
    </div>
    <div id="heatmap-grid"></div>
    <div class="heatmap-legend">
      <span>Less</span>
      <div class="heatmap-legend-cells">
        <div class="heatmap-legend-cell" style="background:var(--surface3)"></div>
        <div class="heatmap-legend-cell" data-level="1"></div>
        <div class="heatmap-legend-cell" data-level="2"></div>
        <div class="heatmap-legend-cell" data-level="3"></div>
        <div class="heatmap-legend-cell" data-level="4"></div>
      </div>
      <span>More</span>
    </div>
  </div>

  <!-- ★ MEDICAL DIARY SCHEDULE SECTION ★ -->
  <div id="home-diary-preview">
    <div class="home-diary-preview-card" id="home-diary-card">
      <!-- rendered by dvRenderHomePreview() -->
    </div>
  </div>

    <div class="quick-planner">
    <div class="planner-title">📋 Today's Study Plan</div>
    <div class="planner-list" id="planner-list"></div>
    <div class="planner-input-row">
      <input class="planner-input" id="planner-input" placeholder="Add a task..." onkeydown="if(event.key==='Enter')addPlannerTask()">
      <button class="btn btn-primary btn-sm" onclick="addPlannerTask()">+ Add</button>
    </div>
  </div>

  <button class="enter-study-btn" onclick="showView('folders')">Enter Study System →</button>
</div>

<!-- ===================================================
     VIEW: FOLDERS
=================================================== -->
<div id="view-folders" class="view">
  <div class="view-header">
    <div><div class="view-title">📁 Study Folders</div><div class="view-subtitle" id="folders-subtitle">Your medical syllabus organized by modules</div></div>
    <div style="display:flex;gap:8px;margin-left:auto;flex-wrap:wrap">
      <button class="btn btn-secondary btn-sm" onclick="openAddFolder()">+ Root Folder</button>
      <button class="btn btn-ghost btn-sm" onclick="openJsonImport()">⬆ Import JSON</button>
    </div>
  </div>
  <div class="folders-grid" id="folders-grid"></div>
</div>

<div id="view-subfolder" class="view">
  <div class="view-header">
    <button class="back-btn" onclick="goBackFromSubfolder()">← Back</button>
    <div><div class="view-title" id="subfolder-view-title">Topics</div></div>
    <div style="margin-left:auto;display:flex;gap:8px">
      <button class="btn btn-ghost btn-sm" onclick="v4OpenAi()">🧠 AI</button>
      <button class="btn btn-secondary btn-sm" id="add-subfolder-btn" onclick="openAddSubfolder()">+ Chapter</button>
      <button class="btn btn-primary btn-sm" onclick="openAddTopic()">+ Topic</button>
    </div>
  </div>
  <div class="breadcrumb" id="subfolder-breadcrumb"></div>
  <div id="subfolder-list-container" style="margin-bottom:16px"></div>
  <div class="topic-filters">
    <input class="topic-search-input" id="topic-search" placeholder="🔍 Search topics..." oninput="filterTopics()">
    <button class="filter-btn active" data-filter="all" onclick="setFilter(this,'all')">All</button>
    <button class="filter-btn" data-filter="starred" onclick="setFilter(this,'starred')">⭐ Starred</button>
    <button class="filter-btn" data-filter="incomplete" onclick="setFilter(this,'incomplete')">Incomplete</button>
    <button class="filter-btn" data-filter="revision" onclick="setFilter(this,'revision')">Due Today</button>
    <button class="filter-btn" data-filter="weak" onclick="setFilter(this,'weak')">Weak</button>
    <button class="filter-btn" data-filter="mastered" onclick="setFilter(this,'mastered')">Mastered</button>
  </div>
  <div id="topics-list"></div>
</div>

<!-- ===================================================
     VIEW: REVISION SESSION
=================================================== -->
<div id="view-revision" class="view">
  <div class="view-header">
    <div><div class="view-title">🔁 Spaced Revision</div><div class="view-subtitle">Science-backed automated spaced repetition</div></div>
    <div style="margin-left:auto"><button class="btn btn-primary" onclick="startRevisionSession()">▶ Start Session</button></div>
  </div>
  <div id="revision-content">
    <div class="rev-empty">
      <div class="rev-empty-icon">🎉</div>
      <div style="font-family:var(--font-head);font-size:18px;font-weight:800;margin-bottom:8px">All caught up!</div>
      <div style="color:var(--text-muted);font-size:14px;margin-bottom:20px">No topics due for review today. Keep studying new topics!</div>
      <button class="btn btn-primary" onclick="showView('folders')">Study New Topics</button>
    </div>
  </div>
</div>

<!-- ===================================================
     VIEW: ANALYTICS
=================================================== -->
<div id="view-analytics" class="view">
  <div class="view-header">
    <div><div class="view-title">📊 Analytics Dashboard</div><div class="view-subtitle">Track your progress and performance</div></div>
  </div>
  <div class="charts-row">
    <div class="chart-card"><h3>📊 Folder Progress</h3><canvas id="chart-subjects" height="200"></canvas></div>
    <div class="chart-card"><h3>🎯 Completion Overview</h3><canvas id="chart-pie" height="200"></canvas></div>
  </div>
  <div class="charts-row equal">
    <div class="chart-card"><h3>📈 Weekly Study Time (hrs)</h3><canvas id="chart-weekly" height="160"></canvas></div>
    <div class="chart-card"><h3>🔁 Revisions Due by Subject</h3><canvas id="chart-revisions" height="160"></canvas></div>
  </div>
  <div class="analytics-grid">
    <div class="ws-card"><div class="ws-title weak">⚠️ Weak Topics</div><div id="weak-topics-list"><div style="color:var(--text-dim);font-size:12px">Practice MCQs to identify weak areas</div></div></div>
    <div class="ws-card"><div class="ws-title strong">✅ Mastered Topics</div><div id="strong-topics-list"><div style="color:var(--text-dim);font-size:12px">Master topics to see them here</div></div></div>
  </div>
</div>

<!-- ═══ ANALYTICS RETENTION MAP ═══ -->
<div id="v4-analytics-ret-inject" style="display:none"></div>


<!-- ═══════════════════════════════════════════════════════════
     MEDICAL DIARY VIEW — MedTrack Pro v4
═══════════════════════════════════════════════════════════ -->
<div id="view-diary" class="view">

  <!-- DIARY HEADER -->
  <div class="diary-view-hdr">
    <div class="diary-view-hdr-left">
      <div class="diary-view-title">📔 Medical Study Diary</div>
      <div class="diary-view-date" id="dv-date-label"></div>
    </div>
    <div class="diary-view-hdr-right">
      <button class="dv-icon-btn" onclick="dvPrevDay()" title="Previous day">‹</button>
      <button class="dv-today-btn" onclick="dvGoToday()">Today</button>
      <button class="dv-icon-btn" onclick="dvNextDay()" title="Next day">›</button>
      <button class="btn btn-primary btn-sm" onclick="dvOpenAddTask()">+ Task</button>
    </div>
  </div>

  <!-- WEEK RIBBON -->
  <div class="diary-week-ribbon" id="dv-week-ribbon"></div>

  <!-- DAILY QUOTE -->
  <div class="diary-quote-bar" id="dv-quote-bar">
    <span class="diary-quote-icon">💬</span>
    <span class="diary-quote-text" id="dv-quote-text"></span>
    <span class="diary-quote-author" id="dv-quote-author"></span>
  </div>

  <!-- MAIN GRID: TIMELINE + SIDEBAR -->
  <div class="diary-main-grid">

    <!-- LEFT: NOTEBOOK TIMELINE -->
    <div class="diary-notebook" id="dv-notebook">

      <!-- Day title input -->
      <div class="dv-day-title-row">
        <div class="dv-day-badge" id="dv-day-badge"></div>
        <input class="dv-day-title-input" id="dv-day-title-input"
          placeholder="Today's Mission Statement..." oninput="dvSaveDayTitle()">
      </div>

      <!-- Progress bar -->
      <div class="dv-progress-row">
        <div class="dv-progress-track">
          <div class="dv-progress-fill" id="dv-progress-fill" style="width:0%"></div>
        </div>
        <div class="dv-progress-text" id="dv-progress-text">0 / 0 tasks</div>
      </div>

      <!-- MASTER AI PROMPT BAR -->
      <div id="dv-mega-bar"></div>

      <!-- Timeline -->
      <div class="dv-timeline" id="dv-timeline">
        <div class="dv-empty-state" id="dv-empty-state">
          <div class="dv-empty-icon">📋</div>
          <div class="dv-empty-title">No tasks for this day yet</div>
          <div class="dv-empty-sub">Click "+ Task" to add your first study mission</div>
          <button class="btn btn-primary btn-sm" onclick="dvOpenAddTask()" style="margin-top:12px">+ Add First Task</button>
        </div>
      </div>

    </div><!-- /diary-notebook -->

    <!-- RIGHT: SIDEBAR -->
    <div class="diary-sidebar">

      <!-- MASTER AI PROMPT SHORTCUT -->
      <div class="dsb-card" id="dv-sidebar-mega" style="background:linear-gradient(135deg,rgba(255,179,64,.06),rgba(0,210,190,.04));border-color:rgba(255,179,64,.25)">
        <div class="dsb-card-title" style="color:var(--yellow)">🚀 Master AI Prompt</div>
        <div style="font-size:11px;color:var(--text-muted);margin-bottom:10px">Generate full study file for all today's topics — paste into Claude</div>
        <button class="btn btn-sm" style="width:100%;background:linear-gradient(135deg,rgba(255,179,64,.15),rgba(0,210,190,.08));border:1px solid rgba(255,179,64,.35);color:var(--yellow);font-weight:700" onclick="dvCopyMegaPrompt()">🚀 Copy Mega Prompt</button>
        <div style="display:flex;gap:5px;flex-wrap:wrap;margin-top:7px">
          <button class="dv-qp-btn" onclick="dvCopyPromptType('recall')">🔁 Recall</button>
          <button class="dv-qp-btn" onclick="dvCopyPromptType('viva')">🩺 Viva</button>
          <button class="dv-qp-btn" onclick="dvCopyPromptType('mcq')">📝 MCQs</button>
          <button class="dv-qp-btn" onclick="dvCopyPromptType('summary')">📋 Summary</button>
        </div>
      </div>

      <!-- DAILY STATS CARD -->
      <div class="dsb-card">
        <div class="dsb-card-title">📊 Day Stats</div>
        <div class="dsb-stats-grid" id="dv-stats-grid"></div>
      </div>

      <!-- REFLECTION CARD -->
      <div class="dsb-card diary-ref-card" style="margin-top:0">
        <div class="dsb-card-title" style="color:var(--purple)">📔 Reflection</div>
        <div class="dv-ref-field">
          <div class="dv-ref-label">⚡ Energy</div>
          <div class="energy-row">
            <input type="range" min="1" max="10" value="7" class="energy-slider" id="dv-energy"
              oninput="document.getElementById('dv-energy-val').textContent=this.value;dvSaveReflection()">
            <div class="energy-val" id="dv-energy-val">7</div>
          </div>
        </div>
        <div class="dv-ref-field">
          <div class="dv-ref-label" style="color:var(--blue)">🎯 Focus</div>
          <div class="energy-row">
            <input type="range" min="1" max="10" value="7" class="energy-slider" id="dv-focus-score"
              oninput="document.getElementById('dv-focus-val').textContent=this.value;dvSaveReflection()" style="accent-color:var(--blue)">
            <div class="energy-val" id="dv-focus-val" style="color:var(--blue)">7</div>
          </div>
        </div>
        <div class="dv-ref-field">
          <div class="dv-ref-label">✅ Accomplished</div>
          <textarea class="ref-input" id="dv-accomplished" rows="2"
            placeholder="What I got done..." oninput="dvSaveReflection()"></textarea>
        </div>
        <div class="dv-ref-field">
          <div class="dv-ref-label">😤 Struggled with</div>
          <textarea class="ref-input" id="dv-struggled" rows="2"
            placeholder="Difficult concepts..." oninput="dvSaveReflection()"></textarea>
        </div>
        <div class="dv-ref-field">
          <div class="dv-ref-label">📅 Tomorrow's priorities</div>
          <textarea class="ref-input" id="dv-tomorrow" rows="2"
            placeholder="Top 3 focus areas..." oninput="dvSaveReflection()"></textarea>
        </div>
      </div>

      <!-- TOPICS SYNCED TODAY -->
      <div class="dsb-card">
        <div class="dsb-card-title">🔗 Topics Synced Today</div>
        <div id="dv-synced-topics" style="font-size:12px;color:var(--text-muted)">Complete tasks to sync topics</div>
      </div>

      <!-- UPCOMING REVISIONS -->
      <div class="dsb-card">
        <div class="dsb-card-title">🔔 Due for Review</div>
        <div id="dv-due-topics"></div>
      </div>

    </div><!-- /diary-sidebar -->

  </div><!-- /diary-main-grid -->

</div><!-- /view-diary -->

<!-- ═══ ADD TASK MODAL ═══ -->
<div class="modal-overlay" id="dv-add-task-overlay" onclick="if(event.target===this)dvCloseAddTask()">
  <div class="modal" style="max-width:520px">
    <button class="modal-close" onclick="dvCloseAddTask()">✕</button>
    <div class="modal-title">📋 Add Study Task</div>
    <div class="modal-subtitle">Link a task to a topic for automatic progress tracking</div>

    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px">
      <div>
        <div class="ref-label" style="margin-bottom:4px">⏰ Time</div>
        <input class="mini-input" id="dv-task-time" type="time" value="09:00" style="width:100%">
      </div>
      <div>
        <div class="ref-label" style="margin-bottom:4px">📌 Type</div>
        <select class="mini-input" id="dv-task-type" style="width:100%;background:var(--surface2);color:var(--text);border:1px solid var(--border);border-radius:var(--r-sm);padding:8px">
          <option value="study">📖 Study</option>
          <option value="revision">🔁 Revision</option>
          <option value="recall">🔁 Active Recall</option>
          <option value="mcq">📝 MCQs</option>
          <option value="clinical">🩺 Clinical</option>
          <option value="video">🎥 Watch Video</option>
          <option value="diagram">✏️ Draw Diagrams</option>
          <option value="notes">📝 Write Notes</option>
          <option value="break">☕ Break</option>
        </select>
      </div>
    </div>

    <div style="margin-bottom:12px">
      <div class="ref-label" style="margin-bottom:4px">📝 Task Title</div>
      <input class="mini-input" id="dv-task-title" placeholder="e.g. Read Brachial Plexus" style="width:100%">
    </div>

    <div style="margin-bottom:12px">
      <div class="ref-label" style="margin-bottom:4px">🔗 Link to Topic (for auto-sync)</div>
      <select class="mini-input" id="dv-task-topic" style="width:100%;background:var(--surface2);color:var(--text);border:1px solid var(--border);border-radius:var(--r-sm);padding:8px">
        <option value="">— No topic link —</option>
      </select>
    </div>

    <div style="margin-bottom:12px" id="dv-stage-select-wrap">
      <div class="ref-label" style="margin-bottom:4px">📋 Auto-update which stage on completion?</div>
      <select class="mini-input" id="dv-task-stage" style="width:100%;background:var(--surface2);color:var(--text);border:1px solid var(--border);border-radius:var(--r-sm);padding:8px">
        <option value="">— Don't update stage —</option>
        <option value="reading">📖 Reading</option>
        <option value="lectures">🎓 Lectures</option>
        <option value="notes">📝 Notes</option>
        <option value="mcqs">📝 MCQs</option>
        <option value="revision">🔁 Revision</option>
        <option value="mastered">🏆 Mastered</option>
      </select>
    </div>

    <div style="margin-bottom:12px">
      <div class="ref-label" style="margin-bottom:4px">✅ Micro-Steps <span style="color:var(--text-muted);font-weight:400">(one per line, optional)</span></div>
      <textarea class="ref-input" id="dv-task-microsteps" rows="4"
        placeholder="Read textbook&#10;Watch video&#10;Draw diagram&#10;Active recall&#10;Solve MCQs"></textarea>
    </div>

    <div style="margin-bottom:12px">
      <div class="ref-label" style="margin-bottom:4px">📝 Notes</div>
      <input class="mini-input" id="dv-task-notes" placeholder="Optional notes for this task" style="width:100%">
    </div>

    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="dvCloseAddTask()">Cancel</button>
      <button class="btn btn-primary" onclick="dvSaveTask()">💾 Add Task</button>
    </div>
  </div>
</div>

<!-- ===================================================
     MODALS
=================================================== -->

<!-- Move Topic -->
<div class="modal-overlay" id="move-topic-overlay" onclick="if(event.target===this)closeMoveTopic()">
  <div class="modal">
    <button class="modal-close" onclick="closeMoveTopic()">✕</button>
    <div class="modal-title">📦 Move Topic</div>
    <div class="modal-subtitle">Organize this topic into any folder or subfolder.</div>
    <div class="form-group">
      <label class="form-label">Destination Folder</label>
      <select class="form-select" id="move-topic-dest"></select>
    </div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeMoveTopic()">Cancel</button>
      <button class="btn btn-primary" onclick="confirmMoveTopic()">Move Topic</button>
    </div>
  </div>
</div>

<!-- Pomodoro -->
<div class="modal-overlay" id="pomo-overlay" onclick="if(event.target===this)closePomodoro()">
  <div class="modal">
    <button class="modal-close" onclick="closePomodoro()">✕</button>
    <div class="modal-title">🍅 Pomodoro Timer</div>
    <div class="modal-subtitle">Focus sessions for deep learning</div>
    <div class="pomo-display">
      <div class="pomo-ring">
        <svg viewBox="0 0 160 160">
          <circle class="ring-bg" cx="80" cy="80" r="70"/>
          <circle class="ring-fill" id="pomo-ring-fill" cx="80" cy="80" r="70" stroke-dasharray="439.8" stroke-dashoffset="0"/>
        </svg>
        <div class="pomo-inner">
          <div class="pomo-time" id="pomo-time">50:00</div>
          <div class="pomo-mode" id="pomo-mode">FOCUS</div>
        </div>
      </div>
      <div class="pomo-sessions" id="pomo-sessions">Sessions completed: 0 🍅</div>
      <div class="pomo-controls">
        <button class="pomo-btn" id="pomo-toggle" onclick="togglePomodoro()">▶ Start</button>
        <button class="pomo-btn secondary" onclick="resetPomodoro()">↺ Reset</button>
        <button class="pomo-btn secondary" onclick="skipPomodoroPhase()">⏭ Skip</button>
      </div>
    </div>
    <div style="margin-top:12px;font-size:12px;color:var(--text-muted);text-align:center">Work: 50 min → Short break: 10 min → Long break: 20 min (every 4 sessions)</div>
  </div>
</div>

<!-- JSON Import -->
<div class="modal-overlay" id="json-import-overlay" onclick="if(event.target===this)closeJsonImport()">
  <div class="modal modal-lg">
    <button class="modal-close" onclick="closeJsonImport()">✕</button>
    <div class="modal-title">📥 JSON Syllabus Import</div>
    <div class="modal-subtitle">Import structured syllabus in standardized JSON format</div>
    <div class="upload-zone" id="json-upload-zone" onclick="document.getElementById('json-file-input').click()" ondragover="handleJsonDragOver(event)" ondrop="handleJsonDrop(event)">
      <div class="upload-icon">📂</div>
      <div class="upload-text"><strong>Click or drag & drop</strong> your JSON syllabus</div>
      <div class="upload-hint">Only .json files accepted</div>
    </div>
    <input type="file" id="json-file-input" accept=".json" style="display:none" onchange="handleJsonFileSelect(event)">
    <div class="json-schema-hint">
<strong style="color:var(--primary)">Required JSON Schema:</strong><br>
{<br>
&nbsp;&nbsp;"folders": [<br>
&nbsp;&nbsp;&nbsp;&nbsp;{ "name": "Module 1", "subfolders": [{ "name": "Block A", "topics": [{ "title": "Cell Structure" }] }], "topics": [{ "title": "General Histology" }] }<br>
&nbsp;&nbsp;]<br>
}
    </div>
    <div class="merge-options">
      <div class="merge-option selected" id="opt-merge" onclick="selectMergeOpt('merge')">🔀 Merge<br><span style="font-size:10px;color:var(--text-muted)">Add to existing</span></div>
      <div class="merge-option" id="opt-replace" onclick="selectMergeOpt('replace')">🔄 Replace<br><span style="font-size:10px;color:var(--text-muted)">Clear and import</span></div>
    </div>
    <div class="import-log" id="import-log" style="display:none"></div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeJsonImport()">Cancel</button>
      <button class="btn btn-primary" id="import-btn" onclick="importJson()" disabled>Import Syllabus</button>
    </div>
  </div>
</div>

<!-- ★ UPGRADED DUAL-INPUT SCHEDULE IMPORT MODAL ★ -->
<div class="modal-overlay" id="schedule-import-overlay" onclick="if(event.target===this)closeScheduleImport()">
  <div class="modal modal-xl">
    <button class="modal-close" onclick="closeScheduleImport()">✕</button>
    <div class="modal-title">📔 Load Schedule into Diary</div>
    <div class="modal-subtitle">Load or paste an AI-generated JSON schedule — links automatically to your folders!</div>

    <!-- AI Prompt Copy -->
    <div style="background:var(--primary-dim);border:1px solid rgba(0,210,190,0.3);border-radius:var(--r-sm);padding:12px 14px;margin-bottom:16px;display:flex;align-items:flex-start;gap:10px;">
      <span style="font-size:20px;flex-shrink:0">🤖</span>
      <div>
        <div style="font-size:12px;font-weight:600;color:var(--primary);margin-bottom:4px">Step 1 — Ask AI to generate your schedule</div>
        <div style="font-size:11px;color:var(--text-muted);margin-bottom:8px">Copy this prompt → paste into Claude / ChatGPT → give it your timetable → get the JSON back → paste it below or upload the file</div>
        <button class="btn btn-secondary btn-sm" onclick="copySchedulePrompt()" id="sched-prompt-copy-btn">📋 Copy AI Prompt</button>
      </div>
    </div>

    <!-- DUAL INPUT: Side-by-Side -->
    <div style="font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:1px;color:var(--text-muted);margin-bottom:10px">Step 2 — Load Your Schedule JSON</div>
    <div class="dual-input-layout">
      <!-- LEFT: Drag & Drop -->
      <div class="dual-input-left">
        <div class="dual-input-label">📂 Upload File <span class="di-badge">Drag & Drop</span></div>
        <div class="upload-zone" id="sched-upload-zone" style="flex:1;min-height:160px;display:flex;flex-direction:column;align-items:center;justify-content:center;"
          onclick="document.getElementById('sched-file-input').click()"
          ondragover="handleSchedDragOver(event)"
          ondrop="handleSchedDrop(event)">
          <div class="upload-icon" style="font-size:32px;margin-bottom:8px">📂</div>
          <div class="upload-text" style="font-size:13px"><strong>Click or drag & drop</strong></div>
          <div class="upload-hint">.json files only</div>
        </div>
        <input type="file" id="sched-file-input" accept=".json" style="display:none" onchange="handleSchedFileSelect(event)">
      </div>

      <!-- DIVIDER -->
      <div class="dual-input-divider"></div>

      <!-- RIGHT: Paste Textarea -->
      <div class="dual-input-right">
        <div class="dual-input-label">✏️ Paste JSON <span class="di-badge">Live Validate</span></div>
        <textarea class="sched-paste-area" id="sched-paste-area"
          placeholder='{"date":"2025-01-20","dayTitle":"Renal Week Day 1","schedule":[{"id":"s1","time":"08:00 - 09:30","subject":"Physiology","topic":"GFR","type":"learn","emoji":"🧬","tags":["renal"],"difficulty":"deep"}]}'
          oninput="handleSchedPaste(this.value)"></textarea>
        <div class="sched-paste-status idle" id="sched-paste-status">Paste your JSON above to validate</div>
      </div>
    </div>

    <div class="schedule-schema-hint" id="sched-schema-display" style="display:none"></div>
    <div class="import-log" id="sched-import-log" style="display:none"></div>
    <div id="sched-preview-area" style="margin-top:12px"></div>

    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeScheduleImport()">Cancel</button>
      <button class="btn btn-primary" id="sched-import-btn" onclick="importScheduleToTrack()" disabled>📔 Import into Diary</button>
    </div>
  </div>
</div>

<!-- ★ MASTERY CHECK MODAL ★ -->
<div class="modal-overlay" id="mastery-check-overlay" onclick="if(event.target===this){}">
  <div class="modal">
    <button class="modal-close" onclick="closeMasteryCheck()">✕</button>
    <div class="modal-title">🏆 Mastery Verification</div>
    <div class="modal-subtitle">Science-based checklist before marking this topic as reviewed</div>
    <div class="mastery-topic-banner" id="mastery-topic-name">Topic Name</div>
    <div class="mastery-progress-track">
      <div class="mastery-pip" id="mpip-0"></div>
      <div class="mastery-pip" id="mpip-1"></div>
      <div class="mastery-pip" id="mpip-2"></div>
      <div class="mastery-pip" id="mpip-3"></div>
    </div>
    <div class="mastery-checklist">
      <label class="mastery-item">
        <input type="checkbox" id="mc-0" onchange="updateMasteryBtn()">
        <div class="mastery-item-content">
          <div class="mastery-item-title">🧠 Active Recall</div>
          <div class="mastery-item-desc">I attempted to recall the core mechanism before reviewing the text.</div>
        </div>
      </label>
      <label class="mastery-item">
        <input type="checkbox" id="mc-1" onchange="updateMasteryBtn()">
        <div class="mastery-item-content">
          <div class="mastery-item-title">🗣️ Feynman Check</div>
          <div class="mastery-item-desc">I can explain this pathway/diagnosis to a peer without using jargon.</div>
        </div>
      </label>
      <label class="mastery-item">
        <input type="checkbox" id="mc-2" onchange="updateMasteryBtn()">
        <div class="mastery-item-content">
          <div class="mastery-item-title">🏥 Clinical Correlation</div>
          <div class="mastery-item-desc">I have linked this topic to a specific clinical presentation or pathology.</div>
        </div>
      </label>
      <label class="mastery-item">
        <input type="checkbox" id="mc-3" onchange="updateMasteryBtn()">
        <div class="mastery-item-content">
          <div class="mastery-item-title">🔍 Metacognition</div>
          <div class="mastery-item-desc">I have identified exactly which part of this topic I found most difficult.</div>
        </div>
      </label>
    </div>
    <button class="btn btn-primary" id="mastery-submit-btn" onclick="submitMastery()" disabled>
      ✅ Submit Revision — Unlock Spaced Repetition
    </button>
  </div>
</div>

<!-- Auth Modal -->
<div class="auth-overlay" id="auth-overlay" onclick="if(event.target===this&&!currentUser)return;if(event.target===this)closeAuthModal()">
  <div class="auth-modal">
    <div class="auth-modal-title">☁️ Cloud Sync</div>
    <div class="auth-modal-sub" id="auth-modal-sub">Sign in with Google to sync your data across all your devices — PC, phone, tablet. Your data follows you everywhere.</div>
    <button class="google-btn" id="auth-google-btn" onclick="signInWithGoogle()">
      <svg class="google-logo" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
        <path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4"/>
        <path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853"/>
        <path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" fill="#FBBC05"/>
        <path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335"/>
      </svg>
      Continue with Google
    </button>
    <div id="auth-signout-section" style="display:none">
      <div style="background:var(--green-dim);border:1px solid rgba(6,255,165,0.2);border-radius:var(--r-sm);padding:12px;margin-bottom:12px;font-size:12px;color:var(--green)">✅ Signed in — your data syncs across all devices automatically!</div>
      <button class="auth-skip-btn" onclick="signOutUser()">Sign Out</button>
    </div>
    <button class="auth-skip-btn" id="auth-skip-btn" onclick="closeAuthModal()" style="margin-top:8px">Continue offline (this device only)</button>
    <div class="auth-note">🔒 Your data is private and only accessible by your Google account</div>
  </div>
</div>

<!-- Add Folder -->
<div class="modal-overlay" id="add-folder-overlay" onclick="if(event.target===this)closeAddFolder()">
  <div class="modal">
    <button class="modal-close" onclick="closeAddFolder()">✕</button>
    <div class="modal-title">📁 Add Root Folder</div>
    <div class="modal-subtitle">Create a new module or subject folder</div>
    <div class="form-group"><label class="form-label">Folder Name *</label><input class="form-input" id="add-folder-name" placeholder="e.g. Module 1 - Anatomy"></div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Icon</label><input class="form-input" id="add-folder-icon" placeholder="📁" style="font-size:20px"></div>
      <div class="form-group"><label class="form-label">Color</label><input type="color" class="form-input" id="add-folder-color" value="#00D2BE" style="height:42px;padding:4px"></div>
    </div>
    <div class="form-group"><label class="form-label">Label (e.g. MBBS-I)</label><input class="form-input" id="add-folder-label" placeholder="MBBS-I"></div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeAddFolder()">Cancel</button>
      <button class="btn btn-primary" onclick="saveAddFolder()">Create Folder</button>
    </div>
  </div>
</div>

<!-- Add Nested Folder -->
<div class="modal-overlay" id="add-subfolder-overlay" onclick="if(event.target===this)closeAddSubfolder()">
  <div class="modal">
    <button class="modal-close" onclick="closeAddSubfolder()">✕</button>
    <div class="modal-title">📂 Add Nested Folder</div>
    <div class="modal-subtitle" id="add-subfolder-subtitle">Add a section inside this folder</div>
    <div class="form-group"><label class="form-label">Subfolder Name *</label><input class="form-input" id="add-subfolder-name" placeholder="e.g. Block A / Prof A"></div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeAddSubfolder()">Cancel</button>
      <button class="btn btn-primary" onclick="saveAddSubfolder()">Create Folder</button>
    </div>
  </div>
</div>

<!-- Add Topic -->
<div class="modal-overlay" id="add-topic-overlay" onclick="if(event.target===this)closeAddTopic()">
  <div class="modal">
    <button class="modal-close" onclick="closeAddTopic()">✕</button>
    <div class="modal-title">📑 Add Topic</div>
    <div class="modal-subtitle">Add a topic to the current folder/subfolder</div>
    <div class="form-group"><label class="form-label">Topic Name *</label><input class="form-input" id="add-topic-name" placeholder="e.g. Cell Membrane Structure"></div>
    <div class="form-row">
      <div class="form-group"><label class="form-label">Difficulty</label><select class="form-select" id="add-topic-difficulty"><option value="easy">Easy</option><option value="medium" selected>Medium</option><option value="hard">Hard</option></select></div>
      <div class="form-group"><label class="form-label">Weightage (1-5)</label><input type="number" class="form-input" id="add-topic-weightage" min="1" max="5" value="2"></div>
    </div>
    <div class="form-group"><label class="form-label">Tags (comma separated)</label><input class="form-input" id="add-topic-tags" placeholder="anatomy, high-yield, clinical"></div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeAddTopic()">Cancel</button>
      <button class="btn btn-primary" onclick="saveAddTopic()">Add Topic</button>
    </div>
  </div>
</div>

<!-- Restore Backup -->
<div class="modal-overlay" id="restore-overlay" onclick="if(event.target===this)closeRestore()">
  <div class="modal">
    <button class="modal-close" onclick="closeRestore()">✕</button>
    <div class="modal-title">♻️ Restore Backup</div>
    <div class="modal-subtitle">Import previously exported MedTrack data</div>
    <div class="upload-zone" onclick="document.getElementById('restore-file-input').click()">
      <div class="upload-icon">📁</div>
      <div class="upload-text">Click to select backup .json file</div>
    </div>
    <input type="file" id="restore-file-input" accept=".json" style="display:none" onchange="handleRestoreFile(event)">
    <div id="restore-status" style="margin-top:12px;font-size:12px;color:var(--text-muted)"></div>
  </div>
</div>

<!-- Settings Modal -->
<div class="settings-overlay" id="settings-overlay" onclick="if(event.target===this)closeSettingsPanel()">
  <div class="settings-modal">
    <button class="modal-close" onclick="closeSettingsPanel()">✕</button>
    <div class="modal-title">⚙️ Settings & Data</div>
    <div class="modal-subtitle">Your data is seamlessly saved to the cloud automatically.</div>
    <div class="settings-section-title">💾 Local Data & Backup</div>
    <div style="display:flex;gap:8px;flex-wrap:wrap">
      <button class="btn btn-secondary btn-sm" onclick="exportBackup();closeSettingsPanel()">💾 Export Backup</button>
      <button class="btn btn-secondary btn-sm" onclick="closeSettingsPanel();document.getElementById('restore-overlay').classList.add('show')">♻️ Restore Backup</button>
      <button class="btn btn-danger btn-sm" onclick="dangerClearData()">⚠️ Clear All Data</button>
    </div>
    <div class="modal-footer"><button class="btn btn-secondary" onclick="closeSettingsPanel()">Close</button></div>
  </div>
</div>

<!-- Weekend Revision Session Modal -->
<div class="modal-overlay" id="weekend-session-overlay" onclick="if(event.target===this)closeWeekendSession()">
  <div class="modal modal-lg">
    <button class="modal-close" onclick="closeWeekendSession()">✕</button>
    <div class="modal-title">📅 Weekend Revision Session</div>
    <div class="modal-subtitle" id="wknd-session-subtitle">Consolidating this week's topics with spaced repetition</div>
    <div id="wknd-session-body"></div>
  </div>
</div>

<!-- Toast -->
<div id="toast-container"></div>


<script>
// ─────────────────────────────────────────────
//  FIREBASE — compat SDK (window.firebase)
// ─────────────────────────────────────────────
var firebaseConfig = {
  apiKey:"AIzaSyAHp57writGRYeSNP6MwTcKPi45mlaUQHw",
  authDomain:"medtrackdb.firebaseapp.com",
  projectId:"medtrackdb",
  storageBucket:"medtrackdb.firebasestorage.app",
  messagingSenderId:"745482457766",
  appId:"1:745482457766:web:4aef20c89fe4a323d21a67"
};
var fbApp=null, fbAuth=null, fbDb=null, currentUser=null, unsubSnap=null, firestoreSaveTimeout=null;

function initFirebase(){
  try{
    fbApp = firebase.initializeApp(firebaseConfig);
    fbAuth = firebase.auth();
    fbDb   = firebase.firestore();
    fbAuth.onAuthStateChanged(function(user){
      if(user){
        currentUser=user; updateNavAuth(user); updateSyncStatus('loading');
        var ref = fbDb.doc('artifacts/medtrack_v3/users/'+user.uid+'/medtrack/state');
        if(unsubSnap) unsubSnap();
        unsubSnap = ref.onSnapshot(function(snap){
          if(snap.exists){
            if(snap.metadata.hasPendingWrites) return;
            var rem = snap.data(); var d = rem.data ? JSON.parse(rem.data) : rem;
            var ls = state.dailySchedule;
            state = Object.assign({}, state, d);
            if(!state.dailySchedule||!state.dailySchedule.items) state.dailySchedule=ls;
            state.dailySchedule = state.dailySchedule||{date:null,dayTitle:'',items:[],completedIds:[],microChecks:{}};
            state.dailySchedule.microChecks = state.dailySchedule.microChecks||{};
            patchTree(state.folders);
            try{localStorage.setItem(APP_KEY,JSON.stringify(state));}catch(e){}
            initTimer(); updatePomodoroDisplay(); renderHome();
            if(currentView==='folders') renderFolders();
            if(currentView==='subfolder') renderSubfolderView();
            if(currentView==='revision') renderRevisionView();
            if(currentView==='analytics') renderAnalytics();
          }
          updateSyncStatus('synced');
        }, function(err){
          console.error('Firestore sync error',err);
          updateSyncStatus('error');
          showToast('⚠️ Sync error — working offline','warn');
        });
      } else {
        currentUser=null; updateNavAuth(null);
        if(unsubSnap){unsubSnap();unsubSnap=null;}
        updateSyncStatus('offline');
      }
    });
  }catch(e){
    console.warn('Firebase init failed (offline mode)',e);
    updateSyncStatus('offline');
  }
}

function saveToFirestore(){
  if(!currentUser||!fbDb) return;
  var ref = fbDb.doc('artifacts/medtrack_v3/users/'+currentUser.uid+'/medtrack/state');
  ref.set({data: JSON.stringify(state)})
    .then(function(){updateSyncStatus('synced');})
    .catch(function(err){
      console.error('Firestore save error',err);
      updateSyncStatus('error');
      showToast('❌ Cloud save failed — data saved locally','error');
    });
}

function updateSyncStatus(s){
  var el=document.getElementById('sync-status'); if(!el) return;
  var m={synced:{text:'☁️ Synced',cls:'synced'},syncing:{text:'🔄 Saving…',cls:'syncing'},error:{text:'⚠️ Error',cls:'error'},offline:{text:'📴 Offline',cls:'error'},loading:{text:'⬇️ Loading…',cls:'syncing'}};
  var v=m[s]||m.offline; el.innerHTML=v.text; el.className='sync-badge hide-mob '+v.cls;
}

// ─────────────────────────────────────────────
//  CONSTANTS
// ─────────────────────────────────────────────
var APP_KEY='medtrack_pro_v3_cmd';
var STAGE_KEYS=['lectures','reading','notes','mcq','mastered'];
var STAGE_LABELS={lectures:'Watch Lectures',reading:'Read Textbook',notes:'Make Notes',mcq:'Practice MCQs',mastered:'Topic Mastered'};
var STAGE_ICONS={lectures:'📺',reading:'📖',notes:'✏️',mcq:'📝',mastered:'🏆'};
var SR_INTERVALS=[1,3,7,14,30,60,90,180];
var MOTIVATIONAL_QUOTES=[
  {text:"The secret of getting ahead is getting started.",author:"Mark Twain"},
  {text:"Success is the sum of small efforts, repeated day in and day out.",author:"Robert Collier"},
  {text:"Medicine is not only a science; it is also an art.",author:"Paracelsus"},
  {text:"Every expert was once a beginner.",author:"Helen Hayes"},
  {text:"Wherever the art of medicine is loved, there is also a love of humanity.",author:"Hippocrates"},
  {text:"The art of medicine consists of keeping the patient amused while nature heals.",author:"Voltaire"}
];
var ISLAMIC_REMINDERS=[
  "وَقُلْ رَبِّ زِدْنِي عِلْمًا — \"And say: My Lord, increase me in knowledge.\" (20:114)",
  "اطلب العلم من المهد إلى اللحد — Seek knowledge from the cradle to the grave.",
  "Your healing hands will be sadaqah for every patient you help. Keep going."
];
var PROMPTS={
  concept:function(t){return 'Explain "'+t+'" for MBBS level in full detail.\n\nInclude:\n• Core concepts & definitions\n• Key mechanisms (step-by-step)\n• Clinical relevance\n• Common exam traps\n• High-yield points & mnemonics';},
  mcq:function(t){return 'Generate 50 MBBS/USMLE style MCQs for "'+t+'".\n\nInclude:\n• Clinical vignettes (≥30)\n• Detailed explanations for each answer\n• Mixed difficulty levels';},
  research:function(t){return 'Latest research and clinical guidelines on "'+t+'".\n\nInclude:\n• Landmark studies\n• Current guidelines (WHO, NICE, AHA)\n• Recent advances & clinical pearls';},
  recall:function(t){return 'Give me 20 active recall questions on "'+t+'" (no answers upfront).\n\nProgress: Basic → Application → Analysis. Include clinical scenarios.';},
  teach:function(t){return 'Teach "'+t+'" to a first-year MBBS student using Feynman technique.\n\nUse: simple analogies, step-by-step logic, clinical scenarios, top 5 must-remember points.';},
  htmlStudyFile:function(t){return 'You are an advanced Medical AI in MedTrack Pro.\n\nGenerate a COMPLETE INTERACTIVE HTML STUDY FILE for: "'+t+'"\n\nUse the FIKRI LEARNING SYSTEM:\n1. Conceptual Foundation\n2. Mechanism/Pathophysiology\n3. System Integration\n4. Clinical Lens\n5. Diagnostic Thinking\n6. Management Logic\n7. Exam Intelligence\n\nOutput: ONE complete self-contained HTML file. Dark UI (bg #070D1A, accent #00D2BE). 7 tabs: Fikri, Notes, MCQs, Recall, Cases, Revision, Schedule. Inline CSS+JS only. ZERO placeholder text.';}
};
var SCHEDULE_AI_PROMPT='You are an advanced Medical AI integrated into MedTrack Pro.\n\nGenerate a DAILY STUDY SCHEDULE. OUTPUT ONLY valid JSON — no markdown, no backticks.\n\nSchema:\n{\n  "date": "YYYY-MM-DD",\n  "hijriDate": "e.g. 15 Dhul Hijjah 1446",\n  "dayTitle": "Meaningful title",\n  "schedule": [\n    {\n      "id": "unique-slug",\n      "time": "08:00 - 09:30",\n      "subject": "Physiology",\n      "topic": "GFR and Filtration Barrier",\n      "type": "learn",\n      "emoji": "🧬",\n      "tags": ["physiology","renal"],\n      "difficulty": "deep",\n      "notes": "Focus on podocyte role",\n      "microSteps": ["Draw GFR diagram","Explain Bowman space","List Starling forces"]\n    }\n  ]\n}\n\nRULES: type ∈ {learn,revise,active-recall,mcq,clinical,break} · difficulty ∈ {light,moderate,deep} · microSteps: 2-4 short sub-tasks per slot · include breaks · max 6-7 hrs/day · hijriDate MANDATORY\n\nMy timetable/topics for today:\n[PASTE YOUR TIMETABLE HERE]';

// ─────────────────────────────────────────────
//  STATE
// ─────────────────────────────────────────────
var state={
  folders:[],
  streak:{current:0,best:0,lastDate:null},
  studyTimer:{todayElapsed:0,weeklyData:[0,0,0,0,0,0,0],totalElapsed:0,lastDate:null},
  heatmap:{},reminders:[],whyStarted:'',islamicOn:false,
  plannerTasks:[],revisionSession:{active:false},weeklyStudied:{},
  dailySchedule:{date:null,dayTitle:'',items:[],completedIds:[],microChecks:{}}
};

function patchTree(folders,parentId){
  parentId=parentId||null;
  folders.forEach(function(f){
    f.parentId=parentId; f.subfolders=f.subfolders||[]; f.topics=f.topics||[];
    f.topics.forEach(function(t){t.parentId=f.id; t.starred=t.starred||false;});
    patchTree(f.subfolders,f.id);
  });
}
function findFolder(id,list){
  list=list||state.folders;
  for(var i=0;i<list.length;i++){if(list[i].id===id)return list[i];var r=findFolder(id,list[i].subfolders);if(r)return r;}
  return null;
}
function findTopicRec(id,list){
  list=list||state.folders;
  for(var i=0;i<list.length;i++){
    var t=list[i].topics.find(function(x){return x.id===id;});
    if(t) return{topic:t,folder:list[i]};
    var r=findTopicRec(id,list[i].subfolders); if(r) return r;
  }
  return null;
}
function findTopic(id){return findTopicRec(id);}
function getAllTopics(list){
  list=list||state.folders; var all=[];
  for(var i=0;i<list.length;i++){list[i].topics.forEach(function(t){all.push({topic:t,folder:list[i]});});all=all.concat(getAllTopics(list[i].subfolders));}
  return all;
}
function getFolderTopicsRec(folder){
  var arr=folder.topics.slice();
  folder.subfolders.forEach(function(sf){arr=arr.concat(getFolderTopicsRec(sf));});
  return arr;
}
function saveState(){
  try{localStorage.setItem(APP_KEY,JSON.stringify(state));}catch(e){console.error('Local save failed',e);}
  if(currentUser){updateSyncStatus('syncing');clearTimeout(firestoreSaveTimeout);firestoreSaveTimeout=setTimeout(saveToFirestore,1500);}
}
function loadState(){
  try{
    var raw=localStorage.getItem(APP_KEY);
    if(raw){var s=JSON.parse(raw);state=Object.assign({},state,s);}
    state.folders=state.folders||[]; state.streak=state.streak||{current:0,best:0,lastDate:null};
    state.studyTimer=state.studyTimer||{todayElapsed:0,weeklyData:[0,0,0,0,0,0,0],totalElapsed:0,lastDate:null};
    state.heatmap=state.heatmap||{}; state.reminders=state.reminders||[];
    state.plannerTasks=state.plannerTasks||[]; state.weeklyStudied=state.weeklyStudied||{};
    state.dailySchedule=state.dailySchedule||{date:null,dayTitle:'',items:[],completedIds:[],microChecks:{}};
    state.dailySchedule.microChecks=state.dailySchedule.microChecks||{};
    patchTree(state.folders);
  }catch(e){console.error('Load failed',e);}
}
function createTopicData(title,parentId,opts){
  opts=opts||{};
  return{id:genId(),name:title,parentId:parentId,difficulty:opts.difficulty||'medium',weightage:opts.weightage||2,
    tags:opts.tags||[],starred:false,stages:{lectures:false,reading:false,notes:false,mcq:false,mastered:false},
    notes:'',resources:{book:'',video:''},links:[],interval:0,repetitions:0,
    lastReviewDate:null,nextReviewDate:null,mcqScore:null,weakFlag:false,createdAt:todayStr()};
}

// ─────────────────────────────────────────────
//  HELPERS
// ─────────────────────────────────────────────
function genId(){return Date.now().toString(36)+Math.random().toString(36).slice(2,5);}
function todayStr(){var d=new Date();return d.getFullYear()+'-'+pad(d.getMonth()+1)+'-'+pad(d.getDate());}
function pad(n){return String(n).padStart(2,'0');}
function addDays(ds,n){var d=new Date(ds+'T00:00:00');d.setDate(d.getDate()+n);return d.getFullYear()+'-'+pad(d.getMonth()+1)+'-'+pad(d.getDate());}
function fmtHrsMin(s){var h=Math.floor(s/3600),m=Math.floor((s%3600)/60);return h>0?h+'h '+m+'m':m+'m';}
function fmtSec(s){var h=Math.floor(s/3600),m=Math.floor((s%3600)/60),ss=s%60;return pad(h)+':'+pad(m)+':'+pad(ss);}
function getTopicCompletion(t){return Math.round((STAGE_KEYS.filter(function(k){return t.stages[k];}).length/STAGE_KEYS.length)*100);}
function calcNextInterval(t){return SR_INTERVALS[Math.min(t.repetitions||0,SR_INTERVALS.length-1)];}
function autoSpacedRepetition(topic){
  var reps=topic.repetitions||0,interval=SR_INTERVALS[Math.min(reps,SR_INTERVALS.length-1)],today=todayStr();
  topic.interval=interval; topic.repetitions=reps+1; topic.lastReviewDate=today; topic.nextReviewDate=addDays(today,interval);
  state.heatmap[today]=(state.heatmap[today]||0)+1; trackWeeklyStudied(topic.id); updateStreak(); saveState();
}
function getDueTopics(){var t=todayStr();return getAllTopics().filter(function(x){return x.topic.nextReviewDate&&x.topic.nextReviewDate<=t;});}
function getNextReviewLabel(topic){
  if(!topic.nextReviewDate) return null; var t=todayStr();
  if(topic.nextReviewDate<t) return{label:'Overdue',cls:'overdue'};
  if(topic.nextReviewDate===t) return{label:'Due Today',cls:'today'};
  var d=Math.round((new Date(topic.nextReviewDate)-new Date(t))/86400000);
  return{label:'In '+d+'d',cls:'future'};
}
function fuzzyMatch(str,pattern){
  str=str.toLowerCase(); pattern=pattern.toLowerCase();
  if(str.includes(pattern)) return true;
  var si=0;
  for(var i=0;i<str.length&&si<pattern.length;i++) if(str[i]===pattern[si]) si++;
  return si===pattern.length;
}
function getWeekKey(){
  var d=new Date(),jan4=new Date(d.getFullYear(),0,4);
  var doy=Math.floor((d-new Date(d.getFullYear(),0,1))/86400000)+1;
  return d.getFullYear()+'-W'+String(Math.ceil((doy+jan4.getDay())/7)).padStart(2,'0');
}
function isWeekend(){var d=new Date().getDay();return d===0||d===6;}
function trackWeeklyStudied(id){
  if(!id) return; var wk=getWeekKey();
  state.weeklyStudied=state.weeklyStudied||{};
  state.weeklyStudied[wk]=state.weeklyStudied[wk]||[];
  if(!state.weeklyStudied[wk].includes(id)) state.weeklyStudied[wk].push(id);
}
function getWeeklyStudiedTopics(){
  var wk=getWeekKey(),ids=state.weeklyStudied[wk]||[];
  if(!ids.length) return[];
  var set=new Set(ids);
  return getAllTopics().filter(function(x){return set.has(x.topic.id);});
}

// ─────────────────────────────────────────────
//  NAVIGATION
// ─────────────────────────────────────────────
var currentView='home', currentFolderId=null, topicFilter='all';
var NAV_TABS={home:'tab-home',folders:'tab-folders',revision:'tab-revision',diary:'tab-diary',analytics:'tab-analytics'};

function showView(view){
  document.querySelectorAll('.view').forEach(function(v){v.classList.remove('active');});
  document.querySelectorAll('.nav-tab').forEach(function(t){t.classList.remove('active');});
  var el=document.getElementById('view-'+view); if(el) el.classList.add('active');
  var tab=document.getElementById(NAV_TABS[view]); if(tab) tab.classList.add('active');
  currentView=view;
  try{
    if(view==='home') renderHome();
    else if(view==='folders'){currentFolderId=null;renderFolders();}
    else if(view==='revision') renderRevisionView();
    else if(view==='diary') dvRenderView();
    else if(view==='analytics') renderAnalytics();
  }catch(e){console.error('View render error',e);}
}
function navigateToFolder(id){
  currentFolderId=id; currentView='subfolder';
  document.querySelectorAll('.view').forEach(function(v){v.classList.remove('active');});
  document.getElementById('view-subfolder').classList.add('active');
  renderSubfolderView();
}
function goBackFromSubfolder(){
  var f=findFolder(currentFolderId);
  if(f&&f.parentId) navigateToFolder(f.parentId);
  else showView('folders');
}
function goToTopic(topicId){
  var r=findTopic(topicId); if(!r) return;
  navigateToFolder(r.topic.parentId||r.folder.id);
  setTimeout(function(){
    var body=document.getElementById('tb-'+topicId),icon=document.getElementById('exp-'+topicId);
    if(body&&!body.classList.contains('open')){body.classList.add('open');if(icon)icon.classList.add('open');}
    var card=document.getElementById('tc-'+topicId);
    if(card) card.scrollIntoView({behavior:'smooth',block:'center'});
  },150);
}

// ─────────────────────────────────────────────
//  HOME RENDER
// ─────────────────────────────────────────────
function renderHome(){
  try{renderBoosterPanel();}catch(e){}
  try{renderHomeStats();}catch(e){}
  try{renderDueToday();}catch(e){}
  try{renderWeekendRevision();}catch(e){}
  try{renderHeatmap();}catch(e){}
  try{renderPlanner();}catch(e){}
  try{renderStreakUI();}catch(e){}
  try{renderReminders();}catch(e){}
  try{renderDailySchedule();}catch(e){}
  var ws=document.getElementById('why-started-input'); if(ws) ws.value=state.whyStarted||'';
  var tog=document.getElementById('islamic-toggle'); if(tog) tog.classList.toggle('on',!!state.islamicOn);
  if(state.islamicOn) showIslamicReminder();
}
function renderBoosterPanel(){
  var h=new Date().getHours(), greet=h<12?'Good morning':h<17?'Good afternoon':'Good evening';
  var el=document.getElementById('booster-greeting'); if(el) el.textContent=greet+', Doctor in training';
  var idx=Math.floor(Date.now()/86400000)%MOTIVATIONAL_QUOTES.length, q=MOTIVATIONAL_QUOTES[idx];
  var qt=document.getElementById('daily-quote'); if(qt) qt.textContent=q.text;
  var qa=document.getElementById('daily-quote-author'); if(qa) qa.textContent='— '+q.author;
  var actions=['dominate','conquer','excel at','master','crush'];
  var ba=document.getElementById('booster-action'); if(ba) ba.textContent=actions[Math.floor(Date.now()/86400000)%actions.length];
}
function renderHomeStats(){
  var all=getAllTopics(),total=all.length,mastered=all.filter(function(x){return x.topic.stages.mastered;}).length;
  var due=getDueTopics().length,pct=total?Math.round((mastered/total)*100):0;
  var grid=document.getElementById('home-stats-grid'); if(!grid) return;
  grid.innerHTML='<div class="stat-card c-teal"><div class="stat-label">Total Topics</div><div class="stat-value">'+total+'</div><div class="stat-sub">in all folders</div><div class="stat-icon">📚</div></div>'+
    '<div class="stat-card c-green"><div class="stat-label">Mastered</div><div class="stat-value">'+mastered+'</div><div class="stat-sub">'+pct+'% complete</div><div class="stat-icon">🏆</div></div>'+
    '<div class="stat-card c-red"><div class="stat-label">Due Today</div><div class="stat-value">'+due+'</div><div class="stat-sub">need review</div><div class="stat-icon">🔔</div></div>'+
    '<div class="stat-card c-blue"><div class="stat-label">Folders</div><div class="stat-value">'+state.folders.length+'</div><div class="stat-sub">root subjects</div><div class="stat-icon">📁</div></div>'+
    '<div class="stat-card c-yellow"><div class="stat-label">Streak</div><div class="stat-value">'+state.streak.current+'</div><div class="stat-sub">days</div><div class="stat-icon">🔥</div></div>'+
    '<div class="stat-card c-purple"><div class="stat-label">Today Time</div><div class="stat-value">'+fmtHrsMin(timerElapsed)+'</div><div class="stat-sub">studied</div><div class="stat-icon">⏱</div></div>';
}
function updateStreak(){
  var today=todayStr(); if(state.streak.lastDate===today) return;
  if(!state.streak.lastDate||state.streak.lastDate===addDays(today,-1)) state.streak.current++;
  else state.streak.current=1;
  if(state.streak.current>state.streak.best) state.streak.best=state.streak.current;
  state.streak.lastDate=today; saveState(); renderStreakUI();
}
function renderStreakUI(){
  var sn=document.getElementById('streak-number'); if(sn) sn.textContent=state.streak.current;
  var sb=document.getElementById('streak-best'); if(sb) sb.textContent=state.streak.best;
}
function renderReminders(){
  var list=document.getElementById('reminders-list'); if(!list) return;
  if(!state.reminders||!state.reminders.length){list.innerHTML='<div style="color:var(--text-muted);font-size:12px;text-align:center;padding:10px 0">No reminders yet</div>';return;}
  list.innerHTML=state.reminders.map(function(r,i){return '<div class="reminder-item"><span style="font-size:16px">⏰</span><span style="flex:1">'+r.text+'</span><button class="del-btn" onclick="deleteReminder('+i+')">✕</button></div>';}).join('');
}
function addReminder(){
  var inp=document.getElementById('reminder-input'); if(!inp||!inp.value.trim()) return;
  state.reminders=state.reminders||[]; state.reminders.push({text:inp.value.trim(),createdAt:todayStr()});
  inp.value=''; saveState(); renderReminders();
}
function deleteReminder(i){state.reminders.splice(i,1); saveState(); renderReminders();}
function saveWhyStarted(){var el=document.getElementById('why-started-input');if(el){state.whyStarted=el.value;saveState();}}
function toggleIslamic(){
  state.islamicOn=!state.islamicOn;
  var tog=document.getElementById('islamic-toggle'); if(tog) tog.classList.toggle('on',state.islamicOn);
  if(state.islamicOn) showIslamicReminder();
  else{var el=document.getElementById('islamic-reminder-text');if(el)el.style.display='none';}
  saveState();
}
function showIslamicReminder(){
  var el=document.getElementById('islamic-reminder-text'); if(!el) return;
  el.textContent=ISLAMIC_REMINDERS[Math.floor(Date.now()/86400000)%ISLAMIC_REMINDERS.length];
  el.style.display='inline';
}
function renderDueToday(){
  var due=getDueTopics(),cnt=document.getElementById('due-today-count'),list=document.getElementById('due-today-list');
  if(cnt) cnt.textContent=due.length; if(!list) return;
  if(!due.length){list.innerHTML='<div style="color:var(--text-muted);font-size:12px">All caught up! No reviews due today 🎉</div>';return;}
  var today=todayStr();
  list.innerHTML=due.slice(0,12).map(function(x){
    var t=x.topic,f=x.folder,nm=t.name.length>30?t.name.slice(0,28)+'…':t.name;
    return '<div class="due-topic-pill '+(t.nextReviewDate<today?'overdue':'')+'" onclick="goToTopic(\''+t.id+'\')">'+nm+' <span style="font-size:10px;opacity:0.7">'+f.name+'</span></div>';
  }).join('');
  if(due.length>12) list.innerHTML+='<div class="due-topic-pill" onclick="startRevisionSession()">+'+(due.length-12)+' more →</div>';
}
function renderHeatmap(){
  var grid=document.getElementById('heatmap-grid'); if(!grid) return;
  var weeks=26,today=new Date(),startDate=new Date(today);
  startDate.setDate(today.getDate()-(weeks*7)+(7-today.getDay()));
  var html='<div class="heatmap-grid">';
  for(var w=0;w<weeks;w++){
    html+='<div class="heatmap-week">';
    for(var d=0;d<7;d++){
      var cur=new Date(startDate); cur.setDate(startDate.getDate()+w*7+d);
      var ds=cur.getFullYear()+'-'+pad(cur.getMonth()+1)+'-'+pad(cur.getDate());
      var c=state.heatmap[ds]||0, lv=c===0?0:c<=2?1:c<=5?2:c<=10?3:4;
      html+='<div class="heatmap-cell" data-level="'+lv+'" title="'+ds+': '+c+' reviews"></div>';
    }
    html+='</div>';
  }
  grid.innerHTML=html+'</div>';
  document.querySelectorAll('.heatmap-legend-cell[data-level]').forEach(function(c){
    var lvl=c.getAttribute('data-level');
    var cols={'1':'rgba(0,210,190,0.2)','2':'rgba(0,210,190,0.4)','3':'rgba(0,210,190,0.65)','4':'#00D2BE'};
    c.style.background=cols[lvl]||'var(--surface3)';
  });
}
function renderPlanner(){
  var list=document.getElementById('planner-list'); if(!list) return;
  var today=todayStr(), tasks=(state.plannerTasks||[]).filter(function(t){return t.date===today;});
  if(!tasks.length){list.innerHTML='<div style="color:var(--text-muted);font-size:12px;padding:8px 0">No tasks yet. Add one below!</div>';return;}
  list.innerHTML=tasks.map(function(t){return '<div class="planner-item"><input type="checkbox" class="planner-cb" '+(t.done?'checked':'')+' onchange="toggleTask(\''+t.id+'\',this.checked)"><span class="planner-text '+(t.done?'done':'')+'">'+t.text+'</span><button class="planner-del" onclick="deleteTask(\''+t.id+'\')">✕</button></div>';}).join('');
}
function addPlannerTask(){
  var inp=document.getElementById('planner-input'); if(!inp||!inp.value.trim()) return;
  state.plannerTasks=state.plannerTasks||[];
  state.plannerTasks.push({id:genId(),text:inp.value.trim(),done:false,date:todayStr()});
  inp.value=''; saveState(); renderPlanner();
}
function toggleTask(id,done){var t=(state.plannerTasks||[]).find(function(t){return t.id===id;});if(t){t.done=done;saveState();renderPlanner();}}
function deleteTask(id){state.plannerTasks=(state.plannerTasks||[]).filter(function(t){return t.id!==id;});saveState();renderPlanner();}

// ─────────────────────────────────────────────
//  FOLDERS VIEW
// ─────────────────────────────────────────────
function renderFolders(){
  var grid=document.getElementById('folders-grid'); if(!grid) return;
  var today=todayStr();
  if(!state.folders.length){grid.innerHTML='<div class="empty-state" style="grid-column:1/-1"><div class="empty-icon">📁</div><div class="empty-text">No folders yet. Create one or import a JSON syllabus!</div><button class="btn btn-primary" onclick="openAddFolder()">+ Create Root Folder</button></div>';return;}
  grid.innerHTML=state.folders.map(function(f){
    var all=getFolderTopicsRec(f),total=all.length,mastered=all.filter(function(t){return t.stages.mastered;}).length;
    var pct=total?Math.round((mastered/total)*100):0,due=all.filter(function(t){return t.nextReviewDate&&t.nextReviewDate<=today;}).length;
    return '<div class="folder-card" onclick="navigateToFolder(\''+f.id+'\')">'+
      '<div class="folder-card-top"><span class="folder-icon">'+f.icon+'</span><span class="folder-badge">'+(f.label||'')+'</span>'+
      '<div class="folder-actions"><button class="icon-btn" onclick="event.stopPropagation();openEditFolder(\''+f.id+'\')" title="Edit">✏️</button>'+
      '<button class="icon-btn danger" onclick="event.stopPropagation();deleteFolder(\''+f.id+'\')" title="Delete">🗑️</button></div></div>'+
      '<div class="folder-name">'+f.name+'</div>'+
      '<div class="folder-stats"><span>📑 '+total+' nested topics</span></div>'+
      '<div class="folder-progress-bar"><div class="folder-progress-fill" style="width:'+pct+'%;background:'+(f.color||'var(--primary)')+'"></div></div>'+
      '<div class="folder-progress-text"><span>'+mastered+'/'+total+' mastered</span><span>'+pct+'%</span></div>'+
      '<div class="due-badge '+(due===0?'none':'')+'">'+( due>0?'🔔 '+due+' due today':'✅ All caught up')+'</div></div>';
  }).join('');
}
function renderSubfolderView(){
  var folder=findFolder(currentFolderId); if(!folder){showView('folders');return;}
  var titleEl=document.getElementById('subfolder-view-title'),bcEl=document.getElementById('subfolder-breadcrumb');
  if(titleEl) titleEl.textContent=folder.icon+' '+folder.name;
  var crumbs=[],cur=folder;
  while(cur){crumbs.unshift(cur);cur=cur.parentId?findFolder(cur.parentId):null;}
  if(bcEl) bcEl.innerHTML='<span class="breadcrumb-item" onclick="showView(\'folders\')">📁 Root Folders</span>'+crumbs.map(function(c,i){return '<span class="breadcrumb-sep">›</span><span class="breadcrumb-item '+(i===crumbs.length-1?'active':'')+'" onclick="navigateToFolder(\''+c.id+'\')">'+(c.icon||'📂')+' '+c.name+'</span>';}).join('');
  renderSubfolderList(folder);renderTopicList(folder.topics);
}
function renderSubfolderList(folder){
  var container=document.getElementById('subfolder-list-container'); if(!container) return;
  if(!folder.subfolders||!folder.subfolders.length){container.innerHTML='';return;}
  var today=todayStr();
  container.innerHTML='<div class="section-title">📂 Nested Sections</div><div class="subfolder-list">'+
    folder.subfolders.map(function(sf){
      var all=getFolderTopicsRec(sf),total=all.length,mastered=all.filter(function(t){return t.stages.mastered;}).length;
      var due=all.filter(function(t){return t.nextReviewDate&&t.nextReviewDate<=today;}).length;
      return '<div class="subfolder-item" onclick="navigateToFolder(\''+sf.id+'\')"><span style="font-size:20px">📂</span><div style="flex:1"><div style="font-family:var(--font-head);font-size:13px;font-weight:700">'+sf.name+'</div><div style="font-size:11px;color:var(--text-muted)">'+total+' topics · '+mastered+' mastered'+(due>0?' · 🔔 '+due+' due':'')+'</div></div><button class="icon-btn danger" style="margin-right:8px" onclick="event.stopPropagation();deleteFolder(\''+sf.id+'\')" title="Delete">🗑️</button><span class="subfolder-arrow">›</span></div>';
    }).join('')+'</div><div class="section-title" style="margin-top:16px">📑 Topics in '+folder.name+'</div>';
}

// ─────────────────────────────────────────────
//  TOPIC RENDERING
// ─────────────────────────────────────────────
function renderTopicList(topics){
  var list=document.getElementById('topics-list'); if(!list) return;
  if(!topics||!topics.length){list.innerHTML='<div class="empty-state"><div class="empty-icon">📑</div><div class="empty-text">No topics in this folder yet.</div><button class="btn btn-primary" onclick="openAddTopic()">+ Add First Topic</button></div>';return;}
  list.innerHTML=getFilteredTopics(topics).map(function(t){return buildTopicCard(t);}).join('');
}
function getFilteredTopics(topics){
  var q=(document.getElementById('topic-search')?document.getElementById('topic-search').value:'').toLowerCase().trim();
  var today=todayStr();
  return topics.filter(function(t){
    if(q&&!fuzzyMatch(t.name,q)) return false;
    if(topicFilter==='starred') return t.starred;
    if(topicFilter==='incomplete') return getTopicCompletion(t)<100;
    if(topicFilter==='revision') return t.nextReviewDate&&t.nextReviewDate<=today;
    if(topicFilter==='weak') return t.weakFlag;
    if(topicFilter==='mastered') return t.stages.mastered;
    return true;
  });
}
function buildTopicCard(topic){
  var pct=getTopicCompletion(topic),circ=100.5,dashoffset=circ*(1-pct/100);
  var revInfo=getNextReviewLabel(topic),diff=topic.difficulty||'medium',wt=topic.weightage||2;
  return '<div class="topic-card '+(topic.stages.mastered?'mastered':'')+' '+(topic.weakFlag?'weak':'')+'" id="tc-'+topic.id+'">'+
    '<div class="topic-card-header" onclick="toggleTopic(\''+topic.id+'\')">'+
    '<div class="topic-completion-ring"><svg viewBox="0 0 36 36"><circle class="ring-bg" cx="18" cy="18" r="16"/><circle class="ring-fill" cx="18" cy="18" r="16" stroke-dasharray="'+circ+'" stroke-dashoffset="'+dashoffset+'" style="stroke:'+(pct===100?'var(--green)':'var(--primary)')+'"/></svg><div class="topic-ring-pct">'+pct+'%</div></div>'+
    '<div class="topic-info"><div class="topic-name">'+topic.name+'</div>'+
    '<div class="topic-meta">'+
      (topic.stages.mastered?'<span style="color:var(--green)">✅ Mastered</span>':'')+
      (topic.stages.reading?'':'<span>📖 Not read</span>')+
      (topic.notes?'<span>📝 Notes</span>':'')+
      (topic.links&&topic.links.length?'<span>🔗 '+topic.links.length+' links</span>':'')+
    '</div></div>'+
    '<div class="topic-right">'+
      '<span class="star-btn '+(topic.starred?'starred':'')+'" onclick="event.stopPropagation();toggleStar(\''+topic.id+'\')">'+(topic.starred?'⭐':'☆')+'</span>'+
      '<span class="diff-badge diff-'+diff+'">'+diff+'</span>'+
      (wt>1?'<span class="wt-badge">★'+wt+'</span>':'')+
      (revInfo?'<span class="next-rev-badge '+revInfo.cls+'">'+revInfo.label+'</span>':'')+
      '<span class="topic-expand-icon" id="exp-'+topic.id+'">▶</span>'+
    '</div></div>'+
    '<div class="topic-body" id="tb-'+topic.id+'">'+buildTopicBody(topic)+'</div>'+
  '</div>';
}
function buildTopicBody(topic){
  var stagesHtml=STAGE_KEYS.map(function(k){return '<div class="stage-item '+(topic.stages[k]?'done':'')+' '+(k==='mastered'?'stage-mastered':'')+'" onclick="toggleStage(\''+topic.id+'\',\''+k+'\')"><div class="stage-checkbox">'+(topic.stages[k]?'✓':'')+'</div><span class="stage-label">'+STAGE_ICONS[k]+' '+STAGE_LABELS[k]+'</span></div>';}).join('');
  var sm2Html='<div class="sm2-panel"><div class="sm2-info"><span>Interval: <b>'+(topic.interval||0)+'d</b></span><span>Reps: <b>'+(topic.repetitions||0)+'</b></span>'+(topic.lastReviewDate?'<span>Last: <b>'+topic.lastReviewDate+'</b></span>':'')+'</div><div class="sm2-buttons"><button class="sm2-btn" style="width:100%" onclick="openMasteryCheck(\''+topic.id+'\')">🏆 Mastery Checklist — Mark Reviewed<br><small>4-point science check · Unlocks spaced repetition · Next: '+calcNextInterval(topic)+'d</small></button></div></div>';
  var notesHtml='<textarea class="notes-textarea" placeholder="Your notes on this topic..." oninput="saveTopic(\''+topic.id+'\',\'notes\',this.value)">'+(topic.notes||'')+'</textarea>';
  var mcqHtml='<div style="display:flex;align-items:center;gap:10px;margin-top:8px;flex-wrap:wrap"><span style="font-size:12px;color:var(--text-muted)">MCQ Score:</span><input type="number" min="0" max="100" value="'+(topic.mcqScore||'')+'" placeholder="%" style="width:70px;background:var(--surface2);border:1px solid var(--border);border-radius:var(--r-sm);padding:4px 8px;color:var(--text);font-size:12px;outline:none" oninput="saveTopic(\''+topic.id+'\',\'mcqScore\',+this.value||null)"><button class="btn btn-ghost btn-sm" onclick="toggleWeak(\''+topic.id+'\')">⚠️ '+(topic.weakFlag?'Remove Weak Flag':'Flag as Weak')+'</button><button class="btn btn-secondary btn-sm" onclick="openMoveTopic(\''+topic.id+'\')" style="margin-left:auto">📦 Move</button></div>';
  var linksHtml=(topic.links||[]).map(function(l,i){return '<div class="resource-link-item"><a href="'+l.url+'" target="_blank">'+(l.title||l.url)+'</a><button class="del-link-btn" onclick="removeLink(\''+topic.id+'\','+i+')">✕</button></div>';}).join('');
  var resourcesHtml='<div class="resources-grid">'+
    '<div class="resource-input-group"><label class="resource-label">📗 Textbook Ref</label><input class="resource-input" placeholder="e.g. Snell\'s, pg 142" value="'+(topic.resources&&topic.resources.book?topic.resources.book:'')+'" oninput="saveTopic(\''+topic.id+'\',\'resources.book\',this.value)"></div>'+
    '<div class="resource-input-group"><label class="resource-label">🎥 Video Ref</label><input class="resource-input" placeholder="e.g. Armando YouTube" value="'+(topic.resources&&topic.resources.video?topic.resources.video:'')+'" oninput="saveTopic(\''+topic.id+'\',\'resources.video\',this.value)"></div>'+
    '</div><div class="resource-links-list">'+linksHtml+'</div>'+
    '<div class="add-resource-row"><input class="resource-input" id="link-url-'+topic.id+'" placeholder="https://..." style="flex:2"><input class="resource-input" id="link-title-'+topic.id+'" placeholder="Title" style="flex:1"><button class="add-res-btn" onclick="addLink(\''+topic.id+'\')">+ Add Link</button></div>';
  var promptTypes=[['concept','🧠 Explain','Deep concept explanation'],['mcq','📝 MCQs','Generate 50 practice MCQs'],['research','🔬 Research','Latest studies & guidelines'],['recall','🔁 Recall','20 active recall questions'],['teach','🗣️ Teach Me','Feynman technique']];
  var promptsHtml=promptTypes.map(function(p){return '<button class="prompt-btn" onclick="copyPrompt(\''+topic.id+'\',\''+p[0]+'\')"><div class="prompt-btn-title">'+p[1]+' <span class="copy-indicator" id="ci-'+topic.id+'-'+p[0]+'">✓ Copied!</span></div><div class="prompt-btn-sub">'+p[2]+'</div></button>';}).join('');
  var genHtml='<button class="prompt-btn gen-html" onclick="copyPrompt(\''+topic.id+'\',\'htmlStudyFile\')"><span style="font-size:26px">✨</span><div><div class="prompt-btn-title">Generate Full Study File <span class="copy-indicator" id="ci-'+topic.id+'-htmlStudyFile">✓ Copied!</span></div><div class="prompt-btn-sub">FIKRI system · 7 tabs · MCQs · Cases · Recall</div></div></button>';
  return '<div class="topic-section-title">📋 Study Stages</div><div class="stages-grid">'+stagesHtml+'</div>'+
    '<div class="topic-section-title">🔁 Automated Spaced Repetition</div>'+sm2Html+
    '<div class="topic-section-title">📝 Notes & Links</div>'+notesHtml+mcqHtml+'<div style="margin-top:12px"></div>'+resourcesHtml+
    '<div class="topic-section-title">🤖 AI Study Prompts</div><div class="prompts-grid">'+promptsHtml+genHtml+'</div>'+
    '<div style="margin-top:12px;text-align:right"><button class="btn btn-danger btn-sm" onclick="deleteTopic(\''+topic.id+'\')">🗑️ Delete Topic</button></div>';
}

// ─────────────────────────────────────────────
//  TOPIC ACTIONS
// ─────────────────────────────────────────────
function toggleTopic(id){
  var body=document.getElementById('tb-'+id),icon=document.getElementById('exp-'+id); if(!body) return;
  body.classList.toggle('open'); if(icon) icon.classList.toggle('open');
}
function toggleStage(topicId,stageKey){
  var r=findTopic(topicId); if(!r) return;
  r.topic.stages[stageKey]=!r.topic.stages[stageKey];
  if(r.topic.stages[stageKey]) trackWeeklyStudied(topicId);
  if(r.topic.stages.mastered&&!r.topic.nextReviewDate){r.topic.nextReviewDate=addDays(todayStr(),1);r.topic.lastReviewDate=todayStr();state.heatmap[todayStr()]=(state.heatmap[todayStr()]||0)+1;updateStreak();}
  saveState(); refreshTopicCard(topicId);
  // Reverse-sync: update diary pills if diary view is open
  try{if(currentView==='diary') dvRefreshStagePills(topicId);}catch(e){}
}
function toggleStar(topicId){
  var r=findTopic(topicId); if(!r) return;
  r.topic.starred=!r.topic.starred; saveState(); refreshTopicCard(topicId);
  showToast(r.topic.starred?'⭐ Topic starred':'☆ Star removed','info');
}
function toggleWeak(topicId){
  var r=findTopic(topicId); if(!r) return;
  r.topic.weakFlag=!r.topic.weakFlag; saveState(); refreshTopicCard(topicId);
  showToast(r.topic.weakFlag?'⚠️ Marked as weak':'✅ Weak flag removed','info');
}
function saveTopic(topicId,field,value){
  var r=findTopic(topicId); if(!r) return;
  if(field.indexOf('.')!==-1){var parts=field.split('.');r.topic[parts[0]]=r.topic[parts[0]]||{};r.topic[parts[0]][parts[1]]=value;}
  else r.topic[field]=value;
  saveState();
}
function addLink(topicId){
  var urlEl=document.getElementById('link-url-'+topicId),titleEl=document.getElementById('link-title-'+topicId);
  if(!urlEl||!urlEl.value.trim()) return;
  var r=findTopic(topicId); if(!r) return;
  r.topic.links=r.topic.links||[]; r.topic.links.push({url:urlEl.value.trim(),title:titleEl?titleEl.value.trim():''});
  urlEl.value=''; if(titleEl) titleEl.value=''; saveState(); refreshTopicCard(topicId);
}
function removeLink(topicId,idx){var r=findTopic(topicId);if(!r)return;r.topic.links.splice(idx,1);saveState();refreshTopicCard(topicId);}
function deleteTopic(topicId){
  if(!confirm('Delete this topic? This cannot be undone.')) return;
  var r=findTopic(topicId); if(!r) return;
  r.folder.topics=r.folder.topics.filter(function(t){return t.id!==topicId;});
  saveState(); if(currentView==='subfolder') renderSubfolderView(); showToast('Topic deleted','warn');
}
function refreshTopicCard(topicId){
  var card=document.getElementById('tc-'+topicId),body=document.getElementById('tb-'+topicId);
  if(!card||!body) return;
  var r=findTopic(topicId); if(!r) return;
  var isOpen=body.classList.contains('open');
  var tmp=document.createElement('div'); tmp.innerHTML=buildTopicCard(r.topic);
  card.replaceWith(tmp.firstElementChild);
  if(isOpen){var nb=document.getElementById('tb-'+topicId),ni=document.getElementById('exp-'+topicId);if(nb)nb.classList.add('open');if(ni)ni.classList.add('open');}
}
function copyPrompt(topicId,type){
  var r=findTopic(topicId); if(!r) return;
  var text=PROMPTS[type](r.topic.name);
  navigator.clipboard.writeText(text).then(function(){
    var ci=document.getElementById('ci-'+topicId+'-'+type);
    if(ci){ci.classList.add('show');setTimeout(function(){ci.classList.remove('show');},2000);}
    showToast('📋 Prompt copied!','success');
  }).catch(function(){showToast('Copy failed — try again','error');});
}
function filterTopics(){var f=findFolder(currentFolderId);if(f)renderTopicList(f.topics);}
function setFilter(btn,filter){
  topicFilter=filter;
  document.querySelectorAll('.filter-btn').forEach(function(b){b.classList.remove('active');});
  btn.classList.add('active'); filterTopics();
}

// ─────────────────────────────────────────────
//  MASTERY CHECK
// ─────────────────────────────────────────────
var _masteryPendingTopicId=null;
function openMasteryCheck(topicId){
  var r=findTopic(topicId); if(!r) return;
  _masteryPendingTopicId=topicId;
  for(var i=0;i<4;i++){
    var cb=document.getElementById('mc-'+i); if(cb) cb.checked=false;
    var pip=document.getElementById('mpip-'+i); if(pip) pip.classList.remove('lit');
  }
  var nameEl=document.getElementById('mastery-topic-name'); if(nameEl) nameEl.textContent=r.topic.name;
  var btn=document.getElementById('mastery-submit-btn'); if(btn) btn.disabled=true;
  document.getElementById('mastery-check-overlay').classList.add('show');
}
function closeMasteryCheck(){document.getElementById('mastery-check-overlay').classList.remove('show');_masteryPendingTopicId=null;}
function updateMasteryBtn(){
  var all=true;
  for(var i=0;i<4;i++){if(!document.getElementById('mc-'+i)||!document.getElementById('mc-'+i).checked){all=false;}var pip=document.getElementById('mpip-'+i);if(pip)pip.classList.toggle('lit',!!(document.getElementById('mc-'+i)&&document.getElementById('mc-'+i).checked));}
  var btn=document.getElementById('mastery-submit-btn'); if(btn) btn.disabled=!all;
}
function submitMastery(){
  if(!_masteryPendingTopicId) return;
  var r=findTopic(_masteryPendingTopicId); if(!r) return;
  autoSpacedRepetition(r.topic); closeMasteryCheck();
  showToast('✅ Mastery confirmed — Next review in '+r.topic.interval+'d','success');
  refreshTopicCard(_masteryPendingTopicId); try{renderHomeStats();}catch(e){}
}

// ─────────────────────────────────────────────
//  FOLDER CRUD
// ─────────────────────────────────────────────
function openAddFolder(){
  document.getElementById('add-folder-overlay').classList.add('show');
  document.getElementById('add-folder-name').value='';
  document.getElementById('add-folder-icon').value='📁';
  document.getElementById('add-folder-label').value='MBBS-I';
  var btn=document.querySelector('#add-folder-overlay .btn-primary');
  if(btn){btn.textContent='Create Folder';btn.onclick=saveAddFolder;}
}
function closeAddFolder(){document.getElementById('add-folder-overlay').classList.remove('show');}
function saveAddFolder(){
  var name=document.getElementById('add-folder-name').value.trim();
  if(!name){showToast('Folder name is required','error');return;}
  var folder={id:genId(),parentId:null,name:name,icon:document.getElementById('add-folder-icon').value.trim()||'📁',color:document.getElementById('add-folder-color').value||'#00D2BE',label:document.getElementById('add-folder-label').value.trim(),subfolders:[],topics:[]};
  state.folders.push(folder); saveState(); closeAddFolder(); renderFolders();
  showToast('📁 Folder "'+name+'" created!','success');
}
function openEditFolder(fid){
  var f=findFolder(fid); if(!f) return;
  document.getElementById('add-folder-name').value=f.name;
  document.getElementById('add-folder-icon').value=f.icon;
  document.getElementById('add-folder-color').value=f.color||'#00D2BE';
  document.getElementById('add-folder-label').value=f.label||'';
  var btn=document.querySelector('#add-folder-overlay .btn-primary');
  if(btn){btn.textContent='Save Changes';btn.onclick=function(){f.name=document.getElementById('add-folder-name').value.trim()||f.name;f.icon=document.getElementById('add-folder-icon').value.trim()||f.icon;f.color=document.getElementById('add-folder-color').value||f.color;f.label=document.getElementById('add-folder-label').value.trim();saveState();closeAddFolder();if(currentFolderId)renderSubfolderView();else renderFolders();showToast('Folder updated','success');btn.textContent='Create Folder';btn.onclick=saveAddFolder;};}
  document.getElementById('add-folder-overlay').classList.add('show');
}
function deleteFolder(fid){
  var f=findFolder(fid); if(!f) return;
  if(!confirm('Delete folder "'+f.name+'" and ALL nested contents? This cannot be undone.')) return;
  if(f.parentId){var p=findFolder(f.parentId);if(p)p.subfolders=p.subfolders.filter(function(x){return x.id!==fid;});}
  else state.folders=state.folders.filter(function(x){return x.id!==fid;});
  saveState(); if(currentFolderId===fid)goBackFromSubfolder();else if(currentFolderId)renderSubfolderView();else renderFolders();
  showToast('Folder deleted','warn');
}
function openAddSubfolder(){
  var f=findFolder(currentFolderId); if(!f) return;
  document.getElementById('add-subfolder-subtitle').textContent='Inside "'+f.name+'"';
  document.getElementById('add-subfolder-name').value='';
  document.getElementById('add-subfolder-overlay').classList.add('show');
}
function closeAddSubfolder(){document.getElementById('add-subfolder-overlay').classList.remove('show');}
function saveAddSubfolder(){
  var name=document.getElementById('add-subfolder-name').value.trim(); if(!name){showToast('Subfolder name required','error');return;}
  var f=findFolder(currentFolderId); if(!f) return;
  f.subfolders=f.subfolders||[];f.subfolders.push({id:genId(),parentId:f.id,name:name,topics:[],subfolders:[]});
  saveState(); closeAddSubfolder(); renderSubfolderView(); showToast('Subfolder "'+name+'" created','success');
}
function openAddTopic(){
  document.getElementById('add-topic-name').value=''; document.getElementById('add-topic-difficulty').value='medium';
  document.getElementById('add-topic-weightage').value=2; document.getElementById('add-topic-tags').value='';
  document.getElementById('add-topic-overlay').classList.add('show');
}
function closeAddTopic(){document.getElementById('add-topic-overlay').classList.remove('show');}
function saveAddTopic(){
  var name=document.getElementById('add-topic-name').value.trim(); if(!name){showToast('Topic name required','error');return;}
  var f=findFolder(currentFolderId); if(!f) return;
  var tags=document.getElementById('add-topic-tags').value.split(',').map(function(t){return t.trim();}).filter(Boolean);
  var topic=createTopicData(name,currentFolderId,{difficulty:document.getElementById('add-topic-difficulty').value,weightage:+document.getElementById('add-topic-weightage').value||2,tags:tags});
  f.topics.push(topic); saveState(); closeAddTopic(); renderSubfolderView(); showToast('Topic "'+name+'" added','success');
}
var movingTopicId=null;
function openMoveTopic(topicId){
  movingTopicId=topicId;
  document.getElementById('move-topic-dest').innerHTML=buildFolderOptions(state.folders,0);
  document.getElementById('move-topic-overlay').classList.add('show');
}
function buildFolderOptions(folders,depth){
  var html='',prefix='\u00a0'.repeat(depth*4)+(depth>0?'↳ ':'');
  folders.forEach(function(f){html+='<option value="'+f.id+'">'+prefix+(f.icon||'📂')+' '+f.name+'</option>';if(f.subfolders)html+=buildFolderOptions(f.subfolders,depth+1);});
  return html;
}
function closeMoveTopic(){document.getElementById('move-topic-overlay').classList.remove('show');}
function confirmMoveTopic(){
  var destId=document.getElementById('move-topic-dest').value;
  var res=findTopic(movingTopicId); if(!res){showToast('Topic not found','error');closeMoveTopic();return;}
  var topic=res.topic,folder=res.folder;
  if(folder.id===destId){closeMoveTopic();return;}
  var dest=findFolder(destId); if(!dest){showToast('Destination not found','error');return;}
  folder.topics=folder.topics.filter(function(t){return t.id!==movingTopicId;}); topic.parentId=destId; dest.topics.push(topic);
  saveState(); closeMoveTopic(); if(currentView==='subfolder') renderSubfolderView();
  showToast('📦 Topic moved to "'+dest.name+'"','success');
}

// ─────────────────────────────────────────────
//  REVISION
// ─────────────────────────────────────────────
function renderRevisionView(){
  var due=getDueTopics(),content=document.getElementById('revision-content'); if(!content) return;
  if(!due.length){content.innerHTML='<div class="rev-empty"><div class="rev-empty-icon">🎉</div><div style="font-family:var(--font-head);font-size:20px;font-weight:800;margin-bottom:8px">All caught up!</div><div style="color:var(--text-muted);font-size:14px;margin-bottom:20px">No topics due for review today.</div><button class="btn btn-primary" onclick="showView(\'folders\')">📚 Study New Topics</button></div>';return;}
  var queueHtml=due.slice(0,10).map(function(item,i){return '<div class="rev-queue-item '+(i===0?'current':'')+'" onclick="goToTopic(\''+item.topic.id+'\')"><div class="q-dot" style="background:'+(i===0?'var(--primary)':'var(--surface3)')+'"></div><span style="flex:1">'+item.topic.name+'</span><span style="color:var(--text-muted);font-size:11px">'+item.folder.name+'</span></div>';}).join('');
  content.innerHTML='<div style="display:grid;grid-template-columns:1fr 280px;gap:16px"><div>'+buildRevCard(due[0])+'</div><div><div class="section-title">📋 Queue ('+due.length+' topics)</div><div class="rev-queue-list">'+queueHtml+'</div>'+(due.length>10?'<div style="font-size:12px;color:var(--text-muted);margin-top:8px;text-align:center">+'+(due.length-10)+' more</div>':'')+'</div></div>';
}
function buildRevCard(item){
  var topic=item.topic,folder=item.folder;
  return '<div class="revision-session-card"><div class="rev-topic-name" onclick="goToTopic(\''+topic.id+'\')">'+topic.name+' 🔗</div><div class="rev-topic-folder">'+(folder.icon||'📂')+' '+folder.name+'</div><div style="font-size:12px;color:var(--text-muted);margin-bottom:16px">Interval: '+(topic.interval||0)+'d · Reps: '+(topic.repetitions||0)+' · '+(topic.lastReviewDate?'Last: '+topic.lastReviewDate:'First review')+'</div>'+(topic.notes?'<div style="background:var(--surface2);border-radius:var(--r-sm);padding:12px;font-size:12px;color:var(--text-muted);text-align:left;margin-bottom:16px;max-height:100px;overflow-y:auto">'+topic.notes+'</div>':'')+'<div class="rev-quality-btns"><button class="rev-q-btn good" style="width:100%;max-width:340px" onclick="openMasteryCheck(\''+topic.id+'\')">🏆 Open Mastery Checklist<div class="rev-q-sub">4-point science check · Next: '+calcNextInterval(topic)+'d</div></button></div></div>';
}
function startRevisionSession(){showView('revision');}
function showWeakAreas(){showView('analytics');}

// ─────────────────────────────────────────────
//  WEEKEND REVISION
// ─────────────────────────────────────────────
var _wkndQueue=[],_wkndIdx=0,_wkndDone=[];
function renderWeekendRevision(){
  var panel=document.getElementById('weekend-revision-panel'); if(!panel) return;
  var items=getWeeklyStudiedTopics(),dayName=['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'][new Date().getDay()];
  panel.style.display='block';
  var statsRow=document.getElementById('weekend-stats-row'),grid=document.getElementById('weekend-topic-grid'); if(!grid) return;
  if(!items.length){panel.style.display=isWeekend()?'block':'none';if(statsRow)statsRow.innerHTML='';grid.innerHTML='<div style="color:var(--text-muted);font-size:12px">No topics studied this week yet!</div>';return;}
  if(!isWeekend()){
    if(statsRow)statsRow.innerHTML='<div class="weekend-stat">📅 <span>'+items.length+'</span> topic'+(items.length!==1?'s':'')+' tracked this week</div>';
    grid.innerHTML=items.slice(0,6).map(function(x){return '<div class="weekend-topic-chip" onclick="goToTopic(\''+x.topic.id+'\')"><span>'+(x.topic.name.length>26?x.topic.name.slice(0,24)+'…':x.topic.name)+'</span><span>'+(x.folder.icon||'📂')+'</span></div>';}).join('')+(items.length>6?'<div class="weekend-topic-chip" style="color:var(--text-muted)">+'+(items.length-6)+' more</div>':'');
    return;
  }
  var reviewed=_wkndDone.length,total=items.length,remaining=items.filter(function(i){return !_wkndDone.includes(i.topic.id);}).length;
  if(statsRow)statsRow.innerHTML='<div class="weekend-stat">📚 <span>'+total+'</span> topics</div><div class="weekend-stat">✅ <span>'+reviewed+'</span> reviewed</div><div class="weekend-stat">⏳ <span>'+remaining+'</span> remaining</div><div class="weekend-stat">📅 <span>'+dayName+'</span></div>';
  grid.innerHTML=items.map(function(x){var done=_wkndDone.includes(x.topic.id);return '<div class="weekend-topic-chip '+(done?'reviewed':'')+'" onclick="goToTopic(\''+x.topic.id+'\')">'+(done?'✅ '+'<span>'+(x.topic.name.length>24?x.topic.name.slice(0,22)+'…':x.topic.name)+'</span>':'<span>'+(x.topic.name.length>24?x.topic.name.slice(0,22)+'…':x.topic.name)+'</span>')+'<span>'+(x.folder.icon||'📂')+'</span><span>'+getTopicCompletion(x.topic)+'%</span></div>';}).join('');
}
function startWeekendSession(){
  var items=getWeeklyStudiedTopics(); if(!items.length){showToast('No topics to review yet!','warn');return;}
  _wkndQueue=items.filter(function(i){return !_wkndDone.includes(i.topic.id);}).concat(items.filter(function(i){return _wkndDone.includes(i.topic.id);}));
  _wkndIdx=0; document.getElementById('weekend-session-overlay').classList.add('show'); renderWkndSessionCard();
}
function closeWeekendSession(){document.getElementById('weekend-session-overlay').classList.remove('show');renderWeekendRevision();}
function renderWkndSessionCard(){
  var body=document.getElementById('wknd-session-body'),sub=document.getElementById('wknd-session-subtitle'); if(!body) return;
  if(_wkndIdx>=_wkndQueue.length){if(sub)sub.textContent='Session complete 🎉';body.innerHTML='<div class="wknd-done-banner"><h2>🎉 Weekend Revision Done!</h2><p style="color:var(--text-muted);font-size:14px;margin-bottom:18px">You reviewed all <strong>'+_wkndQueue.length+'</strong> topics.</p><div style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap"><button class="btn btn-primary" onclick="closeWeekendSession()">✅ Close</button><button class="btn btn-secondary" onclick="_wkndIdx=0;_wkndDone=[];renderWkndSessionCard()">🔁 Restart</button></div></div>';return;}
  var topic=_wkndQueue[_wkndIdx].topic,folder=_wkndQueue[_wkndIdx].folder,total=_wkndQueue.length,pct=Math.round((_wkndIdx/total)*100);
  if(sub)sub.textContent='Topic '+(_wkndIdx+1)+' of '+total;
  body.innerHTML='<div class="wknd-progress-bar"><div class="wknd-progress-fill" style="width:'+pct+'%"></div></div><div class="wknd-counter">'+_wkndIdx+' of '+total+' reviewed · '+(total-_wkndIdx)+' remaining</div><div class="wknd-session-card"><div class="wknd-session-topic">'+topic.name+'</div><div class="wknd-session-folder">'+(folder.icon||'📂')+' '+folder.name+'</div>'+(topic.notes?'<div class="wknd-session-notes">'+topic.notes+'</div>':'')+'<div class="wknd-btns"><button class="wknd-btn skip" onclick="wkndSkip()">⏭ Skip</button><button class="wknd-btn good" style="flex:2;background:var(--blue-dim);color:var(--blue);border:1px solid rgba(58,134,255,0.3);" onclick="wkndRate()">✅ Reviewed<small>Next: '+calcNextInterval(topic)+'d</small></button></div></div><div style="text-align:center;margin-top:4px"><button class="btn btn-ghost btn-sm" onclick="closeWeekendSession()">✕ End</button></div>';
}
function wkndRate(){if(_wkndIdx>=_wkndQueue.length)return;var topic=_wkndQueue[_wkndIdx].topic;autoSpacedRepetition(topic);if(!_wkndDone.includes(topic.id))_wkndDone.push(topic.id);_wkndIdx++;renderWkndSessionCard();showToast('✅ Reviewed — next in '+topic.interval+'d','success');}
function wkndSkip(){if(_wkndIdx<_wkndQueue.length){_wkndIdx++;renderWkndSessionCard();}}

// ─────────────────────────────────────────────
//  ANALYTICS
// ─────────────────────────────────────────────
var chartInstances={};
function renderAnalytics(){renderWeakStrong();setTimeout(function(){renderSubjectChart();renderPieChart();renderWeeklyChart();renderRevisionChart();},50);}
function renderWeakStrong(){
  var all=getAllTopics(),weak=all.filter(function(x){return x.topic.weakFlag||(x.topic.mcqScore!==null&&x.topic.mcqScore<60);}),strong=all.filter(function(x){return x.topic.stages.mastered;});
  var wl=document.getElementById('weak-topics-list'),sl=document.getElementById('strong-topics-list');
  if(wl)wl.innerHTML=!weak.length?'<div style="color:var(--text-dim);font-size:12px">No weak topics flagged</div>':weak.slice(0,8).map(function(x){return '<div class="ws-item"><span style="color:var(--red)">⚠️</span><span style="flex:1;cursor:pointer" onclick="goToTopic(\''+x.topic.id+'\')">'+x.topic.name+'</span><span style="color:var(--text-muted);font-size:11px">'+x.folder.name+'</span></div>';}).join('');
  if(sl)sl.innerHTML=!strong.length?'<div style="color:var(--text-dim);font-size:12px">No mastered topics yet</div>':strong.slice(0,8).map(function(x){return '<div class="ws-item"><span style="color:var(--green)">✅</span><span style="flex:1;cursor:pointer" onclick="goToTopic(\''+x.topic.id+'\')">'+x.topic.name+'</span><span style="color:var(--text-muted);font-size:11px">'+x.folder.name+'</span></div>';}).join('');
}
function destroyChart(id){if(chartInstances[id]){try{chartInstances[id].destroy();}catch(e){}delete chartInstances[id];}}
function renderSubjectChart(){destroyChart('subjects');var ctx=document.getElementById('chart-subjects');if(!ctx)return;var labels=state.folders.map(function(f){return f.name.length>14?f.name.slice(0,14)+'…':f.name;});var data=state.folders.map(function(f){var all=getFolderTopicsRec(f);return all.length?Math.round((all.filter(function(t){return t.stages.mastered;}).length/all.length)*100):0;});chartInstances.subjects=new Chart(ctx,{type:'bar',data:{labels:labels,datasets:[{label:'Progress %',data:data,backgroundColor:state.folders.map(function(f){return (f.color||'#00D2BE')+'99';}),borderColor:state.folders.map(function(f){return f.color||'#00D2BE';}),borderWidth:1,borderRadius:6}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true,max:100,ticks:{color:'#4D7A9A',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}},x:{ticks:{color:'#4D7A9A',font:{size:10}},grid:{display:false}}}}});}
function renderPieChart(){destroyChart('pie');var ctx=document.getElementById('chart-pie');if(!ctx)return;var all=getAllTopics(),mastered=all.filter(function(x){return x.topic.stages.mastered;}).length,inProg=all.filter(function(x){return !x.topic.stages.mastered&&getTopicCompletion(x.topic)>0;}).length,ns=all.length-mastered-inProg;chartInstances.pie=new Chart(ctx,{type:'doughnut',data:{labels:['Mastered','In Progress','Not Started'],datasets:[{data:[mastered,inProg,ns],backgroundColor:['rgba(6,255,165,0.7)','rgba(0,210,190,0.7)','rgba(21,36,56,0.9)'],borderColor:['#06FFA5','#00D2BE','#2A4A6A'],borderWidth:1}]},options:{responsive:true,plugins:{legend:{position:'bottom',labels:{color:'#4D7A9A',font:{size:10}}}}}});}
function renderWeeklyChart(){destroyChart('weekly');var ctx=document.getElementById('chart-weekly');if(!ctx)return;var days=['Sun','Mon','Tue','Wed','Thu','Fri','Sat'],today=new Date().getDay();var labels=[];for(var i=0;i<7;i++)labels.push(days[(today-6+i+7)%7]);var data=(state.studyTimer.weeklyData||[0,0,0,0,0,0,0]).map(function(s){return +(s/3600).toFixed(1);});chartInstances.weekly=new Chart(ctx,{type:'line',data:{labels:labels,datasets:[{label:'Hours',data:data,fill:true,backgroundColor:'rgba(0,210,190,0.08)',borderColor:'#00D2BE',borderWidth:2,pointBackgroundColor:'#00D2BE',tension:0.4}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true,ticks:{color:'#4D7A9A',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}},x:{ticks:{color:'#4D7A9A',font:{size:10}},grid:{display:false}}}}});}
function renderRevisionChart(){destroyChart('revisions');var ctx=document.getElementById('chart-revisions');if(!ctx)return;var today=todayStr(),labels=state.folders.map(function(f){return f.name.length>10?f.name.slice(0,10)+'…':f.name;}),data=state.folders.map(function(f){var all=getFolderTopicsRec(f);return all.filter(function(t){return t.nextReviewDate&&t.nextReviewDate<=today;}).length;});chartInstances.revisions=new Chart(ctx,{type:'bar',data:{labels:labels,datasets:[{label:'Due',data:data,backgroundColor:'rgba(255,71,87,0.5)',borderColor:'#FF4757',borderWidth:1,borderRadius:4}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true,ticks:{color:'#4D7A9A',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}},x:{ticks:{color:'#4D7A9A',font:{size:10}},grid:{display:false}}}}});}

// ─────────────────────────────────────────────
//  DATA MANAGEMENT
// ─────────────────────────────────────────────
function openSettingsPanel(){document.getElementById('settings-overlay').classList.add('show');}
function closeSettingsPanel(){document.getElementById('settings-overlay').classList.remove('show');}
var pendingJsonData=null,mergeOption='merge';
function openJsonImport(){pendingJsonData=null;document.getElementById('import-btn').disabled=true;document.getElementById('import-log').style.display='none';document.getElementById('json-import-overlay').classList.add('show');selectMergeOpt('merge');}
function closeJsonImport(){document.getElementById('json-import-overlay').classList.remove('show');}
function selectMergeOpt(opt){mergeOption=opt;document.getElementById('opt-merge').classList.toggle('selected',opt==='merge');document.getElementById('opt-replace').classList.toggle('selected',opt==='replace');}
function handleJsonDragOver(e){e.preventDefault();document.getElementById('json-upload-zone').classList.add('drag');}
function handleJsonDrop(e){e.preventDefault();document.getElementById('json-upload-zone').classList.remove('drag');var f=e.dataTransfer.files[0];if(f)processJsonFile(f);}
function handleJsonFileSelect(e){var f=e.target.files[0];if(f)processJsonFile(f);}
function processJsonFile(file){
  if(!file.name.endsWith('.json')){showImportLog([{type:'error',msg:'❌ Invalid file type. Only .json accepted.'}]);return;}
  var reader=new FileReader();reader.onload=function(e){try{var data=JSON.parse(e.target.result);if(!data.folders)throw new Error("Missing 'folders' array.");pendingJsonData=data;document.getElementById('import-btn').disabled=false;showImportLog([{type:'success',msg:'✅ Valid JSON. Click Import Syllabus to proceed.'}]);}catch(err){showImportLog([{type:'error',msg:'❌ Invalid JSON: '+err.message}]);}};reader.readAsText(file);
}
function showImportLog(entries){var log=document.getElementById('import-log');log.style.display='block';log.innerHTML=entries.map(function(e){return '<div class="log-'+e.type+'">'+e.msg+'</div>';}).join('');}
function importJson(){
  if(!pendingJsonData)return;if(mergeOption==='replace')state.folders=[];
  pendingJsonData.folders.forEach(function(raw){
    var ef=state.folders.find(function(f){return f.name.toLowerCase()===raw.name.toLowerCase();});
    if(!ef){ef={id:genId(),parentId:null,name:raw.name,icon:raw.icon||'📁',color:raw.color||'#00D2BE',label:raw.label||'',subfolders:[],topics:[]};state.folders.push(ef);}
    (raw.topics||[]).forEach(function(rt){var tn=rt.title||rt.name;if(!ef.topics.find(function(t){return t.name.toLowerCase()===tn.toLowerCase();}))ef.topics.push(createTopicData(tn,ef.id));});
  });
  saveState();closeJsonImport();showToast('✅ Import complete!','success');if(currentView==='folders'){currentFolderId=null;renderFolders();}
}
function exportBackup(){try{var blob=new Blob([JSON.stringify(state,null,2)],{type:'application/json'});var url=URL.createObjectURL(blob);var a=document.createElement('a');a.href=url;a.download='medtrack-backup-'+todayStr()+'.json';a.click();URL.revokeObjectURL(url);showToast('💾 Backup downloaded!','success');}catch(e){showToast('Backup failed: '+e.message,'error');}}
function handleRestoreFile(e){var file=e.target.files[0];if(!file)return;var reader=new FileReader();reader.onload=function(ev){try{var data=JSON.parse(ev.target.result);if(!data.folders)throw new Error('Invalid backup');state=Object.assign({},state,data);patchTree(state.folders);saveState();closeRestore();showToast('♻️ Backup restored!','success');showView('home');}catch(err){document.getElementById('restore-status').textContent='❌ Error: '+err.message;}};reader.readAsText(file);}
function closeRestore(){document.getElementById('restore-overlay').classList.remove('show');}
function dangerClearData(){if(!confirm('⚠️ Clear ALL study data? This cannot be undone!'))return;localStorage.clear();showToast('All data cleared. Refreshing…','warn');setTimeout(function(){location.reload();},1500);}

// ─────────────────────────────────────────────
//  SEARCH
// ─────────────────────────────────────────────
function doSearch(){
  var q=document.getElementById('global-search')?document.getElementById('global-search').value.trim():'';
  var dd=document.getElementById('search-dropdown');
  if(!q||q.length<2){dd.classList.remove('show');return;}
  var results=[];
  function scan(folders){folders.forEach(function(f){if(fuzzyMatch(f.name,q))results.push({type:'folder',icon:f.icon||'📁',name:f.name,sub:'Folder',action:"navigateToFolder('"+f.id+"')"});f.topics.forEach(function(t){if(fuzzyMatch(t.name,q))results.push({type:'topic',icon:'📑',name:t.name,sub:f.name,action:"goToTopic('"+t.id+"')"});});if(f.subfolders)scan(f.subfolders);});}
  scan(state.folders);
  if(!results.length){dd.classList.remove('show');return;}
  dd.innerHTML=results.slice(0,8).map(function(r){return '<div class="sr-item" onclick="'+r.action+';document.getElementById(\'global-search\').value=\'\';document.getElementById(\'search-dropdown\').classList.remove(\'show\')"><span class="sr-icon">'+r.icon+'</span><div><div class="sr-name">'+r.name+'</div><div class="sr-sub">'+r.sub+'</div></div><span class="sr-badge">'+r.type+'</span></div>';}).join('');
  dd.classList.add('show');
}
document.addEventListener('click',function(e){var dd=document.getElementById('search-dropdown'),inp=document.getElementById('global-search');if(dd&&inp&&!dd.contains(e.target)&&!inp.contains(e.target))dd.classList.remove('show');});

// ─────────────────────────────────────────────
//  TIMER
// ─────────────────────────────────────────────
var timerRunning=false,timerElapsed=0,timerInterval=null;
function initTimer(){
  var today=todayStr();
  if(state.studyTimer.lastDate!==today){state.studyTimer.weeklyData=state.studyTimer.weeklyData||[0,0,0,0,0,0,0];state.studyTimer.weeklyData.push(state.studyTimer.todayElapsed||0);state.studyTimer.weeklyData=state.studyTimer.weeklyData.slice(-7);state.studyTimer.todayElapsed=0;state.studyTimer.lastDate=today;saveState();}
  timerElapsed=state.studyTimer.todayElapsed||0;updateTimerDisplays();
}
function toggleTimer(){if(timerRunning)pauseTimer();else startTimer();}
function startTimer(){if(timerRunning)return;timerRunning=true;timerInterval=setInterval(function(){timerElapsed++;updateTimerDisplays();},1000);var btn=document.getElementById('start-stop-btn');if(btn)btn.textContent='⏸ Pause';var disp=document.getElementById('timer-display');if(disp)disp.classList.add('running');updateStreak();}
function pauseTimer(){timerRunning=false;clearInterval(timerInterval);state.studyTimer.totalElapsed=(state.studyTimer.totalElapsed||0)+Math.max(0,timerElapsed-(state.studyTimer.todayElapsed||0));state.studyTimer.todayElapsed=timerElapsed;saveState();var btn=document.getElementById('start-stop-btn');if(btn)btn.textContent='▶ Start';var disp=document.getElementById('timer-display');if(disp)disp.classList.remove('running');}
function resetDayTimer(){pauseTimer();timerElapsed=0;state.studyTimer.todayElapsed=0;saveState();updateTimerDisplays();showToast('Timer reset','info');}
function updateTimerDisplays(){var s=fmtSec(timerElapsed);var disp=document.getElementById('timer-display');if(disp)disp.textContent='⏱ '+s;var big=document.getElementById('dash-timer-big');if(big)big.textContent=s;var td=document.getElementById('stat-today-time');if(td)td.textContent=fmtHrsMin(timerElapsed);var wk=document.getElementById('stat-week-time');if(wk)wk.textContent=fmtHrsMin((state.studyTimer.weeklyData||[]).reduce(function(a,b){return a+b;},0)+timerElapsed);var tot=document.getElementById('stat-total-time');if(tot)tot.textContent=fmtHrsMin((state.studyTimer.totalElapsed||0)+timerElapsed);}

// ─────────────────────────────────────────────
//  POMODORO
// ─────────────────────────────────────────────
var POMO_DURATIONS={work:50*60,short:10*60,long:20*60};
var pomodoroTimeLeft=POMO_DURATIONS.work,pomodoroRunning=false,pomodoroMode='work',pomodoroSessions=0,pomodoroInterval=null;
function openPomodoro(){document.getElementById('pomo-overlay').classList.add('show');updatePomodoroDisplay();}
function closePomodoro(){document.getElementById('pomo-overlay').classList.remove('show');}
function togglePomodoro(){if(pomodoroRunning){pomodoroRunning=false;clearInterval(pomodoroInterval);document.getElementById('pomo-toggle').textContent='▶ Start';}else{pomodoroRunning=true;pomodoroInterval=setInterval(tickPomodoro,1000);document.getElementById('pomo-toggle').textContent='⏸ Pause';if(pomodoroMode==='work'&&!timerRunning)startTimer();}}
function tickPomodoro(){pomodoroTimeLeft--;updatePomodoroDisplay();if(pomodoroTimeLeft<=0){pomodoroRunning=false;clearInterval(pomodoroInterval);document.getElementById('pomo-toggle').textContent='▶ Start';if(pomodoroMode==='work'){pomodoroSessions++;showToast('🎉 Pomodoro done! Take a break.','success');pomodoroMode=pomodoroSessions%4===0?'long':'short';}else{showToast('⚡ Break over! Focus time!','info');pomodoroMode='work';}pomodoroTimeLeft=POMO_DURATIONS[pomodoroMode];updatePomodoroDisplay();}}
function resetPomodoro(){pomodoroRunning=false;clearInterval(pomodoroInterval);pomodoroMode='work';pomodoroTimeLeft=POMO_DURATIONS.work;document.getElementById('pomo-toggle').textContent='▶ Start';updatePomodoroDisplay();}
function skipPomodoroPhase(){pomodoroTimeLeft=0;if(pomodoroRunning)tickPomodoro();else{pomodoroMode=pomodoroMode==='work'?'short':'work';pomodoroTimeLeft=POMO_DURATIONS[pomodoroMode];updatePomodoroDisplay();}}
function updatePomodoroDisplay(){var m=Math.floor(pomodoroTimeLeft/60),s=pomodoroTimeLeft%60;var pt=document.getElementById('pomo-time');if(pt)pt.textContent=pad(m)+':'+pad(s);var pm=document.getElementById('pomo-mode');if(pm)pm.textContent={work:'FOCUS',short:'SHORT BREAK',long:'LONG BREAK'}[pomodoroMode];var ps=document.getElementById('pomo-sessions');if(ps)ps.textContent='Sessions: '+pomodoroSessions+' '+'🍅'.repeat(Math.min(pomodoroSessions,8));var ring=document.getElementById('pomo-ring-fill');if(ring){ring.style.strokeDashoffset=439.8*(pomodoroTimeLeft/POMO_DURATIONS[pomodoroMode]);ring.classList.toggle('break',pomodoroMode!=='work');}}

// ─────────────────────────────────────────────
//  THEME & TOAST
// ─────────────────────────────────────────────
function toggleTheme(){if(document.body.classList.contains('light')){document.body.classList.remove('light');localStorage.setItem('medtrack_theme','dark');document.getElementById('theme-toggle-btn').textContent='☀️';}else{document.body.classList.add('light');localStorage.setItem('medtrack_theme','light');document.getElementById('theme-toggle-btn').textContent='🌙';}}
function loadTheme(){if(localStorage.getItem('medtrack_theme')==='light'){document.body.classList.add('light');var btn=document.getElementById('theme-toggle-btn');if(btn)btn.textContent='🌙';}}
function showToast(msg,type){
  type=type||'info';var icons={success:'✅',error:'❌',warn:'⚠️',info:'ℹ️'};
  var tc=document.getElementById('toast-container');if(!tc)return;
  var t=document.createElement('div');t.className='toast '+type;t.innerHTML='<span>'+(icons[type]||'ℹ️')+'</span>'+msg;
  tc.appendChild(t);setTimeout(function(){try{tc.removeChild(t);}catch(e){}},3000);
}

// ─────────────────────────────────────────────
//  AUTH
// ─────────────────────────────────────────────
function openAuthModal(){
  var overlay=document.getElementById('auth-overlay');if(!overlay)return;
  var signoutSec=document.getElementById('auth-signout-section'),signinBtn=document.getElementById('auth-google-btn'),skipBtn=document.getElementById('auth-skip-btn'),sub=document.getElementById('auth-modal-sub');
  if(currentUser&&!currentUser.isAnonymous){if(signoutSec)signoutSec.style.display='';if(signinBtn)signinBtn.style.display='none';if(skipBtn)skipBtn.style.display='none';if(sub)sub.textContent='Signed in as '+(currentUser.displayName||currentUser.email||'Google User')+'.';}
  else{if(signoutSec)signoutSec.style.display='none';if(signinBtn)signinBtn.style.display='';if(skipBtn)skipBtn.style.display='';}
  overlay.classList.add('show');
}
function closeAuthModal(){var o=document.getElementById('auth-overlay');if(o)o.classList.remove('show');}
function signInWithGoogle(){
  if(!fbAuth){showToast('Firebase not ready…','warn');return;}
  var provider=new firebase.auth.GoogleAuthProvider();
  fbAuth.signInWithPopup(provider).then(function(result){
    showToast('✅ Signed in as '+(result.user.displayName||result.user.email)+'!','success');
    closeAuthModal();currentUser=result.user;updateNavAuth(result.user);saveState();
  }).catch(function(e){
    if(e.code==='auth/unauthorized-domain'){var sub=document.getElementById('auth-modal-sub');if(sub)sub.innerHTML='<span style="color:var(--red);font-weight:bold">Domain Not Authorized!</span><br>Firebase Console → Auth → Authorized domains → Add: <strong>'+window.location.hostname+'</strong>';showToast('Domain not authorized','error');}
    else if(e.code==='auth/popup-blocked')showToast('⚠️ Popup blocked — allow popups','error');
    else showToast('Sign-in error: '+e.message,'error');
  });
}
function signOutUser(){
  if(!fbAuth)return;
  if(unsubSnap){unsubSnap();unsubSnap=null;}
  fbAuth.signOut().then(function(){currentUser=null;updateNavAuth(null);updateSyncStatus('offline');closeAuthModal();showToast('Signed out. Data saved locally.','info');}).catch(function(e){showToast('Sign-out error: '+e.message,'error');});
}
function updateNavAuth(user){
  var signinBtn=document.getElementById('nav-signin-btn'),chip=document.getElementById('nav-user-chip'),avatar=document.getElementById('nav-user-avatar'),nameEl=document.getElementById('nav-user-name');
  if(!signinBtn||!chip)return;
  if(user&&!user.isAnonymous){signinBtn.style.display='none';chip.style.display='flex';if(avatar){avatar.src=user.photoURL||'';avatar.style.display=user.photoURL?'':'none';}if(nameEl)nameEl.textContent=user.displayName?user.displayName.split(' ')[0]:(user.email||'User');}
  else{signinBtn.style.display='';chip.style.display='none';}
}

// ─────────────────────────────────────────────
//  SCHEDULE SYSTEM
// ─────────────────────────────────────────────
var pendingScheduleData=null;
function openScheduleImport(){
  pendingScheduleData=null;
  var btn=document.getElementById('sched-import-btn');if(btn)btn.disabled=true;
  var log=document.getElementById('sched-import-log');if(log){log.style.display='none';log.innerHTML='';}
  var preview=document.getElementById('sched-preview-area');if(preview)preview.innerHTML='';
  var schema=document.getElementById('sched-schema-display');if(schema)schema.style.display='none';
  var fi=document.getElementById('sched-file-input');if(fi)fi.value='';
  var pa=document.getElementById('sched-paste-area');if(pa)pa.value='';
  var ps=document.getElementById('sched-paste-status');if(ps){ps.textContent='Paste your JSON above to validate';ps.className='sched-paste-status idle';}
  document.getElementById('schedule-import-overlay').classList.add('show');
}
function closeScheduleImport(){document.getElementById('schedule-import-overlay').classList.remove('show');}
function copySchedulePrompt(){navigator.clipboard.writeText(SCHEDULE_AI_PROMPT).then(function(){var btn=document.getElementById('sched-prompt-copy-btn');if(btn){btn.textContent='✅ Copied!';setTimeout(function(){btn.textContent='📋 Copy AI Prompt';},2500);}showToast('📋 AI prompt copied! Paste into Claude or ChatGPT.','success');}).catch(function(){showToast('Copy failed','warn');});}
function handleSchedDragOver(e){e.preventDefault();document.getElementById('sched-upload-zone').classList.add('drag');}
function handleSchedDrop(e){e.preventDefault();document.getElementById('sched-upload-zone').classList.remove('drag');var f=e.dataTransfer.files[0];if(f)processScheduleFile(f);}
function handleSchedFileSelect(e){var f=e.target.files[0];if(f)processScheduleFile(f);}
function processScheduleFile(file){
  if(!file.name.endsWith('.json')){showSchedLog([{type:'error',msg:'❌ Invalid file type. Only .json accepted.'}]);return;}
  var reader=new FileReader();reader.onload=function(ev){var raw=ev.target.result;var pa=document.getElementById('sched-paste-area');if(pa)pa.value=raw;validateAndPreviewScheduleJSON(raw);};reader.readAsText(file);
}
function handleSchedPaste(value){
  if(!value||!value.trim()){var ps=document.getElementById('sched-paste-status');if(ps){ps.textContent='Paste your JSON above to validate';ps.className='sched-paste-status idle';}pendingScheduleData=null;var btn=document.getElementById('sched-import-btn');if(btn)btn.disabled=true;return;}
  validateAndPreviewScheduleJSON(value);
}
function validateAndPreviewScheduleJSON(raw){
  var ps=document.getElementById('sched-paste-status'),preview=document.getElementById('sched-preview-area'),schema=document.getElementById('sched-schema-display'),btn=document.getElementById('sched-import-btn');
  try{
    var data=JSON.parse(raw);if(!data.schedule||!Array.isArray(data.schedule))throw new Error("Missing 'schedule' array.");if(!data.schedule.length)throw new Error('Schedule is empty.');
    pendingScheduleData=data;
    if(ps){ps.textContent='✅ Valid — '+data.schedule.length+' sessions found';ps.className='sched-paste-status valid';}
    if(schema){schema.style.display='block';schema.textContent='Date: '+(data.date||'today')+' | Title: '+(data.dayTitle||'—')+' | Slots: '+data.schedule.length+(data.hijriDate?' | '+data.hijriDate:'');}
    if(preview)preview.innerHTML='<div style="font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:1px;color:var(--text-muted);margin-bottom:8px">Preview ('+data.schedule.length+' slots)</div><div style="display:flex;flex-direction:column;gap:4px;max-height:200px;overflow-y:auto">'+data.schedule.map(function(s){return '<div style="display:flex;gap:10px;align-items:center;background:var(--surface2);border-radius:6px;padding:7px 10px;font-size:12px"><span>'+(s.emoji||'📚')+'</span><span style="font-family:var(--font-mono);color:var(--primary);min-width:110px;flex-shrink:0">'+(s.time||'—')+'</span><span style="font-weight:600;flex:1">'+s.topic+'</span><span style="color:var(--text-muted)">'+s.subject+'</span><span class="slot-diff '+(s.difficulty||'moderate')+'">'+(s.difficulty||'mod')+'</span></div>';}).join('')+'</div>';
    if(btn)btn.disabled=false;showSchedLog([{type:'success',msg:'✅ Valid — '+data.schedule.length+' sessions ready. Click "Load Schedule".'}]);
  }catch(err){
    pendingScheduleData=null;if(ps){ps.textContent='❌ '+err.message;ps.className='sched-paste-status invalid';}
    if(btn)btn.disabled=true;if(preview)preview.innerHTML='';if(schema)schema.style.display='none';
    showSchedLog([{type:'error',msg:'❌ Invalid JSON: '+err.message}]);
  }
}
function showSchedLog(entries){var log=document.getElementById('sched-import-log');if(!log)return;log.style.display='block';log.innerHTML=entries.map(function(e){return '<div class="log-'+e.type+'">'+e.msg+'</div>';}).join('');}
function copyCurrentJson(){
  if(!state.dailySchedule||!state.dailySchedule.items||!state.dailySchedule.items.length){showToast('No active schedule to copy','warn');return;}
  navigator.clipboard.writeText(JSON.stringify(state.dailySchedule,null,2)).then(function(){showToast('📋 Current schedule JSON copied!','success');var btn=document.getElementById('copy-json-btn');if(btn){var o=btn.textContent;btn.textContent='✅ Copied!';setTimeout(function(){btn.textContent=o;},2000);}}).catch(function(){showToast('Copy failed','error');});
}
function importScheduleToTrack(){
  if(!pendingScheduleData)return;
  var schedDate=pendingScheduleData.date||todayStr();
  var topicsAdded=0;
  var dayData=dvGetDayData(schedDate);

  // Convert each schedule slot into a diary task
  var newTasks=pendingScheduleData.schedule.map(function(slotData){
    // Auto-create folder/topic if missing (preserve original logic)
    var folder=state.folders.find(function(f){
      return f.name.toLowerCase()===(slotData.subject||'').toLowerCase();
    });
    if(!folder){
      folder={id:genId(),parentId:null,name:slotData.subject||'Subject',
        icon:slotData.emoji||'📁',color:'#3A86FF',label:'',subfolders:[],topics:[]};
      state.folders.push(folder);
    }
    function findMatch(f){
      var t=f.topics.find(function(x){
        return x.name.toLowerCase()===(slotData.topic||'').toLowerCase();
      });
      if(t)return t;
      for(var i=0;i<f.subfolders.length;i++){var r=findMatch(f.subfolders[i]);if(r)return r;}
      return null;
    }
    var mt=findMatch(folder);
    if(!mt){
      mt=createTopicData(slotData.topic||'Untitled',folder.id,{
        difficulty:slotData.difficulty,weightage:slotData.weightage,tags:slotData.tags||[]
      });
      mt.notes=slotData.notes||'';
      if(slotData.importance&&Array.isArray(slotData.importance)) mt.importance=slotData.importance;
      folder.topics.push(mt);
      topicsAdded++;
    }

    // Map slot type to stage key
    var stageMap={study:'reading',revision:'revision',recall:'revision',
      mcq:'mcqs',notes:'notes',lectures:'lectures',video:'lectures',
      diagram:'notes',clinical:'mcqs'};
    var stageKey=stageMap[slotData.type||'study']||'reading';

    // Build diary task
    return {
      id:slotData.id||genId(),
      time:slotData.time||'',
      title:slotData.topic||(slotData.subject+' session'),
      type:slotData.type||'study',
      topicId:mt.id,
      stageKey:stageKey,
      completed:false,
      notes:slotData.notes||'',
      microSteps:Array.isArray(slotData.microSteps)?slotData.microSteps:
        dvDefaultMicroSteps(slotData.type||'study'),
      microChecks:{},
      subject:slotData.subject||'',
      tags:Array.isArray(slotData.tags)?slotData.tags:[],
      difficulty:slotData.difficulty||'moderate',
      source:'json'
    };
  });

  // Merge into diary day (don't duplicate by id)
  var existingIds=dayData.tasks.map(function(t){return t.id;});
  var added=0;
  newTasks.forEach(function(t){
    if(existingIds.indexOf(t.id)<0){dayData.tasks.push(t);added++;}
  });

  // Also keep backward-compat: store in state.dailySchedule for old code
  state.dailySchedule={
    date:schedDate,
    dayTitle:pendingScheduleData.dayTitle||"Today's Schedule",
    hijriDate:pendingScheduleData.hijriDate||'',
    items:newTasks,
    completedIds:[],
    microChecks:{}
  };

  // Set day title from JSON
  if(pendingScheduleData.dayTitle) dayData.title=pendingScheduleData.dayTitle;

  dvSaveDayData(schedDate,dayData);
  saveState();
  closeScheduleImport();

  // Navigate to the Diary view to see the loaded schedule
  dvCurrentDate=schedDate;
  showView('diary');

  showToast('📔 Schedule loaded into Diary! '+added+' tasks · '+topicsAdded+' topics auto-created','success');
}

function dvDefaultMicroSteps(type){
  var defaults={
    study:['Read textbook','Highlight key points','Write summary notes','Active recall'],
    revision:['Review notes','Cover & recall','Weak points check','Final review'],
    recall:['Write from memory','Check against notes','Repeat weak points'],
    mcq:['Attempt questions','Review wrong answers','Note weak areas'],
    video:['Watch lecture','Pause & recall','Write key points'],
    diagram:['Draw from memory','Label structures','Compare with textbook'],
    clinical:['Read case','Identify findings','Differential diagnosis','Management plan'],
    notes:['Organise notes','Add diagrams','Create summary table'],
    break:[]
  };
  return defaults[type]||[];
}

function toggleScheduleSlot(id){
  // Bridge: delegate to diary task toggle for the current schedule date
  var schedDate=state.dailySchedule&&state.dailySchedule.date?state.dailySchedule.date:todayStr();
  var prevDate=dvCurrentDate;
  dvCurrentDate=schedDate;
  dvToggleTask(id);
  dvCurrentDate=prevDate;
  // Also update legacy completedIds for backward compat
  if(state.dailySchedule){
    state.dailySchedule.completedIds=state.dailySchedule.completedIds||[];
    var dayData=dvGetDayData(schedDate);
    var task=dayData.tasks.find(function(t){return t.id===id;});
    if(task){
      var cidx=state.dailySchedule.completedIds.indexOf(id);
      if(task.completed&&cidx<0) state.dailySchedule.completedIds.push(id);
      else if(!task.completed&&cidx>=0) state.dailySchedule.completedIds.splice(cidx,1);
    }
    saveState();
  }
  dvRenderHomePreview();
}

function toggleMicroStep(slotId,stepIdx){
  state.dailySchedule.microChecks=state.dailySchedule.microChecks||{};
  var key=slotId+'_'+stepIdx;state.dailySchedule.microChecks[key]=!state.dailySchedule.microChecks[key];saveState();
  var cb=document.getElementById('mcb-'+slotId+'-'+stepIdx);var lbl=document.getElementById('mct-'+slotId+'-'+stepIdx);
  if(cb)cb.checked=!!state.dailySchedule.microChecks[key];if(lbl)lbl.classList.toggle('done',!!state.dailySchedule.microChecks[key]);
}
function clearSchedule(){if(!confirm("Clear today's schedule?"))return;state.dailySchedule={date:null,dayTitle:'',items:[],completedIds:[],microChecks:{}};saveState();renderDailySchedule();showToast('Schedule cleared','info');}
function getActiveSlotId(){
  var sched=state.dailySchedule;if(!sched||!sched.items||!sched.items.length)return null;
  var now=new Date(),nowMin=now.getHours()*60+now.getMinutes();
  for(var i=0;i<sched.items.length;i++){
    var slot=sched.items[i];if(!slot.time)continue;
    var parts=slot.time.split('-').map(function(s){return s.trim();});if(parts.length<2)continue;
    var pt=function(t){var m=t.match(/(\d{1,2}):(\d{2})/);return m?parseInt(m[1])*60+parseInt(m[2]):null;};
    var start=pt(parts[0]),end=pt(parts[1]);if(start!==null&&end!==null&&nowMin>=start&&nowMin<end)return slot.id;
  }return null;
}

// ─────────────────────────────────────────────
//  MEDICAL DIARY RENDER
// ─────────────────────────────────────────────
function renderDailySchedule(){
  // Replaced by diary system — render home preview instead
  try{dvRenderHomePreview();}catch(e){}
}

// ─────────────────────────────────────────────
//  MEGA PROMPT
// ─────────────────────────────────────────────
function copyAllTopicsPrompt(){
  // Use diary tasks for today (unified source)
  var tasks = dvGetMergedTasks(dvCurrentDate);
  var studySlots = tasks.filter(function(t){ return t.type !== 'break'; });
  if(!studySlots.length){showToast('No study tasks in diary today','warn');return;}
  // Build enriched topic list from diary + topic data
  var topicList = studySlots.map(function(s,i){
    var r = s.topicId ? findTopic(s.topicId) : null;
    var subj = (s.subject) || (r && r.folder ? r.folder.name : 'General');
    var topicName = r ? r.topic.name : s.title;
    var stagesDone = r ? STAGE_KEYS.filter(function(k){return r.topic.stages[k];}).map(function(k){return STAGE_LABELS[k];}) : [];
    var imp = r && r.topic.importance && r.topic.importance.length ? r.topic.importance.join(', ') : '';
    var notes = s.notes || (r && r.topic.notes ? r.topic.notes.slice(0,80) : '');
    return (i+1)+'. ['+subj+'] '+topicName+
      (s.type && s.type!=='study' ? ' ('+s.type+')' : '')+
      (stagesDone.length ? ' ✅ '+stagesDone.join(', ') : '')+
      (imp ? ' 🏷️ '+imp : '')+
      (notes ? ' — '+notes : '');
  }).join('\n');
  var dayData = dvGetDayData(dvCurrentDate);
  var sched = state.dailySchedule || {};
  var prompt='You are an advanced Medical AI integrated into MedTrack Pro.\n\nGenerate a COMPLETE INTERACTIVE SINGLE-PAGE HTML STUDY FILE for ALL of today\'s topics.\n\nDATE: '+dvCurrentDate+'\nDAY TITLE: '+(dayData.title||sched.dayTitle||'Daily Study Session')+'\n'+(sched.hijriDate?'HIJRI DATE: '+sched.hijriDate:'')+'\n\n━━━━━━━━━━━━━━━━━━━━━━━\n📚 TODAY\'S TOPICS ('+studySlots.length+' topics)\n━━━━━━━━━━━━━━━━━━━━━━━\n'+topicList+'\n\n━━━━━━━━━━━━━━━━━━━━━━━\n🔶 MANDATORY OUTPUT FORMAT\n━━━━━━━━━━━━━━━━━━━━━━━\n\nGenerate ONE complete HTML file with:\n\n1. TOP NAVIGATION — a tab for each topic\n2. DEDICATED HTML SECTION for EACH TOPIC with ALL 6 subsections:\n   A) 🧠 FIKRI BREAKDOWN (Conceptual Foundation, Mechanism, System Integration, Clinical Lens, Diagnostic Thinking, Management Logic, Exam Intelligence)\n   B) 📋 QUICK NOTES — condensed bullet points & mnemonics\n   C) ❓ MCQs — 5 clinical vignette MCQs with click-to-reveal answers & score tracker\n   D) 🔁 ACTIVE RECALL — 5 flip-card questions\n   E) 🏥 CLINICAL CASE — 1 realistic patient vignette\n   F) 📅 REVISION SCHEDULE — Day 1/3/7/14/30 plan\n\n3. DESIGN: Dark UI (bg #070D1A, accent #00D2BE), smooth transitions, collapsible subsections, 100% self-contained inline CSS+JS, ZERO placeholder text.\n4. FOOTER: Day summary, cross-references, overall completion tracker.\n\nOUTPUT: COMPLETE READY-TO-OPEN HTML FILE. No text outside the HTML.';
  navigator.clipboard.writeText(prompt).then(function(){
    showToast('🚀 Mega prompt copied! '+studySlots.length+' topics — paste into Claude for your full study file','success');
    var btn=document.getElementById('mega-prompt-btn');if(btn){btn.style.borderColor='var(--green)';btn.style.background='linear-gradient(135deg,rgba(6,255,165,0.12),rgba(0,210,190,0.08))';setTimeout(function(){btn.style.borderColor='';btn.style.background='';},2500);}
  }).catch(function(){showToast('Copy failed — try again','error');});
}

// ─────────────────────────────────────────────
//  INIT
// ─────────────────────────────────────────────

// ═══════════════════════════════════════════════════════════════
//  MEDTRACK PRO v4 — NEW FEATURES
//  All code inside the main <script> block — guaranteed scope
// ═══════════════════════════════════════════════════════════════

// ── CONFIG ──
var IMP_CONFIG=[
  {key:'highYield',icon:'⭐',label:'High Yield',cls:'ib-hy',onCls:'on-hy'},
  {key:'vivaImportant',icon:'🔥',label:'Viva',cls:'ib-viva',onCls:'on-viva'},
  {key:'conceptual',icon:'🧠',label:'Conceptual',cls:'ib-concept',onCls:'on-concept'},
  {key:'clinical',icon:'🩺',label:'Clinical',cls:'ib-clinical',onCls:'on-clinical'}
];
var STATE_LABELS={'new':'New','learning':'Learning','struggling':'⚠ Struggling','risk':'⏰ Risk','stable':'Stable','mastered':'✅ Mastered','examready':'🏆 Exam Ready'};
var STATE_CLS={'new':'sp-new','learning':'sp-learning','struggling':'sp-struggling','risk':'sp-risk','stable':'sp-stable','mastered':'sp-mastered','examready':'sp-examready'};

// ── DATA MIGRATION ──
function v4Migrate(){
  getAllTopics().forEach(function(x){
    x.topic.importance=x.topic.importance||[];
  });
  state.v4Diary=state.v4Diary||{};
  state.examMode=state.examMode||false;
  state.viewMode=state.viewMode||'chapters';
  state.folders.forEach(function(f){f.subfolders=f.subfolders||[];f.topics=f.topics||[];(f.subfolders||[]).forEach(function(sf){sf.subfolders=sf.subfolders||[];sf.topics=sf.topics||[];});});
}

// ── HELPERS ──
function v4EscHtml(s){return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');}
function v4GetState(topic){
  var today=todayStr();
  if(!topic.stages.reading&&!topic.stages.lectures&&!topic.stages.notes)return 'new';
  if(topic.stages.mastered&&topic.interval>=60)return 'examready';
  if(topic.stages.mastered)return 'mastered';
  if(topic.weakFlag)return 'struggling';
  if(topic.nextReviewDate&&topic.nextReviewDate<today)return 'risk';
  if(topic.repetitions>=3)return 'stable';
  if(topic.stages.reading||topic.stages.lectures)return 'learning';
  return 'new';
}
function v4ImpHtml(imp){
  if(!imp||!imp.length)return '';
  return imp.map(function(k){var c=IMP_CONFIG.find(function(x){return x.key===k;});return c?'<span class="imp-badge '+c.cls+'">'+c.icon+' '+c.label+'</span>':'';}).join('');
}
function v4ImpToggles(topicId,imp){
  imp=imp||[];
  return IMP_CONFIG.map(function(c){
    var on=imp.indexOf(c.key)>=0;
    return '<button class="imp-btn '+(on?c.onCls:'')+'" onclick="v4ToggleImp(\''+topicId+'\',\''+c.key+'\')" title="'+c.label+'">'+c.icon+' '+c.label+'</button>';
  }).join('');
}
function v4ToggleImp(topicId,key){
  var r=findTopic(topicId);if(!r)return;
  var t=r.topic;t.importance=t.importance||[];
  var idx=t.importance.indexOf(key);
  if(idx>=0)t.importance.splice(idx,1);else t.importance.push(key);
  saveState();
  // Update imp badges in whatever row is showing
  var badgesEl=document.getElementById('v4ib-'+topicId);
  if(badgesEl)badgesEl.innerHTML=v4ImpHtml(t.importance);
  // Update focus overlay if open
  if(v4FocusId===topicId&&document.getElementById('v4-focus-overlay').classList.contains('show')){
    document.getElementById('v4-focus-imp-hdr').innerHTML=v4ImpHtml(t.importance);
    document.getElementById('v4-focus-imp-toggles').innerHTML=v4ImpToggles(topicId,t.importance);
  }
}

// ── IMPORTANCE IN EXISTING TOPIC BODY ──
var _v4OrigBuildTopicBody=buildTopicBody;
buildTopicBody=function(topic){
  var html=_v4OrigBuildTopicBody(topic);
  var sk=v4GetState(topic);
  var tid=topic.id;
  // Build injection using a helper to avoid quote nesting issues
  var inject=v4BuildTopicInjection(tid,sk,topic.importance||[]);
  // Find and replace the original delete div
  var needle='<div style="margin-top:12px;text-align:right"><button class="btn btn-danger btn-sm" onclick="deleteTopic(\'\''+tid+'\'\')\">\\u{1F5D1}\\uFE0F Delete Topic</button></div>';
  var i=html.indexOf('<div style="margin-top:12px;text-align:right">');
  if(i>=0){html=html.slice(0,i)+inject+html.slice(html.indexOf('</div>',i)+6);}
  else{html+=inject;}
  return html;
};
function v4BuildTopicInjection(tid,sk,importance){
  var focusOnclick='v4OpenFocus(&quot;'+tid+'&quot;)';
  var deleteOnclick='deleteTopic(&quot;'+tid+'&quot;)';
  return '<div class="topic-section-title" style="margin-top:14px">'+
    '\\u{1F3F7}\\uFE0F Classification '+
    '<span class="state-pill '+STATE_CLS[sk]+'" style="display:inline;margin-left:6px">'+STATE_LABELS[sk]+'</span>'+
    '</div>'+
    '<div class="imp-toggle-row">'+v4ImpToggles(tid,importance)+'</div>'+
    '<div class="imp-badges-row" id="v4ib-'+tid+'">'+v4ImpHtml(importance)+'</div>'+
    '<div style="margin-top:12px;display:flex;align-items:center;gap:8px;flex-wrap:wrap">'+
      '<button class="btn btn-primary btn-sm" onclick="'+focusOnclick+'">\\u{1F3AF} Deep Focus Mode</button>'+
      '<button class="btn btn-danger btn-sm" onclick="'+deleteOnclick+'">\\u{1F5D1}\\uFE0F Delete Topic</button>'+
    '</div>';
}

// ── CHAPTER CARD SYSTEM ──
var v4ChapExp={};
function v4GetTopicPct(topic){return Math.round((STAGE_KEYS.filter(function(k){return topic.stages[k];}).length/STAGE_KEYS.length)*100);}
function v4Ring(pct,size,sw,color){var r=size/2-sw;var circ=2*Math.PI*r;var offset=circ*(1-pct/100);return '<svg width="'+size+'" height="'+size+'" viewBox="0 0 '+size+' '+size+'" style="transform:rotate(-90deg)"><circle cx="'+size/2+'" cy="'+size/2+'" r="'+r+'" fill="none" stroke="var(--surface3)" stroke-width="'+sw+'"/><circle cx="'+size/2+'" cy="'+size/2+'" r="'+r+'" fill="none" stroke="'+color+'" stroke-width="'+sw+'" stroke-linecap="round" stroke-dasharray="'+circ+'" stroke-dashoffset="'+offset+'"/></svg>';}

function v4BuildNbRow(topic){
  var pct=v4GetTopicPct(topic);
  var stateKey=v4GetState(topic);
  var color=pct===100?'var(--green)':'var(--primary)';
  var rev=getNextReviewLabel(topic);
  return '<div class="nb-row '+(topic.stages.mastered?'ms':'')+'" id="v4nbr-'+topic.id+'">'+
    '<div class="topic-mini-ring">'+v4Ring(pct,26,3,color)+'<div class="topic-mini-pct">'+pct+'%</div></div>'+
    '<div style="flex:1;min-width:0" onclick="v4OpenFocus(\''+topic.id+'\')">'+
      '<div class="nb-name">'+v4EscHtml(topic.name)+'</div>'+
      '<div class="imp-badges-row" id="v4ib-'+topic.id+'">'+v4ImpHtml(topic.importance||[])+'</div>'+
    '</div>'+
    '<div class="nb-meta">'+
      (rev?'<span class="next-rev-badge '+rev.cls+'" style="font-size:10px">'+rev.label+'</span>':'')+
      '<span class="state-pill '+STATE_CLS[stateKey]+'">'+STATE_LABELS[stateKey]+'</span>'+
      '<span onclick="event.stopPropagation();v4ToggleStar(\''+topic.id+'\')" style="font-size:13px;cursor:pointer" title="Star">'+(topic.starred?'⭐':'☆')+'</span>'+
      '<button class="icon-btn" onclick="event.stopPropagation();v4OpenFocus(\''+topic.id+'\')" title="Deep Focus" style="font-size:11px;padding:3px 5px">🎯</button>'+
      '<button class="icon-btn danger" onclick="event.stopPropagation();if(confirm(\'Delete this topic?\'))deleteTopic(\''+topic.id+'\')" style="font-size:11px;padding:3px 5px">🗑️</button>'+
    '</div>'+
  '</div>';
}

function v4ToggleStar(topicId){
  var r=findTopic(topicId);if(!r)return;
  r.topic.starred=!r.topic.starred;saveState();
  var row=document.getElementById('v4nbr-'+topicId);
  if(row){var star=row.querySelector('.nb-meta span');if(star)star.textContent=r.topic.starred?'⭐':'☆';}
}

function v4BuildChapterCard(sf){
  var today=todayStr();
  var allT=getFolderTopicsRec(sf);
  var total=allT.length,mastered=allT.filter(function(t){return t.stages.mastered;}).length;
  var due=allT.filter(function(t){return t.nextReviewDate&&t.nextReviewDate<=today;}).length;
  var hy=allT.filter(function(t){return t.importance&&t.importance.indexOf('highYield')>=0;}).length;
  var weak=allT.filter(function(t){return t.weakFlag;}).length;
  var pct=total?Math.round((mastered/total)*100):0;
  var color=pct>=80?'var(--green)':pct>=40?'var(--primary)':'var(--yellow)';
  var isExp=(v4ChapExp[sf.id]!==false);

  var displayT=allT;
  if(state.examMode){
    displayT=allT.filter(function(t){return (t.importance&&(t.importance.indexOf('highYield')>=0||t.importance.indexOf('vivaImportant')>=0))||t.weakFlag;});
  }

  var topicsHtml=displayT.length
    ?displayT.map(function(t){return v4BuildNbRow(t);}).join('')
    :'<div style="color:var(--text-muted);font-size:12px;padding:8px 4px">'+(state.examMode?'No high-yield topics in this chapter.':'No topics yet.')+'</div>';

  return '<div class="chapter-card '+(isExp?'expanded':'')+' '+(due>0?'has-due':'')+'" id="v4cc-'+sf.id+'">'+
    '<div class="chapter-card-header" onclick="v4ToggleChap(\''+sf.id+'\')">'+
      '<div class="chapter-exp-icon">▶</div>'+
      '<span style="font-size:18px">📂</span>'+
      '<div class="chapter-name">'+v4EscHtml(sf.name)+'</div>'+
      '<div class="chapter-meta">'+
        (state.examMode?'<span class="chapter-badge" style="color:var(--red);border-color:rgba(255,71,87,.3);background:var(--red-dim)">⚔ WAR</span>':'')+
        (hy?'<span class="chapter-badge" style="color:var(--yellow);border-color:rgba(255,179,64,.3);background:rgba(255,179,64,.1)">⭐ '+hy+'</span>':'')+
        (due?'<span class="chapter-badge due">🔔 '+due+'</span>':'')+
        (mastered===total&&total?'<span class="chapter-badge mastered">✅</span>':'')+
        '<div class="chapter-ring">'+v4Ring(pct,34,3,color)+'<div class="chapter-pct" style="color:'+color+'">'+pct+'%</div></div>'+
      '</div>'+
      '<button class="icon-btn danger" onclick="event.stopPropagation();deleteFolder(\''+sf.id+'\')" style="font-size:11px;padding:3px 5px;flex-shrink:0" title="Delete">🗑️</button>'+
    '</div>'+
    '<div class="chapter-stats-row">'+
      '<div class="chapter-stat">📑 <b>'+total+'</b> topics</div>'+
      '<div class="chapter-stat">✅ <b class="g">'+mastered+'</b> mastered</div>'+
      (due?'<div class="chapter-stat">🔔 <b class="r">'+due+'</b> due</div>':'')+
      (weak?'<div class="chapter-stat">⚠️ <b class="y">'+weak+'</b> weak</div>':'')+
      '<div style="margin-left:auto"><button class="btn btn-primary btn-sm" onclick="event.stopPropagation();v4AddTopicToChap(\''+sf.id+'\')">+ Topic</button></div>'+
    '</div>'+
    '<div class="chapter-progress"><div class="chapter-progress-fill" style="width:'+pct+'%"></div></div>'+
    '<div class="chapter-topics-body" id="v4ctb-'+sf.id+'">'+topicsHtml+
      '<div class="chapter-add-row" onclick="v4AddTopicToChap(\''+sf.id+'\')"><span>＋</span> Add topic to '+v4EscHtml(sf.name)+'</div>'+
    '</div>'+
  '</div>';
}

function v4ToggleChap(sfId){
  v4ChapExp[sfId]=(v4ChapExp[sfId]===false)?true:false;
  var card=document.getElementById('v4cc-'+sfId);if(!card)return;
  card.classList.toggle('expanded',v4ChapExp[sfId]!==false);
}

function v4AddTopicToChap(folderId){currentFolderId=folderId;openAddTopic();}

function v4RenderChapterView(folder){
  var container=document.getElementById('subfolder-list-container');
  var topicsEl=document.getElementById('topics-list');
  if(!container||!topicsEl)return;
  var today=todayStr();
  var allT=getFolderTopicsRec(folder);
  var urgent=allT.filter(function(t){return t.nextReviewDate&&t.nextReviewDate<=today&&!t.stages.mastered;});
  var html='';

  // Urgent panel
  if(urgent.length){
    html+='<div class="urgent-panel"><div class="urgent-title">⚡ Urgent Revision ('+urgent.length+')</div><div class="urgent-chips">'+
      urgent.slice(0,10).map(function(t){return '<div class="uchip" onclick="v4OpenFocus(\''+t.id+'\')">'+v4EscHtml(t.name)+'</div>';}).join('')+
    '</div></div>';
  }

  // Controls row
  html+='<div style="display:flex;align-items:center;gap:8px;margin-bottom:12px;flex-wrap:wrap">'+
    '<div class="view-mode-toggle">'+
      '<button class="vm-btn '+(state.viewMode==='chapters'?'active':'')+'" onclick="v4SetMode(\'chapters\')">📂 Chapters</button>'+
      '<button class="vm-btn '+(state.viewMode==='list'?'active':'')+'" onclick="v4SetMode(\'list\')">📋 List</button>'+
    '</div>'+
    (state.examMode?
      '<div class="exam-banner" style="flex:1;margin-bottom:0"><span class="exam-badge">⚔ WAR</span><span class="exam-text">High Yield + Weak only &mdash; <strong>'+allT.filter(function(t){return (t.importance&&(t.importance.indexOf('highYield')>=0||t.importance.indexOf('vivaImportant')>=0))||t.weakFlag;}).length+' topics</strong></span><button class="btn btn-danger btn-sm" onclick="v4ToggleExam()">Exit</button></div>':
      '<button class="btn btn-sm" style="background:var(--red-dim);border:1px solid rgba(255,71,87,.3);color:var(--red);margin-left:auto" onclick="v4ToggleExam()">⚔ Exam War Mode</button>'
    )+
  '</div>';

  if(state.viewMode==='chapters'){
    if(folder.subfolders&&folder.subfolders.length){
      html+='<div class="chapter-cards-wrap">'+folder.subfolders.map(function(sf){return v4BuildChapterCard(sf);}).join('')+'</div>';
    }
    if(folder.topics&&folder.topics.length){
      html+='<div class="section-title" style="margin:14px 0 8px">📑 Uncategorized in '+v4EscHtml(folder.name)+'</div>';
      topicsEl.innerHTML=getFilteredTopics(folder.topics).map(function(t){return buildTopicCard(t);}).join('');
    }else{topicsEl.innerHTML='';}
    container.innerHTML=html;
  }else{
    // Classic list mode
    if(folder.subfolders&&folder.subfolders.length){
      html+='<div class="subfolder-list">'+folder.subfolders.map(function(sf){
        var a=getFolderTopicsRec(sf),tot=a.length,mas=a.filter(function(t){return t.stages.mastered;}).length;
        var d=a.filter(function(t){return t.nextReviewDate&&t.nextReviewDate<=today;}).length;
        return '<div class="subfolder-item" onclick="navigateToFolder(\''+sf.id+'\')"><span style="font-size:18px">📂</span><div style="flex:1"><div style="font-family:var(--font-head);font-size:13px;font-weight:700">'+v4EscHtml(sf.name)+'</div><div style="font-size:11px;color:var(--text-muted)">'+tot+' topics · '+mas+' mastered'+(d?' · 🔔 '+d+' due':'')+'</div></div><button class="icon-btn danger" onclick="event.stopPropagation();deleteFolder(\''+sf.id+'\')" style="margin-right:6px">🗑️</button><span class="subfolder-arrow">›</span></div>';
      }).join('')+'</div>';
      if(folder.topics&&folder.topics.length)html+='<div class="section-title" style="margin-top:14px">📑 Topics in '+v4EscHtml(folder.name)+'</div>';
    }
    topicsEl.innerHTML=getFilteredTopics(folder.topics).map(function(t){return buildTopicCard(t);}).join('');
    container.innerHTML=html;
  }
}

function v4SetMode(m){state.viewMode=m;saveState();renderSubfolderView();}
function v4ToggleExam(){state.examMode=!state.examMode;saveState();renderSubfolderView();showToast(state.examMode?'⚔️ Exam War Mode ON — High Yield only!':'Exam mode off','info');}

// ── OVERRIDE renderSubfolderView ──
renderSubfolderView=function(){
  var folder=findFolder(currentFolderId);if(!folder){showView('folders');return;}
  var titleEl=document.getElementById('subfolder-view-title');
  var bcEl=document.getElementById('subfolder-breadcrumb');
  if(titleEl)titleEl.textContent=(folder.icon||'📂')+' '+folder.name;
  var crumbs=[],cur=folder;
  while(cur){crumbs.unshift(cur);cur=cur.parentId?findFolder(cur.parentId):null;}
  if(bcEl)bcEl.innerHTML='<span class="breadcrumb-item" onclick="showView(\'folders\')">📁 Root</span>'+crumbs.map(function(c,i){return '<span class="breadcrumb-sep">›</span><span class="breadcrumb-item '+(i===crumbs.length-1?'active':'')+'" onclick="navigateToFolder(\''+c.id+'\')">'+(c.icon||'📂')+' '+v4EscHtml(c.name)+'</span>';}).join('');
  v4RenderChapterView(folder);
};

// ── DEEP FOCUS MODE ──
var v4FocusId=null,v4FocusTick=0,v4FocusIv=null;
function v4OpenFocus(topicId){
  var r=findTopic(topicId);if(!r)return;
  v4FocusId=topicId;var t=r.topic;
  var ov=document.getElementById('v4-focus-overlay');if(!ov)return;
  ov.classList.add('show');ov.scrollTop=0;
  document.getElementById('v4-focus-title').textContent=t.name;
  document.getElementById('v4-focus-imp-hdr').innerHTML=v4ImpHtml(t.importance||[]);
  document.getElementById('v4-ai-resp')||null;
  var ar=document.getElementById('v4-ai-resp');if(ar){ar.style.display='none';ar.textContent='';}
  document.getElementById('v4-focus-imp-toggles').innerHTML=v4ImpToggles(topicId,t.importance||[]);
  document.getElementById('v4-focus-notes').value=t.notes||'';
  v4RenderFocusStages(t);
  v4RenderFocusRes(t);
  v4RenderFocusSm2(t);
  v4RenderFocusPrompts(topicId);
  v4RenderFocusMicro(topicId);
  v4FocusTick=0;clearInterval(v4FocusIv);
  v4FocusIv=setInterval(function(){
    v4FocusTick++;
    var m=Math.floor(v4FocusTick/60),s=v4FocusTick%60;
    var cl=document.getElementById('v4-focus-clock');if(cl)cl.textContent=pad(m)+':'+pad(s);
  },1000);
}
function v4CloseFocus(){
  clearInterval(v4FocusIv);
  var ov=document.getElementById('v4-focus-overlay');if(ov)ov.classList.remove('show');
  v4FocusId=null;
  if(currentView==='subfolder')renderSubfolderView();
}
function v4SaveNotes(){
  if(!v4FocusId)return;var r=findTopic(v4FocusId);if(!r)return;
  r.topic.notes=document.getElementById('v4-focus-notes').value;saveState();
}
function v4RenderFocusStages(t){
  var el=document.getElementById('v4-focus-stages');if(!el)return;
  el.innerHTML=STAGE_KEYS.map(function(k){
    var done=t.stages[k];
    return '<button class="focus-stage '+(done?'done ':'')+( k==='mastered'?'ms-stage':'')+'" onclick="v4FocusToggleStage(\''+t.id+'\',\''+k+'\')">'+
      '<span style="font-size:16px">'+STAGE_ICONS[k]+'</span>'+
      '<span>'+STAGE_LABELS[k]+(done?' ✓':'')+'</span>'+
    '</button>';
  }).join('');
}
function v4FocusToggleStage(topicId,key){
  toggleStage(topicId,key);
  var r=findTopic(topicId);if(!r)return;
  v4RenderFocusStages(r.topic);
  v4RenderFocusSm2(r.topic);
}
function v4RenderFocusRes(t){
  var el=document.getElementById('v4-focus-res');if(!el)return;
  var links=(t.links||[]).map(function(l){return '<a href="'+v4EscHtml(l.url)+'" target="_blank" class="chapter-badge" style="text-decoration:none;color:var(--blue);margin-right:4px">🔗 '+(l.title||'Link')+'</a>';}).join('');
  el.innerHTML=
    '<div style="display:flex;gap:6px;flex-wrap:wrap;margin-bottom:8px">'+
      (t.resources&&t.resources.book?'<span class="chapter-badge">📗 '+v4EscHtml(t.resources.book)+'</span>':'')+
      (t.resources&&t.resources.video?'<span class="chapter-badge">🎥 '+v4EscHtml(t.resources.video)+'</span>':'')+
      links+
    '</div>'+
    '<div style="display:flex;gap:6px">'+
      '<input class="mini-input" placeholder="📗 Textbook ref" value="'+(t.resources&&t.resources.book?v4EscHtml(t.resources.book):'')+'" oninput="v4SaveRes(\'book\',this.value)" style="flex:1">'+
      '<input class="mini-input" placeholder="🎥 Video ref" value="'+(t.resources&&t.resources.video?v4EscHtml(t.resources.video):'')+'" oninput="v4SaveRes(\'video\',this.value)" style="flex:1">'+
    '</div>';
}
function v4SaveRes(key,val){if(!v4FocusId)return;var r=findTopic(v4FocusId);if(!r)return;r.topic.resources=r.topic.resources||{};r.topic.resources[key]=val;saveState();}
function v4RenderFocusSm2(t){
  var el=document.getElementById('v4-focus-sm2');if(!el)return;
  el.innerHTML='<div class="sm2-panel"><div class="sm2-info">'+
    '<span>Interval: <b>'+(t.interval||0)+'d</b></span>'+
    '<span>Reps: <b>'+(t.repetitions||0)+'</b></span>'+
    (t.lastReviewDate?'<span>Last: <b>'+t.lastReviewDate+'</b></span>':'')+
    (t.nextReviewDate?'<span>Next: <b>'+t.nextReviewDate+'</b></span>':'')+
    '</div>'+
    '<button class="btn btn-primary btn-sm" style="margin-top:8px" onclick="openMasteryCheck(\''+t.id+'\')">🏆 Mastery Check — Next: '+calcNextInterval(t)+'d</button>'+
  '</div>';
}
function v4RenderFocusPrompts(topicId){
  var el=document.getElementById('v4-focus-prompts');if(!el)return;
  var types=[['concept','🧠 Explain'],['mcq','📝 MCQs'],['recall','🔁 Recall'],['teach','🗣️ Feynman'],['htmlStudyFile','✨ Full File']];
  el.innerHTML=types.map(function(p){
    return '<button class="f-ai-btn" onclick="copyPrompt(\''+topicId+'\',\''+p[0]+'\')">'+p[1]+' <span id="ci-'+topicId+'-'+p[0]+'" class="copy-indicator"></span></button>';
  }).join('');
}
function v4RenderFocusMicro(topicId){
  var sec=document.getElementById('v4-micro-sec');var cont=document.getElementById('v4-micro-steps');if(!sec||!cont)return;
  var sched=state.dailySchedule;if(!sched||!sched.items){sec.style.display='none';return;}
  var slot=sched.items.find(function(s){return s.topicId===topicId;});
  if(!slot||!slot.microSteps||!slot.microSteps.length){sec.style.display='none';return;}
  sec.style.display='';var mc=sched.microChecks||{};
  cont.innerHTML=slot.microSteps.map(function(step,si){
    var key=slot.id+'_'+si,chk=!!mc[key];
    return '<div class="focus-micro-item '+(chk?'done-m':'')+'" onclick="toggleMicroStep(\''+slot.id+'\','+si+')">'+
      '<input type="checkbox" '+(chk?'checked':'')+' style="width:15px;height:15px;accent-color:var(--primary);flex-shrink:0">'+
      '<span>'+v4EscHtml(step)+'</span></div>';
  }).join('');
}

// ── FOCUS AI CALLS ──
async function v4AiCall(prompt){
  var respEl=document.getElementById('v4-ai-resp');if(!respEl)return;
  respEl.style.display='block';respEl.textContent='⏳ Generating...';
  var r=findTopic(v4FocusId);if(!r){respEl.textContent='Error: topic not found.';return;}
  try{
    var resp=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({
      model:'claude-sonnet-4-20250514',max_tokens:1000,
      system:'You are a concise medical education expert for MBBS students. Be exam-focused and clinically accurate. Use bullet points.',
      messages:[{role:'user',content:prompt+'\n\nTopic: '+r.topic.name}]
    })});
    var data=await resp.json();
    var text=data.content&&data.content[0]?data.content[0].text:'No response.';
    respEl.textContent=text;
  }catch(e){respEl.textContent='Error: '+e.message;}
}
function v4AiRecall(){v4AiCall('Generate 5 active recall questions (no answers upfront) that test deep understanding.');}
function v4AiViva(){v4AiCall('Generate 5 viva voce questions a clinical examiner would ask, with brief expected answer points.');}
function v4AiMnemo(){v4AiCall('Create 2-3 powerful, memorable mnemonics for the most important facts.');}
function v4AiSummary(){v4AiCall('Give a concise high-yield summary in 8-10 bullet points covering the key MBBS exam facts.');}

// ── AI ASSISTANT MODAL ──
var v4AiMsgs=[];
function v4OpenAi(){document.getElementById('v4-ai-overlay').classList.add('show');}
function v4CloseAi(){document.getElementById('v4-ai-overlay').classList.remove('show');}
function v4AiQuick(msg){var inp=document.getElementById('v4-ai-input');if(inp)inp.value=msg;v4SendAi();}
async function v4SendAi(){
  var inp=document.getElementById('v4-ai-input');if(!inp)return;
  var msg=inp.value.trim();if(!msg)return;inp.value='';
  var btn=document.getElementById('v4-ai-send');if(btn){btn.disabled=true;btn.textContent='...';}
  var allT=getAllTopics();var due=getDueTopics();var weak=allT.filter(function(x){return x.topic.weakFlag;});
  var ctx='User has '+allT.length+' topics, '+due.length+' due today, '+weak.length+' weak topics. Streak: '+state.streak.current+' days.';
  if(weak.length)ctx+='\nWeak: '+weak.slice(0,5).map(function(x){return x.topic.name;}).join(', ');
  v4AiMsgs.push({role:'user',content:msg});
  v4AppendAiMsg('u','👤 You',msg);
  var chat=document.getElementById('v4-ai-chat');
  var loadEl=document.createElement('div');loadEl.className='ai-msg';
  loadEl.innerHTML='<div class="ai-msg-role a">🧠 MedTrack AI</div><div class="ai-msg-body">⏳ Thinking...</div>';
  if(chat){chat.appendChild(loadEl);chat.scrollTop=chat.scrollHeight;}
  try{
    var resp=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({
      model:'claude-sonnet-4-20250514',max_tokens:1000,
      system:'You are MedTrack AI, an intelligent medical study assistant. Context: '+ctx+'\n\nBe concise, clinically accurate, and encouraging.',
      messages:v4AiMsgs.map(function(m){return{role:m.role,content:m.content};})
    })});
    var data=await resp.json();
    var text=data.content&&data.content[0]?data.content[0].text:'No response.';
    v4AiMsgs.push({role:'assistant',content:text});
    var bodyEl=loadEl.querySelector('.ai-msg-body');
    bodyEl.textContent='';
    var words=text.split(' ');var wi=0;
    var iv=setInterval(function(){if(wi>=words.length){clearInterval(iv);return;}bodyEl.textContent+=(wi>0?' ':'')+words[wi++];if(chat)chat.scrollTop=chat.scrollHeight;},15);
  }catch(e){loadEl.querySelector('.ai-msg-body').textContent='Connection error. Please try again.';}
  if(btn){btn.disabled=false;btn.textContent='Send';}
}
function v4AppendAiMsg(roleClass,roleName,text){
  var chat=document.getElementById('v4-ai-chat');if(!chat)return;
  var el=document.createElement('div');el.className='ai-msg';
  el.innerHTML='<div class="ai-msg-role '+roleClass+'">'+roleName+'</div><div class="ai-msg-body '+(roleClass==='u'?'u-msg':'')+'">'+v4EscHtml(text)+'</div>';
  chat.appendChild(el);chat.scrollTop=chat.scrollHeight;
}

// ── REFLECTION MODAL ──
function v4OpenReflect(){
  var today=todayStr();
  var el=document.getElementById('v4-reflect-date');if(el)el.textContent='Journal — '+today;
  var entry=(state.v4Diary&&state.v4Diary[today])||{};
  var es=document.getElementById('v4-energy');if(es){es.value=entry.energy||7;document.getElementById('v4-energy-val').textContent=entry.energy||7;}
  var fs=document.getElementById('v4-focus-score');if(fs){fs.value=entry.focus||7;document.getElementById('v4-focus-val').textContent=entry.focus||7;}
  var a=document.getElementById('v4-accomplished');if(a)a.value=entry.accomplished||'';
  var s=document.getElementById('v4-struggled');if(s)s.value=entry.struggled||'';
  var t=document.getElementById('v4-tomorrow');if(t)t.value=entry.tomorrow||'';
  document.getElementById('v4-reflect-overlay').classList.add('show');
}
function v4CloseReflect(){document.getElementById('v4-reflect-overlay').classList.remove('show');}
function v4SaveReflect(){
  var today=todayStr();state.v4Diary=state.v4Diary||{};
  state.v4Diary[today]={
    energy:parseInt(document.getElementById('v4-energy').value)||7,
    focus:parseInt(document.getElementById('v4-focus-score').value)||7,
    accomplished:document.getElementById('v4-accomplished').value,
    struggled:document.getElementById('v4-struggled').value,
    tomorrow:document.getElementById('v4-tomorrow').value
  };
  saveState();v4CloseReflect();showToast('📔 Reflection saved!','success');
  v4RenderReflectPreview();
}
function v4RenderReflectPreview(){
  var el=document.getElementById('v4-reflect-preview');if(!el)return;
  var today=todayStr();var e=(state.v4Diary&&state.v4Diary[today]);
  el.textContent=e?'⚡ Energy: '+e.energy+'/10 · 🎯 Focus: '+e.focus+'/10'+(e.accomplished?' · '+e.accomplished.slice(0,40):''):'No reflection yet today.';
}

// ── RETENTION MAP ──
function v4RenderRetentionMap(){
  var el=document.getElementById('v4-ret-map');if(!el)return;
  var today=todayStr();
  if(!state.folders.length){el.innerHTML='<div style="color:var(--text-muted);font-size:12px;padding:16px;text-align:center">No subjects yet.</div>';return;}
  var allT=getAllTopics();var totalAll=allT.length;var mastAll=allT.filter(function(x){return x.topic.stages.mastered;}).length;
  var overallPct=totalAll?Math.round((mastAll/totalAll)*100):0;
  var oc=overallPct>=60?'var(--green)':'var(--primary)';
  var html='<div style="display:flex;align-items:center;gap:10px;margin-bottom:12px">'+
    '<div style="font-size:12px;color:var(--text-muted);flex-shrink:0">Overall</div>'+
    '<div style="flex:1;height:5px;background:var(--surface3);border-radius:3px;overflow:hidden"><div style="height:100%;width:'+overallPct+'%;background:linear-gradient(90deg,var(--primary),var(--green));border-radius:3px;transition:width .6s"></div></div>'+
    '<div style="font-family:var(--font-mono);font-weight:800;color:'+oc+'">'+overallPct+'%</div>'+
  '</div><div class="ret-grid">';
  state.folders.forEach(function(f){
    var a=getFolderTopicsRec(f);var tot=a.length;if(!tot)return;
    var mas=a.filter(function(t){return t.stages.mastered;}).length;
    var due=a.filter(function(t){return t.nextReviewDate&&t.nextReviewDate<=today;}).length;
    var wk=a.filter(function(t){return t.weakFlag;}).length;
    var p=Math.round((mas/tot)*100);
    var c=p>=80?'var(--green)':p>=50?'var(--primary)':p>=25?'var(--yellow)':'var(--red)';
    html+='<div class="ret-subject" onclick="navigateToFolder(\''+f.id+'\')">'+
      '<div class="ret-name">'+(f.icon||'📁')+' '+v4EscHtml(f.name)+'</div>'+
      '<div class="ret-pct" style="color:'+c+'">'+p+'%</div>'+
      '<div class="ret-bar"><div class="ret-fill" style="width:'+p+'%;background:'+c+'"></div></div>'+
      '<div class="ret-sub">'+mas+'/'+tot+(due?' · 🔔'+due:'')+(wk?' · ⚠️'+wk:'')+'</div>'+
    '</div>';
  });
  html+='</div>';el.innerHTML=html;
}

// ── OVERRIDE renderAnalytics ──
var _v4OrigAnalytics=renderAnalytics;
renderAnalytics=function(){
  try{_v4OrigAnalytics();}catch(e){console.warn('analytics err',e);}
  setTimeout(function(){try{v4RenderRetentionMap();}catch(e2){}},80);
};

// ── INTELLIGENCE BRIEF ──
function v4RenderBrief(){
  var el=document.getElementById('v4-brief');if(!el)return;
  var today=todayStr();var allT=getAllTopics();
  if(!allT.length){el.innerHTML='';return;}
  var due=getDueTopics();
  var overdue=due.filter(function(x){return x.topic.nextReviewDate<today;});
  var dueToday=due.filter(function(x){return x.topic.nextReviewDate===today;});
  var weak=allT.filter(function(x){return x.topic.weakFlag;});
  var hy=allT.filter(function(x){return x.topic.importance&&x.topic.importance.indexOf('highYield')>=0;});
  var mas=allT.filter(function(x){return x.topic.stages.mastered;});
  var s=state.streak.current;
  var ms=s===7?'🎉 7-Day Streak!':s===14?'🔥 2 Weeks!':s===30?'🏆 30 Days!':s>0&&s%10===0?'💪 '+s+' Days!':'';
  var html=ms?'<div style="text-align:center;margin-bottom:9px"><span class="milestone">'+ms+'</span></div>':'';
  html+='<div class="brief-row">';
  if(overdue.length)html+='<div class="brief-chip" style="border-color:rgba(255,71,87,.3);cursor:pointer" onclick="startRevisionSession()"><span>⚡</span><div><div class="bc-val" style="color:var(--red)">'+overdue.length+'</div><div class="bc-lbl">Overdue</div></div></div>';
  if(dueToday.length)html+='<div class="brief-chip" style="border-color:rgba(0,210,190,.2);cursor:pointer" onclick="startRevisionSession()"><span>🔔</span><div><div class="bc-val" style="color:var(--primary)">'+dueToday.length+'</div><div class="bc-lbl">Due Today</div></div></div>';
  if(weak.length)html+='<div class="brief-chip" style="border-color:rgba(255,179,64,.2);cursor:pointer" onclick="showWeakAreas()"><span>⚠️</span><div><div class="bc-val" style="color:var(--yellow)">'+weak.length+'</div><div class="bc-lbl">Weak</div></div></div>';
  if(hy.length)html+='<div class="brief-chip" onclick="showView(\'folders\')" style="cursor:pointer"><span>⭐</span><div><div class="bc-val" style="color:var(--yellow)">'+hy.length+'</div><div class="bc-lbl">High Yield</div></div></div>';
  html+='<div class="brief-chip"><span>🏆</span><div><div class="bc-val" style="color:var(--green)">'+mas.length+'</div><div class="bc-lbl">Mastered</div></div></div>';
  html+='</div>';el.innerHTML=html;
}

// ── CONFIDENCE REVISION (override startRevisionSession + renderRevisionView) ──
var v4RevQueue=[],v4RevIdx=0;
function startRevisionSession(){
  var due=getDueTopics();
  if(!due.length){showToast('🎉 All caught up!','success');return;}
  due.sort(function(a,b){var aw=a.topic.weakFlag?1:0,bw=b.topic.weakFlag?1:0;if(bw!==aw)return bw-aw;return (a.topic.nextReviewDate||'').localeCompare(b.topic.nextReviewDate||'');});
  v4RevQueue=due;v4RevIdx=0;showView('revision');v4RenderRevCard();
}
function renderRevisionView(){
  var due=getDueTopics();
  var cont=document.getElementById('revision-content');if(!cont)return;
  if(!due.length){cont.innerHTML='<div class="rev-empty"><div class="rev-empty-icon">🎉</div><div style="font-family:var(--font-head);font-size:18px;font-weight:800;margin-bottom:8px">All caught up!</div><div style="color:var(--text-muted);font-size:14px;margin-bottom:18px">No topics due. Keep studying!</div><button class="btn btn-primary" onclick="showView(\'folders\')">Study New Topics</button></div>';return;}
  if(!v4RevQueue.length){v4RevQueue=due;v4RevIdx=0;}
  v4RenderRevCard();
}
function v4RenderRevCard(){
  var cont=document.getElementById('revision-content');if(!cont)return;
  var pb=document.getElementById('v4-rev-pb');var pf=document.getElementById('v4-rev-pf');
  if(pb&&v4RevQueue.length){pb.style.display='';if(pf)pf.style.width=Math.round((v4RevIdx/v4RevQueue.length)*100)+'%';}
  if(v4RevIdx>=v4RevQueue.length){
    if(pb)pb.style.display='none';
    cont.innerHTML='<div class="rev-empty"><div class="rev-empty-icon">🏆</div><div style="font-family:var(--font-head);font-size:20px;font-weight:800;margin-bottom:8px">Session Complete!</div><div style="color:var(--text-muted);margin-bottom:18px">Reviewed '+v4RevIdx+' topics. Excellent work!</div><button class="btn btn-primary" onclick="startRevisionSession()">New Session</button></div>';
    return;
  }
  var x=v4RevQueue[v4RevIdx],t=x.topic,f=x.folder;
  var sk=v4GetState(t);var ni=calcNextInterval(t);
  cont.innerHTML=
    '<div style="font-size:12px;color:var(--text-muted);margin-bottom:10px">'+(v4RevIdx+1)+' / '+v4RevQueue.length+' &nbsp;·&nbsp; '+(v4RevQueue.length-v4RevIdx)+' remaining</div>'+
    '<div class="rev-card-v4">'+
      '<div class="rcv-title">'+v4EscHtml(t.name)+'</div>'+
      '<div class="rcv-folder">'+(f.icon||'📁')+' '+v4EscHtml(f.name)+' &nbsp;<span class="state-pill '+STATE_CLS[sk]+'" style="display:inline">'+STATE_LABELS[sk]+'</span></div>'+
      (v4ImpHtml(t.importance)?'<div style="margin-bottom:10px">'+v4ImpHtml(t.importance)+'</div>':'')+
      (t.notes?'<div style="background:var(--surface2);border-radius:var(--r-sm);padding:10px;font-size:12px;line-height:1.5;margin-bottom:12px;max-height:80px;overflow-y:auto">📝 '+v4EscHtml(t.notes.slice(0,200))+'</div>':'')+
      '<div style="font-size:11px;color:var(--text-muted);margin-bottom:12px">Interval: <b>'+(t.interval||0)+'d</b> &nbsp; Reps: <b>'+(t.repetitions||0)+'</b>'+(t.lastReviewDate?' &nbsp; Last: <b>'+t.lastReviewDate+'</b>':'')+'</div>'+
      '<div class="focus-sec-title" style="margin-bottom:8px">How well did you recall this?</div>'+
      '<div class="conf-btns">'+
        '<button class="conf-btn cb-0" onclick="v4SubmitConf(0)">😵 Blackout<br><small style="font-weight:400">Reset → tomorrow</small></button>'+
        '<button class="conf-btn cb-2" onclick="v4SubmitConf(2)">😓 Hard<br><small style="font-weight:400">Short interval</small></button>'+
        '<button class="conf-btn cb-4" onclick="v4SubmitConf(4)">👍 Good<br><small style="font-weight:400">+'+ni+'d</small></button>'+
        '<button class="conf-btn cb-5" onclick="v4SubmitConf(5)">🚀 Easy<br><small style="font-weight:400">+'+Math.round(ni*1.2)+'d</small></button>'+
      '</div>'+
      '<div style="margin-top:11px;display:flex;gap:7px;flex-wrap:wrap">'+
        '<button class="btn btn-ghost btn-sm" onclick="v4OpenFocus(\''+t.id+'\')">🎯 Deep Study</button>'+
        '<button class="btn btn-ghost btn-sm" onclick="copyPrompt(\''+t.id+'\',\'recall\')">📋 Copy Prompt</button>'+
        '<button class="btn btn-ghost btn-sm" onclick="v4RevIdx++;v4RenderRevCard()">⏭ Skip</button>'+
      '</div>'+
    '</div>';
}
function v4SubmitConf(score){
  if(v4RevIdx>=v4RevQueue.length)return;
  var x=v4RevQueue[v4RevIdx],t=x.topic;
  var reps=t.repetitions||0;
  if(score<3){t.repetitions=0;t.interval=1;if(score===0)t.weakFlag=true;}
  else{if(score===5)t.weakFlag=false;t.repetitions=reps+1;var base=SR_INTERVALS[Math.min(t.repetitions,SR_INTERVALS.length-1)];t.interval=Math.round(base*(score===5?1.2:1.0));}
  var today=todayStr();t.lastReviewDate=today;t.nextReviewDate=addDays(today,t.interval);
  state.heatmap[today]=(state.heatmap[today]||0)+1;trackWeeklyStudied(t.id);updateStreak();saveState();
  if(score>=4)showToast('🚀 "'+t.name+'" — next in '+t.interval+'d','success');
  else if(score===0)showToast('💪 Rescheduled "'+t.name+'" for tomorrow','info');
  v4RevIdx++;v4RenderRevCard();
}

// ── ADD NAV BUTTONS ──
function v4AddNavBtns(){
  var nr=document.querySelector('.nav-right');if(!nr)return;
  if(document.getElementById('v4-btn-ai'))return;
  var b1=document.createElement('button');b1.className='nav-btn';b1.id='v4-btn-ai';
  b1.innerHTML='🧠 <span class="hide-mob">AI</span>';b1.title='Medical AI Assistant';b1.onclick=v4OpenAi;
  var b2=document.createElement('button');b2.className='nav-btn';b2.id='v4-btn-reflect';
  b2.innerHTML='📔 <span class="hide-mob">Reflect</span>';b2.title='Daily Study Reflection';b2.onclick=v4OpenReflect;
  nr.insertBefore(b2,nr.firstChild);nr.insertBefore(b1,nr.firstChild);
}

// ── ADD RETENTION MAP & BRIEF TO DOM ──
function v4AddDomElements(){
  // Brief above stats grid
  var sg=document.getElementById('home-stats-grid');
  if(sg&&!document.getElementById('v4-brief')){
    var div=document.createElement('div');div.id='v4-brief';div.style.marginBottom='14px';
    sg.parentNode.insertBefore(div,sg);
  }
  // Diary preview is already in HTML (home-diary-preview) - just render it
  try{dvRenderHomePreview();}catch(e){}
  // Retention map in analytics
  var av=document.getElementById('view-analytics');
  if(av&&!document.getElementById('v4-ret-map')){
    var rc=document.createElement('div');rc.className='retention-card';
    rc.innerHTML='<div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:6px"><div class="section-title" style="margin-bottom:0">🗺 Mastery Retention Map</div></div><div style="font-size:12px;color:var(--text-muted);margin-bottom:4px">Visual mastery distribution — click any subject to navigate</div><div id="v4-ret-map">Loading...</div>';
    av.appendChild(rc);
  }
  // Progress bar for revision
  var rv=document.getElementById('view-revision');
  if(rv&&!document.getElementById('v4-rev-pb')){
    var rc2=document.getElementById('revision-content');
    if(rc2){var pb=document.createElement('div');pb.id='v4-rev-pb';pb.className='rev-prog';pb.style.display='none';pb.innerHTML='<div id="v4-rev-pf" class="rev-prog-fill" style="width:0%"></div>';rv.insertBefore(pb,rc2);}
  }
  // Add AI button to subfolder view header
  var sfh=document.querySelector('#view-subfolder .view-header > div[style]');
  // handled by nav buttons
  // Add AI + Reflect to quick-actions
  var qa=document.querySelector('.quick-actions');
  if(qa&&!document.getElementById('v4-qa-ai')){
    var ab=document.createElement('button');ab.className='btn btn-ghost btn-sm';ab.id='v4-qa-ai';ab.textContent='🧠 AI';ab.onclick=v4OpenAi;
    var rb=document.createElement('button');rb.className='btn btn-ghost btn-sm';rb.id='v4-qa-reflect';rb.textContent='📔 Reflect';rb.onclick=v4OpenReflect;
    qa.appendChild(ab);qa.appendChild(rb);
  }
}

// ── OVERRIDE renderHome to include v4 data ──
var _v4OrigRenderHome=renderHome;
renderHome=function(){
  _v4OrigRenderHome();
  try{v4RenderBrief();}catch(e){}
  try{v4RenderReflectPreview();}catch(e){}
  try{dvRenderHomePreview();}catch(e){}
};

// ── FOLDER CARDS: inject imp & state info ──
var _v4OrigRenderFolders=renderFolders;
renderFolders=function(){
  _v4OrigRenderFolders();
  // Enhance folder cards with chapter count & weak count
  var today=todayStr();
  state.folders.forEach(function(f){
    var card=document.querySelector('#folders-grid .folder-card:nth-child('+( state.folders.indexOf(f)+1)+')');
    // Simple: let original render handle it; we enhance via CSS classes
  });
};

// ── KEYBOARD SHORTCUTS ──
document.addEventListener('keydown',function(e){
  if(e.key==='Escape'){
    if(document.getElementById('v4-focus-overlay').classList.contains('show')){v4CloseFocus();return;}
    if(document.getElementById('v4-ai-overlay').classList.contains('show')){v4CloseAi();return;}
    if(document.getElementById('v4-reflect-overlay').classList.contains('show')){v4CloseReflect();return;}
  }
  if(e.altKey&&e.key.toLowerCase()==='f'){var due=getDueTopics();if(due.length)v4OpenFocus(due[0].topic.id);}
  if(e.altKey&&e.key.toLowerCase()==='a'){v4OpenAi();}
  if(e.altKey&&e.key.toLowerCase()==='r'){startRevisionSession();}
});

// ── v4 INIT ──
function v4Init(){
  try{v4Migrate();}catch(e){console.warn('v4 migrate',e);}
  try{v4AddNavBtns();}catch(e){console.warn('v4 navbtns',e);}
  try{v4AddDomElements();}catch(e){console.warn('v4 dom',e);}
  try{v4RenderBrief();}catch(e){console.warn('v4 brief',e);}
  try{v4RenderReflectPreview();}catch(e){}
}


// ═══════════════════════════════════════════════════════════════
//  MEDICAL DIARY SYSTEM — MedTrack Pro v4
// ═══════════════════════════════════════════════════════════════

var dvCurrentDate = todayStr();
var dvEditTaskId = null;

var DV_TYPE_EMOJIS = {
  study:'📖', revision:'🔁', recall:'🔁', mcq:'📝',
  clinical:'🩺', video:'🎥', diagram:'✏️', notes:'📝', break:'☕'
};
var DV_TYPE_LABELS = {
  study:'Study', revision:'Revision', recall:'Active Recall', mcq:'MCQs',
  clinical:'Clinical', video:'Video', diagram:'Diagrams', notes:'Notes', break:'Break'
};
var DV_MEDICAL_QUOTES = [
  {text:"The good physician treats the disease; the great physician treats the patient who has the disease.", author:"Sir William Osler"},
  {text:"Medicine is learned by the bedside and not in the classroom.", author:"Sir William Osler"},
  {text:"The secret of getting ahead is getting started.", author:"Mark Twain"},
  {text:"Small daily improvements over time lead to stunning results.", author:"Robin Sharma"},
  {text:"Success is the sum of small efforts repeated day in and day out.", author:"Robert Collier"},
  {text:"The only way to do great work is to love what you do.", author:"Steve Jobs"},
  {text:"An investment in knowledge pays the best interest.", author:"Benjamin Franklin"},
  {text:"A doctor who cannot take a good history has a serious handicap.", author:"Paul Dudley White"},
  {text:"Wherever the art of medicine is loved, there is also a love of humanity.", author:"Hippocrates"},
  {text:"The art of medicine consists of amusing the patient while nature cures the disease.", author:"Voltaire"},
  {text:"Study without desire spoils the memory, and it retains nothing that it takes in.", author:"Leonardo da Vinci"},
  {text:"Excellence is not a destination but a continuous journey.", author:"Brian Tracy"},
  {text:"Do not wait; the time will never be just right.", author:"Napoleon Hill"},
  {text:"Energy and persistence conquer all things.", author:"Benjamin Franklin"},
  {text:"In medicine the patient must combat the disease along with the physician.", author:"Hippocrates"}
];

// ── DATA ACCESS HELPERS ──────────────────────────────────────────
function dvGetDayData(dateStr) {
  state.v4Diary = state.v4Diary || {};
  if (!state.v4Diary[dateStr]) {
    state.v4Diary[dateStr] = {
      title: '', tasks: [], reflection: {energy:7, focus:7, accomplished:'', struggled:'', tomorrow:''}
    };
  }
  // Ensure reflection object
  var d = state.v4Diary[dateStr];
  if (!d.reflection) d.reflection = {energy:7, focus:7, accomplished:'', struggled:'', tomorrow:''};
  if (!d.tasks) d.tasks = [];
  return d;
}

function dvSaveDayData(dateStr, data) {
  state.v4Diary = state.v4Diary || {};
  state.v4Diary[dateStr] = data;
  saveState();
}

// ── VIEW RENDER ──────────────────────────────────────────────────
function dvRenderView() {
  dvRenderWeekRibbon();
  dvRenderDateLabel();
  dvRenderQuote();
  dvRenderTimeline();
  dvRenderSidebar();
}

function dvRenderDateLabel() {
  var el = document.getElementById('dv-date-label');
  if (!el) return;
  var d = new Date(dvCurrentDate + 'T00:00:00');
  var opts = {weekday:'long', year:'numeric', month:'long', day:'numeric'};
  el.textContent = d.toLocaleDateString('en-US', opts);
}

function dvRenderQuote() {
  var qt = document.getElementById('dv-quote-text');
  var qa = document.getElementById('dv-quote-author');
  if (!qt || !qa) return;
  // Pick quote based on day of year for consistency
  var d = new Date(dvCurrentDate + 'T00:00:00');
  var dayOfYear = Math.floor((d - new Date(d.getFullYear(), 0, 0)) / 86400000);
  var q = DV_MEDICAL_QUOTES[dayOfYear % DV_MEDICAL_QUOTES.length];
  qt.textContent = '"' + q.text + '"';
  qa.textContent = '— ' + q.author;
}

function dvRenderWeekRibbon() {
  var el = document.getElementById('dv-week-ribbon');
  if (!el) return;
  var today = todayStr();
  // Show 7 days centered around today, extending to ±10 days
  var days = [];
  for (var i = -6; i <= 6; i++) {
    days.push(addDays(today, i));
  }
  el.innerHTML = days.map(function(d) {
    var dt = new Date(d + 'T00:00:00');
    var dayNames = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
    var dn = dayNames[dt.getDay()];
    var dm = dt.getDate();
    var isToday = d === today;
    var isSelected = d === dvCurrentDate;
    var dayData = state.v4Diary && state.v4Diary[d];
    var hasTasks = dayData && dayData.tasks && dayData.tasks.length > 0;
    var cls = 'dv-day-chip' + (isToday ? ' today' : '') + (isSelected ? ' selected' : '') + (hasTasks ? ' has-tasks' : '');
    return '<div class="' + cls + '" onclick="dvGoDate(\'' + d + '\')">' +
      '<div style="font-size:10px">' + dn + '</div>' +
      '<div style="font-size:13px;font-weight:700">' + dm + '</div>' +
    '</div>';
  }).join('');
  // Scroll selected into view
  setTimeout(function() {
    var selected = el.querySelector('.selected');
    if (selected) selected.scrollIntoView({block:'nearest', inline:'center', behavior:'smooth'});
  }, 50);
}

function dvRenderTimeline() {
  var tl = document.getElementById('dv-timeline');
  var dayBadge = document.getElementById('dv-day-badge');
  var titleInput = document.getElementById('dv-day-title-input');
  var progFill = document.getElementById('dv-progress-fill');
  var progText = document.getElementById('dv-progress-text');
  var emptyState = document.getElementById('dv-empty-state');
  if (!tl) return;

  var data = dvGetDayData(dvCurrentDate);
  var today = todayStr();

  // Day badge
  var dt = new Date(dvCurrentDate + 'T00:00:00');
  var months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  if (dayBadge) dayBadge.textContent = months[dt.getMonth()] + ' ' + dt.getDate();
  if (titleInput) titleInput.value = data.title || '';

  // Merge scheduled items from state.dailySchedule if same date
  var mergedTasks = dvGetMergedTasks(dvCurrentDate);

  // Progress
  var total = mergedTasks.length;
  var done = mergedTasks.filter(function(t) { return t.completed; }).length;
  if (progFill) progFill.style.width = total ? Math.round((done/total)*100) + '%' : '0%';
  if (progText) progText.textContent = done + ' / ' + total + ' tasks' + (total ? ' (' + Math.round((done/total)*100) + '%)' : '');

  if (!total) {
    if (emptyState) emptyState.style.display = '';
    tl.innerHTML = document.getElementById('dv-empty-state') ? tl.innerHTML : '<div class="dv-empty-state"><div class="dv-empty-icon">📋</div><div class="dv-empty-title">No tasks yet</div><div class="dv-empty-sub">Click "+ Task" to add your study missions</div><button class="btn btn-primary btn-sm" onclick="dvOpenAddTask()" style="margin-top:12px">+ Add First Task</button></div>';
    return;
  }
  if (emptyState) emptyState.style.display = 'none';

  // Sort by time
  mergedTasks.sort(function(a, b) {
    return (a.time || '00:00').localeCompare(b.time || '00:00');
  });

  // Current time for active slot detection
  var now = new Date();
  var nowTime = pad(now.getHours()) + ':' + pad(now.getMinutes());
  var isToday = dvCurrentDate === today;

  tl.innerHTML = mergedTasks.map(function(task, idx) {
    return dvBuildSlotHtml(task, idx, mergedTasks.length, nowTime, isToday);
  }).join('');

  // Render mega prompt bar
  try { dvRenderMegaBar(); } catch(e) {}
}

function dvGetMergedTasks(dateStr) {
  var data = dvGetDayData(dateStr);
  var tasks = (data.tasks || []).slice(); // diary tasks

  // Also pull from state.dailySchedule if same date
  if (state.dailySchedule && state.dailySchedule.date === dateStr && state.dailySchedule.items) {
    var existingIds = tasks.map(function(t) { return t.id; });
    state.dailySchedule.items.forEach(function(item) {
      if (existingIds.indexOf(item.id) < 0) {
        // Convert schedule item to diary task format
        tasks.push({
          id: item.id,
          time: item.time || '',
          title: item.topic || item.subject || 'Study Task',
          type: item.type || 'study',
          topicId: item.topicId || null,
          stageKey: null,
          completed: (state.dailySchedule.completedIds || []).indexOf(item.id) >= 0,
          microSteps: item.microSteps || [],
          microChecks: {},
          notes: item.notes || '',
          source: 'schedule'  // mark as coming from old schedule system
        });
      }
    });
  }
  return tasks;
}

function dvBuildSlotHtml(task, idx, total, nowTime, isToday) {
  var isDone = task.completed;
  var emoji = DV_TYPE_EMOJIS[task.type] || '📚';
  var typeLabel = DV_TYPE_LABELS[task.type] || task.type;
  var microChecks = task.microChecks || {};
  var microTotal = (task.microSteps || []).length;
  var microDone = Object.keys(microChecks).filter(function(k) { return microChecks[k]; }).length;
  var hasMicro = microTotal > 0;
  var microPct = microTotal ? Math.round((microDone/microTotal)*100) : 0;

  // Determine if this is the current/active slot
  var isActive = isToday && !isDone && task.time && task.time <= nowTime &&
    (idx === total - 1 || true); // simplified: highlight current time slot

  // Linked topic info
  var linkedTopic = null;
  if (task.topicId) {
    var r = findTopic(task.topicId);
    if (r) linkedTopic = r.topic;
  }

  var slotClass = 'dv-slot dv-type-' + (task.type || 'study') +
    (isDone ? ' done' : '') + (isActive && !isDone ? ' active-slot' : '');

  var checkIcon = isDone ? '✓' : '';

  var badgesHtml = '';
  if (typeLabel) badgesHtml += '<span class="dv-slot-badge">' + emoji + ' ' + typeLabel + '</span>';
  if (linkedTopic) badgesHtml += '<span class="dv-slot-badge linked">🔗 ' + linkedTopic.name.slice(0,20) + '</span>';
  if (task.stageKey) badgesHtml += '<span class="dv-slot-badge stage">→ ' + (STAGE_LABELS[task.stageKey] || task.stageKey) + '</span>';
  if (isDone && task.topicId && task.stageKey) badgesHtml += '<span class="dv-slot-badge sync-done">✅ Synced</span>';

  var notesHtml = task.notes ? '<div style="font-size:11px;color:var(--text-muted);margin-top:4px;margin-left:28px">' + task.notes + '</div>' : '';

  // ── STAGE PILLS (bidirectional sync with topic dashboard) ──
  var stagePillsHtml = '';
  if (task.topicId) {
    var stageR = findTopic(task.topicId);
    if (stageR) {
      var stageTopic = stageR.topic;
      stagePillsHtml =
        '<div class="dv-stage-pills" id="dvsp-' + task.id + '">' +
        STAGE_KEYS.map(function(k) {
          var done = stageTopic.stages[k];
          return '<div class="dv-stage-pill ' + (done ? 'sp-done' : '') + '" ' +
            'onclick="event.stopPropagation();dvToggleStage(\'' + task.id + '\',\'' + task.topicId + '\',\'' + k + '\')" ' +
            'title="' + STAGE_LABELS[k] + '">' +
            STAGE_ICONS[k] + ' ' + STAGE_LABELS[k] +
          '</div>';
        }).join('') +
        '</div>';
    }
  }

  var microHtml = '';
  if (hasMicro) {
    var stepsHtml = (task.microSteps || []).map(function(step, si) {
      var key = 'ms_' + si;
      var chk = !!microChecks[key];
      return '<div class="dv-micro-item ' + (chk ? 'done-micro' : '') + '" onclick="dvToggleMicro(\'' + task.id + '\',' + si + ')">' +
        '<div class="dv-micro-cb">' + (chk ? '✓' : '') + '</div>' +
        '<span>' + step + '</span>' +
      '</div>';
    }).join('');
    microHtml =
      '<div class="dv-micro-progress"><div class="dv-micro-fill" style="width:' + microPct + '%"></div></div>' +
      '<div class="dv-microsteps" id="dvms-' + task.id + '">' + stepsHtml + '</div>';
  }

  return '<div class="' + slotClass + '" id="dvslot-' + task.id + '">' +
    '<div class="dv-slot-time">' + (task.time || '--:--') + '</div>' +
    '<div class="dv-slot-rail">' +
      '<div class="dv-slot-dot"></div>' +
      (idx < total - 1 ? '<div class="dv-slot-line"></div>' : '') +
    '</div>' +
    '<div class="dv-slot-body">' +
      '<div class="dv-slot-top">' +
        '<div class="dv-slot-check" onclick="dvToggleTask(\'' + task.id + '\')">' + checkIcon + '</div>' +
        '<span class="dv-slot-emoji">' + emoji + '</span>' +
        '<div class="dv-slot-title">' + task.title + '</div>' +
        '<div class="dv-slot-actions">' +
          (hasMicro ? '<button class="dv-slot-act-btn" onclick="dvToggleMicroPanel(\'' + task.id + '\')">☰ Steps</button>' : '') +
          '<button class="dv-slot-act-btn" onclick="dvEditTask(\'' + task.id + '\')">✏️</button>' +
          '<button class="dv-slot-act-btn del" onclick="dvDeleteTask(\'' + task.id + '\')">🗑️</button>' +
        '</div>' +
      '</div>' +
      (badgesHtml ? '<div class="dv-slot-badges">' + badgesHtml + '</div>' : '') +
      stagePillsHtml +
      notesHtml +
      microHtml +
    '</div>' +
  '</div>';
}

// ── TASK TOGGLE (with topic auto-sync) ───────────────────────────
function dvToggleTask(taskId) {
  var data = dvGetDayData(dvCurrentDate);
  var task = data.tasks.find(function(t) { return t.id === taskId; });

  // Also check merged schedule tasks
  var fromSchedule = false;
  if (!task && state.dailySchedule && state.dailySchedule.items) {
    var sItem = state.dailySchedule.items.find(function(i) { return i.id === taskId; });
    if (sItem) {
      fromSchedule = true;
      var cids = state.dailySchedule.completedIds || [];
      var cidIdx = cids.indexOf(taskId);
      var wasCompleted = cidIdx >= 0;
      if (wasCompleted) cids.splice(cidIdx, 1);
      else { cids.push(taskId); dvSyncTopicFromTask(sItem, !wasCompleted); }
      state.dailySchedule.completedIds = cids;
      saveState();
      dvRenderTimeline();
      dvRenderSidebar();
      return;
    }
  }

  if (!task) return;
  task.completed = !task.completed;

  // ── AUTO-SYNC TO TOPIC ──────────────────────────────────────────
  if (task.completed) {
    dvSyncTopicFromTask(task, true);
  } else {
    // Un-complete: optionally revert stage (skip for now, just update metrics)
  }

  dvSaveDayData(dvCurrentDate, data);
  dvRenderTimeline();
  dvRenderSidebar();

  // Flash animation
  setTimeout(function() {
    var el = document.getElementById('dvslot-' + taskId);
    if (el) el.classList.add('synced-flash');
    setTimeout(function() {
      if (el) el.classList.remove('synced-flash');
    }, 600);
  }, 50);
}

function dvSyncTopicFromTask(task, completing) {
  if (!task.topicId) return;
  var r = findTopic(task.topicId);
  if (!r) return;
  var t = r.topic;

  if (completing) {
    // Update the linked stage
    if (task.stageKey && STAGE_KEYS.indexOf(task.stageKey) >= 0) {
      t.stages[task.stageKey] = true;
    }
    // Update last studied date
    t.lastStudied = dvCurrentDate;
    // Improve repetition count slightly
    if ((t.repetitions || 0) < 1) t.repetitions = 1;
    // Update heatmap
    var today = todayStr();
    state.heatmap[today] = (state.heatmap[today] || 0) + 1;
    // Track weekly
    if (typeof trackWeeklyStudied === 'function') trackWeeklyStudied(t.id);
    // Update streak
    if (typeof updateStreak === 'function') updateStreak();

    saveState();
    showToast('✅ "' + t.name + '" ' + (task.stageKey ? '→ ' + (STAGE_LABELS[task.stageKey] || task.stageKey) + ' updated' : 'progress synced') + ' 🔗', 'success');
  }
}

// ── MICRO STEPS ──────────────────────────────────────────────────
function dvToggleMicroPanel(taskId) {
  var el = document.getElementById('dvms-' + taskId);
  if (!el) return;
  el.classList.toggle('open');
}

function dvToggleMicro(taskId, stepIdx) {
  var data = dvGetDayData(dvCurrentDate);
  var task = data.tasks.find(function(t) { return t.id === taskId; });

  // Also handle schedule items
  if (!task && state.dailySchedule && state.dailySchedule.items) {
    // Use existing toggleMicroStep for schedule items
    if (typeof toggleMicroStep === 'function') toggleMicroStep(taskId, stepIdx);
    dvRenderTimeline();
    return;
  }

  if (!task) return;
  task.microChecks = task.microChecks || {};
  var key = 'ms_' + stepIdx;
  task.microChecks[key] = !task.microChecks[key];

  // Calculate micro completion
  var microTotal = (task.microSteps || []).length;
  var microDone = Object.keys(task.microChecks).filter(function(k) { return task.microChecks[k]; }).length;
  var microPct = microTotal ? (microDone / microTotal) : 0;

  // Auto-complete task if all micro-steps done
  if (microDone === microTotal && microTotal > 0 && !task.completed) {
    task.completed = true;
    dvSyncTopicFromTask(task, true);
    showToast('🎉 All micro-steps done — task completed & synced!', 'success');
  }

  // Boost mastery based on micro completion
  if (task.topicId && microPct > 0) {
    var r = findTopic(task.topicId);
    if (r && r.topic) {
      // Partially update topic based on micro-step completion
      var t = r.topic;
      // Map micro completion to study stages progressively
      var stageKeyMap = ['reading','lectures','notes','mcqs','revision','mastered'];
      var stageToMark = Math.floor(microPct * stageKeyMap.length);
      for (var si = 0; si < stageToMark; si++) {
        if (stageKeyMap[si]) t.stages[stageKeyMap[si]] = true;
      }
    }
  }

  dvSaveDayData(dvCurrentDate, data);
  dvRenderTimeline();
}

// ── ADD / EDIT TASK MODAL ─────────────────────────────────────────
function dvOpenAddTask() {
  dvEditTaskId = null;
  document.getElementById('dv-task-time').value = dvGetNextHour();
  document.getElementById('dv-task-title').value = '';
  document.getElementById('dv-task-notes').value = '';
  document.getElementById('dv-task-microsteps').value = '';
  document.getElementById('dv-task-type').value = 'study';
  document.getElementById('dv-task-stage').value = '';
  dvPopulateTopicSelect('');
  document.getElementById('dv-add-task-overlay').classList.add('show');
  setTimeout(function() { document.getElementById('dv-task-title').focus(); }, 100);
}

function dvEditTask(taskId) {
  var data = dvGetDayData(dvCurrentDate);
  var task = data.tasks.find(function(t) { return t.id === taskId; });
  if (!task) return;
  dvEditTaskId = taskId;
  document.getElementById('dv-task-time').value = task.time || '';
  document.getElementById('dv-task-title').value = task.title || '';
  document.getElementById('dv-task-notes').value = task.notes || '';
  document.getElementById('dv-task-microsteps').value = (task.microSteps || []).join('\n');
  document.getElementById('dv-task-type').value = task.type || 'study';
  document.getElementById('dv-task-stage').value = task.stageKey || '';
  dvPopulateTopicSelect(task.topicId || '');
  document.getElementById('dv-add-task-overlay').classList.add('show');
}

function dvCloseAddTask() {
  dvEditTaskId = null;
  document.getElementById('dv-add-task-overlay').classList.remove('show');
}

function dvPopulateTopicSelect(selectedId) {
  var sel = document.getElementById('dv-task-topic');
  if (!sel) return;
  sel.innerHTML = '<option value="">— No topic link —</option>';
  var allTopics = getAllTopics();
  // Group by folder
  var byFolder = {};
  allTopics.forEach(function(x) {
    var fn = x.folder ? x.folder.name : 'Other';
    if (!byFolder[fn]) byFolder[fn] = [];
    byFolder[fn].push(x);
  });
  Object.keys(byFolder).forEach(function(fname) {
    var grp = document.createElement('optgroup');
    grp.label = fname;
    byFolder[fname].forEach(function(x) {
      var opt = document.createElement('option');
      opt.value = x.topic.id;
      opt.textContent = x.topic.name;
      if (x.topic.id === selectedId) opt.selected = true;
      grp.appendChild(opt);
    });
    sel.appendChild(grp);
  });
}

function dvSaveTask() {
  var time = document.getElementById('dv-task-time').value;
  var title = document.getElementById('dv-task-title').value.trim();
  var type = document.getElementById('dv-task-type').value;
  var topicId = document.getElementById('dv-task-topic').value;
  var stageKey = document.getElementById('dv-task-stage').value;
  var notes = document.getElementById('dv-task-notes').value.trim();
  var microRaw = document.getElementById('dv-task-microsteps').value;
  var microSteps = microRaw.split('\n').map(function(s) { return s.trim(); }).filter(Boolean);

  if (!title) { showToast('Please enter a task title', 'error'); return; }

  var data = dvGetDayData(dvCurrentDate);

  if (dvEditTaskId) {
    // Edit existing
    var task = data.tasks.find(function(t) { return t.id === dvEditTaskId; });
    if (task) {
      task.time = time; task.title = title; task.type = type;
      task.topicId = topicId || null; task.stageKey = stageKey || null;
      task.notes = notes; task.microSteps = microSteps;
      task.microChecks = task.microChecks || {};
    }
  } else {
    // New task
    var newTask = {
      id: genId(),
      time: time,
      title: title,
      type: type,
      topicId: topicId || null,
      stageKey: stageKey || null,
      completed: false,
      notes: notes,
      microSteps: microSteps,
      microChecks: {}
    };
    data.tasks.push(newTask);
  }

  dvSaveDayData(dvCurrentDate, data);
  dvCloseAddTask();
  dvRenderTimeline();
  dvRenderSidebar();
  showToast('📋 Task ' + (dvEditTaskId ? 'updated' : 'added') + '!', 'success');
  dvEditTaskId = null;
}

function dvDeleteTask(taskId) {
  if (!confirm('Delete this task?')) return;
  var data = dvGetDayData(dvCurrentDate);
  data.tasks = data.tasks.filter(function(t) { return t.id !== taskId; });
  dvSaveDayData(dvCurrentDate, data);
  dvRenderTimeline();
  dvRenderSidebar();
}

// ── SIDEBAR ──────────────────────────────────────────────────────
function dvRenderSidebar() {
  dvRenderDayStats();
  dvRenderReflectionInputs();
  dvRenderSyncedTopics();
  dvRenderDueTopics();
}

function dvRenderDayStats() {
  var el = document.getElementById('dv-stats-grid');
  if (!el) return;
  var tasks = dvGetMergedTasks(dvCurrentDate);
  var total = tasks.length;
  var done = tasks.filter(function(t) { return t.completed; }).length;
  var pct = total ? Math.round((done/total)*100) : 0;
  var synced = tasks.filter(function(t) { return t.completed && t.topicId; }).length;
  var microTotal = tasks.reduce(function(s, t) { return s + (t.microSteps||[]).length; }, 0);
  var microDone = tasks.reduce(function(s, t) {
    return s + Object.keys(t.microChecks||{}).filter(function(k){return t.microChecks[k];}).length;
  }, 0);
  el.innerHTML =
    '<div class="dsb-stat"><div class="dsb-stat-val" style="color:var(--primary)">' + total + '</div><div class="dsb-stat-lbl">Total Tasks</div></div>' +
    '<div class="dsb-stat"><div class="dsb-stat-val" style="color:var(--green)">' + done + '</div><div class="dsb-stat-lbl">Done</div></div>' +
    '<div class="dsb-stat"><div class="dsb-stat-val" style="color:' + (pct>=80?'var(--green)':pct>=50?'var(--yellow)':'var(--red)') + '">' + pct + '%</div><div class="dsb-stat-lbl">Progress</div></div>' +
    '<div class="dsb-stat"><div class="dsb-stat-val" style="color:var(--blue)">' + synced + '</div><div class="dsb-stat-lbl">Topics Synced</div></div>';
}

function dvRenderReflectionInputs() {
  var data = dvGetDayData(dvCurrentDate);
  var ref = data.reflection;
  var e = document.getElementById('dv-energy');
  var f = document.getElementById('dv-focus-score');
  var a = document.getElementById('dv-accomplished');
  var s = document.getElementById('dv-struggled');
  var t = document.getElementById('dv-tomorrow');
  var ev = document.getElementById('dv-energy-val');
  var fv = document.getElementById('dv-focus-val');
  if (e) { e.value = ref.energy || 7; }
  if (f) { f.value = ref.focus || 7; }
  if (ev) ev.textContent = ref.energy || 7;
  if (fv) fv.textContent = ref.focus || 7;
  if (a) a.value = ref.accomplished || '';
  if (s) s.value = ref.struggled || '';
  if (t) t.value = ref.tomorrow || '';
}

function dvSaveReflection() {
  var data = dvGetDayData(dvCurrentDate);
  data.reflection = {
    energy: parseInt((document.getElementById('dv-energy')||{value:7}).value) || 7,
    focus: parseInt((document.getElementById('dv-focus-score')||{value:7}).value) || 7,
    accomplished: (document.getElementById('dv-accomplished')||{value:''}).value,
    struggled: (document.getElementById('dv-struggled')||{value:''}).value,
    tomorrow: (document.getElementById('dv-tomorrow')||{value:''}).value
  };
  dvSaveDayData(dvCurrentDate, data);
}

function dvSaveDayTitle() {
  var input = document.getElementById('dv-day-title-input');
  if (!input) return;
  var data = dvGetDayData(dvCurrentDate);
  data.title = input.value;
  dvSaveDayData(dvCurrentDate, data);
}

function dvRenderSyncedTopics() {
  var el = document.getElementById('dv-synced-topics');
  if (!el) return;
  var tasks = dvGetMergedTasks(dvCurrentDate);
  var synced = tasks.filter(function(t) { return t.completed && t.topicId; });
  if (!synced.length) { el.textContent = 'Complete tasks to sync topics'; return; }
  el.innerHTML = synced.map(function(task) {
    var r = findTopic(task.topicId);
    if (!r) return '';
    var t = r.topic;
    var pct = Math.round((STAGE_KEYS.filter(function(k){return t.stages[k];}).length / STAGE_KEYS.length) * 100);
    return '<div class="dv-synced-chip" onclick="v4OpenFocus(\'' + t.id + '\')">' +
      '<span style="color:var(--green)">✅</span>' +
      '<span style="flex:1;font-weight:600">' + t.name.slice(0,22) + '</span>' +
      '<span style="color:var(--primary);font-family:var(--font-mono);font-size:10px">' + pct + '%</span>' +
    '</div>';
  }).filter(Boolean).join('');
}

function dvRenderDueTopics() {
  var el = document.getElementById('dv-due-topics');
  if (!el) return;
  var due = getDueTopics().slice(0, 6);
  if (!due.length) { el.innerHTML = '<div style="font-size:12px;color:var(--text-muted)">✅ Nothing due today</div>'; return; }
  el.innerHTML = due.map(function(x) {
    var t = x.topic;
    return '<div class="dv-due-chip" onclick="dvAddDueTopicTask(\'' + t.id + '\')">' +
      '<span>🔔</span>' +
      '<span style="flex:1;font-size:11px">' + t.name.slice(0,22) + '</span>' +
      '<span style="font-size:10px;color:var(--primary)" title="Add to today">+ Add</span>' +
    '</div>';
  }).join('');
}

function dvAddDueTopicTask(topicId) {
  var r = findTopic(topicId);
  if (!r) return;
  var data = dvGetDayData(dvCurrentDate);
  // Don't duplicate
  if (data.tasks.some(function(t) { return t.topicId === topicId; })) {
    showToast('Topic already in today\'s diary', 'info'); return;
  }
  data.tasks.push({
    id: genId(),
    time: dvGetNextHour(),
    title: 'Revise: ' + r.topic.name,
    type: 'revision',
    topicId: topicId,
    stageKey: 'revision',
    completed: false,
    notes: '',
    microSteps: ['Review notes', 'Active recall', 'Solve MCQs'],
    microChecks: {}
  });
  dvSaveDayData(dvCurrentDate, data);
  dvRenderTimeline();
  dvRenderSidebar();
  showToast('📋 "' + r.topic.name + '" added to diary!', 'success');
}

// ── NAVIGATION ────────────────────────────────────────────────────
function dvGoDate(dateStr) {
  dvCurrentDate = dateStr;
  dvRenderView();
}
function dvGoToday() { dvCurrentDate = todayStr(); dvRenderView(); }
function dvPrevDay() { dvCurrentDate = addDays(dvCurrentDate, -1); dvRenderView(); }
function dvNextDay() { dvCurrentDate = addDays(dvCurrentDate, 1); dvRenderView(); }

function dvGetNextHour() {
  var data = dvGetDayData(dvCurrentDate);
  var tasks = data.tasks;
  if (!tasks.length) return '08:00';
  var times = tasks.map(function(t) { return t.time || '08:00'; }).sort();
  var last = times[times.length - 1];
  var parts = last.split(':');
  var h = parseInt(parts[0] || 8);
  return pad(Math.min(h + 1, 22)) + ':00';
}


// ─────────────────────────────────────────────────────────────────
//  DIARY HOME PREVIEW (replaces old schedule section)
// ─────────────────────────────────────────────────────────────────
function dvRenderHomePreview(){
  var el=document.getElementById('home-diary-card');
  if(!el)return;
  var today=todayStr();
  var dayData=dvGetDayData(today);
  var tasks=dvGetMergedTasks(today);
  var total=tasks.length;
  var done=tasks.filter(function(t){return t.completed;}).length;
  var pct=total?Math.round((done/total)*100):0;
  var upcoming=tasks.filter(function(t){return !t.completed;}).slice(0,4);
  var ref=dayData.reflection||{};

  if(!total){
    el.innerHTML=
      '<div class="home-diary-empty">'+
        '<div style="font-size:28px;margin-bottom:8px">📔</div>'+
        '<div style="font-family:var(--font-head);font-size:14px;font-weight:700;margin-bottom:4px">No tasks today yet</div>'+
        '<div style="font-size:12px;color:var(--text-muted);margin-bottom:12px">Load a JSON schedule or add tasks manually in the Diary</div>'+
        '<div style="display:flex;gap:8px;justify-content:center;flex-wrap:wrap">'+
          '<button class="btn btn-primary btn-sm" onclick="showView(\'diary\')">📔 Open Diary</button>'+
          '<button class="btn btn-secondary btn-sm" onclick="openScheduleImport()">📥 Load JSON Schedule</button>'+
        '</div>'+
      '</div>';
    return;
  }

  // Color progress
  var pc=pct>=80?'var(--green)':pct>=40?'var(--primary)':'var(--yellow)';

  el.innerHTML=
    '<div class="home-diary-hdr">'+
      '<div>'+
        '<div class="home-diary-hdr-title">📔 '+(dayData.title||"Today's Medical Diary")+'</div>'+
        '<div class="home-diary-hdr-sub">'+done+' / '+total+' tasks · '+today+'</div>'+
      '</div>'+
      '<div style="display:flex;gap:6px;align-items:center">'+
        (ref.energy?'<span class="home-diary-badge" style="color:var(--purple)">⚡ '+ref.energy+'/10</span>':'')+
        (ref.focus?'<span class="home-diary-badge" style="color:var(--blue)">🎯 '+ref.focus+'/10</span>':'')+
        '<button class="btn btn-primary btn-sm" onclick="showView(\'diary\')">Open Full Diary →</button>'+
      '</div>'+
    '</div>'+
    // Progress bar
    '<div style="height:5px;background:var(--surface3);border-radius:3px;overflow:hidden;margin-bottom:14px">'+
      '<div style="height:100%;width:'+pct+'%;background:'+pc+';border-radius:3px;transition:width .4s"></div>'+
    '</div>'+
    // Task mini-list
    '<div class="home-diary-tasks">'+
      upcoming.map(function(task){
        var emoji=DV_TYPE_EMOJIS[task.type]||'📚';
        var microTotal=(task.microSteps||[]).length;
        var microDone=Object.keys(task.microChecks||{}).filter(function(k){return task.microChecks[k];}).length;
        return '<div class="home-diary-task-row" onclick="dvToggleTask(\''+task.id+'\')">'+
          '<div class="home-diary-task-check">'+(task.completed?'✓':'')+'</div>'+
          '<span style="font-size:13px">'+emoji+'</span>'+
          '<div style="flex:1;min-width:0">'+
            '<div class="home-diary-task-title">'+task.title+'</div>'+
            (task.time?'<div style="font-size:10px;color:var(--text-muted);font-family:var(--font-mono)">'+task.time+'</div>':'')+
          '</div>'+
          (microTotal?'<div style="font-size:10px;color:var(--text-muted)">'+microDone+'/'+microTotal+' steps</div>':'')+
        '</div>';
      }).join('')+
      (tasks.length>4?'<div style="text-align:center;font-size:11px;color:var(--text-muted);padding:6px;cursor:pointer" onclick="showView(\'diary\')">'+(tasks.length-4)+' more tasks in diary →</div>':'')+
    '</div>'+
    // Bottom actions
    '<div style="display:flex;gap:8px;margin-top:12px;flex-wrap:wrap">'+
      '<button class="btn btn-secondary btn-sm" onclick="openScheduleImport()">📥 Load JSON</button>'+
      '<button class="btn btn-ghost btn-sm" onclick="dvOpenAddTask();showView(\'diary\')">+ Add Task</button>'+
      (pct===100?'<span style="color:var(--green);font-size:12px;font-weight:700;align-self:center">🎉 All done today!</span>':'')+
    '</div>';
}


// ── BIDIRECTIONAL STAGE SYNC ─────────────────────────────────────
// Ticking a stage pill in the diary syncs it to the topic dashboard
function dvToggleStage(taskId, topicId, stageKey) {
  // 1. Toggle the stage in the topic data (uses existing toggleStage)
  toggleStage(topicId, stageKey);

  // 2. Re-render the stage pills for this task slot
  var pillsEl = document.getElementById('dvsp-' + taskId);
  if (pillsEl) {
    var r = findTopic(topicId);
    if (r) {
      pillsEl.innerHTML = STAGE_KEYS.map(function(k) {
        var done = r.topic.stages[k];
        return '<div class="dv-stage-pill ' + (done ? 'sp-done' : '') + '" ' +
          'onclick="event.stopPropagation();dvToggleStage(\'' + taskId + '\',\'' + topicId + '\',\'' + k + '\')" ' +
          'title="' + STAGE_LABELS[k] + '">' +
          STAGE_ICONS[k] + ' ' + STAGE_LABELS[k] +
        '</div>';
      }).join('');
    }
  }

  // 3. Update the notebook row ring % in chapter view (if visible)
  var r2 = findTopic(topicId);
  if (r2) {
    var pct = Math.round((STAGE_KEYS.filter(function(k){return r2.topic.stages[k];}).length / STAGE_KEYS.length) * 100);
    var mRing = document.getElementById('v4nbr-' + topicId);
    if (mRing) {
      var pctEl = mRing.querySelector('.topic-mini-pct');
      if (pctEl) pctEl.textContent = pct + '%';
    }
  }

  // 4. Flash the task slot to confirm sync
  var slot = document.getElementById('dvslot-' + taskId);
  if (slot) {
    slot.classList.add('synced-flash');
    setTimeout(function() { slot.classList.remove('synced-flash'); }, 600);
  }

  // 5. If ALL stages done → auto-complete the diary task
  var r3 = findTopic(topicId);
  if (r3) {
    var allStageDone = STAGE_KEYS.every(function(k) { return r3.topic.stages[k]; });
    if (allStageDone) {
      var data = dvGetDayData(dvCurrentDate);
      var task = data.tasks.find(function(t) { return t.id === taskId; });
      if (task && !task.completed) {
        task.completed = true;
        dvSaveDayData(dvCurrentDate, data);
        dvRenderTimeline();
        showToast('🏆 All stages complete — "' + r3.topic.name + '" mastered & synced!', 'success');
        return;
      }
    }
  }

  // Update progress bar
  dvRenderDayStats();
  dvRenderHomePreview();
}

// Reverse sync: when topic stage toggled elsewhere, refresh diary pills
function dvRefreshStagePills(topicId) {
  // Find all diary task slots for this topic and update their pills
  var data = dvGetDayData(dvCurrentDate);
  var r = findTopic(topicId);
  if (!r) return;
  data.tasks.forEach(function(task) {
    if (task.topicId !== topicId) return;
    var pillsEl = document.getElementById('dvsp-' + task.id);
    if (!pillsEl) return;
    pillsEl.innerHTML = STAGE_KEYS.map(function(k) {
      var done = r.topic.stages[k];
      return '<div class="dv-stage-pill ' + (done ? 'sp-done' : '') + '" ' +
        'onclick="event.stopPropagation();dvToggleStage(\'' + task.id + '\',\'' + topicId + '\',\'' + k + '\')" ' +
        'title="' + STAGE_LABELS[k] + '">' +
        STAGE_ICONS[k] + ' ' + STAGE_LABELS[k] +
      '</div>';
    }).join('');
  });
}


// ── ALL-DAY MASTER AI PROMPT BAR ─────────────────────────────────
function dvRenderMegaBar() {
  var el = document.getElementById('dv-mega-bar');
  if (!el) return;
  var tasks = dvGetMergedTasks(dvCurrentDate);
  var studyTasks = tasks.filter(function(t) { return t.type !== 'break'; });
  if (!studyTasks.length) { el.innerHTML = ''; return; }

  // Count topics with importance flags
  var hyCount = studyTasks.filter(function(t) {
    var r = t.topicId ? findTopic(t.topicId) : null;
    return r && r.topic.importance && r.topic.importance.indexOf('highYield') >= 0;
  }).length;
  var vivaCount = studyTasks.filter(function(t) {
    var r = t.topicId ? findTopic(t.topicId) : null;
    return r && r.topic.importance && r.topic.importance.indexOf('vivaImportant') >= 0;
  }).length;

  el.innerHTML =
    '<div class="dv-mega-btn" onclick="dvCopyMegaPrompt()" id="dv-mega-btn">' +
      '<div class="dv-mega-inner">' +
        '<div class="dv-mega-icon">🚀</div>' +
        '<div class="dv-mega-text">' +
          '<div class="dv-mega-title">Generate Full Study File for All ' + studyTasks.length + ' Topics</div>' +
          '<div class="dv-mega-sub">' +
            studyTasks.length + ' tasks' +
            (hyCount ? ' · ⭐ ' + hyCount + ' High Yield' : '') +
            (vivaCount ? ' · 🔥 ' + vivaCount + ' Viva' : '') +
            ' — Copy AI prompt → paste into Claude' +
          '</div>' +
        '</div>' +
        '<div class="dv-mega-badge" id="dv-mega-badge">' + studyTasks.length + ' topics</div>' +
      '</div>' +
    '</div>' +
    // Quick individual prompts row
    '<div class="dv-quick-prompts">' +
      '<span style="font-size:11px;color:var(--text-muted);align-self:center;margin-right:4px">Quick:</span>' +
      '<button class="dv-qp-btn" onclick="dvCopyPromptType(\'recall\')">🔁 Recall All</button>' +
      '<button class="dv-qp-btn" onclick="dvCopyPromptType(\'viva\')">🩺 Viva Drill</button>' +
      '<button class="dv-qp-btn" onclick="dvCopyPromptType(\'mcq\')">📝 MCQs</button>' +
      '<button class="dv-qp-btn" onclick="dvCopyPromptType(\'summary\')">📋 Summary</button>' +
    '</div>';
}

function dvCopyMegaPrompt() {
  copyAllTopicsPrompt();
  var badge = document.getElementById('dv-mega-badge');
  var btn = document.getElementById('dv-mega-btn');
  if (badge) badge.textContent = '✅ Copied!';
  if (btn) { btn.style.borderColor = 'var(--green)'; btn.style.background = 'linear-gradient(135deg,rgba(6,255,165,0.12),rgba(0,210,190,0.08))'; }
  setTimeout(function() {
    var tasks = dvGetMergedTasks(dvCurrentDate);
    var sc = tasks.filter(function(t){return t.type!=='break';}).length;
    if (badge) badge.textContent = sc + ' topics';
    if (btn) { btn.style.borderColor = ''; btn.style.background = ''; }
  }, 2500);
}

function dvCopyPromptType(type) {
  var tasks = dvGetMergedTasks(dvCurrentDate);
  var studyTasks = tasks.filter(function(t) { return t.type !== 'break'; });
  if (!studyTasks.length) { showToast('No study tasks today', 'warn'); return; }
  var topicNames = studyTasks.map(function(t) {
    var r = t.topicId ? findTopic(t.topicId) : null;
    return r ? r.topic.name : t.title;
  }).join(', ');
  var dayData = dvGetDayData(dvCurrentDate);

  var prompts = {
    recall: 'Generate 5 active recall questions (no answers upfront) for EACH of these MBBS topics:\n\n' + topicNames + '\n\nGroup by topic. Progress: Basic → Application → Clinical.',
    viva: 'Act as a clinical examiner. Give me 3 tough viva questions for EACH topic below, with model answer bullet points:\n\n' + topicNames,
    mcq: 'Generate 10 clinical vignette MCQs for each topic below. Include: stem, 5 options, correct answer, brief explanation.\n\nTopics: ' + topicNames,
    summary: 'Give a high-yield MBBS exam summary for each topic below (8–10 bullets each, exam-focused):\n\n' + topicNames + '\n\nDate: ' + dvCurrentDate + (dayData.title ? '\nSession: ' + dayData.title : '')
  };
  var text = prompts[type] || prompts.recall;
  navigator.clipboard.writeText(text).then(function() {
    showToast('📋 ' + type.charAt(0).toUpperCase() + type.slice(1) + ' prompt copied for ' + studyTasks.length + ' topics!', 'success');
  }).catch(function() { showToast('Copy failed', 'error'); });
}

window.addEventListener('DOMContentLoaded',function(){
  loadTheme(); loadState(); initTimer(); updatePomodoroDisplay(); renderHome();
  var mob=document.getElementById('mob-nav');if(mob&&window.innerWidth<=768)mob.style.display='block';
  setInterval(function(){if(timerRunning){state.studyTimer.todayElapsed=timerElapsed;saveState();}},30000);
  setInterval(function(){
    if(!state.dailySchedule||!state.dailySchedule.items||!state.dailySchedule.items.length)return;
    var activeId=getActiveSlotId();
    document.querySelectorAll('.diary-slot').forEach(function(el){
      var id=el.id.replace('sslot-',''),isDone=(state.dailySchedule.completedIds||[]).includes(id);
      if(!isDone){el.classList.toggle('active-now',id===activeId);var pill=el.querySelector('.active-now-pill');
        if(id===activeId&&!pill){pill=document.createElement('div');pill.className='active-now-pill';pill.textContent='▶ NOW';el.prepend(pill);}
        else if(id!==activeId&&pill)pill.remove();}
    });
  },60000);
  try{initFirebase();}catch(e){console.warn('Firebase unavailable — running offline',e);}
  setTimeout(v4Init,150);
});
</script>
<!-- ═══ DEEP FOCUS OVERLAY v4 ═══ -->
<div class="focus-overlay" id="v4-focus-overlay">
  <div class="focus-hdr">
    <div class="focus-title" id="v4-focus-title">Loading...</div>
    <div id="v4-focus-imp-hdr" style="display:flex;gap:4px;flex-wrap:wrap"></div>
    <div class="focus-clock" id="v4-focus-clock">00:00</div>
    <button class="btn btn-ghost btn-sm" onclick="openMasteryCheck(v4FocusId)">🏆 Mastery</button>
    <button class="btn btn-secondary btn-sm" onclick="v4CloseFocus()">✕ Exit</button>
  </div>
  <div class="focus-body">
    <div class="focus-sec">
      <div class="focus-sec-title">📋 Study Stages</div>
      <div class="focus-stages" id="v4-focus-stages"></div>
    </div>
    <div class="focus-sec">
      <div class="focus-sec-title">🏷️ Topic Classification</div>
      <div class="imp-toggle-row" id="v4-focus-imp-toggles"></div>
    </div>
    <div class="focus-sec" id="v4-micro-sec" style="display:none">
      <div class="focus-sec-title">✅ Scheduled Micro-Steps</div>
      <div id="v4-micro-steps"></div>
    </div>
    <div class="focus-sec">
      <div class="focus-sec-title">📝 Notes</div>
      <textarea class="focus-notes" id="v4-focus-notes" placeholder="Write notes, mnemonics, diagrams..." oninput="v4SaveNotes()"></textarea>
    </div>
    <div class="focus-sec">
      <div class="focus-sec-title">🤖 AI Study Aids</div>
      <div class="focus-ai-btns">
        <button class="f-ai-btn" onclick="v4AiRecall()">🔁 Recall Questions</button>
        <button class="f-ai-btn" onclick="v4AiViva()">🩺 Viva Questions</button>
        <button class="f-ai-btn" onclick="v4AiMnemo()">🧠 Mnemonic</button>
        <button class="f-ai-btn" onclick="v4AiSummary()">📋 Quick Summary</button>
      </div>
      <div class="focus-ai-resp" id="v4-ai-resp"></div>
    </div>
    <div class="focus-sec">
      <div class="focus-sec-title">📚 Resources</div>
      <div id="v4-focus-res"></div>
    </div>
    <div class="focus-sec">
      <div class="focus-sec-title">🔁 Spaced Repetition</div>
      <div id="v4-focus-sm2"></div>
    </div>
    <div class="focus-sec">
      <div class="focus-sec-title">🤖 Copy AI Prompt</div>
      <div style="display:flex;gap:5px;flex-wrap:wrap" id="v4-focus-prompts"></div>
    </div>
    <div style="margin-top:6px;text-align:right">
      <button class="btn btn-danger btn-sm" onclick="v4CloseFocus();deleteTopic(v4FocusId)">🗑️ Delete Topic</button>
    </div>
  </div>
</div>

<!-- ═══ AI ASSISTANT MODAL v4 ═══ -->
<div class="ai-overlay" id="v4-ai-overlay" onclick="if(event.target===this)v4CloseAi()">
  <div class="ai-modal">
    <div class="ai-modal-hdr">
      <span style="font-size:20px">🧠</span>
      <div class="ai-modal-title">MedTrack AI Assistant</div>
      <button class="modal-close" onclick="v4CloseAi()">✕</button>
    </div>
    <div class="ai-quick-btns">
      <button class="ai-qbtn" onclick="v4AiQuick(&quot;What are the most important topics I should revise today?&quot;)">📊 Today's Focus</button>
      <button class="ai-qbtn" onclick="v4AiQuick(&quot;Give me a 5-question viva drill on my weak topics&quot;)">🩺 Viva Drill</button>
      <button class="ai-qbtn" onclick="v4AiQuick(&quot;Create a mnemonic for my hardest topic&quot;)">🧠 Mnemonic</button>
      <button class="ai-qbtn" onclick="v4AiQuick(&quot;Analyze my study pattern and suggest improvements&quot;)">📈 Analysis</button>
    </div>
    <div class="ai-chat" id="v4-ai-chat">
      <div class="ai-msg">
        <div class="ai-msg-role a">🧠 MedTrack AI</div>
        <div class="ai-msg-body">Hello, Doctor in training! I'm your intelligent medical study assistant. Ask me anything — recall questions, viva prep, mnemonics, topic explanations, or study strategy.</div>
      </div>
    </div>
    <div class="ai-input-row">
      <textarea class="ai-input" id="v4-ai-input" placeholder="Ask about your topics, viva prep, revision strategy..." onkeydown="if(event.key==='Enter'&&!event.shiftKey){event.preventDefault();v4SendAi()}" rows="2"></textarea>
      <button class="btn btn-primary" onclick="v4SendAi()" id="v4-ai-send">Send</button>
    </div>
  </div>
</div>

<!-- ═══ DAILY REFLECTION MODAL v4 ═══ -->
<div class="modal-overlay" id="v4-reflect-overlay" onclick="if(event.target===this)v4CloseReflect()">
  <div class="modal">
    <button class="modal-close" onclick="v4CloseReflect()">✕</button>
    <div class="modal-title">📔 Daily Study Reflection</div>
    <div class="modal-subtitle" id="v4-reflect-date">Today's Journal</div>
    <div class="ref-field">
      <div class="ref-label">⚡ Energy Level (1-10)</div>
      <div class="energy-row"><input type="range" min="1" max="10" value="7" class="energy-slider" id="v4-energy" oninput="document.getElementById('v4-energy-val').textContent=this.value"><div class="energy-val" id="v4-energy-val">7</div></div>
    </div>
    <div class="ref-field">
      <div class="ref-label">🎯 Focus Score (1-10)</div>
      <div class="energy-row"><input type="range" min="1" max="10" value="7" class="energy-slider" id="v4-focus-score" oninput="document.getElementById('v4-focus-val').textContent=this.value" style="accent-color:var(--blue)"><div class="energy-val" id="v4-focus-val" style="color:var(--blue)">7</div></div>
    </div>
    <div class="ref-field"><div class="ref-label">✅ What I accomplished today</div><textarea class="ref-input" id="v4-accomplished" rows="2" placeholder="Topics covered, milestones..."></textarea></div>
    <div class="ref-field"><div class="ref-label">😤 What I struggled with</div><textarea class="ref-input" id="v4-struggled" rows="2" placeholder="Difficult concepts, distractions..."></textarea></div>
    <div class="ref-field"><div class="ref-label">📅 Tomorrow's priorities</div><textarea class="ref-input" id="v4-tomorrow" rows="2" placeholder="Top 3 focus areas for tomorrow..."></textarea></div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="v4CloseReflect()">Cancel</button>
      <button class="btn btn-primary" onclick="v4SaveReflect()">💾 Save Reflection</button>
    </div>
  </div>
</div>

</body>
</html>
