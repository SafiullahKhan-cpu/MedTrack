<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MedTrack Pro v2 — Medical Command Center</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,400&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>

<style>
/* ===================================================
   MEDTRACK PRO — DESIGN SYSTEM
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
.nav-search-wrap {
  flex: 1; max-width: 380px; position: relative;
}
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
.sr-item {
  display: flex; align-items: center; gap: 10px;
  padding: 10px 14px; cursor: pointer; transition: background 0.15s;
}
.sr-item:hover { background: var(--surface2); }
.sr-item .sr-icon { font-size: 18px; }
.sr-item .sr-name { font-size: 13px; font-weight: 600; }
.sr-item .sr-sub { font-size: 11px; color: var(--text-muted); }
.sr-item .sr-badge {
  margin-left: auto; font-size: 10px; font-family: var(--font-mono);
  background: var(--surface3); border-radius: 4px; padding: 1px 6px; color: var(--text-muted);
}
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
.view {
  display: none; min-height: 100vh; padding: 72px 22px 40px;
  max-width: 1320px; margin: 0 auto; position: relative; z-index: 1;
}
.view.active { display: block; animation: fadeUp 0.25s ease; }
@keyframes fadeUp { from{opacity:0;transform:translateY(8px)} to{opacity:1;transform:none} }

/* ===== BUTTONS ===== */
.btn {
  display: inline-flex; align-items: center; gap: 6px;
  border-radius: var(--r-sm); cursor: pointer; font-family: var(--font-body);
  font-size: 13px; border: none; transition: all 0.2s; padding: 8px 16px;
}
.btn-primary { background: var(--primary); color: var(--bg); font-weight: 600; }
.btn-primary:hover { filter: brightness(1.1); box-shadow: 0 4px 20px rgba(0,210,190,0.3); }
.btn-secondary { background: var(--surface2); border: 1px solid var(--border); color: var(--text); }
.btn-secondary:hover { border-color: var(--primary); color: var(--primary); }
.btn-danger { background: var(--red-dim); border: 1px solid rgba(255,71,87,0.3); color: var(--red); }
.btn-danger:hover { background: rgba(255,71,87,0.25); }
.btn-ghost { background: none; border: 1px solid var(--border); color: var(--text-muted); }
.btn-ghost:hover { color: var(--text); background: var(--surface2); }
.btn-sm { padding: 5px 10px; font-size: 12px; }

/* ===== SECTION TITLES ===== */
.section-title {
  font-family: var(--font-head); font-size: 11px; font-weight: 700;
  text-transform: uppercase; letter-spacing: 1.5px; color: var(--text-muted); margin-bottom: 12px;
}

/* ===================================================
   HOME / BOOSTER PANEL
=================================================== */
.home-grid {
  display: grid;
  grid-template-columns: 1fr 320px;
  gap: 18px;
  margin-bottom: 20px;
}
@media(max-width:900px){ .home-grid { grid-template-columns: 1fr; } }

.booster-panel {
  background: linear-gradient(135deg, rgba(0,210,190,0.06), rgba(58,134,255,0.04));
  border: 1px solid rgba(0,210,190,0.15);
  border-radius: var(--r); padding: 24px;
  position: relative; overflow: hidden;
}
.booster-panel::before {
  content: ''; position: absolute; top: -40px; right: -40px;
  width: 200px; height: 200px; border-radius: 50%;
  background: radial-gradient(circle, rgba(0,210,190,0.06), transparent 70%);
  pointer-events: none;
}
.booster-greeting {
  font-size: 12px; color: var(--text-muted); text-transform: uppercase;
  letter-spacing: 1px; margin-bottom: 4px;
}
.booster-title {
  font-family: var(--font-head); font-size: 26px; font-weight: 800;
  letter-spacing: -0.5px; margin-bottom: 16px;
}
.booster-title span { color: var(--primary); }
.quote-box {
  background: rgba(0,210,190,0.05); border-left: 3px solid var(--primary);
  border-radius: 0 var(--r-sm) var(--r-sm) 0; padding: 12px 16px; margin-bottom: 16px;
}
.quote-text { font-size: 14px; color: var(--text); font-style: italic; line-height: 1.5; }
.quote-author { font-size: 11px; color: var(--text-muted); margin-top: 4px; }
.why-started-box { margin-bottom: 16px; }
.why-started-label { font-size: 11px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 1px; margin-bottom: 6px; }
.why-started-input {
  width: 100%; background: var(--surface2); border: 1px solid var(--border2);
  border-radius: var(--r-sm); padding: 10px 14px; color: var(--text);
  font-family: var(--font-body); font-size: 13px; outline: none;
  resize: none; transition: border-color 0.2s; min-height: 60px;
}
.why-started-input:focus { border-color: var(--primary); }
.quick-actions { display: flex; gap: 8px; flex-wrap: wrap; }
.islamic-toggle {
  margin-top: 14px; display: flex; align-items: center; gap: 8px;
  font-size: 12px; color: var(--text-muted);
}
.toggle-switch {
  width: 36px; height: 20px; background: var(--surface3); border-radius: 10px;
  position: relative; cursor: pointer; transition: background 0.2s;
}
.toggle-switch.on { background: var(--primary); }
.toggle-knob {
  position: absolute; top: 3px; left: 3px; width: 14px; height: 14px;
  background: white; border-radius: 50%; transition: left 0.2s;
}
.toggle-switch.on .toggle-knob { left: 19px; }

/* streak + reminders panel */
.side-panel { display: flex; flex-direction: column; gap: 14px; }
.streak-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 18px 20px; text-align: center;
}
.streak-fire { font-size: 40px; line-height: 1; margin-bottom: 6px; }
.streak-number { font-family: var(--font-mono); font-size: 36px; font-weight: 700; color: var(--orange); line-height: 1; }
.streak-label { font-size: 11px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 1px; margin-top: 4px; }
.streak-best { font-size: 12px; color: var(--text-muted); margin-top: 8px; }
.streak-best span { color: var(--yellow); font-weight: 600; }

.reminders-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 16px; flex: 1;
}
.reminders-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
.reminder-item {
  display: flex; align-items: center; gap: 10px; padding: 8px 10px;
  background: var(--surface2); border-radius: var(--r-sm); margin-bottom: 6px; font-size: 13px;
}
.reminder-item .del-btn { background: none; border: none; color: var(--text-dim); cursor: pointer; font-size: 14px; margin-left: auto; transition: color 0.2s; }
.reminder-item .del-btn:hover { color: var(--red); }
.reminder-input-row { display: flex; gap: 6px; margin-top: 8px; }
.mini-input {
  flex: 1; background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 7px 10px; color: var(--text);
  font-family: var(--font-body); font-size: 12px; outline: none;
}
.mini-input:focus { border-color: var(--primary); }
.mini-input::placeholder { color: var(--text-muted); }

/* ===== STAT CARDS ===== */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 12px; margin-bottom: 22px;
}
.stat-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 16px 18px; position: relative;
  overflow: hidden; transition: transform 0.2s, border-color 0.2s;
}
.stat-card::before {
  content: ''; position: absolute; top: 0; left: 0;
  width: 3px; height: 100%; border-radius: 2px 0 0 2px;
}
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
.due-today-section {
  background: var(--surface); border: 1px solid rgba(255,71,87,0.2);
  border-radius: var(--r); padding: 18px 20px; margin-bottom: 18px;
}
.due-today-header {
  display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px;
}
.due-count-badge {
  background: var(--red-dim); border: 1px solid rgba(255,71,87,0.3);
  color: var(--red); border-radius: 20px; padding: 2px 10px;
  font-size: 12px; font-family: var(--font-mono); font-weight: 700;
}
.due-topic-list { display: flex; flex-wrap: wrap; gap: 8px; }
.due-topic-pill {
  background: var(--surface2); border: 1px solid var(--border);
  border-radius: 20px; padding: 5px 14px; font-size: 12px;
  cursor: pointer; display: flex; align-items: center; gap: 6px;
  transition: all 0.2s;
}
.due-topic-pill:hover { border-color: var(--primary); color: var(--primary); }
.due-topic-pill.overdue { border-color: rgba(255,71,87,0.3); color: var(--red); }

/* ===================================================
   HEATMAP
=================================================== */
.heatmap-wrap {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 18px 20px; margin-bottom: 18px;
  overflow-x: auto;
}
.heatmap-grid { display: flex; gap: 3px; }
.heatmap-week { display: flex; flex-direction: column; gap: 3px; }
.heatmap-cell {
  width: 11px; height: 11px; border-radius: 2px;
  background: var(--surface3); cursor: pointer; transition: transform 0.1s;
  position: relative;
}
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
.chart-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 18px;
}
.chart-card h3 { font-family: var(--font-head); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: var(--text-muted); margin-bottom: 14px; }
.chart-card canvas { max-height: 200px; }

/* ===================================================
   FOLDERS VIEW
=================================================== */
.view-header {
  display: flex; align-items: center; gap: 14px; margin-bottom: 24px;
  flex-wrap: wrap;
}
.view-title { font-family: var(--font-head); font-size: 24px; font-weight: 800; letter-spacing: -0.5px; }
.view-subtitle { color: var(--text-muted); font-size: 12px; margin-top: 2px; }
.back-btn {
  background: var(--surface2); border: 1px solid var(--border); color: var(--text-muted);
  border-radius: var(--r-sm); padding: 7px 12px; cursor: pointer; font-size: 12px;
  font-family: var(--font-body); transition: all 0.2s; display: flex; align-items: center; gap: 5px;
}
.back-btn:hover { color: var(--primary); border-color: var(--primary); background: var(--primary-dim); }
.breadcrumb {
  display: flex; align-items: center; gap: 6px; font-size: 12px;
  color: var(--text-muted); margin-bottom: 16px; flex-wrap: wrap;
}
.breadcrumb-item { cursor: pointer; transition: color 0.2s; }
.breadcrumb-item:hover { color: var(--primary); }
.breadcrumb-item.active { color: var(--text); }
.breadcrumb-sep { color: var(--text-dim); }

.folders-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 14px; margin-bottom: 20px;
}
.folder-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 20px; cursor: pointer;
  transition: all 0.25s; position: relative; overflow: hidden;
}
.folder-card:hover { transform: translateY(-3px); border-color: var(--primary); box-shadow: 0 8px 30px rgba(0,0,0,0.3); }
.folder-card-top { display: flex; align-items: flex-start; justify-content: space-between; margin-bottom: 12px; }
.folder-icon { font-size: 26px; line-height: 1; }
.folder-badge {
  font-size: 10px; font-family: var(--font-mono); color: var(--text-muted);
  background: var(--surface2); border: 1px solid var(--border2);
  border-radius: 4px; padding: 2px 6px;
}
.folder-name { font-family: var(--font-head); font-size: 15px; font-weight: 700; margin-bottom: 4px; }
.folder-stats { display: flex; gap: 10px; font-size: 11px; color: var(--text-muted); margin-bottom: 10px; }
.folder-progress-bar { height: 3px; background: var(--surface3); border-radius: 2px; overflow: hidden; }
.folder-progress-fill { height: 100%; border-radius: 2px; transition: width 0.6s ease; }
.folder-progress-text { display: flex; justify-content: space-between; font-size: 10px; color: var(--text-muted); margin-top: 4px; }
.due-badge {
  margin-top: 8px; font-size: 10px; background: var(--yellow-dim);
  color: var(--yellow); border-radius: 4px; padding: 2px 8px; display: inline-block;
}
.due-badge.none { background: transparent; color: var(--text-dim); }
.folder-actions {
  display: flex; gap: 4px; opacity: 0; transition: opacity 0.2s;
  position: absolute; top: 14px; right: 14px;
}
.folder-card:hover .folder-actions { opacity: 1; }
.icon-btn {
  background: var(--surface3); border: none; color: var(--text-muted);
  cursor: pointer; width: 26px; height: 26px; border-radius: 6px;
  display: flex; align-items: center; justify-content: center; font-size: 12px;
  transition: all 0.15s;
}
.icon-btn:hover { background: var(--surface2); color: var(--primary); }
.icon-btn.danger:hover { color: var(--red); }

/* Subfolder */
.subfolder-list { margin-bottom: 16px; }
.subfolder-item {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 12px 16px; margin-bottom: 8px;
  cursor: pointer; display: flex; align-items: center; gap: 12px;
  transition: all 0.2s;
}
.subfolder-item:hover { border-color: var(--primary); background: var(--primary-dim); }
.subfolder-arrow { color: var(--text-dim); font-size: 12px; margin-left: auto; }

/* ===================================================
   TOPICS
=================================================== */
.topic-filters {
  display: flex; gap: 8px; margin-bottom: 14px; flex-wrap: wrap; align-items: center;
}
.topic-search-input {
  background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 7px 12px; color: var(--text);
  font-family: var(--font-body); font-size: 12px; outline: none;
  flex: 1; min-width: 200px; transition: border-color 0.2s;
}
.topic-search-input:focus { border-color: var(--primary); }
.filter-btn {
  background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 6px 12px; color: var(--text-muted);
  font-size: 11px; cursor: pointer; transition: all 0.2s; white-space: nowrap;
}
.filter-btn.active { background: var(--primary-dim); border-color: var(--primary); color: var(--primary); }
.filter-btn:hover { color: var(--primary); border-color: var(--primary); }

.topic-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); margin-bottom: 8px; overflow: hidden;
  transition: border-color 0.2s;
}
.topic-card.mastered { border-color: rgba(6,255,165,0.15); }
.topic-card.weak { border-left: 3px solid var(--red); }
.topic-card-header {
  padding: 14px 18px; cursor: pointer; display: flex;
  align-items: center; gap: 12px; transition: background 0.2s; user-select: none;
}
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
.next-rev-badge {
  font-size: 10px; font-family: var(--font-mono); background: var(--yellow-dim);
  color: var(--yellow); border-radius: 4px; padding: 2px 6px; white-space: nowrap;
}
.next-rev-badge.overdue { background: var(--red-dim); color: var(--red); }
.next-rev-badge.today { background: rgba(255,71,87,0.25); color: var(--red); animation: blink-badge 1.5s infinite; }
.next-rev-badge.future { background: var(--blue-dim); color: var(--blue); }
@keyframes blink-badge { 0%,100%{opacity:1} 50%{opacity:0.5} }
.topic-expand-icon { color: var(--text-muted); font-size: 11px; transition: transform 0.3s; }
.topic-expand-icon.open { transform: rotate(90deg); }
.wt-badge {
  font-size: 10px; font-family: var(--font-mono); background: var(--purple-dim);
  color: var(--purple); border-radius: 4px; padding: 2px 6px;
}

.topic-body { display: none; padding: 0 18px 18px; border-top: 1px solid var(--border2); }
.topic-body.open { display: block; }
.topic-section-title { font-family: var(--font-head); font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.2px; color: var(--text-muted); margin: 16px 0 8px; }

/* ===== STAGES ===== */
.stages-grid { display: flex; flex-wrap: wrap; gap: 7px; }
.stage-item {
  display: flex; align-items: center; gap: 7px;
  background: var(--surface2); border: 1px solid var(--border2);
  border-radius: var(--r-sm); padding: 7px 12px; cursor: pointer;
  transition: all 0.2s; min-width: 150px; user-select: none;
}
.stage-item.done { background: var(--primary-dim); border-color: var(--border); }
.stage-item:hover { border-color: var(--primary); }
.stage-checkbox {
  width: 15px; height: 15px; border: 2px solid var(--text-dim); border-radius: 3px;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0; transition: all 0.2s; font-size: 9px;
}
.stage-item.done .stage-checkbox { background: var(--primary); border-color: var(--primary); color: var(--bg); }
.stage-label { font-size: 12px; color: var(--text); }
.stage-item.done .stage-label { color: var(--primary); }
.stage-item.stage-mastered.done { background: var(--green-dim); border-color: rgba(6,255,165,0.35); }
.stage-item.stage-mastered.done .stage-checkbox { background: var(--green); border-color: var(--green); }
.stage-item.stage-mastered.done .stage-label { color: var(--green); }

/* ===== SM-2 REVIEW ===== */
.sm2-panel {
  background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 12px 14px; margin-top: 8px;
}
.sm2-info { display: flex; gap: 16px; font-size: 11px; color: var(--text-muted); margin-bottom: 10px; flex-wrap: wrap; }
.sm2-info span b { color: var(--text); }
.sm2-buttons { display: flex; gap: 6px; flex-wrap: wrap; }
.sm2-btn {
  flex: 1; border: none; cursor: pointer;
  border-radius: var(--r-sm); padding: 10px; font-size: 13px;
  font-family: var(--font-body); font-weight: 600; transition: all 0.2s;
  background: var(--blue-dim); color: var(--blue); border: 1px solid rgba(58,134,255,0.3);
}
.sm2-btn:hover { filter: brightness(1.2); transform: translateY(-1px); }

/* Notes & Resources */
.notes-textarea {
  width: 100%; background: var(--surface2); border: 1px solid var(--border2);
  border-radius: var(--r-sm); padding: 10px 12px; color: var(--text);
  font-family: var(--font-body); font-size: 12px; outline: none; resize: vertical;
  min-height: 80px; transition: border-color 0.2s;
}
.notes-textarea:focus { border-color: var(--primary); }
.resources-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
.resource-input-group { display: flex; flex-direction: column; gap: 5px; }
.resource-label { font-size: 10px; color: var(--text-muted); font-weight: 600; text-transform: uppercase; letter-spacing: 0.8px; }
.resource-input {
  background: var(--surface2); border: 1px solid var(--border2); border-radius: var(--r-sm);
  padding: 6px 10px; color: var(--text); font-family: var(--font-body); font-size: 12px;
  outline: none; width: 100%; transition: border-color 0.2s;
}
.resource-input:focus { border-color: var(--primary); }
.resource-links-list { display: flex; flex-direction: column; gap: 3px; margin-top: 3px; }
.resource-link-item {
  display: flex; align-items: center; gap: 7px; background: var(--surface3);
  border-radius: 5px; padding: 4px 9px; font-size: 11px; overflow: hidden;
}
.resource-link-item a { color: var(--blue); text-decoration: none; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; flex: 1; }
.resource-link-item a:hover { color: var(--primary); }
.del-link-btn { background: none; border: none; color: var(--text-dim); cursor: pointer; font-size: 13px; padding: 0 2px; transition: color 0.2s; }
.del-link-btn:hover { color: var(--red); }
.add-resource-row { display: flex; gap: 5px; margin-top: 3px; }
.add-res-btn {
  background: var(--surface2); border: 1px solid var(--border); border-radius: var(--r-sm);
  color: var(--primary); cursor: pointer; padding: 6px 10px; font-size: 11px; transition: all 0.2s;
}
.add-res-btn:hover { background: var(--primary-dim); }

/* AI Prompts */
.prompts-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(190px, 1fr)); gap: 7px; }
.prompt-btn {
  background: var(--surface2); border: 1px solid var(--border2); border-radius: var(--r-sm);
  padding: 10px 12px; color: var(--text); cursor: pointer; text-align: left;
  transition: all 0.2s; font-family: var(--font-body); font-size: 11px;
}
.prompt-btn:hover { border-color: var(--primary); background: var(--primary-dim); color: var(--primary); }
.prompt-btn-title { font-weight: 600; margin-bottom: 2px; font-size: 11px; }
.prompt-btn-sub { color: var(--text-muted); font-size: 10px; }
.prompt-btn:hover .prompt-btn-sub { color: rgba(0,210,190,0.7); }
.copy-indicator { display: inline-block; font-size: 9px; background: var(--green-dim); color: var(--green); border-radius: 3px; padding: 1px 5px; margin-left: 4px; opacity: 0; transition: opacity 0.3s; }
.copy-indicator.show { opacity: 1; }
.prompt-btn.gen-html {
  grid-column: 1 / -1; background: linear-gradient(135deg, rgba(0,210,190,0.08), rgba(58,134,255,0.08));
  border: 1px solid rgba(0,210,190,0.3); padding: 14px 16px;
  display: flex; align-items: center; gap: 12px; text-align: left; overflow: hidden;
}
.prompt-btn.gen-html:hover { border-color: var(--primary); }

/* ===================================================
   REVISION SESSION VIEW
=================================================== */
.revision-queue {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 20px; margin-bottom: 16px;
}
.revision-session-card {
  background: linear-gradient(135deg, rgba(0,210,190,0.06), rgba(58,134,255,0.04));
  border: 1px solid rgba(0,210,190,0.15); border-radius: var(--r);
  padding: 28px; text-align: center; margin-bottom: 20px;
}
.rev-topic-name { font-family: var(--font-head); font-size: 22px; font-weight: 800; margin-bottom: 8px; cursor: pointer; transition: color 0.2s; }
.rev-topic-name:hover { color: var(--primary); text-decoration: underline; }
.rev-topic-folder { font-size: 12px; color: var(--text-muted); margin-bottom: 20px; }
.rev-quality-btns { display: flex; gap: 10px; justify-content: center; flex-wrap: wrap; }
.rev-q-btn {
  flex: 1; min-width: 100px; max-width: 250px; border: none; cursor: pointer;
  border-radius: var(--r-sm); padding: 14px 20px; font-size: 14px; font-weight: 700;
  font-family: var(--font-head); transition: all 0.2s;
  background: var(--blue-dim); color: var(--blue); border: 2px solid rgba(58,134,255,0.4);
}
.rev-q-btn:hover { transform: translateY(-2px); filter: brightness(1.2); }
.rev-q-sub { font-size: 11px; opacity: 0.7; font-weight: 400; margin-top: 4px; font-family: var(--font-body); }
.rev-queue-list { display: flex; flex-direction: column; gap: 6px; }
.rev-queue-item {
  background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 10px 14px; display: flex;
  align-items: center; gap: 12px; font-size: 12px; cursor: pointer;
  transition: border-color 0.2s;
}
.rev-queue-item:hover { border-color: var(--primary); }
.rev-queue-item .q-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
.rev-queue-item.current { border-color: var(--primary); }
.rev-queue-item.done { opacity: 0.5; }
.rev-empty {
  text-align: center; padding: 40px 20px; color: var(--text-muted);
}
.rev-empty .rev-empty-icon { font-size: 48px; margin-bottom: 12px; }
.rev-done-banner {
  text-align: center; background: var(--green-dim); border: 1px solid rgba(6,255,165,0.3);
  border-radius: var(--r); padding: 24px;
}
.rev-done-banner h2 { font-family: var(--font-head); font-size: 22px; color: var(--green); }

/* ===================================================
   ANALYTICS VIEW
=================================================== */
.analytics-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-bottom: 16px; }
@media(max-width:768px) { .analytics-grid { grid-template-columns: 1fr; } }
.ws-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 18px;
}
.ws-title { font-family: var(--font-head); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 12px; }
.ws-title.weak { color: var(--red); }
.ws-title.strong { color: var(--green); }
.ws-item {
  display: flex; align-items: center; gap: 10px; padding: 7px 0;
  border-bottom: 1px solid var(--border2); font-size: 12px;
}
.ws-item:last-child { border-bottom: none; }

/* ===================================================
   PLANNER
=================================================== */
.quick-planner {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 18px; margin-bottom: 18px;
}
.planner-title { font-family: var(--font-head); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; color: var(--text-muted); margin-bottom: 12px; }
.planner-item {
  display: flex; align-items: center; gap: 10px; padding: 8px 0;
  border-bottom: 1px solid var(--border2); font-size: 12px;
}
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

/* ===================================================
   TIMER PANEL
=================================================== */
.timer-panel {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 20px; margin-bottom: 18px; text-align: center;
}
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
.modal-overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(0,0,0,0.75); backdrop-filter: blur(4px);
  z-index: 2000; align-items: center; justify-content: center; padding: 20px;
}
.modal-overlay.show { display: flex; }
.modal {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 28px; max-width: 540px; width: 100%;
  position: relative; max-height: 90vh; overflow-y: auto;
  animation: modalIn 0.2s ease;
}
.modal-lg { max-width: 720px; }
@keyframes modalIn { from{opacity:0;transform:scale(0.95)} to{opacity:1;transform:none} }
.modal-close {
  position: absolute; top: 14px; right: 14px; background: var(--surface2);
  border: 1px solid var(--border); color: var(--text-muted); border-radius: 6px;
  cursor: pointer; width: 28px; height: 28px; display: flex; align-items: center;
  justify-content: center; font-size: 13px; transition: all 0.2s;
}
.modal-close:hover { color: var(--red); border-color: var(--red); }
.modal-title { font-family: var(--font-head); font-size: 19px; font-weight: 800; margin-bottom: 4px; }
.modal-subtitle { color: var(--text-muted); font-size: 12px; margin-bottom: 20px; }
.form-group { margin-bottom: 14px; }
.form-label { font-size: 11px; color: var(--text-muted); text-transform: uppercase; letter-spacing: 0.8px; margin-bottom: 5px; display: block; }
.form-input {
  width: 100%; background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 9px 12px; color: var(--text);
  font-family: var(--font-body); font-size: 13px; outline: none; transition: border-color 0.2s;
}
.form-input:focus { border-color: var(--primary); }
.form-select {
  width: 100%; background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 9px 12px; color: var(--text);
  font-family: var(--font-body); font-size: 13px; outline: none; cursor: pointer;
}
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
.modal-footer { display: flex; gap: 8px; justify-content: flex-end; margin-top: 20px; }
.import-log {
  background: var(--bg2); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 12px; font-family: var(--font-mono);
  font-size: 11px; max-height: 150px; overflow-y: auto; margin-top: 12px;
}
.log-success { color: var(--green); }
.log-error { color: var(--red); }
.log-warn { color: var(--yellow); }
.log-info { color: var(--blue); }

/* Upload zone */
.upload-zone {
  border: 2px dashed var(--border); border-radius: var(--r);
  padding: 28px; text-align: center; cursor: pointer; transition: all 0.2s;
  background: var(--surface2);
}
.upload-zone:hover, .upload-zone.drag { border-color: var(--primary); background: var(--primary-dim); }
.upload-icon { font-size: 40px; margin-bottom: 10px; }
.upload-text { font-size: 14px; color: var(--text); margin-bottom: 6px; }
.upload-hint { font-size: 12px; color: var(--text-muted); }
.json-schema-hint {
  background: var(--bg2); border: 1px solid var(--border2); border-radius: var(--r-sm);
  padding: 12px; font-family: var(--font-mono); font-size: 11px;
  color: var(--text-muted); margin-top: 12px; overflow-x: auto;
}
.merge-options { display: flex; gap: 8px; margin-top: 12px; flex-wrap: wrap; }
.merge-option {
  flex: 1; min-width: 120px; background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 10px; cursor: pointer; text-align: center;
  font-size: 12px; transition: all 0.2s;
}
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
.toast {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r-sm); padding: 10px 16px; font-size: 13px;
  display: flex; align-items: center; gap: 8px;
  animation: toastIn 0.25s ease, toastOut 0.25s ease 2.7s forwards;
  max-width: 340px; box-shadow: var(--shadow);
}
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
  #topnav {
    height: auto;
    flex-wrap: wrap;
    padding: 8px 12px;
  }
  .nav-tabs { display: none; }
  .nav-brand { flex: 1; }
  .nav-right { flex: none; justify-content: flex-end; }
  .nav-search-wrap {
    order: 3;
    flex: 1 1 100%;
    margin-top: 10px;
    max-width: none;
  }
  .hide-mob { display: none; }
  .view { padding-top: 120px; } /* Adjust because topnav is taller */

  .folders-grid { grid-template-columns: 1fr 1fr; }
  .resources-grid { grid-template-columns: 1fr; }
  .form-row { grid-template-columns: 1fr; }
  .timer-big { font-size: 32px; }
  .btn, .nav-btn, .filter-btn, .back-btn, .icon-btn {
    min-height: 44px;
    min-width: 44px;
    justify-content: center;
  }
  .topic-card-header { min-height: 48px; }
  .auth-chip-name { display: none; }
}
@media(max-width:480px) {
  .folders-grid { grid-template-columns: 1fr; }
  .stats-grid { grid-template-columns: 1fr 1fr; }
}

/* ===== ENTER BTN ===== */
.enter-study-btn {
  display: block; width: 100%; max-width: 420px; margin: 24px auto 0;
  background: linear-gradient(135deg, var(--primary), var(--blue));
  color: var(--bg); border: none; border-radius: var(--r);
  padding: 16px 28px; font-family: var(--font-head); font-size: 15px;
  font-weight: 800; letter-spacing: 0.5px; cursor: pointer; transition: all 0.3s;
  box-shadow: 0 4px 28px rgba(0,210,190,0.28);
}
.enter-study-btn:hover { transform: translateY(-3px); box-shadow: 0 8px 36px rgba(0,210,190,0.4); }

/* ===================================================
   LIGHT / DARK THEME TOGGLE
=================================================== */
body.light {
  --bg: #EEF2F7;
  --bg2: #E4ECF5;
  --surface: #FFFFFF;
  --surface2: #F4F7FB;
  --surface3: #DDE6F0;
  --border: rgba(0,160,150,0.18);
  --border2: rgba(0,0,0,0.07);
  --primary-dim: rgba(0,180,165,0.10);
  --blue-dim: rgba(40,110,230,0.10);
  --green-dim: rgba(0,180,100,0.10);
  --yellow-dim: rgba(200,130,30,0.10);
  --red-dim: rgba(210,50,60,0.10);
  --purple-dim: rgba(140,70,210,0.10);
  --text: #1A2E44;
  --text-muted: #527090;
  --text-dim: #96B3C8;
  --shadow: 0 4px 24px rgba(0,0,0,0.10);
}
body.light::before {
  background:
    radial-gradient(ellipse 80% 60% at 10% 0%, rgba(0,210,190,0.05) 0%, transparent 60%),
    radial-gradient(ellipse 60% 80% at 90% 100%, rgba(58,134,255,0.04) 0%, transparent 60%);
}
body.light #topnav {
  background: rgba(238,242,247,0.96);
}
body.light ::-webkit-scrollbar-track { background: var(--bg2); }
body.light ::-webkit-scrollbar-thumb { background: var(--surface3); }

/* Theme toggle button */
#theme-toggle-btn {
  background: var(--surface2); border: 1px solid var(--border);
  border-radius: var(--r-sm); color: var(--text); cursor: pointer;
  padding: 5px 9px; font-size: 14px; font-family: var(--font-body);
  transition: all 0.2s; display: flex; align-items: center; gap: 4px;
  white-space: nowrap; line-height:1;
}
#theme-toggle-btn:hover { background: var(--surface3); border-color: var(--primary); }

/* Sync status badge */
.sync-badge {
  font-family: var(--font-mono); font-size: 11px;
  border-radius: var(--r-sm); padding: 4px 8px;
  display: flex; align-items: center; gap: 4px;
  white-space: nowrap; transition: all 0.2s;
  border: 1px solid transparent;
}
.sync-badge.synced { color: var(--green); background: var(--green-dim); border-color: rgba(6,255,165,0.2); }
.sync-badge.syncing { color: var(--yellow); background: var(--yellow-dim); border-color: rgba(255,179,64,0.2); animation: pulse-timer 1.5s infinite; }
.sync-badge.error { color: var(--red); background: var(--red-dim); border-color: rgba(255,71,87,0.2); }

/* Settings modal */
.settings-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.75); backdrop-filter: blur(4px); z-index: 2000; align-items: flex-start; justify-content: center; padding: 60px 20px 20px; overflow-y: auto; }
.settings-overlay.show { display: flex; }
.settings-modal { background: var(--surface); border: 1px solid var(--border); border-radius: var(--r); padding: 28px; max-width: 480px; width: 100%; position: relative; animation: modalIn 0.2s ease; }
.settings-section-title { font-family: var(--font-head); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 1.5px; color: var(--text-muted); margin: 20px 0 12px; padding-top: 16px; border-top: 1px solid var(--border2); }
.settings-section-title:first-of-type { margin-top: 0; padding-top: 0; border-top: none; }

/* ===================================================
   WEEKEND REVISION PANEL
=================================================== */
.weekend-panel {
  background: linear-gradient(135deg, rgba(199,125,255,0.07), rgba(58,134,255,0.05));
  border: 1px solid rgba(199,125,255,0.22);
  border-radius: var(--r); padding: 20px 22px; margin-bottom: 18px;
  position: relative; overflow: hidden;
}
.weekend-panel::before {
  content: ''; position: absolute; top: -30px; right: -30px;
  width: 160px; height: 160px; border-radius: 50%;
  background: radial-gradient(circle, rgba(199,125,255,0.09), transparent 70%);
  pointer-events: none;
}
.weekend-panel-header {
  display: flex; align-items: center; justify-content: space-between;
  margin-bottom: 14px; flex-wrap: wrap; gap: 10px;
}
.weekend-panel-title {
  font-family: var(--font-head); font-size: 15px; font-weight: 800;
  color: var(--purple); display: flex; align-items: center; gap: 8px;
}
.weekend-panel-sub {
  font-size: 12px; color: var(--text-muted); margin-top: 2px;
}
.weekend-topic-grid {
  display: flex; flex-wrap: wrap; gap: 7px; margin-bottom: 14px;
}
.weekend-topic-chip {
  background: var(--surface2); border: 1px solid rgba(199,125,255,0.2);
  border-radius: 20px; padding: 5px 13px; font-size: 12px;
  display: flex; align-items: center; gap: 6px; cursor: pointer;
  transition: all 0.2s; color: var(--text);
}
.weekend-topic-chip:hover { border-color: var(--purple); color: var(--purple); }
.weekend-topic-chip .chip-folder { font-size: 10px; color: var(--text-muted); }
.weekend-topic-chip .chip-status { font-size: 10px; font-family: var(--font-mono); }
.weekend-topic-chip.reviewed { border-color: rgba(6,255,165,0.3); color: var(--green); background: var(--green-dim); }
.weekend-stats-row {
  display: flex; gap: 16px; margin-bottom: 14px; flex-wrap: wrap;
}
.weekend-stat {
  font-size: 11px; color: var(--text-muted); display: flex; align-items: center; gap: 5px;
}
.weekend-stat span { font-family: var(--font-mono); font-weight: 700; color: var(--purple); }
/* Weekend session modal */
.wknd-session-card {
  background: var(--surface); border: 1px solid rgba(199,125,255,0.2);
  border-radius: var(--r); padding: 28px; text-align: center; margin-bottom: 14px;
}
.wknd-session-topic { font-family: var(--font-head); font-size: 20px; font-weight: 800; margin-bottom: 6px; }
.wknd-session-folder { font-size: 12px; color: var(--text-muted); margin-bottom: 16px; }
.wknd-session-notes {
  background: var(--surface2); border-radius: var(--r-sm); padding: 12px;
  font-size: 12px; color: var(--text-muted); text-align: left; margin-bottom: 18px;
  max-height: 90px; overflow-y: auto; line-height: 1.5;
}
.wknd-progress-bar {
  background: var(--surface3); border-radius: 4px; height: 5px;
  overflow: hidden; margin-bottom: 8px;
}
.wknd-progress-fill {
  height: 100%; border-radius: 4px;
  background: linear-gradient(90deg, var(--purple), var(--blue));
  transition: width 0.4s ease;
}
.wknd-counter { font-size: 11px; color: var(--text-muted); margin-bottom: 20px; text-align: center; }
.wknd-btns { display: flex; gap: 8px; justify-content: center; flex-wrap: wrap; }
.wknd-btn {
  border-radius: var(--r-sm); padding: 10px 18px; font-size: 13px; font-weight: 600;
  border: none; cursor: pointer; font-family: var(--font-body); transition: all 0.2s;
  display: flex; flex-direction: column; align-items: center; gap: 2px; min-width: 80px;
}
.wknd-btn small { font-size: 10px; font-weight: 400; opacity: 0.8; }
.wknd-btn.skip { background: var(--surface3); color: var(--text-muted); }
.wknd-btn.skip:hover { background: var(--surface2); color: var(--text); }
.wknd-btn.good { background: var(--blue-dim); color: var(--blue); border: 1px solid rgba(58,134,255,0.3); }
.wknd-done-banner {
  background: var(--green-dim); border: 1px solid rgba(6,255,165,0.3);
  border-radius: var(--r); padding: 28px; text-align: center;
}
.wknd-done-banner h2 { font-family: var(--font-head); font-size: 22px; color: var(--green); margin-bottom: 8px; }

/* ===================================================
   AUTH CHIP
=================================================== */
.auth-chip {
  display: flex; align-items: center; gap: 6px;
  background: var(--surface2); border: 1px solid var(--border);
  border-radius: 20px; padding: 3px 10px 3px 4px;
  cursor: pointer; transition: all 0.2s; flex-shrink: 0;
}
.auth-chip:hover { border-color: var(--primary); }
.auth-chip img { width: 22px; height: 22px; border-radius: 50%; object-fit: cover; }
.auth-chip-name { font-size: 11px; color: var(--text); max-width: 80px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.auth-chip-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--green); flex-shrink: 0; }
.sign-in-btn {
  background: linear-gradient(135deg, var(--blue-dim), var(--primary-dim));
  border: 1px solid rgba(58,134,255,0.3); border-radius: var(--r-sm);
  color: var(--blue); cursor: pointer; padding: 5px 10px; font-size: 11px;
  font-family: var(--font-body); transition: all 0.2s; display: flex;
  align-items: center; gap: 5px; white-space: nowrap; flex-shrink: 0;
}
.sign-in-btn:hover { border-color: var(--blue); background: var(--blue-dim); }

/* ===================================================
   DAILY SCHEDULE SECTION
=================================================== */
.schedule-section {
  background: var(--surface); border: 1px solid rgba(58,134,255,0.2);
  border-radius: var(--r); padding: 18px 20px; margin-bottom: 18px;
}
.schedule-header {
  display: flex; align-items: center; justify-content: space-between;
  margin-bottom: 14px; flex-wrap: wrap; gap: 8px;
}
.schedule-title-wrap { display: flex; flex-direction: column; }
.schedule-title { font-family: var(--font-head); font-size: 14px; font-weight: 700; }
.schedule-subtitle { font-size: 11px; color: var(--text-muted); margin-top: 2px; }
.schedule-day-label {
  font-family: var(--font-mono); font-size: 10px; background: var(--blue-dim);
  border: 1px solid rgba(58,134,255,0.3); color: var(--blue);
  border-radius: 4px; padding: 2px 8px;
}
.schedule-actions { display: flex; gap: 6px; }
.schedule-timeline { display: flex; flex-direction: column; gap: 6px; }
.schedule-slot {
  display: flex; align-items: flex-start; gap: 12px; padding: 10px 14px;
  background: var(--surface2); border-radius: var(--r-sm);
  border: 1px solid var(--border2); transition: all 0.15s; cursor: default;
  position: relative;
}
.schedule-slot.completed { opacity: 0.5; }
.schedule-slot.completed .slot-topic { text-decoration: line-through; }
.slot-done-cb {
  width: 18px; height: 18px; flex-shrink: 0; cursor: pointer;
  accent-color: var(--primary); margin-top: 1px;
}
.slot-time {
  font-family: var(--font-mono); font-size: 11px; color: var(--primary);
  min-width: 90px; flex-shrink: 0; padding-top: 1px;
}
.slot-body { flex: 1; min-width: 0; }
.slot-topic { font-size: 13px; font-weight: 600; color: var(--text); }
.slot-subject { font-size: 11px; color: var(--text-muted); margin-top: 2px; }
.slot-tags { display: flex; flex-wrap: wrap; gap: 4px; margin-top: 5px; }
.slot-tag {
  background: var(--surface3); border-radius: 3px; padding: 1px 6px;
  font-size: 10px; color: var(--text-muted); font-family: var(--font-mono);
}
.slot-emoji { font-size: 18px; flex-shrink: 0; }
.slot-diff {
  flex-shrink: 0; font-size: 10px; font-family: var(--font-mono);
  border-radius: 3px; padding: 2px 6px; text-transform: uppercase;
}
.slot-diff.easy { background: var(--green-dim); color: var(--green); }
.slot-diff.medium { background: var(--yellow-dim); color: var(--yellow); }
.slot-diff.hard { background: var(--red-dim); color: var(--red); }
.schedule-empty {
  text-align: center; padding: 24px 0; color: var(--text-muted); font-size: 13px;
}
.schedule-progress {
  display: flex; align-items: center; gap: 10px; margin-bottom: 14px;
}
.schedule-progress-bar {
  flex: 1; height: 5px; background: var(--surface3); border-radius: 4px; overflow: hidden;
}
.schedule-progress-fill {
  height: 100%; border-radius: 4px;
  background: linear-gradient(90deg, var(--blue), var(--primary));
  transition: width 0.4s ease;
}
.schedule-progress-text { font-size: 11px; color: var(--text-muted); font-family: var(--font-mono); white-space: nowrap; }

/* Schedule Import Modal */
.schedule-schema-hint {
  background: var(--bg2); border: 1px solid var(--border2); border-radius: var(--r-sm);
  padding: 12px; font-family: var(--font-mono); font-size: 10px;
  color: var(--text-muted); margin-top: 12px; overflow-x: auto; max-height: 160px;
  overflow-y: auto; white-space: pre; line-height: 1.5;
}

/* Auth sign-in overlay */
.auth-overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(0,0,0,0.8); backdrop-filter: blur(4px);
  z-index: 3000; align-items: center; justify-content: center;
}
.auth-overlay.show { display: flex; }
.auth-modal {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--r); padding: 32px; max-width: 380px; width: 90%;
  text-align: center; animation: modalIn 0.2s ease;
}
.auth-modal-title { font-family: var(--font-head); font-size: 22px; font-weight: 800; margin-bottom: 6px; }
.auth-modal-sub { font-size: 13px; color: var(--text-muted); margin-bottom: 24px; line-height: 1.5; }
.google-btn {
  width: 100%; display: flex; align-items: center; justify-content: center;
  gap: 10px; background: white; color: #333; border: none; border-radius: var(--r-sm);
  padding: 12px 20px; font-size: 14px; font-weight: 600; cursor: pointer;
  transition: all 0.2s; font-family: var(--font-body); margin-bottom: 10px;
}
.google-btn:hover { background: #f0f0f0; box-shadow: 0 4px 20px rgba(255,255,255,0.15); }
.google-logo { width: 18px; height: 18px; }
.auth-skip-btn {
  background: none; border: 1px solid var(--border); color: var(--text-muted);
  border-radius: var(--r-sm); padding: 9px 20px; font-size: 12px;
  cursor: pointer; transition: all 0.2s; font-family: var(--font-body); width: 100%;
}
.auth-skip-btn:hover { border-color: var(--primary); color: var(--primary); }
.auth-note { font-size: 11px; color: var(--text-dim); margin-top: 14px; }
</style>
</head>
<body>

<!-- ===================================================
     TOP NAVIGATION
=================================================== -->
<nav id="topnav">
  <div class="nav-brand" onclick="showView('home')">🏥 MedTrack Pro</div>

  <div class="nav-tabs">
    <button class="nav-tab active" id="tab-home" onclick="showView('home')">🏠 Home</button>
    <button class="nav-tab" id="tab-folders" onclick="showView('folders')">📁 Subjects</button>
    <button class="nav-tab" id="tab-revision" onclick="showView('revision')">🔁 Revision</button>
    <button class="nav-tab" id="tab-analytics" onclick="showView('analytics')">📊 Analytics</button>
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
    
    <!-- Auth UI -->
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
     VIEW: HOME (Booster Panel + Dashboard)
=================================================== -->
<div id="view-home" class="view active">

  <div class="home-grid">
    <!-- Booster Panel -->
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
        <div class="toggle-switch" id="islamic-toggle" onclick="toggleIslamic()">
          <div class="toggle-knob"></div>
        </div>
        <span>Islamic reminder</span>
        <span id="islamic-reminder-text" style="color:var(--primary);display:none;margin-left:8px;font-size:11px;"></span>
      </div>
    </div>

    <!-- Side Panel: Streak + Reminders -->
    <div class="side-panel">
      <div class="streak-card">
        <div class="streak-fire">🔥</div>
        <div class="streak-number" id="streak-number">0</div>
        <div class="streak-label">Day Streak</div>
        <div class="streak-best">Best: <span id="streak-best">0</span> days</div>
      </div>

      <div class="reminders-card">
        <div class="reminders-header">
          <div class="section-title" style="margin-bottom:0">📅 Reminders</div>
        </div>
        <div id="reminders-list">
          <div style="color:var(--text-muted);font-size:12px;text-align:center;padding:10px 0">No reminders yet</div>
        </div>
        <div class="reminder-input-row">
          <input class="mini-input" id="reminder-input" placeholder="Add reminder..." onkeydown="if(event.key==='Enter')addReminder()">
          <button class="btn btn-primary btn-sm" onclick="addReminder()">+</button>
        </div>
      </div>
    </div>
  </div>

  <!-- Stats -->
  <div class="stats-grid" id="home-stats-grid"></div>

  <!-- Due Today -->
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

  <!-- Weekend Revision Panel (shown on Sat & Sun, always visible on weekends) -->
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
      <div class="weekend-topic-grid" id="weekend-topic-grid">
        <div style="color:var(--text-muted);font-size:12px">No topics studied this week yet.</div>
      </div>
    </div>
  </div>

  <!-- Timer -->
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

  <!-- Heatmap -->
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

  <!-- Daily Schedule -->
  <div id="daily-schedule-section" style="display:none">
    <div class="schedule-section">
      <div class="schedule-header">
        <div class="schedule-title-wrap">
          <div class="schedule-title">📅 <span id="schedule-day-title">Today's Study Schedule</span></div>
          <div class="schedule-subtitle" id="schedule-subtitle-text">Load a JSON schedule from the AI to see your day plan</div>
        </div>
        <div class="schedule-actions">
          <span class="schedule-day-label" id="schedule-date-badge">—</span>
          <button class="btn btn-secondary btn-sm" onclick="openScheduleImport()">⬆ Load New</button>
          <button class="btn btn-ghost btn-sm" onclick="clearSchedule()">✕ Clear</button>
        </div>
      </div>
      <div class="schedule-progress" id="schedule-progress-wrap" style="display:none">
        <div class="schedule-progress-bar"><div class="schedule-progress-fill" id="schedule-progress-fill" style="width:0%"></div></div>
        <div class="schedule-progress-text" id="schedule-progress-text">0 / 0 done</div>
      </div>
      <div class="schedule-timeline" id="schedule-timeline"></div>
    </div>
  </div>

  <!-- Planner -->
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
     VIEW: FOLDERS (Subjects) & SUBFOLDERS
=================================================== -->
<div id="view-folders" class="view">
  <div class="view-header">
    <div>
      <div class="view-title">📁 Study Folders</div>
      <div class="view-subtitle" id="folders-subtitle">Your medical syllabus organized by modules</div>
    </div>
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
    <div>
      <div class="view-title" id="subfolder-view-title">Topics</div>
    </div>
    <div style="margin-left:auto;display:flex;gap:8px">
      <button class="btn btn-secondary btn-sm" id="add-subfolder-btn" onclick="openAddSubfolder()">+ Nested Folder</button>
      <button class="btn btn-primary btn-sm" onclick="openAddTopic()">+ Add Topic</button>
    </div>
  </div>
  <div class="breadcrumb" id="subfolder-breadcrumb"></div>

  <!-- Subfolders -->
  <div id="subfolder-list-container" style="margin-bottom:16px"></div>

  <!-- Topic Filters -->
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
    <div>
      <div class="view-title">🔁 Spaced Revision</div>
      <div class="view-subtitle">Science-backed automated spaced repetition</div>
    </div>
    <div style="margin-left:auto">
      <button class="btn btn-primary" onclick="startRevisionSession()">▶ Start Session</button>
    </div>
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
    <div class="ws-card">
      <div class="ws-title weak">⚠️ Weak Topics</div>
      <div id="weak-topics-list"><div style="color:var(--text-dim);font-size:12px">Practice MCQs to identify weak areas</div></div>
    </div>
    <div class="ws-card">
      <div class="ws-title strong">✅ Mastered Topics</div>
      <div id="strong-topics-list"><div style="color:var(--text-dim);font-size:12px">Master topics to see them here</div></div>
    </div>
  </div>
</div>

<!-- ===================================================
     MODALS
=================================================== -->

<!-- Move Topic Modal -->
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
    <div style="margin-top:12px;font-size:12px;color:var(--text-muted);text-align:center">
      Work: 50 min → Short break: 10 min → Long break: 20 min (every 4 sessions)
    </div>
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
&nbsp;&nbsp;&nbsp;&nbsp;{<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "Module 1",<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"subfolders": [{ "name": "Block A", "topics": [{ "title": "Cell Structure", "weightage": 3, "difficulty": "medium" }] }],<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"topics": [{ "title": "General Histology", "weightage": 2, "difficulty": "easy" }]<br>
&nbsp;&nbsp;&nbsp;&nbsp;}<br>
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

<!-- Daily Schedule Import Modal -->
<div class="modal-overlay" id="schedule-import-overlay" onclick="if(event.target===this)closeScheduleImport()">
  <div class="modal modal-lg">
    <button class="modal-close" onclick="closeScheduleImport()">✕</button>
    <div class="modal-title">📅 Daily Study Schedule</div>
    <div class="modal-subtitle">Load an AI-generated JSON schedule for today — it links to your folders automatically!</div>

    <!-- AI Prompt Copy -->
    <div style="background:var(--primary-dim);border:1px solid rgba(0,210,190,0.3);border-radius:var(--r-sm);padding:12px 14px;margin-bottom:14px;display:flex;align-items:flex-start;gap:10px;">
      <span style="font-size:20px;flex-shrink:0">🤖</span>
      <div>
        <div style="font-size:12px;font-weight:600;color:var(--primary);margin-bottom:4px">Step 1 — Ask AI to generate your schedule</div>
        <div style="font-size:11px;color:var(--text-muted);margin-bottom:8px">Copy this prompt → paste into Claude / ChatGPT → give it your timetable → get the JSON back</div>
        <button class="btn btn-secondary btn-sm" onclick="copySchedulePrompt()" id="sched-prompt-copy-btn">📋 Copy AI Prompt</button>
      </div>
    </div>

    <div class="upload-zone" id="sched-upload-zone"
      onclick="document.getElementById('sched-file-input').click()"
      ondragover="handleSchedDragOver(event)"
      ondrop="handleSchedDrop(event)">
      <div class="upload-icon">📂</div>
      <div class="upload-text"><strong>Click or drag & drop</strong> your daily schedule JSON</div>
      <div class="upload-hint">Only .json files accepted — generated by AI using the prompt above</div>
    </div>
    <input type="file" id="sched-file-input" accept=".json" style="display:none" onchange="handleSchedFileSelect(event)">

    <div class="schedule-schema-hint" id="sched-schema-display" style="display:none"></div>

    <div class="import-log" id="sched-import-log" style="display:none"></div>
    <div id="sched-preview-area" style="margin-top:12px"></div>

    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeScheduleImport()">Cancel</button>
      <button class="btn btn-primary" id="sched-import-btn" onclick="importScheduleToTrack()" disabled>📥 Load Schedule</button>
    </div>
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
      <div style="background:var(--green-dim);border:1px solid rgba(6,255,165,0.2);border-radius:var(--r-sm);padding:12px;margin-bottom:12px;font-size:12px;color:var(--green)">
        ✅ Signed in — your data syncs across all devices automatically!
      </div>
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
    <div class="form-group">
      <label class="form-label">Folder Name *</label>
      <input class="form-input" id="add-folder-name" placeholder="e.g. Module 1 - Anatomy">
    </div>
    <div class="form-row">
      <div class="form-group">
        <label class="form-label">Icon</label>
        <input class="form-input" id="add-folder-icon" placeholder="📁" style="font-size:20px">
      </div>
      <div class="form-group">
        <label class="form-label">Color</label>
        <input type="color" class="form-input" id="add-folder-color" value="#00D2BE" style="height:42px;padding:4px">
      </div>
    </div>
    <div class="form-group">
      <label class="form-label">Label (e.g. MBBS-I)</label>
      <input class="form-input" id="add-folder-label" placeholder="MBBS-I">
    </div>
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
    <div class="form-group">
      <label class="form-label">Subfolder Name *</label>
      <input class="form-input" id="add-subfolder-name" placeholder="e.g. Block A / Prof A">
    </div>
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
    <div class="form-group">
      <label class="form-label">Topic Name *</label>
      <input class="form-input" id="add-topic-name" placeholder="e.g. Cell Membrane Structure">
    </div>
    <div class="form-row">
      <div class="form-group">
        <label class="form-label">Difficulty</label>
        <select class="form-select" id="add-topic-difficulty">
          <option value="easy">Easy</option>
          <option value="medium" selected>Medium</option>
          <option value="hard">Hard</option>
        </select>
      </div>
      <div class="form-group">
        <label class="form-label">Weightage (1-5)</label>
        <input type="number" class="form-input" id="add-topic-weightage" min="1" max="5" value="2">
      </div>
    </div>
    <div class="form-group">
      <label class="form-label">Tags (comma separated)</label>
      <input class="form-input" id="add-topic-tags" placeholder="anatomy, high-yield, clinical">
    </div>
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

<!-- General Settings Modal -->
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

    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeSettingsPanel()">Close</button>
    </div>
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

<!-- ===================================================
     JAVASCRIPT (Firebase Configured + App Logic)
=================================================== -->
<script type="module">
'use strict';

// ─────────────────────────────────────────────
//  FIREBASE INITIALIZATION
// ─────────────────────────────────────────────
import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
import { getAnalytics } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-analytics.js";
import { getAuth, signInAnonymously, signInWithCustomToken, signInWithPopup, signOut, GoogleAuthProvider, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
import { getFirestore, doc, setDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyAHp57writGRYeSNP6MwTcKPi45mlaUQHw",
  authDomain: "medtrackdb.firebaseapp.com",
  projectId: "medtrackdb",
  storageBucket: "medtrackdb.firebasestorage.app",
  messagingSenderId: "745482457766",
  appId: "1:745482457766:web:4aef20c89fe4a323d21a67",
  measurementId: "G-1CSSN4PT46"
};

const fallbackAppId = "medtrack_v2";

let app, analytics, auth, db, currentUser;
let unsubscribeSnapshot = null;

async function initFirebase() {
  try {
    const currentConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : firebaseConfig;
    app = initializeApp(currentConfig);
    try { analytics = getAnalytics(app); } catch(e) {}
    auth = getAuth(app);
    db = getFirestore(app);

    if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
      await signInWithCustomToken(auth, __initial_auth_token);
    }
    onAuthStateChanged(auth, (user) => {
      if (user) {
        currentUser = user;
        updateNavAuth(user);
        updateSyncStatus('loading');

        const currentAppId = typeof __app_id !== 'undefined' ? __app_id : fallbackAppId;
        const stateRef = doc(db, 'artifacts', currentAppId, 'users', currentUser.uid, 'medtrack', 'state');

        if (unsubscribeSnapshot) unsubscribeSnapshot();
        unsubscribeSnapshot = onSnapshot(stateRef, (docSnap) => {
          if (docSnap.exists()) {
            if (docSnap.metadata.hasPendingWrites) return;
            const docData = docSnap.data();
            const remoteState = docData.data ? JSON.parse(docData.data) : docData;
            const localSched = state.dailySchedule;
            state = { ...state, ...remoteState };
            if (!state.dailySchedule || !state.dailySchedule.items) {
              state.dailySchedule = localSched;
            }
            state.dailySchedule = state.dailySchedule || { date: null, items: [], completedIds: [] };
            patchTree(state.folders); // Apply infinite nested linking
            localStorage.setItem(APP_KEY, JSON.stringify(state));
            initTimer();
            updatePomodoroDisplay();
            renderHome();
            if (currentView === 'folders') renderFolders();
            if (currentView === 'subfolder') renderSubfolderView();
            if (currentView === 'revision') renderRevisionView();
            if (currentView === 'analytics') renderAnalytics();
          }
          updateSyncStatus('synced');
        }, (err) => {
          console.error('Firestore sync error', err);
          updateSyncStatus('error');
        });

      } else {
        currentUser = null;
        updateNavAuth(null);
        if (unsubscribeSnapshot) { unsubscribeSnapshot(); unsubscribeSnapshot = null; }
        updateSyncStatus('offline');
      }
    });
  } catch(e) {
    console.error('Firebase Init Failed', e);
    updateSyncStatus('error');
  }
}

let firestoreSaveTimeout = null;
async function saveToFirestore() {
  if (!currentUser || !db) return;
  const currentAppId = typeof __app_id !== 'undefined' ? __app_id : fallbackAppId;
  const stateRef = doc(db, 'artifacts', currentAppId, 'users', currentUser.uid, 'medtrack', 'state');
  
  try {
    await setDoc(stateRef, { data: JSON.stringify(state) });
    updateSyncStatus('synced');
  } catch (err) {
    console.error("Firestore save error", err);
    updateSyncStatus('error');
  }
}

function updateSyncStatus(status) {
  const el = document.getElementById('sync-status');
  if (!el) return;
  const map = {
    synced:  { text:'☁️ Synced', cls:'synced'  },
    syncing: { text:'🔄 Saving…', cls:'syncing' },
    error:   { text:'⚠️ Error',   cls:'error'   },
    offline: { text:'📴 Offline', cls:'error'   },
    loading: { text:'⬇️ Load…',   cls:'syncing' }
  };
  const s = map[status] || map.offline;
  el.innerHTML = s.text;
  el.className = 'sync-badge hide-mob ' + s.cls;
}

// ─────────────────────────────────────────────
//  CONSTANTS & DATA
// ─────────────────────────────────────────────
const APP_KEY = 'medtrack_pro_v2_stable';
const STAGE_KEYS = ['lectures','reading','notes','mcq','mastered'];
const STAGE_LABELS = { lectures:'Watch Lectures', reading:'Read Textbook', notes:'Make Notes', mcq:'Practice MCQs', mastered:'Topic Mastered' };
const STAGE_ICONS = { lectures:'📺', reading:'📖', notes:'✏️', mcq:'📝', mastered:'🏆' };
const SR_INTERVALS = [1, 3, 7, 14, 30, 60, 90, 180];

const MOTIVATIONAL_QUOTES = [
  { text: "The secret of getting ahead is getting started.", author: "Mark Twain" },
  { text: "Success is the sum of small efforts, repeated day in and day out.", author: "Robert Collier" },
  { text: "Medicine is not only a science; it is also an art.", author: "Paracelsus" },
  { text: "Every expert was once a beginner.", author: "Helen Hayes" },
];

const ISLAMIC_REMINDERS = [
  "وَقُلْ رَبِّ زِدْنِي عِلْمًا — \"And say: My Lord, increase me in knowledge.\" (20:114)",
  "اطلب العلم من المهد إلى اللحد — Seek knowledge from the cradle to the grave.",
  "Your healing hands will be a sadaqah for every patient you help.",
];

const PROMPTS = {
  concept:(t)=>`Explain "${t}" for MBBS level in detail.\n\nInclude:\n• Core concepts and definitions\n• Key mechanisms\n• Clinical relevance\n• Common exam traps\n• High-yield points\n• Mnemonics`,
  mcq:(t)=>`Generate 50 MBBS/USMLE style MCQs for "${t}".\n\nInclude:\n• Clinical vignettes (≥30)\n• Concept-based questions\n• Detailed explanations\n• Difficulty levels`,
  research:(t)=>`Latest research and clinical guidelines on "${t}".\n\nInclude:\n• Landmark studies\n• Current guidelines (WHO, NICE, AHA)\n• Recent advances\n• Clinical pearls`,
  recall:(t)=>`Give me 20 active recall questions on "${t}" (no answers).\n\nProgress from:\n• Basic recall → Application → Analysis\n• Include clinical mini-scenarios`,
  teach:(t)=>`Teach "${t}" to a first-year MBBS student.\n\nUse:\n• Simple analogies\n• Step-by-step logic\n• Clinical scenarios\n• Top 5 must-remember points`,
  htmlStudyFile:(t)=>`You are an advanced Medical AI integrated into a structured system called "MedTrack Pro".\n\nGenerate a COMPLETE INTERACTIVE HTML STUDY FILE for the topic: "${t}"\n\nYou MUST follow TWO CORE PROTOCOLS:\n1. FIKRI LEARNING SYSTEM (for content structure)\n2. SCHEDULE INTELLIGENCE SYSTEM (for daily execution)\n\n━━━━━━━━━━━━━━━━━━━━━━━\n🔶 PART 1: FIKRI SYSTEM (MANDATORY CORE)\n━━━━━━━━━━━━━━━━━━━━━━━\n\nStructure the topic into this deep intellectual framework:\n\n1. Conceptual Foundation → What is it? Why does it exist?\n2. Mechanism / Pathophysiology → Step-by-step causal chain (clear + layered)\n3. System Integration → Link with other body systems\n4. Clinical Lens → Symptoms, signs, real patient framing\n5. Diagnostic Thinking → How a doctor approaches this logically\n6. Management Logic → WHY treatments work (not just lists)\n7. Exam Intelligence → High-yield traps, patterns, frequently tested ideas\n8. Deep Fikri Links → Conceptual connections (ربط فکری)\n\nRULES: No shallow bullet dumping. Build understanding step-by-step. Feel like a thinking system, not notes.\n\n━━━━━━━━━━━━━━━━━━━━━━━\n🔶 PART 2: INTERACTIVE HTML OUTPUT\n━━━━━━━━━━━━━━━━━━━━━━━\n\nGenerate a COMPLETE SINGLE HTML FILE with:\n• Inline CSS + JS only — NO external dependencies\n• Dark modern UI (bg #070D1A, accent #00D2BE)\n• ALL content must be complete — no placeholder text\n\nTABS (must include all 7):\n1. 🧠 Fikri Breakdown — full structured analysis using the 8-point framework above\n2. 📋 Quick Notes — high-yield condensed revision points\n3. ❓ MCQs — 10 clinically relevant multiple choice questions with answers + explanations\n4. 🔁 Active Recall — 10 open-ended questions for self-testing (click to reveal answer)\n5. 🏥 Clinical Cases — 2 realistic patient vignettes with diagnostic reasoning\n6. 📅 Revision Plan — spaced repetition schedule (Day 1 / 3 / 7 / 14 / 30)\n7. 🗓️ Schedule — full daily study schedule (see Part 3)\n\nUI REQUIREMENTS:\n• Collapsible sections with smooth expand/collapse\n• Card-based layout with clean typography\n• Progress tracking within MCQ tab (score counter)\n• Active Recall flip-card style (click to reveal)\n\n━━━━━━━━━━━━━━━━━━━━━━━\n🔶 PART 3: SCHEDULE INTELLIGENCE SYSTEM\n━━━━━━━━━━━━━━━━━━━━━━━\n\nIn the Schedule tab, generate a FULL DAILY STUDY PLAN in diary style (NOT a table):\n\n📅 For each day include:\n• Gregorian Date + Hijri Date (MANDATORY)\n• Day Theme (a meaningful intellectual title)\n\nEach time block MUST include:\n• Time range (e.g. 08:00–09:30)\n• Topic + subtopic\n• Action Type: Learn / Revise / Active Recall / Test (MCQs)\n• Specific Task (clear, actionable)\n• Tags (#system #clinical #concept)\n• Depth: Light / Moderate / Deep\n\nINTELLIGENCE RULES:\n• Balance New Learning, Revision, and Active Recall across the day\n• Include short breaks (10–15 min) between each session\n• Keep workload realistic (max 6–7 study hours per day)\n• Prioritize high-yield and clinically tested subtopics\n• Avoid overplanning and repetitive tasks\n\n━━━━━━━━━━━━━━━━━━━━━━━\n🔶 PART 4: REFLECTION + PROGRESS (end of Schedule tab)\n━━━━━━━━━━━━━━━━━━━━━━━\n\n📌 Reflection:\n• What was deeply understood\n• What remains weak\n• 1 key fikri insight\n\n📊 Progress:\n• Completion estimate (%)\n• Effort rating (1–5 stars)\n• Consistency note\n\n━━━━━━━━━━━━━━━━━━━━━━━\n🔶 FINAL OUTPUT RULE\n━━━━━━━━━━━━━━━━━━━━━━━\n\nYour response MUST be a COMPLETE HTML FILE — ready to open in a browser — fully styled and interactive. Do NOT add any explanation outside the HTML.`
};

// ─────────────────────────────────────────────
//  STATE & INFINITE NESTING HELPERS
// ─────────────────────────────────────────────
let state = {
  folders: [],
  streak: { current: 0, best: 0, lastDate: null },
  studyTimer: { todayElapsed: 0, weeklyData: [0,0,0,0,0,0,0], totalElapsed: 0, lastDate: null },
  heatmap: {},
  reminders: [],
  whyStarted: '',
  islamicOn: false,
  plannerTasks: [],
  revisionSession: { active: false },
  weeklyStudied: {},
  dailySchedule: { date: null, items: [], completedIds: [] }
};

// Infinitely nested linking
function patchTree(folders, parentId = null) {
  folders.forEach(f => {
    f.parentId = parentId;
    f.subfolders = f.subfolders || [];
    f.topics = f.topics || [];
    f.topics.forEach(t => { t.parentId = f.id; t.starred = t.starred || false; });
    patchTree(f.subfolders, f.id);
  });
}

function findFolder(id, list = state.folders) {
  for (let f of list) {
    if (f.id === id) return f;
    const found = findFolder(id, f.subfolders);
    if (found) return found;
  }
  return null;
}

function findTopicRec(id, list = state.folders) {
  for (let f of list) {
    let t = f.topics.find(x => x.id === id);
    if (t) return { topic: t, folder: f }; 
    let found = findTopicRec(id, f.subfolders);
    if (found) return found;
  }
  return null;
}
function findTopic(id) { return findTopicRec(id); }

function getAllTopics(list = state.folders) {
  let all = [];
  for (let f of list) {
    f.topics.forEach(t => all.push({ topic: t, folder: f }));
    all = all.concat(getAllTopics(f.subfolders));
  }
  return all;
}

function getFolderTopicsRec(folder) {
  let arr = [...folder.topics];
  folder.subfolders.forEach(sf => arr = arr.concat(getFolderTopicsRec(sf)));
  return arr;
}

function saveState() {
  try { localStorage.setItem(APP_KEY, JSON.stringify(state)); }
  catch(e) { console.error('[MedTrack] Local save failed:', e); }
  
  if (currentUser) {
    updateSyncStatus('syncing');
    clearTimeout(firestoreSaveTimeout);
    firestoreSaveTimeout = setTimeout(saveToFirestore, 1500);
  }
}

function loadState() {
  try {
    const raw = localStorage.getItem(APP_KEY);
    if (raw) {
      const saved = JSON.parse(raw);
      state = { ...state, ...saved };
    }
    state.folders = state.folders || [];
    state.streak = state.streak || { current:0, best:0, lastDate:null };
    state.studyTimer = state.studyTimer || { todayElapsed:0, weeklyData:[0,0,0,0,0,0,0], totalElapsed:0, lastDate:null };
    state.heatmap = state.heatmap || {};
    state.reminders = state.reminders || [];
    state.plannerTasks = state.plannerTasks || [];
    state.weeklyStudied = state.weeklyStudied || {};
    state.dailySchedule = state.dailySchedule || { date: null, items: [], completedIds: [] };
    
    patchTree(state.folders); // Link all nodes infinitely
  } catch(e) {
    console.error('[MedTrack] Load failed:', e);
  }
}

function createTopicData(title, parentId, opts = {}) {
  return {
    id: genId(),
    name: title,
    parentId,
    difficulty: opts.difficulty || 'medium',
    weightage: opts.weightage || 2,
    tags: opts.tags || [],
    starred: false,
    stages: { lectures:false, reading:false, notes:false, mcq:false, mastered:false },
    notes: '',
    resources: { book:'', video:'' },
    links: [],
    interval: 0,
    repetitions: 0,
    lastReviewDate: null,
    nextReviewDate: null,
    mcqScore: null,
    weakFlag: false,
    createdAt: todayStr()
  };
}

// ─────────────────────────────────────────────
//  HELPERS
// ─────────────────────────────────────────────
function genId() { return Date.now().toString(36) + Math.random().toString(36).slice(2, 5); }
function todayStr() {
  const d = new Date();
  return `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())}`;
}
function pad(n) { return String(n).padStart(2,'0'); }
function addDays(dateStr, n) {
  const d = new Date(dateStr + 'T00:00:00');
  d.setDate(d.getDate() + n);
  return `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())}`;
}
function fmtHrsMin(sec) {
  const h = Math.floor(sec/3600), m = Math.floor((sec%3600)/60);
  return h > 0 ? `${h}h ${m}m` : `${m}m`;
}
function fmtSec(sec) {
  const h = Math.floor(sec/3600), m = Math.floor((sec%3600)/60), s = sec%60;
  return `${pad(h)}:${pad(m)}:${pad(s)}`;
}
function getTopicCompletion(topic) {
  const keys = STAGE_KEYS;
  const done = keys.filter(k => topic.stages[k]).length;
  return Math.round((done / keys.length) * 100);
}

// ─────────────────────────────────────────────
//  AUTOMATED SPACED REPETITION 
// ─────────────────────────────────────────────
function calcNextInterval(topic) {
  let reps = topic.repetitions || 0;
  return SR_INTERVALS[Math.min(reps, SR_INTERVALS.length - 1)];
}

function autoSpacedRepetition(topic) {
  let reps = topic.repetitions || 0;
  let interval = SR_INTERVALS[Math.min(reps, SR_INTERVALS.length - 1)];
  
  topic.interval = interval;
  topic.repetitions = reps + 1;
  const today = todayStr();
  topic.lastReviewDate = today;
  topic.nextReviewDate = addDays(today, interval);

  state.heatmap[today] = (state.heatmap[today] || 0) + 1;
  trackWeeklyStudied(topic.id);
  updateStreak();
  saveState();
}

function getDueTopics() {
  const today = todayStr();
  return getAllTopics().filter(({ topic }) => {
    if (!topic.nextReviewDate) return false;
    return topic.nextReviewDate <= today;
  });
}

function getNextReviewLabel(topic) {
  if (!topic.nextReviewDate) return null;
  const today = todayStr();
  if (topic.nextReviewDate < today) return { label: 'Overdue', cls: 'overdue' };
  if (topic.nextReviewDate === today) return { label: 'Due Today', cls: 'today' };
  const days = Math.round((new Date(topic.nextReviewDate) - new Date(today)) / 86400000);
  return { label: `In ${days}d`, cls: 'future' };
}

// ─────────────────────────────────────────────
//  WEEKLY TRACKING
// ─────────────────────────────────────────────
function getWeekKey() {
  const d = new Date();
  const jan4 = new Date(d.getFullYear(), 0, 4);
  const dayOfYear = Math.floor((d - new Date(d.getFullYear(), 0, 1)) / 86400000) + 1;
  const weekNum = Math.ceil((dayOfYear + jan4.getDay()) / 7);
  return `${d.getFullYear()}-W${String(weekNum).padStart(2, '0')}`;
}
function isWeekend() { const day = new Date().getDay(); return day === 0 || day === 6; }
function trackWeeklyStudied(topicId) {
  if (!topicId) return;
  const wk = getWeekKey();
  state.weeklyStudied = state.weeklyStudied || {};
  state.weeklyStudied[wk] = state.weeklyStudied[wk] || [];
  if (!state.weeklyStudied[wk].includes(topicId)) state.weeklyStudied[wk].push(topicId);
}
function getWeeklyStudiedTopics() {
  const wk = getWeekKey();
  const ids = new Set(state.weeklyStudied[wk] || []);
  if (ids.size === 0) return [];
  return getAllTopics().filter(({topic}) => ids.has(topic.id));
}

// ─────────────────────────────────────────────
//  NAVIGATION (INFINITE FOLDERS)
// ─────────────────────────────────────────────
let currentView = 'home';
let currentFolderId = null;
let topicFilter = 'all';

const NAV_TABS = { home:'tab-home', folders:'tab-folders', revision:'tab-revision', analytics:'tab-analytics' };

function showView(view) {
  document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
  const el = document.getElementById('view-' + view);
  if (el) el.classList.add('active');
  const tab = document.getElementById(NAV_TABS[view]);
  if (tab) tab.classList.add('active');
  currentView = view;

  try {
    if (view === 'home') renderHome();
    if (view === 'folders') { currentFolderId = null; renderFolders(); }
    if (view === 'revision') renderRevisionView();
    if (view === 'analytics') renderAnalytics();
  } catch(e) { console.error('[MedTrack] View render error:', e); }
}

function navigateToFolder(folderId) {
  currentFolderId = folderId;
  currentView = 'subfolder';
  document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
  document.getElementById('view-subfolder').classList.add('active');
  renderSubfolderView();
}

function goBackFromSubfolder() {
  if (!currentFolderId) { showView('folders'); return; }
  const folder = findFolder(currentFolderId);
  if (folder && folder.parentId) {
    navigateToFolder(folder.parentId);
  } else {
    showView('folders');
  }
}

// Instant Navigation
function goToTopic(topicId) {
  const r = findTopic(topicId);
  if (!r) return;
  
  navigateToFolder(r.topic.parentId || r.folder.id);
  
  setTimeout(() => {
    const body = document.getElementById('tb-' + topicId);
    const icon = document.getElementById('exp-' + topicId);
    if (body && !body.classList.contains('open')) {
      body.classList.add('open');
      if(icon) icon.classList.add('open');
    }
    const el = document.getElementById('tc-' + topicId);
    if (el) el.scrollIntoView({ behavior:'smooth', block:'center' });
  }, 150);
}

// ─────────────────────────────────────────────
//  HOME RENDER
// ─────────────────────────────────────────────
function renderHome() {
  renderBoosterPanel();
  renderHomeStats();
  renderDueToday();
  renderWeekendRevision();
  renderHeatmap();
  renderPlanner();
  renderStreakUI();
  renderReminders();
  renderDailySchedule();
  const ws = document.getElementById('why-started-input');
  if (ws) ws.value = state.whyStarted || '';
  const tog = document.getElementById('islamic-toggle');
  if (tog) { if (state.islamicOn) tog.classList.add('on'); else tog.classList.remove('on'); }
  if (state.islamicOn) showIslamicReminder();
}

function renderBoosterPanel() {
  const hour = new Date().getHours();
  let greet = 'Good morning';
  if (hour >= 12 && hour < 17) greet = 'Good afternoon';
  else if (hour >= 17) greet = 'Good evening';
  const el = document.getElementById('booster-greeting');
  if (el) el.textContent = greet + ', Doctor in training';

  const idx = Math.floor(Date.now() / 86400000) % MOTIVATIONAL_QUOTES.length;
  const q = MOTIVATIONAL_QUOTES[idx];
  const qt = document.getElementById('daily-quote');
  const qa = document.getElementById('daily-quote-author');
  if (qt) qt.textContent = q.text;
  if (qa) qa.textContent = '— ' + q.author;

  const actions = ['dominate', 'conquer', 'excel at', 'master', 'crush'];
  const ai = Math.floor(Date.now() / 86400000) % actions.length;
  const ba = document.getElementById('booster-action');
  if (ba) ba.textContent = actions[ai];
}

function renderHomeStats() {
  const all = getAllTopics();
  const total = all.length;
  const mastered = all.filter(({topic}) => topic.stages.mastered).length;
  const due = getDueTopics().length;
  const pct = total ? Math.round((mastered/total)*100) : 0;
  
  const grid = document.getElementById('home-stats-grid');
  if (!grid) return;
  grid.innerHTML = `
    <div class="stat-card c-teal"><div class="stat-label">Total Topics</div><div class="stat-value">${total}</div><div class="stat-sub">in all folders</div><div class="stat-icon">📚</div></div>
    <div class="stat-card c-green"><div class="stat-label">Mastered</div><div class="stat-value">${mastered}</div><div class="stat-sub">${pct}% complete</div><div class="stat-icon">🏆</div></div>
    <div class="stat-card c-red"><div class="stat-label">Due Today</div><div class="stat-value">${due}</div><div class="stat-sub">need review</div><div class="stat-icon">🔔</div></div>
    <div class="stat-card c-blue"><div class="stat-label">Folders</div><div class="stat-value">${state.folders.length}</div><div class="stat-sub">root subjects</div><div class="stat-icon">📁</div></div>
    <div class="stat-card c-yellow"><div class="stat-label">Streak</div><div class="stat-value">${state.streak.current}</div><div class="stat-sub">day${state.streak.current!==1?'s':''}</div><div class="stat-icon">🔥</div></div>
    <div class="stat-card c-purple"><div class="stat-label">Today Time</div><div class="stat-value">${fmtHrsMin(timerElapsed)}</div><div class="stat-sub">studied</div><div class="stat-icon">⏱</div></div>
  `;
}

function updateStreak() {
  const today = todayStr();
  if (state.streak.lastDate === today) return;
  
  if (!state.streak.lastDate || state.streak.lastDate === addDays(today, -1)) {
    state.streak.current++;
  } else if (state.streak.lastDate !== today) {
    state.streak.current = 1;
  }
  
  if (state.streak.current > state.streak.best) {
    state.streak.best = state.streak.current;
  }
  state.streak.lastDate = today;
  saveState();
  renderStreakUI();
}

function renderStreakUI() {
  const sn = document.getElementById('streak-number');
  const sb = document.getElementById('streak-best');
  if (sn) sn.textContent = state.streak.current;
  if (sb) sb.textContent = state.streak.best;
}

function renderReminders() {
  const list = document.getElementById('reminders-list');
  if (!list) return;
  if (!state.reminders || state.reminders.length === 0) {
    list.innerHTML = '<div style="color:var(--text-muted);font-size:12px;text-align:center;padding:10px 0">No reminders yet</div>';
    return;
  }
  list.innerHTML = state.reminders.map((r,i) => `
    <div class="reminder-item">
      <span style="font-size:16px">⏰</span>
      <span style="flex:1">${r.text}</span>
      <button class="del-btn" onclick="deleteReminder(${i})">✕</button>
    </div>
  `).join('');
}

function addReminder() {
  const inp = document.getElementById('reminder-input');
  if (!inp || !inp.value.trim()) return;
  state.reminders = state.reminders || [];
  state.reminders.push({ text: inp.value.trim(), createdAt: todayStr() });
  inp.value = '';
  saveState();
  renderReminders();
}

function deleteReminder(i) {
  state.reminders.splice(i, 1);
  saveState();
  renderReminders();
}

function saveWhyStarted() {
  const el = document.getElementById('why-started-input');
  if (el) { state.whyStarted = el.value; saveState(); }
}

function toggleIslamic() {
  state.islamicOn = !state.islamicOn;
  const tog = document.getElementById('islamic-toggle');
  if (tog) { if (state.islamicOn) tog.classList.add('on'); else tog.classList.remove('on'); }
  if (state.islamicOn) showIslamicReminder();
  else {
    const el = document.getElementById('islamic-reminder-text');
    if (el) el.style.display = 'none';
  }
  saveState();
}

function showIslamicReminder() {
  const el = document.getElementById('islamic-reminder-text');
  if (!el) return;
  const idx = Math.floor(Date.now() / 86400000) % ISLAMIC_REMINDERS.length;
  el.textContent = ISLAMIC_REMINDERS[idx];
  el.style.display = 'inline';
}

function renderDueToday() {
  const due = getDueTopics();
  const cnt = document.getElementById('due-today-count');
  const list = document.getElementById('due-today-list');
  if (cnt) cnt.textContent = due.length;
  if (!list) return;
  if (due.length === 0) {
    list.innerHTML = '<div style="color:var(--text-muted);font-size:12px">All caught up! No reviews due today 🎉</div>';
    return;
  }
  const today = todayStr();
  list.innerHTML = due.slice(0,12).map(({topic, folder}) => {
    const isOverdue = topic.nextReviewDate < today;
    return `<div class="due-topic-pill ${isOverdue?'overdue':''}" onclick="goToTopic('${topic.id}')">
      ${topic.name.length > 30 ? topic.name.slice(0,28)+'…' : topic.name}
      <span style="font-size:10px;opacity:0.7">${folder.name}</span>
    </div>`;
  }).join('');
  if (due.length > 12) list.innerHTML += `<div class="due-topic-pill" onclick="startRevisionSession()">+${due.length-12} more →</div>`;
}

function renderHeatmap() {
  const grid = document.getElementById('heatmap-grid');
  if (!grid) return;
  const weeks = 26;
  const today = new Date();
  const todayDay = today.getDay();
  const startDate = new Date(today);
  startDate.setDate(today.getDate() - (weeks * 7) + (7 - todayDay));

  let html = '<div class="heatmap-grid">';
  for (let w = 0; w < weeks; w++) {
    html += '<div class="heatmap-week">';
    for (let d = 0; d < 7; d++) {
      const cur = new Date(startDate);
      cur.setDate(startDate.getDate() + w*7 + d);
      const ds = `${cur.getFullYear()}-${pad(cur.getMonth()+1)}-${pad(cur.getDate())}`;
      const count = state.heatmap[ds] || 0;
      const level = count === 0 ? 0 : count <= 2 ? 1 : count <= 5 ? 2 : count <= 10 ? 3 : 4;
      html += `<div class="heatmap-cell" data-level="${level}" title="${ds}: ${count} reviews"></div>`;
    }
    html += '</div>';
  }
  html += '</div>';
  grid.innerHTML = html;
  document.querySelectorAll('.heatmap-legend-cell[data-level]').forEach(c => {
    const lvl = c.getAttribute('data-level');
    const colors = { '1':'rgba(0,210,190,0.2)','2':'rgba(0,210,190,0.4)','3':'rgba(0,210,190,0.65)','4':'#00D2BE' };
    c.style.background = colors[lvl] || 'var(--surface3)';
  });
}

function renderPlanner() {
  const list = document.getElementById('planner-list');
  if (!list) return;
  const today = todayStr();
  const tasks = (state.plannerTasks || []).filter(t => t.date === today);
  if (tasks.length === 0) {
    list.innerHTML = '<div style="color:var(--text-muted);font-size:12px;padding:8px 0">No tasks yet. Add one below!</div>';
    return;
  }
  list.innerHTML = tasks.map(t => `
    <div class="planner-item">
      <input type="checkbox" class="planner-cb" ${t.done?'checked':''} onchange="toggleTask('${t.id}',this.checked)">
      <span class="planner-text ${t.done?'done':''}">${t.text}</span>
      <button class="planner-del" onclick="deleteTask('${t.id}')">✕</button>
    </div>
  `).join('');
}

function addPlannerTask() {
  const inp = document.getElementById('planner-input');
  if (!inp || !inp.value.trim()) return;
  state.plannerTasks = state.plannerTasks || [];
  state.plannerTasks.push({ id:genId(), text:inp.value.trim(), done:false, date:todayStr() });
  inp.value = '';
  saveState();
  renderPlanner();
}

function toggleTask(id, done) {
  const t = (state.plannerTasks||[]).find(t => t.id === id);
  if (t) { t.done = done; saveState(); renderPlanner(); }
}

function deleteTask(id) {
  state.plannerTasks = (state.plannerTasks||[]).filter(t => t.id !== id);
  saveState();
  renderPlanner();
}

// ─────────────────────────────────────────────
//  FOLDERS & SUBFOLDERS VIEW
// ─────────────────────────────────────────────
function renderFolders() {
  const grid = document.getElementById('folders-grid');
  if (!grid) return;
  const today = todayStr();
  grid.innerHTML = state.folders.map(f => {
    const allTopics = getFolderTopicsRec(f);
    const total = allTopics.length;
    const mastered = allTopics.filter(t => t.stages.mastered).length;
    const pct = total ? Math.round((mastered/total)*100) : 0;
    const due = allTopics.filter(t => t.nextReviewDate && t.nextReviewDate <= today).length;

    return `
    <div class="folder-card" onclick="navigateToFolder('${f.id}')" style="--card-color:${f.color}">
      <div class="folder-card-top">
        <span class="folder-icon">${f.icon}</span>
        <span class="folder-badge">${f.label || ''}</span>
        <div class="folder-actions">
          <button class="icon-btn" onclick="event.stopPropagation();openEditFolder('${f.id}')" title="Edit">✏️</button>
          <button class="icon-btn danger" onclick="event.stopPropagation();deleteFolder('${f.id}')" title="Delete">🗑️</button>
        </div>
      </div>
      <div class="folder-name">${f.name}</div>
      <div class="folder-stats">
        <span>📑 ${total} nested topics</span>
      </div>
      <div class="folder-progress-bar">
        <div class="folder-progress-fill" style="width:${pct}%;background:${f.color}"></div>
      </div>
      <div class="folder-progress-text">
        <span>${mastered}/${total} mastered</span><span>${pct}%</span>
      </div>
      <div class="due-badge ${due===0?'none':''}">${due > 0 ? `🔔 ${due} due today` : '✅ All caught up'}</div>
    </div>`;
  }).join('');

  if (state.folders.length === 0) {
    grid.innerHTML = `<div class="empty-state" style="grid-column:1/-1">
      <div class="empty-icon">📁</div>
      <div class="empty-text">No folders yet. Create one or import a JSON syllabus!</div>
      <button class="btn btn-primary" onclick="openAddFolder()">+ Create Root Folder</button>
    </div>`;
  }
}

function renderSubfolderView() {
  const folder = findFolder(currentFolderId);
  if (!folder) { showView('folders'); return; }

  const titleEl = document.getElementById('subfolder-view-title');
  const bcEl = document.getElementById('subfolder-breadcrumb');

  if (titleEl) titleEl.textContent = `${folder.icon} ${folder.name}`;

  // Build Breadcrumbs
  let crumbs = [];
  let cur = folder;
  while (cur) {
    crumbs.unshift(cur);
    cur = cur.parentId ? findFolder(cur.parentId) : null;
  }

  if (bcEl) {
    bcEl.innerHTML = `
      <span class="breadcrumb-item" onclick="showView('folders')">📁 Root Folders</span>
      ${crumbs.map((c, i) => `
        <span class="breadcrumb-sep">›</span>
        <span class="breadcrumb-item ${i === crumbs.length-1 ? 'active' : ''}" onclick="navigateToFolder('${c.id}')">${c.icon || '📂'} ${c.name}</span>
      `).join('')}
    `;
  }

  renderSubfolderList(folder);
  renderTopicList(folder.topics);
}

function renderSubfolderList(folder) {
  const container = document.getElementById('subfolder-list-container');
  if (!container) return;
  if (!folder.subfolders || folder.subfolders.length === 0) {
    container.innerHTML = ''; return;
  }
  const today = todayStr();
  container.innerHTML = `
    <div class="section-title">📂 Nested Sections</div>
    <div class="subfolder-list">
      ${folder.subfolders.map(sf => {
        const allTopics = getFolderTopicsRec(sf);
        const total = allTopics.length;
        const mastered = allTopics.filter(t => t.stages.mastered).length;
        const due = allTopics.filter(t => t.nextReviewDate && t.nextReviewDate <= today).length;
        return `
        <div class="subfolder-item" onclick="navigateToFolder('${sf.id}')">
          <span style="font-size:20px">📂</span>
          <div style="flex:1">
            <div style="font-family:var(--font-head);font-size:13px;font-weight:700">${sf.name}</div>
            <div style="font-size:11px;color:var(--text-muted)">${total} topics · ${mastered} mastered${due>0?` · 🔔 ${due} due`:''}</div>
          </div>
          <button class="icon-btn danger" style="margin-right:8px" onclick="event.stopPropagation();deleteFolder('${sf.id}')" title="Delete">🗑️</button>
          <span class="subfolder-arrow">›</span>
        </div>`;
      }).join('')}
    </div>
    <div class="section-title" style="margin-top:16px">📑 Topics in ${folder.name}</div>
  `;
}

// ─────────────────────────────────────────────
//  TOPIC LIST
// ─────────────────────────────────────────────
function renderTopicList(topics) {
  const list = document.getElementById('topics-list');
  if (!list) return;
  if (!topics || topics.length === 0) {
    list.innerHTML = `<div class="empty-state">
      <div class="empty-icon">📑</div>
      <div class="empty-text">No topics in this folder yet.</div>
      <button class="btn btn-primary" onclick="openAddTopic()">+ Add First Topic</button>
    </div>`;
    return;
  }

  const filtered = getFilteredTopics(topics);
  list.innerHTML = filtered.map(t => buildTopicCard(t)).join('');
}

function getFilteredTopics(topics) {
  const q = (document.getElementById('topic-search')?.value || '').toLowerCase().trim();
  return topics.filter(t => {
    if (q && !fuzzyMatch(t.name, q)) return false;
    const today = todayStr();
    if (topicFilter === 'starred') return t.starred;
    if (topicFilter === 'incomplete') return getTopicCompletion(t) < 100;
    if (topicFilter === 'revision') return t.nextReviewDate && t.nextReviewDate <= today;
    if (topicFilter === 'weak') return t.weakFlag;
    if (topicFilter === 'mastered') return t.stages.mastered;
    return true;
  });
}

function buildTopicCard(topic) {
  const pct = getTopicCompletion(topic);
  const circ = 100.5;
  const dashoffset = circ * (1 - pct/100);
  const revInfo = getNextReviewLabel(topic);
  const diff = topic.difficulty || 'medium';
  const wt = topic.weightage || 2;

  return `<div class="topic-card ${topic.stages.mastered?'mastered':''} ${topic.weakFlag?'weak':''}" id="tc-${topic.id}">
    <div class="topic-card-header" onclick="toggleTopic('${topic.id}')">
      <div class="topic-completion-ring">
        <svg viewBox="0 0 36 36">
          <circle class="ring-bg" cx="18" cy="18" r="16"/>
          <circle class="ring-fill" cx="18" cy="18" r="16" stroke-dasharray="${circ}" stroke-dashoffset="${dashoffset}"
            style="stroke:${pct===100?'var(--green)':'var(--primary)'}"/>
        </svg>
        <div class="topic-ring-pct">${pct}%</div>
      </div>
      <div class="topic-info">
        <div class="topic-name">${topic.name}</div>
        <div class="topic-meta">
          ${topic.stages.mastered ? '<span style="color:var(--green)">✅ Mastered</span>' : ''}
          ${topic.stages.reading ? '' : '<span>📖 Not read</span>'}
          ${topic.notes ? '<span>📝 Notes</span>' : ''}
          ${topic.links?.length ? `<span>🔗 ${topic.links.length} links</span>` : ''}
          ${topic.tags?.length ? `<span>#${topic.tags.slice(0,2).join(' #')}</span>` : ''}
        </div>
      </div>
      <div class="topic-right">
        <span class="star-btn ${topic.starred?'starred':''}" onclick="event.stopPropagation(); toggleStar('${topic.id}')" title="Star important topic">
          ${topic.starred ? '⭐' : '☆'}
        </span>
        <span class="diff-badge diff-${diff}">${diff}</span>
        ${wt > 1 ? `<span class="wt-badge">★${wt}</span>` : ''}
        ${revInfo ? `<span class="next-rev-badge ${revInfo.cls}">${revInfo.label}</span>` : ''}
        <span class="topic-expand-icon" id="exp-${topic.id}">▶</span>
      </div>
    </div>
    <div class="topic-body" id="tb-${topic.id}">
      ${buildTopicBody(topic)}
    </div>
  </div>`;
}

function buildTopicBody(topic) {
  const revInfo = getNextReviewLabel(topic);

  const stagesHtml = STAGE_KEYS.map(k => `
    <div class="stage-item ${topic.stages[k]?'done':''} ${k==='mastered'?'stage-mastered':''}" onclick="toggleStage('${topic.id}','${k}')">
      <div class="stage-checkbox">${topic.stages[k]?'✓':''}</div>
      <span class="stage-label">${STAGE_ICONS[k]} ${STAGE_LABELS[k]}</span>
    </div>`).join('');

  const sm2Html = `
    <div class="sm2-panel">
      <div class="sm2-info">
        <span>Interval: <b>${topic.interval||0}d</b></span>
        <span>Reps: <b>${topic.repetitions||0}</b></span>
        ${topic.lastReviewDate ? `<span>Last: <b>${topic.lastReviewDate}</b></span>` : ''}
        ${revInfo ? `<span>Next: <b style="color:var(--${revInfo.cls==='overdue'?'red':revInfo.cls==='today'?'red':'primary'})">${topic.nextReviewDate}</b></span>` : ''}
      </div>
      <div class="sm2-buttons">
        <button class="sm2-btn" style="width:100%" onclick="reviewTopic('${topic.id}')">✅ Mark as Reviewed<br><small>Next review: ${calcNextInterval(topic)}d</small></button>
      </div>
    </div>`;

  const notesHtml = `<textarea class="notes-textarea" placeholder="Your notes on this topic..." oninput="saveTopic('${topic.id}','notes',this.value)">${topic.notes||''}</textarea>`;

  const mcqHtml = `<div style="display:flex;align-items:center;gap:10px;margin-top:8px">
    <span style="font-size:12px;color:var(--text-muted)">MCQ Score:</span>
    <input type="number" min="0" max="100" value="${topic.mcqScore||''}" placeholder="%" style="width:70px;background:var(--surface2);border:1px solid var(--border);border-radius:var(--r-sm);padding:4px 8px;color:var(--text);font-size:12px;outline:none" oninput="saveTopic('${topic.id}','mcqScore',+this.value||null)">
    <button class="btn btn-ghost btn-sm" onclick="toggleWeak('${topic.id}')">⚠️ ${topic.weakFlag?'Remove Weak Flag':'Flag as Weak'}</button>
    <button class="btn btn-secondary btn-sm" onclick="openMoveTopic('${topic.id}')" style="margin-left:auto">📦 Move</button>
  </div>`;

  const linksHtml = topic.links?.map((l,i) => `
    <div class="resource-link-item">
      <a href="${l.url}" target="_blank">${l.title||l.url}</a>
      <button class="del-link-btn" onclick="removeLink('${topic.id}',${i})">✕</button>
    </div>`).join('') || '';

  const resourcesHtml = `
    <div class="resources-grid">
      <div class="resource-input-group">
        <label class="resource-label">📗 Textbook Ref</label>
        <input class="resource-input" placeholder="e.g. Snell's, pg 142" value="${topic.resources?.book||''}" oninput="saveTopic('${topic.id}','resources.book',this.value)">
      </div>
      <div class="resource-input-group">
        <label class="resource-label">🎥 Video Ref</label>
        <input class="resource-input" placeholder="e.g. Armando YouTube" value="${topic.resources?.video||''}" oninput="saveTopic('${topic.id}','resources.video',this.value)">
      </div>
    </div>
    <div class="resource-links-list">${linksHtml}</div>
    <div class="add-resource-row">
      <input class="resource-input" id="link-url-${topic.id}" placeholder="https://..." style="flex:2">
      <input class="resource-input" id="link-title-${topic.id}" placeholder="Title" style="flex:1">
      <button class="add-res-btn" onclick="addLink('${topic.id}')">+ Add Link</button>
    </div>`;

  const promptsHtml = Object.entries({
    concept:['🧠 Explain', 'Deep concept explanation'],
    mcq:['📝 MCQs', 'Generate 50 practice MCQs'],
    research:['🔬 Research', 'Latest studies & guidelines'],
    recall:['🔁 Recall', '20 active recall questions'],
    teach:['🗣️ Teach Me', 'Teach like I\'m a beginner'],
  }).map(([k,[title,sub]]) => `
    <button class="prompt-btn" onclick="copyPrompt('${topic.id}','${k}')">
      <div class="prompt-btn-title">${title} <span class="copy-indicator" id="ci-${topic.id}-${k}">✓ Copied!</span></div>
      <div class="prompt-btn-sub">${sub}</div>
    </button>`).join('');

  const genHtml = `<button class="prompt-btn gen-html" onclick="copyPrompt('${topic.id}','htmlStudyFile')">
    <span style="font-size:26px">✨</span>
    <div>
      <div class="prompt-btn-title">Generate Full Study File <span class="copy-indicator" id="ci-${topic.id}-htmlStudyFile">✓ Copied!</span></div>
      <div class="prompt-btn-sub">FIKRI system: 7 tabs · MCQs · Cases · Schedule · Recall → paste into AI</div>
    </div>
  </button>`;

  const deleteBtnHtml = `<div style="margin-top:12px;text-align:right">
    <button class="btn btn-danger btn-sm" onclick="deleteTopic('${topic.id}')">🗑️ Delete Topic</button>
  </div>`;

  return `
    <div class="topic-section-title">📋 Study Stages</div>
    <div class="stages-grid">${stagesHtml}</div>
    <div class="topic-section-title">🔁 Automated Spaced Repetition</div>
    ${sm2Html}
    <div class="topic-section-title">📝 Vault: Notes & Links</div>
    ${notesHtml}
    ${mcqHtml}
    <div style="margin-top:12px"></div>
    ${resourcesHtml}
    <div class="topic-section-title">🤖 AI Study Prompts</div>
    <div class="prompts-grid">${promptsHtml}${genHtml}</div>
    ${deleteBtnHtml}
  `;
}

// ─────────────────────────────────────────────
//  TOPIC ACTIONS
// ─────────────────────────────────────────────
function toggleTopic(topicId) {
  const body = document.getElementById('tb-' + topicId);
  const icon = document.getElementById('exp-' + topicId);
  if (!body) return;
  body.classList.toggle('open');
  if (icon) icon.classList.toggle('open');
}

function toggleStage(topicId, stageKey) {
  const r = findTopic(topicId);
  if (!r) return;
  const { topic } = r;
  topic.stages[stageKey] = !topic.stages[stageKey];
  if (topic.stages[stageKey]) trackWeeklyStudied(topicId);
  
  if (topic.stages.mastered && !topic.nextReviewDate) {
    topic.nextReviewDate = addDays(todayStr(), 1);
    topic.lastReviewDate = todayStr();
    state.heatmap[todayStr()] = (state.heatmap[todayStr()]||0) + 1;
    updateStreak();
  }
  saveState();
  refreshTopicCard(topicId);
}

function reviewTopic(topicId) {
  const r = findTopic(topicId);
  if (!r) return;
  autoSpacedRepetition(r.topic);
  showToast(`✅ Reviewed — Next review in ${r.topic.interval}d`, 'success');
  refreshTopicCard(topicId);
  renderHomeStats && renderHomeStats();
}

function toggleWeak(topicId) {
  const r = findTopic(topicId);
  if (!r) return;
  r.topic.weakFlag = !r.topic.weakFlag;
  saveState();
  refreshTopicCard(topicId);
  showToast(r.topic.weakFlag ? '⚠️ Marked as weak' : '✅ Weak flag removed', 'info');
}

function toggleStar(topicId) {
  const r = findTopic(topicId);
  if (!r) return;
  r.topic.starred = !r.topic.starred;
  saveState();
  refreshTopicCard(topicId);
}

function saveTopic(topicId, field, value) {
  const r = findTopic(topicId);
  if (!r) return;
  if (field.includes('.')) {
    const [a,b] = field.split('.');
    r.topic[a] = r.topic[a] || {};
    r.topic[a][b] = value;
  } else {
    r.topic[field] = value;
  }
  saveState();
}

function addLink(topicId) {
  const urlEl = document.getElementById('link-url-' + topicId);
  const titleEl = document.getElementById('link-title-' + topicId);
  if (!urlEl || !urlEl.value.trim()) return;
  const r = findTopic(topicId);
  if (!r) return;
  r.topic.links = r.topic.links || [];
  r.topic.links.push({ url: urlEl.value.trim(), title: titleEl?.value.trim() || '' });
  urlEl.value = ''; if (titleEl) titleEl.value = '';
  saveState();
  refreshTopicCard(topicId);
}

function removeLink(topicId, idx) {
  const r = findTopic(topicId);
  if (!r) return;
  r.topic.links.splice(idx, 1);
  saveState();
  refreshTopicCard(topicId);
}

function deleteTopic(topicId) {
  if (!confirm('Delete this topic? This cannot be undone.')) return;
  const r = findTopic(topicId);
  if (!r) return;
  r.folder.topics = r.folder.topics.filter(t => t.id !== topicId);
  saveState();
  if (currentView === 'subfolder') renderSubfolderView();
  showToast('Topic deleted', 'warn');
}

function refreshTopicCard(topicId) {
  const card = document.getElementById('tc-' + topicId);
  const body = document.getElementById('tb-' + topicId);
  if (!card || !body) return;
  const r = findTopic(topicId);
  if (!r) return;
  const { topic } = r;
  const isOpen = body.classList.contains('open');
  const newCard = document.createElement('div');
  newCard.innerHTML = buildTopicCard(topic);
  const newEl = newCard.firstElementChild;
  card.replaceWith(newEl);
  if (isOpen) {
    document.getElementById('tb-' + topicId)?.classList.add('open');
    document.getElementById('exp-' + topicId)?.classList.add('open');
  }
}

function copyPrompt(topicId, type) {
  const r = findTopic(topicId);
  if (!r) return;
  const text = PROMPTS[type](r.topic.name);
  navigator.clipboard.writeText(text).then(() => {
    const ci = document.getElementById(`ci-${topicId}-${type}`);
    if (ci) { ci.classList.add('show'); setTimeout(() => ci.classList.remove('show'), 2000); }
  }).catch(() => showToast('Copy failed. Please try again.', 'error'));
}

function filterTopics() {
  const folder = findFolder(currentFolderId);
  if (!folder) return;
  renderTopicList(folder.topics);
}

function setFilter(btn, filter) {
  topicFilter = filter;
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  filterTopics();
}

// ─────────────────────────────────────────────
//  FOLDER & MOVE MANAGEMENT
// ─────────────────────────────────────────────
function openAddFolder() {
  document.getElementById('add-folder-overlay').classList.add('show');
  document.getElementById('add-folder-name').value = '';
  document.getElementById('add-folder-icon').value = '📁';
  document.getElementById('add-folder-label').value = 'MBBS-I';
}
function closeAddFolder() { document.getElementById('add-folder-overlay').classList.remove('show'); }
function saveAddFolder() {
  const name = document.getElementById('add-folder-name').value.trim();
  if (!name) { showToast('Folder name is required', 'error'); return; }
  const folder = {
    id: genId(), parentId: null, name,
    icon: document.getElementById('add-folder-icon').value.trim() || '📁',
    color: document.getElementById('add-folder-color').value || '#00D2BE',
    label: document.getElementById('add-folder-label').value.trim(),
    subfolders: [], topics: []
  };
  state.folders.push(folder);
  saveState(); closeAddFolder(); renderFolders();
  showToast(`📁 Folder "${name}" created!`, 'success');
}

function openEditFolder(fid) {
  const f = findFolder(fid);
  if (!f) return;
  document.getElementById('add-folder-name').value = f.name;
  document.getElementById('add-folder-icon').value = f.icon;
  document.getElementById('add-folder-color').value = f.color || '#00D2BE';
  document.getElementById('add-folder-label').value = f.label || '';
  const saveBtn = document.querySelector('#add-folder-overlay .btn-primary');
  if (saveBtn) {
    saveBtn.textContent = 'Save Changes';
    saveBtn.onclick = () => {
      f.name = document.getElementById('add-folder-name').value.trim() || f.name;
      f.icon = document.getElementById('add-folder-icon').value.trim() || f.icon;
      f.color = document.getElementById('add-folder-color').value || f.color;
      f.label = document.getElementById('add-folder-label').value.trim();
      saveState(); closeAddFolder(); 
      if (currentFolderId) renderSubfolderView(); else renderFolders();
      showToast('Folder updated', 'success');
      saveBtn.textContent = 'Create Folder'; saveBtn.onclick = saveAddFolder;
    };
  }
  document.getElementById('add-folder-overlay').classList.add('show');
}

function deleteFolder(fid) {
  const f = findFolder(fid);
  if (!f) return;
  if (!confirm(`Delete folder "${f.name}" and ALL nested contents? This cannot be undone.`)) return;
  
  if (f.parentId) {
    const parent = findFolder(f.parentId);
    parent.subfolders = parent.subfolders.filter(x => x.id !== fid);
  } else {
    state.folders = state.folders.filter(x => x.id !== fid);
  }
  saveState(); 
  
  if (currentFolderId === fid) {
    goBackFromSubfolder();
  } else if (currentFolderId) {
    renderSubfolderView();
  } else {
    renderFolders();
  }
  showToast(`Folder deleted`, 'warn');
}

function openAddSubfolder() {
  const f = findFolder(currentFolderId);
  if (!f) return;
  document.getElementById('add-subfolder-subtitle').textContent = `Inside "${f.name}"`;
  document.getElementById('add-subfolder-name').value = '';
  document.getElementById('add-subfolder-overlay').classList.add('show');
}
function closeAddSubfolder() { document.getElementById('add-subfolder-overlay').classList.remove('show'); }
function saveAddSubfolder() {
  const name = document.getElementById('add-subfolder-name').value.trim();
  if (!name) { showToast('Subfolder name required', 'error'); return; }
  const f = findFolder(currentFolderId);
  if (!f) return;
  f.subfolders = f.subfolders || [];
  f.subfolders.push({ id: genId(), parentId: f.id, name, topics: [], subfolders: [] });
  saveState(); closeAddSubfolder(); renderSubfolderView();
  showToast(`Subfolder "${name}" created`, 'success');
}

function openAddTopic() {
  document.getElementById('add-topic-name').value = '';
  document.getElementById('add-topic-difficulty').value = 'medium';
  document.getElementById('add-topic-weightage').value = 2;
  document.getElementById('add-topic-tags').value = '';
  document.getElementById('add-topic-overlay').classList.add('show');
}
function closeAddTopic() { document.getElementById('add-topic-overlay').classList.remove('show'); }
function saveAddTopic() {
  const name = document.getElementById('add-topic-name').value.trim();
  if (!name) { showToast('Topic name required', 'error'); return; }
  const f = findFolder(currentFolderId);
  if (!f) return;
  const tags = document.getElementById('add-topic-tags').value.split(',').map(t=>t.trim()).filter(Boolean);
  const topic = createTopicData(name, currentFolderId, {
    difficulty: document.getElementById('add-topic-difficulty').value,
    weightage: +document.getElementById('add-topic-weightage').value || 2,
    tags
  });
  f.topics.push(topic);
  saveState(); closeAddTopic(); renderSubfolderView();
  showToast(`Topic "${name}" added`, 'success');
}

// Move Topic
let movingTopicId = null;
function openMoveTopic(topicId) {
  movingTopicId = topicId;
  const select = document.getElementById('move-topic-dest');
  select.innerHTML = buildFolderOptions(state.folders, 0);
  document.getElementById('move-topic-overlay').classList.add('show');
}
function buildFolderOptions(folders, depth) {
  let html = '';
  const prefix = '&nbsp;'.repeat(depth * 4) + (depth > 0 ? '↳ ' : '');
  folders.forEach(f => {
    html += `<option value="${f.id}">${prefix}${f.icon||'📂'} ${f.name}</option>`;
    if(f.subfolders) html += buildFolderOptions(f.subfolders, depth + 1);
  });
  return html;
}
function closeMoveTopic() { document.getElementById('move-topic-overlay').classList.remove('show'); }
function confirmMoveTopic() {
  const destId = document.getElementById('move-topic-dest').value;
  const { topic, folder } = findTopic(movingTopicId);
  if (folder.id === destId) { closeMoveTopic(); return; } 
  
  const destFolder = findFolder(destId);
  if(!destFolder) return;

  folder.topics = folder.topics.filter(t => t.id !== movingTopicId);
  topic.parentId = destId;
  destFolder.topics.push(topic);

  saveState();
  closeMoveTopic();
  if (currentView === 'subfolder') renderSubfolderView();
  showToast('Topic moved successfully', 'success');
}

// ─────────────────────────────────────────────
//  REVISION SESSION VIEW
// ─────────────────────────────────────────────
function renderRevisionView() {
  const due = getDueTopics();
  const content = document.getElementById('revision-content');
  if (!content) return;

  if (due.length === 0) {
    content.innerHTML = `<div class="rev-empty">
      <div class="rev-empty-icon">🎉</div>
      <div style="font-family:var(--font-head);font-size:20px;font-weight:800;margin-bottom:8px">All caught up!</div>
      <div style="color:var(--text-muted);font-size:14px;margin-bottom:20px">No topics due for review today.</div>
      <button class="btn btn-primary" onclick="showView('folders')">📚 Study New Topics</button>
    </div>`;
    return;
  }

  const queueHtml = due.slice(0, 10).map((item, i) => `
    <div class="rev-queue-item ${i===0?'current':''}" onclick="goToTopic('${item.topic.id}')" title="Go to topic">
      <div class="q-dot" style="background:${i===0?'var(--primary)':'var(--surface3)'}"></div>
      <span style="flex:1">${item.topic.name}</span>
      <span style="color:var(--text-muted);font-size:11px">${item.folder.name}</span>
    </div>`).join('');

  content.innerHTML = `
    <div style="display:grid;grid-template-columns:1fr 280px;gap:16px">
      <div id="rev-session-area">
        ${buildRevCard(due[0])}
      </div>
      <div>
        <div class="section-title">📋 Queue (${due.length} topics)</div>
        <div class="rev-queue-list">${queueHtml}</div>
        ${due.length > 10 ? `<div style="font-size:12px;color:var(--text-muted);margin-top:8px;text-align:center">+${due.length-10} more topics</div>` : ''}
      </div>
    </div>`;
}

function buildRevCard({ topic, folder }) {
  return `
    <div class="revision-session-card">
      <div class="rev-topic-name" onclick="goToTopic('${topic.id}')" title="Go to Topic Folder">${topic.name} 🔗</div>
      <div class="rev-topic-folder">${folder.icon||'📂'} ${folder.name}</div>
      <div style="font-size:12px;color:var(--text-muted);margin-bottom:16px">
        Interval: ${topic.interval||0}d · Reps: ${topic.repetitions||0} · 
        ${topic.lastReviewDate ? `Last: ${topic.lastReviewDate}` : 'First review'}
      </div>
      ${topic.notes ? `<div style="background:var(--surface2);border-radius:var(--r-sm);padding:12px;font-size:12px;color:var(--text-muted);text-align:left;margin-bottom:16px;max-height:100px;overflow-y:auto">${topic.notes}</div>` : ''}
      <div class="rev-quality-btns">
        <button class="rev-q-btn good" style="width:100%;max-width:300px" onclick="sessionReview('${topic.id}')">
          ✅ Mark Reviewed
          <div class="rev-q-sub">Next Review: In ${calcNextInterval(topic)} days</div>
        </button>
      </div>
    </div>`;
}

function startRevisionSession() { showView('revision'); }

function sessionReview(topicId) {
  const r = findTopic(topicId);
  if (!r) return;
  autoSpacedRepetition(r.topic);
  showToast(`✅ Reviewed — Next: ${r.topic.interval}d`, 'success');
  setTimeout(() => renderRevisionView(), 300);
}

function showWeakAreas() { showView('analytics'); }

// ─────────────────────────────────────────────
//  WEEKEND REVISION PANEL
// ─────────────────────────────────────────────
let _wkndQueue = [];
let _wkndIdx = 0;
let _wkndDone = [];

function renderWeekendRevision() {
  const panel = document.getElementById('weekend-revision-panel');
  if (!panel) return;
  const items = getWeeklyStudiedTopics();
  const dayName = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'][new Date().getDay()];
  
  panel.style.display = 'block';
  const statsRow = document.getElementById('weekend-stats-row');
  const grid = document.getElementById('weekend-topic-grid');
  if (!grid) return;

  if (items.length === 0) {
    panel.style.display = isWeekend() ? 'block' : 'none';
    if (statsRow) statsRow.innerHTML = '';
    grid.innerHTML = `<div style="color:var(--text-muted);font-size:12px">No topics studied this week yet — check back here on the weekend!</div>`;
    return;
  }

  if (!isWeekend()) {
    if (statsRow) statsRow.innerHTML = `
      <div class="weekend-stat">📅 <span>${items.length}</span> topic${items.length !== 1 ? 's' : ''} tracked this week</div>
      <div class="weekend-stat" style="color:var(--text-dim);font-size:11px">— weekend revision unlocks Sat & Sun</div>`;
    grid.innerHTML = items.slice(0, 6).map(({ topic, folder }) => `
      <div class="weekend-topic-chip" onclick="goToTopic('${topic.id}')">
        <span>${topic.name.length > 26 ? topic.name.slice(0, 24) + '…' : topic.name}</span>
        <span class="chip-folder">${folder.icon||'📂'}</span>
      </div>`).join('') + (items.length > 6 ? `<div class="weekend-topic-chip" style="color:var(--text-muted)">+${items.length - 6} more</div>` : '');
    return;
  }

  const reviewed = _wkndDone.length;
  const total = items.length;
  const remaining = items.filter(i => !_wkndDone.includes(i.topic.id)).length;

  if (statsRow) statsRow.innerHTML = `
    <div class="weekend-stat">📚 <span>${total}</span> topics this week</div>
    <div class="weekend-stat">✅ <span>${reviewed}</span> reviewed</div>
    <div class="weekend-stat">⏳ <span>${remaining}</span> remaining</div>
    <div class="weekend-stat">📅 <span>${dayName}</span> — great day to consolidate!</div>`;

  grid.innerHTML = items.map(({ topic, folder }) => {
    const done = _wkndDone.includes(topic.id);
    const pct = getTopicCompletion(topic);
    return `<div class="weekend-topic-chip ${done ? 'reviewed' : ''}" onclick="goToTopic('${topic.id}')">
      ${done ? '✅ ' : ''}<span>${topic.name.length > 24 ? topic.name.slice(0,22)+'…' : topic.name}</span>
      <span class="chip-folder">${folder.icon||'📂'}</span>
      <span class="chip-status">${pct}%</span>
    </div>`;
  }).join('');
}

function startWeekendSession() {
  const items = getWeeklyStudiedTopics();
  if (items.length === 0) { showToast('No topics to review yet!', 'warn'); return; }
  _wkndQueue = [...items.filter(i => !_wkndDone.includes(i.topic.id)), ...items.filter(i => _wkndDone.includes(i.topic.id))];
  _wkndIdx = 0;
  document.getElementById('weekend-session-overlay').classList.add('show');
  renderWkndSessionCard();
}

function closeWeekendSession() {
  document.getElementById('weekend-session-overlay').classList.remove('show');
  renderWeekendRevision();
}

function renderWkndSessionCard() {
  const body = document.getElementById('wknd-session-body');
  const sub = document.getElementById('wknd-session-subtitle');
  if (!body) return;

  if (_wkndIdx >= _wkndQueue.length) {
    const total = _wkndQueue.length;
    if (sub) sub.textContent = 'Session complete 🎉';
    body.innerHTML = `
      <div class="wknd-done-banner">
        <h2>🎉 Weekend Revision Done!</h2>
        <p style="color:var(--text-muted);font-size:14px;margin-bottom:18px">You reviewed all <strong>${total}</strong> topics from this week.</p>
        <div style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap">
          <button class="btn btn-primary" onclick="closeWeekendSession()">✅ Close Session</button>
          <button class="btn btn-secondary" onclick="_wkndIdx=0;_wkndDone=[];renderWkndSessionCard()">🔁 Restart</button>
        </div>
      </div>`;
    return;
  }

  const { topic, folder } = _wkndQueue[_wkndIdx];
  const total = _wkndQueue.length;
  const pct = Math.round((_wkndIdx / total) * 100);
  
  if (sub) sub.textContent = `Topic ${_wkndIdx + 1} of ${total}`;

  body.innerHTML = `
    <div class="wknd-progress-bar"><div class="wknd-progress-fill" style="width:${pct}%"></div></div>
    <div class="wknd-counter">${_wkndIdx} of ${total} reviewed · ${total - _wkndIdx} remaining</div>
    <div class="wknd-session-card">
      <div class="wknd-session-topic">${topic.name}</div>
      <div class="wknd-session-folder">${folder.icon||'📂'} ${folder.name}</div>
      ${topic.notes ? `<div class="wknd-session-notes">${topic.notes}</div>` : ''}
      <div class="wknd-btns">
        <button class="wknd-btn skip" onclick="wkndSkip()">⏭ Skip</button>
        <button class="wknd-btn good" style="flex:2;background:var(--blue-dim);color:var(--blue);border:1px solid rgba(58,134,255,0.3);" onclick="wkndRate()">
          ✅ Reviewed<small>Next: ${calcNextInterval(topic)}d</small>
        </button>
      </div>
    </div>
    <div style="text-align:center;margin-top:4px"><button class="btn btn-ghost btn-sm" onclick="closeWeekendSession()">✕ End Session</button></div>`;
}

function wkndRate() {
  if (_wkndIdx >= _wkndQueue.length) return;
  const { topic } = _wkndQueue[_wkndIdx];
  autoSpacedRepetition(topic);
  if (!_wkndDone.includes(topic.id)) _wkndDone.push(topic.id);
  _wkndIdx++;
  renderWkndSessionCard();
  showToast(`✅ Reviewed — next in ${topic.interval}d`, 'success');
}

function wkndSkip() {
  if (_wkndIdx >= _wkndQueue.length) return;
  _wkndIdx++;
  renderWkndSessionCard();
}

// ─────────────────────────────────────────────
//  ANALYTICS
// ─────────────────────────────────────────────
let chartInstances = {};
function renderAnalytics() {
  renderWeakStrong();
  setTimeout(() => {
    renderSubjectChart();
    renderPieChart();
    renderWeeklyChart();
    renderRevisionChart();
  }, 50);
}

function renderWeakStrong() {
  const all = getAllTopics();
  const weak = all.filter(({topic}) => topic.weakFlag || (topic.mcqScore !== null && topic.mcqScore < 60));
  const strong = all.filter(({topic}) => topic.stages.mastered);
  const wl = document.getElementById('weak-topics-list');
  const sl = document.getElementById('strong-topics-list');
  if (wl) wl.innerHTML = weak.length === 0 ? '<div style="color:var(--text-dim);font-size:12px">No weak topics flagged</div>' : weak.slice(0,8).map(({topic,folder}) => `<div class="ws-item"><span style="color:var(--red)">⚠️</span><span style="flex:1" onclick="goToTopic('${topic.id}')" style="cursor:pointer">${topic.name}</span><span style="color:var(--text-muted);font-size:11px">${folder.name}</span></div>`).join('');
  if (sl) sl.innerHTML = strong.length === 0 ? '<div style="color:var(--text-dim);font-size:12px">No mastered topics yet</div>' : strong.slice(0,8).map(({topic,folder}) => `<div class="ws-item"><span style="color:var(--green)">✅</span><span style="flex:1" onclick="goToTopic('${topic.id}')" style="cursor:pointer">${topic.name}</span><span style="color:var(--text-muted);font-size:11px">${folder.name}</span></div>`).join('');
}

function destroyChart(id) {
  if (chartInstances[id]) { try { chartInstances[id].destroy(); } catch(e){} delete chartInstances[id]; }
}

function renderSubjectChart() {
  destroyChart('subjects');
  const ctx = document.getElementById('chart-subjects');
  if (!ctx) return;
  const labels = state.folders.map(f => f.name.length > 14 ? f.name.slice(0,14)+'…' : f.name);
  const data = state.folders.map(f => {
    const all = getFolderTopicsRec(f);
    return all.length > 0 ? Math.round((all.filter(t=>t.stages.mastered).length/all.length)*100) : 0;
  });
  chartInstances.subjects = new Chart(ctx, {
    type: 'bar',
    data: { labels, datasets: [{ label:'Progress %', data, backgroundColor: state.folders.map(f=>f.color+'99'), borderColor: state.folders.map(f=>f.color), borderWidth:1, borderRadius:6 }] },
    options: { responsive:true, plugins:{ legend:{display:false} }, scales:{ y:{beginAtZero:true,max:100,ticks:{color:'#4D7A9A',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}}, x:{ticks:{color:'#4D7A9A',font:{size:10}},grid:{display:false}} } }
  });
}

function renderPieChart() {
  destroyChart('pie');
  const ctx = document.getElementById('chart-pie');
  if (!ctx) return;
  const all = getAllTopics();
  const mastered = all.filter(({topic})=>topic.stages.mastered).length;
  const inProg = all.filter(({topic})=>!topic.stages.mastered&&getTopicCompletion(topic)>0).length;
  const notStarted = all.length - mastered - inProg;
  chartInstances.pie = new Chart(ctx, {
    type:'doughnut',
    data:{ labels:['Mastered','In Progress','Not Started'], datasets:[{ data:[mastered,inProg,notStarted], backgroundColor:['rgba(6,255,165,0.7)','rgba(0,210,190,0.7)','rgba(21,36,56,0.9)'], borderColor:['#06FFA5','#00D2BE','#2A4A6A'], borderWidth:1 }] },
    options:{ responsive:true, plugins:{ legend:{ position:'bottom', labels:{ color:'#4D7A9A',font:{size:10} } } } }
  });
}

function renderWeeklyChart() {
  destroyChart('weekly');
  const ctx = document.getElementById('chart-weekly');
  if (!ctx) return;
  const days = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
  const today = new Date().getDay();
  const labels = [...Array(7)].map((_,i)=>days[(today-6+i+7)%7]);
  const data = (state.studyTimer.weeklyData||[0,0,0,0,0,0,0]).map(s=>+(s/3600).toFixed(1));
  chartInstances.weekly = new Chart(ctx, {
    type:'line',
    data:{ labels, datasets:[{ label:'Hours', data, fill:true, backgroundColor:'rgba(0,210,190,0.08)', borderColor:'#00D2BE', borderWidth:2, pointBackgroundColor:'#00D2BE', tension:0.4 }] },
    options:{ responsive:true, plugins:{legend:{display:false}}, scales:{ y:{beginAtZero:true,ticks:{color:'#4D7A9A',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}}, x:{ticks:{color:'#4D7A9A',font:{size:10}},grid:{display:false}} } }
  });
}

function renderRevisionChart() {
  destroyChart('revisions');
  const ctx = document.getElementById('chart-revisions');
  if (!ctx) return;
  const today = todayStr();
  const labels = state.folders.map(f=>f.name.length>10?f.name.slice(0,10)+'…':f.name);
  const data = state.folders.map(f=>{
    const all = getFolderTopicsRec(f);
    return all.filter(t=>t.nextReviewDate&&t.nextReviewDate<=today).length;
  });
  chartInstances.revisions = new Chart(ctx, {
    type:'bar',
    data:{ labels, datasets:[{ label:'Due', data, backgroundColor:'rgba(255,71,87,0.5)', borderColor:'#FF4757', borderWidth:1, borderRadius:4 }] },
    options:{ responsive:true, plugins:{legend:{display:false}}, scales:{ y:{beginAtZero:true,ticks:{color:'#4D7A9A',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}}, x:{ticks:{color:'#4D7A9A',font:{size:10}},grid:{display:false}} } }
  });
}

// ─────────────────────────────────────────────
//  SETTINGS & JSON
// ─────────────────────────────────────────────
function openSettingsPanel() { document.getElementById('settings-overlay').classList.add('show'); }
function closeSettingsPanel() { document.getElementById('settings-overlay').classList.remove('show'); }

let pendingJsonData = null;
let mergeOption = 'merge';

function openJsonImport() {
  pendingJsonData = null;
  document.getElementById('import-btn').disabled = true;
  document.getElementById('import-log').style.display = 'none';
  document.getElementById('json-import-overlay').classList.add('show');
  selectMergeOpt('merge');
}
function closeJsonImport() { document.getElementById('json-import-overlay').classList.remove('show'); }
function selectMergeOpt(opt) {
  mergeOption = opt;
  document.getElementById('opt-merge').classList.toggle('selected', opt==='merge');
  document.getElementById('opt-replace').classList.toggle('selected', opt==='replace');
}
function handleJsonDragOver(e) { e.preventDefault(); document.getElementById('json-upload-zone').classList.add('drag'); }
function handleJsonDrop(e) { e.preventDefault(); document.getElementById('json-upload-zone').classList.remove('drag'); const f = e.dataTransfer.files[0]; if (f) processJsonFile(f); }
function handleJsonFileSelect(e) { const f = e.target.files[0]; if (f) processJsonFile(f); }

function processJsonFile(file) {
  if (!file.name.endsWith('.json')) { showImportLog([{ type:'error', msg:`❌ Invalid file type: ${file.name}. Only .json accepted.` }]); return; }
  const reader = new FileReader();
  reader.onload = (e) => {
    try {
      const data = JSON.parse(e.target.result);
      if (!data.folders) throw new Error("Missing 'folders' array.");
      pendingJsonData = data;
      document.getElementById('import-btn').disabled = false;
      showImportLog([{ type:'success', msg:`✅ Valid JSON detected. Click Import Syllabus to proceed.` }]);
    } catch(err) {
      showImportLog([{ type:'error', msg:`❌ Invalid JSON: ${err.message}` }]);
    }
  };
  reader.readAsText(file);
}
function showImportLog(entries) {
  const log = document.getElementById('import-log');
  log.style.display = 'block';
  log.innerHTML = entries.map(e => `<div class="log-${e.type}">${e.msg}</div>`).join('');
}

function importJson() {
  if (!pendingJsonData) return;
  if (mergeOption === 'replace') state.folders = [];
  
  pendingJsonData.folders.forEach(rawFolder => {
    let existingFolder = state.folders.find(f => f.name.toLowerCase() === rawFolder.name.toLowerCase());
    if (!existingFolder) {
      existingFolder = { id: genId(), parentId: null, name: rawFolder.name, icon: rawFolder.icon || '📁', color: rawFolder.color || '#00D2BE', label: rawFolder.label || '', subfolders: [], topics: [] };
      state.folders.push(existingFolder);
    }
    (rawFolder.topics || []).forEach(rt => {
      const tname = rt.title || rt.name;
      if (!existingFolder.topics.find(t => t.name.toLowerCase() === tname.toLowerCase())) existingFolder.topics.push(createTopicData(tname, existingFolder.id));
    });
  });
  saveState();
  closeJsonImport();
  showToast(`✅ Import complete!`, 'success');
  if (currentView === 'folders') { currentFolderId = null; renderFolders(); }
}

function exportBackup() {
  try {
    const data = JSON.stringify(state, null, 2);
    const blob = new Blob([data], { type:'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url; a.download = `medtrack-backup-${todayStr()}.json`;
    a.click(); URL.revokeObjectURL(url);
    showToast('💾 Backup downloaded!', 'success');
  } catch(e) { showToast('Backup failed: ' + e.message, 'error'); }
}
function handleRestoreFile(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = (ev) => {
    try {
      const data = JSON.parse(ev.target.result);
      if (!data.folders) throw new Error('Invalid backup file');
      state = { ...state, ...data };
      patchTree(state.folders);
      saveState();
      closeRestore();
      showToast('♻️ Backup restored and synced!', 'success');
      showView('home');
    } catch(err) {
      document.getElementById('restore-status').textContent = '❌ Error: ' + err.message;
    }
  };
  reader.readAsText(file);
}
function closeRestore() { document.getElementById('restore-overlay').classList.remove('show'); }
function dangerClearData() {
  if (!confirm('⚠️ Clear ALL study data? This cannot be undone!')) return;
  localStorage.clear();
  showToast('All data cleared. Refreshing…', 'warn');
  setTimeout(() => location.reload(), 1500);
}

// ─────────────────────────────────────────────
//  FUZZY SEARCH
// ─────────────────────────────────────────────
function fuzzyMatch(str, pattern) {
  str = str.toLowerCase(); pattern = pattern.toLowerCase();
  if (str.includes(pattern)) return true;
  let si = 0;
  for (let i = 0; i < str.length && si < pattern.length; i++) if (str[i] === pattern[si]) si++;
  return si === pattern.length;
}

function doSearch() {
  const q = document.getElementById('global-search')?.value?.trim();
  const dd = document.getElementById('search-dropdown');
  if (!q || q.length < 2) { dd.classList.remove('show'); return; }

  const results = [];
  const addFolders = (folders) => {
    folders.forEach(f => {
      if (fuzzyMatch(f.name, q)) results.push({ type:'folder', icon:f.icon||'📁', name:f.name, sub:'Folder', action:`MapsToFolder('${f.id}')` });
      f.topics.forEach(t => { if (fuzzyMatch(t.name, q)) results.push({ type:'topic', icon:'📑', name:t.name, sub:f.name, action:`goToTopic('${t.id}')` }); });
      if (f.subfolders) addFolders(f.subfolders);
    });
  }
  addFolders(state.folders);

  if (results.length === 0) { dd.classList.remove('show'); return; }

  dd.innerHTML = results.slice(0, 8).map(r => `<div class="sr-item" onclick="${r.action};document.getElementById('global-search').value='';document.getElementById('search-dropdown').classList.remove('show')"><span class="sr-icon">${r.icon}</span><div><div class="sr-name">${r.name}</div><div class="sr-sub">${r.sub}</div></div><span class="sr-badge">${r.type}</span></div>`).join('');
  dd.classList.add('show');
}
document.addEventListener('click', e => { const dd = document.getElementById('search-dropdown'); const inp = document.getElementById('global-search'); if (dd && inp && !dd.contains(e.target) && !inp.contains(e.target)) dd.classList.remove('show'); });

// ─────────────────────────────────────────────
//  TIMER & POMODORO
// ─────────────────────────────────────────────
let timerRunning = false;
let timerElapsed = 0;
let timerInterval = null;

function initTimer() {
  const today = todayStr();
  if (state.studyTimer.lastDate !== today) {
    state.studyTimer.weeklyData = state.studyTimer.weeklyData || [0,0,0,0,0,0,0];
    state.studyTimer.weeklyData.push(state.studyTimer.todayElapsed || 0);
    state.studyTimer.weeklyData = state.studyTimer.weeklyData.slice(-7);
    state.studyTimer.todayElapsed = 0;
    state.studyTimer.lastDate = today;
    saveState();
  }
  timerElapsed = state.studyTimer.todayElapsed || 0;
  updateTimerDisplays();
}

function toggleTimer() { if (timerRunning) pauseTimer(); else startTimer(); }
function startTimer() {
  if (timerRunning) return;
  timerRunning = true;
  timerInterval = setInterval(() => { timerElapsed++; updateTimerDisplays(); }, 1000);
  document.getElementById('start-stop-btn').textContent = '⏸ Pause';
  document.getElementById('timer-display').classList.add('running');
  updateStreak();
}
function pauseTimer() {
  timerRunning = false; clearInterval(timerInterval);
  state.studyTimer.totalElapsed = (state.studyTimer.totalElapsed || 0) + Math.max(0, timerElapsed - (state.studyTimer.todayElapsed || 0));
  state.studyTimer.todayElapsed = timerElapsed;
  saveState();
  document.getElementById('start-stop-btn').textContent = '▶ Start';
  document.getElementById('timer-display').classList.remove('running');
}
function resetDayTimer() { pauseTimer(); timerElapsed = 0; state.studyTimer.todayElapsed = 0; saveState(); updateTimerDisplays(); showToast('Timer reset', 'info'); }
function updateTimerDisplays() {
  const s = fmtSec(timerElapsed);
  const disp = document.getElementById('timer-display'); if (disp) disp.textContent = '⏱ ' + s;
  const big = document.getElementById('dash-timer-big'); if (big) big.textContent = s;
  const td = document.getElementById('stat-today-time'); if (td) td.textContent = fmtHrsMin(timerElapsed);
  const wk = document.getElementById('stat-week-time'); if (wk) wk.textContent = fmtHrsMin((state.studyTimer.weeklyData||[]).reduce((a,b)=>a+b,0) + timerElapsed);
  const tot = document.getElementById('stat-total-time'); if (tot) tot.textContent = fmtHrsMin((state.studyTimer.totalElapsed||0) + timerElapsed);
}

const POMO_DURATIONS = { work:50*60, short:10*60, long:20*60 };
let pomodoroTimeLeft = POMO_DURATIONS.work;
let pomodoroRunning = false;
let pomodoroMode = 'work';
let pomodoroSessions = 0;
let pomodoroInterval = null;

function openPomodoro() { document.getElementById('pomo-overlay').classList.add('show'); updatePomodoroDisplay(); }
function closePomodoro() { document.getElementById('pomo-overlay').classList.remove('show'); }
function togglePomodoro() {
  if (pomodoroRunning) {
    pomodoroRunning = false; clearInterval(pomodoroInterval); document.getElementById('pomo-toggle').textContent = '▶ Start';
  } else {
    pomodoroRunning = true; pomodoroInterval = setInterval(tickPomodoro, 1000); document.getElementById('pomo-toggle').textContent = '⏸ Pause';
    if (pomodoroMode === 'work' && !timerRunning) startTimer();
  }
}
function tickPomodoro() {
  pomodoroTimeLeft--; updatePomodoroDisplay();
  if (pomodoroTimeLeft <= 0) {
    pomodoroRunning = false; clearInterval(pomodoroInterval); document.getElementById('pomo-toggle').textContent = '▶ Start';
    if (pomodoroMode === 'work') {
      pomodoroSessions++; showToast('🎉 Pomodoro done! Take a break.', 'success');
      pomodoroMode = pomodoroSessions % 4 === 0 ? 'long' : 'short';
    } else { showToast('⚡ Break over! Focus time!', 'info'); pomodoroMode = 'work'; }
    pomodoroTimeLeft = POMO_DURATIONS[pomodoroMode]; updatePomodoroDisplay();
  }
}
function resetPomodoro() { pomodoroRunning = false; clearInterval(pomodoroInterval); pomodoroMode = 'work'; pomodoroTimeLeft = POMO_DURATIONS.work; document.getElementById('pomo-toggle').textContent = '▶ Start'; updatePomodoroDisplay(); }
function skipPomodoroPhase() { pomodoroTimeLeft = 0; if (pomodoroRunning) tickPomodoro(); else { pomodoroMode = pomodoroMode === 'work' ? 'short' : 'work'; pomodoroTimeLeft = POMO_DURATIONS[pomodoroMode]; updatePomodoroDisplay(); } }
function updatePomodoroDisplay() {
  const m = Math.floor(pomodoroTimeLeft/60), s = pomodoroTimeLeft%60;
  document.getElementById('pomo-time').textContent = `${pad(m)}:${pad(s)}`;
  document.getElementById('pomo-mode').textContent = { work:'FOCUS', short:'SHORT BREAK', long:'LONG BREAK' }[pomodoroMode];
  document.getElementById('pomo-sessions').textContent = `Sessions: ${pomodoroSessions} ${'🍅'.repeat(Math.min(pomodoroSessions,8))}`;
  const offset = 439.8 * (pomodoroTimeLeft/POMO_DURATIONS[pomodoroMode]);
  const ring = document.getElementById('pomo-ring-fill');
  if (ring) { 
    ring.style.strokeDashoffset = offset; 
    if (pomodoroMode !== 'work') {
      ring.classList.add('break');
    } else {
      ring.classList.remove('break');
    }
  }
}

// ─────────────────────────────────────────────
//  THEME & TOAST
// ─────────────────────────────────────────────
function toggleTheme() {
  if (document.body.classList.contains('light')) { document.body.classList.remove('light'); localStorage.setItem('medtrack_theme', 'dark'); document.getElementById('theme-toggle-btn').textContent = '☀️'; }
  else { document.body.classList.add('light'); localStorage.setItem('medtrack_theme', 'light'); document.getElementById('theme-toggle-btn').textContent = '🌙'; }
}
function loadTheme() {
  if (localStorage.getItem('medtrack_theme') === 'light') { document.body.classList.add('light'); document.getElementById('theme-toggle-btn').textContent = '🌙'; }
}
function showToast(msg, type = 'info') {
  const icons = { success:'✅', error:'❌', warn:'⚠️', info:'ℹ️' };
  const tc = document.getElementById('toast-container');
  if (!tc) return;
  const t = document.createElement('div');
  t.className = `toast ${type}`;
  t.innerHTML = `<span>${icons[type]||'ℹ️'}</span>${msg}`;
  tc.appendChild(t);
  setTimeout(() => { try { tc.removeChild(t); } catch(e) {} }, 3000);
}

// ─────────────────────────────────────────────
//  GOOGLE AUTH
// ─────────────────────────────────────────────
function openAuthModal() {
  const overlay = document.getElementById('auth-overlay');
  if (!overlay) return;
  const signoutSec = document.getElementById('auth-signout-section');
  const signinBtn = document.getElementById('auth-google-btn');
  const skipBtn = document.getElementById('auth-skip-btn');
  const sub = document.getElementById('auth-modal-sub');
  if (currentUser && !currentUser.isAnonymous) {
    if (signoutSec) signoutSec.style.display = '';
    if (signinBtn) signinBtn.style.display = 'none';
    if (skipBtn) skipBtn.style.display = 'none';
    if (sub) sub.textContent = `Signed in as ${currentUser.displayName || currentUser.email || 'Google User'}. Your data syncs across all devices.`;
  } else {
    if (signoutSec) signoutSec.style.display = 'none';
    if (signinBtn) signinBtn.style.display = '';
    if (skipBtn) skipBtn.style.display = '';
    if (sub) sub.textContent = 'Sign in with Google to sync your data across all your devices — PC, phone, tablet. Your data follows you everywhere.';
  }
  overlay.classList.add('show');
}

function closeAuthModal() {
  const overlay = document.getElementById('auth-overlay');
  if (overlay) overlay.classList.remove('show');
}

async function signInWithGoogle() {
  if (!auth) { showToast('Firebase not ready, please wait…', 'warn'); return; }
  try {
    const provider = new GoogleAuthProvider();
    const result = await signInWithPopup(auth, provider);
    showToast(`✅ Signed in as ${result.user.displayName || result.user.email}!`, 'success');
    closeAuthModal();
    currentUser = result.user;
    updateNavAuth(result.user);
    saveState();
  } catch(e) {
    if (e.code === 'auth/unauthorized-domain') {
      const sub = document.getElementById('auth-modal-sub');
      let domain = window.location.hostname;
      if (window.location.protocol === 'file:' || !domain) {
        if (sub) {
          sub.innerHTML = `<span style="color:var(--red);font-weight:bold;">Cannot use Google Sign-in from local files (file://)</span><br><br>
          You must run this file through a local web server so it opens via <strong>http://localhost</strong>.`;
        }
      } else {
        if (sub) {
          sub.innerHTML = `<span style="color:var(--red);font-weight:bold;">Domain Not Authorized!</span><br><br>
          Go to Firebase Console → Authentication → Settings → Authorized domains.<br><br>
          Add: <strong style="color:var(--bg);user-select:all;background:var(--primary);padding:4px 8px;border-radius:4px;display:inline-block;margin-top:8px">${domain}</strong>`;
        }
      }
      showToast(`Domain not authorized`, 'error');
    } else if (e.code === 'auth/popup-blocked') {
      showToast('⚠️ Popup was blocked. Please allow popups for this site.', 'error');
    } else {
      showToast('Sign-in error: ' + e.message, 'error');
    }
  }
}

async function signOutUser() {
  if (!auth) return;
  try {
    if (unsubscribeSnapshot) { unsubscribeSnapshot(); unsubscribeSnapshot = null; }
    await signOut(auth);
    currentUser = null;
    updateNavAuth(null);
    updateSyncStatus('offline');
    closeAuthModal();
    showToast('Signed out. Data still saved locally.', 'info');
  } catch(e) {
    showToast('Sign-out error: ' + e.message, 'error');
  }
}

function updateNavAuth(user) {
  const signinBtn = document.getElementById('nav-signin-btn');
  const chip = document.getElementById('nav-user-chip');
  const avatar = document.getElementById('nav-user-avatar');
  const nameEl = document.getElementById('nav-user-name');
  if (!signinBtn || !chip) return;
  if (user && !user.isAnonymous) {
    signinBtn.style.display = 'none';
    chip.style.display = 'flex';
    if (avatar) { avatar.src = user.photoURL || ''; avatar.style.display = user.photoURL ? '' : 'none'; }
    if (nameEl) nameEl.textContent = user.displayName ? user.displayName.split(' ')[0] : (user.email || 'User');
  } else {
    signinBtn.style.display = '';
    chip.style.display = 'none';
  }
}

// ─────────────────────────────────────────────
//  DAILY SCHEDULE
// ─────────────────────────────────────────────
const SCHEDULE_AI_PROMPT = `You are an advanced Medical AI integrated into MedTrack Pro — a structured study command center.

Your task is to generate a DAILY STUDY SCHEDULE following the SCHEDULE INTELLIGENCE SYSTEM below.

OUTPUT ONLY valid JSON — no markdown, no explanation, no backticks. Strictly follow this schema:

{
  "date": "YYYY-MM-DD",
  "hijriDate": "e.g. 15 Dhul Hijjah 1446",
  "dayTitle": "Meaningful intellectual title e.g. Renal Week — Day 2: Filtration & Function",
  "schedule": [
    {
      "id": "unique-slug-id",
      "time": "08:00 - 09:30",
      "subject": "Physiology",
      "topic": "GFR and Filtration Barrier",
      "type": "learn",
      "action": "Learn + Mechanism Mapping",
      "task": "Understand filtration barrier layers and draw the GFR flow diagram",
      "emoji": "🧬",
      "tags": ["physiology", "renal", "core"],
      "difficulty": "deep",
      "weightage": 4,
      "notes": "Focus on podocyte role and Starling forces"
    }
  ],
  "reflection": {
    "understood": "What was deeply understood today",
    "weak": "What remains weak or needs review",
    "fikriInsight": "One key conceptual connection discovered today"
  },
  "progress": {
    "completionEstimate": 80,
    "effortRating": 4,
    "consistencyNote": "Strong focus session — maintained depth throughout"
  }
}

INTELLIGENCE RULES (MANDATORY):
- "type" must be one of: learn, revise, active-recall, mcq, clinical, break
- "difficulty" must be: light, moderate, or deep
- "weightage" is 1–5 (importance/yield)
- "tags" are short lowercase strings (3–5 tags per session)
- "id" must be unique slug format (e.g. "physiology-gfr-learn")
- "hijriDate" MUST be included — calculate from the date field
- "dayTitle" must be a meaningful intellectual theme (not generic)
- Balance: include New Learning + Revision + Active Recall sessions
- Include break slots (10–15 min) between study sessions
- Keep total study time realistic (max 6–7 hours/day)
- Prioritize high-yield and clinically tested subtopics
- The "reflection" and "progress" fields are MANDATORY

My timetable/topics for today:
[PASTE YOUR TIMETABLE / TOPICS HERE]`

let pendingScheduleData = null;

function openScheduleImport() {
  pendingScheduleData = null;
  const btn = document.getElementById('sched-import-btn');
  if (btn) btn.disabled = true;
  const log = document.getElementById('sched-import-log');
  if (log) { log.style.display = 'none'; log.innerHTML = ''; }
  const preview = document.getElementById('sched-preview-area');
  if (preview) preview.innerHTML = '';
  const schema = document.getElementById('sched-schema-display');
  if (schema) schema.style.display = 'none';
  const fileInput = document.getElementById('sched-file-input');
  if (fileInput) fileInput.value = '';
  document.getElementById('schedule-import-overlay').classList.add('show');
}

function closeScheduleImport() {
  document.getElementById('schedule-import-overlay').classList.remove('show');
}

function copySchedulePrompt() {
  navigator.clipboard.writeText(SCHEDULE_AI_PROMPT).then(() => {
    const btn = document.getElementById('sched-prompt-copy-btn');
    if (btn) { btn.textContent = '✅ Copied!'; setTimeout(() => { btn.textContent = '📋 Copy AI Prompt'; }, 2000); }
    showToast('📋 AI prompt copied! Paste into Claude or ChatGPT.', 'success');
  }).catch(() => {
    showToast('Copy failed — please copy manually', 'warn');
  });
}

function handleSchedDragOver(e) {
  e.preventDefault();
  document.getElementById('sched-upload-zone').classList.add('drag');
}

function handleSchedDrop(e) {
  e.preventDefault();
  document.getElementById('sched-upload-zone').classList.remove('drag');
  const f = e.dataTransfer.files[0];
  if (f) processScheduleFile(f);
}

function handleSchedFileSelect(e) {
  const f = e.target.files[0];
  if (f) processScheduleFile(f);
}

function processScheduleFile(file) {
  const log = document.getElementById('sched-import-log');
  const preview = document.getElementById('sched-preview-area');
  const schema = document.getElementById('sched-schema-display');

  function showLog(entries) {
    if (!log) return;
    log.style.display = 'block';
    log.innerHTML = entries.map(e => `<div class="log-${e.type}">${e.msg}</div>`).join('');
  }

  if (!file.name.endsWith('.json')) {
    showLog([{ type:'error', msg:'❌ Invalid file type. Only .json accepted.' }]);
    return;
  }

  const reader = new FileReader();
  reader.onload = (ev) => {
    try {
      const data = JSON.parse(ev.target.result);
      if (!data.schedule || !Array.isArray(data.schedule)) throw new Error("Missing 'schedule' array.");
      if (data.schedule.length === 0) throw new Error("Schedule is empty.");

      pendingScheduleData = data;

      if (schema) {
        schema.style.display = 'block';
        schema.textContent = `Date: ${data.date || 'today'}  |  Title: ${data.dayTitle || '—'}  |  Slots: ${data.schedule.length}`;
      }

      if (preview) {
        preview.innerHTML = `
          <div style="font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:1px;color:var(--text-muted);margin-bottom:8px">Preview (${data.schedule.length} slots)</div>
          <div style="display:flex;flex-direction:column;gap:4px;max-height:200px;overflow-y:auto;">
            ${data.schedule.map(s => `
              <div style="display:flex;gap:10px;align-items:center;background:var(--surface2);border-radius:6px;padding:7px 10px;font-size:12px;">
                <span>${s.emoji || '📚'}</span>
                <span style="font-family:var(--font-mono);color:var(--primary);min-width:100px;flex-shrink:0">${s.time || '—'}</span>
                <span style="font-weight:600;flex:1">${s.topic}</span>
                <span style="color:var(--text-muted)">${s.subject}</span>
                <span class="slot-diff ${s.difficulty||'medium'}">${s.difficulty||'medium'}</span>
              </div>`).join('')}
          </div>`;
      }

      const btn = document.getElementById('sched-import-btn');
      if (btn) btn.disabled = false;
      showLog([{ type:'success', msg:`✅ Valid schedule detected — ${data.schedule.length} sessions. Click "Load Schedule" to apply.` }]);
    } catch(err) {
      showLog([{ type:'error', msg:`❌ Invalid JSON: ${err.message}` }]);
      pendingScheduleData = null;
    }
  };
  reader.readAsText(file);
}

function importScheduleToTrack() {
  if (!pendingScheduleData) return;

  const today = todayStr();
  const schedDate = pendingScheduleData.date || today;

  let topicsAdded = 0;
  
  const items = pendingScheduleData.schedule.map(slotData => {
    let folder = state.folders.find(f => f.name.toLowerCase() === (slotData.subject||'').toLowerCase());
    if (!folder) {
      folder = { id: genId(), parentId: null, name: slotData.subject||'Subject', icon: slotData.emoji || '📁', color: '#3A86FF', label: '', subfolders: [], topics: [] };
      state.folders.push(folder);
    }
    
    // Find topic globally or create inside that folder
    let matchingTopic = null;
    const findMatchingTopic = (f) => {
        let t = f.topics.find(x => x.name.toLowerCase() === (slotData.topic||'').toLowerCase());
        if(t) return t;
        for(let sf of f.subfolders) {
            t = findMatchingTopic(sf);
            if(t) return t;
        }
        return null;
    };
    matchingTopic = findMatchingTopic(folder);

    if (!matchingTopic) {
      matchingTopic = createTopicData(slotData.topic||'Untitled', folder.id, { difficulty: slotData.difficulty, weightage: slotData.weightage, tags: slotData.tags });
      matchingTopic.notes = slotData.notes || '';
      folder.topics.push(matchingTopic);
      topicsAdded++;
    }

    // Add to planner tasks
    const taskText = `${slotData.emoji||'📚'} [${slotData.time||'--'}] ${slotData.topic} (${slotData.subject})`;
    const alreadyExists = (state.plannerTasks || []).some(t => t.text === taskText && t.date === today);
    if (!alreadyExists) {
      state.plannerTasks = state.plannerTasks || [];
      state.plannerTasks.push({ id: genId(), text: taskText, done: false, date: today });
    }

    return {
        id: slotData.id || genId(),
        time: slotData.time || '',
        subject: slotData.subject || '',
        topic: slotData.topic || 'Untitled',
        topicId: matchingTopic.id, // Store Reference!
        type: slotData.type || 'study',
        emoji: slotData.emoji || '📚',
        tags: Array.isArray(slotData.tags) ? slotData.tags : [],
        difficulty: slotData.difficulty || 'medium',
        weightage: slotData.weightage || 2,
        notes: slotData.notes || ''
    };
  });

  state.dailySchedule = { date: schedDate, dayTitle: pendingScheduleData.dayTitle || 'Today\'s Schedule', items, completedIds: [] };

  saveState();
  closeScheduleImport();
  showToast(`📅 Schedule loaded! ${items.length} slots · ${topicsAdded} topics added to folders`, 'success');
  renderHome();
  if (currentView === 'folders') { currentFolderId = null; renderFolders(); }
}

function toggleScheduleSlot(id) {
  if (!state.dailySchedule) return;
  state.dailySchedule.completedIds = state.dailySchedule.completedIds || [];
  const idx = state.dailySchedule.completedIds.indexOf(id);
  
  if (idx >= 0) {
    state.dailySchedule.completedIds.splice(idx, 1);
  } else {
    state.dailySchedule.completedIds.push(id);
    
    // Feature: Auto-Mastery when checking off schedule
    const slot = state.dailySchedule.items.find(s => s.id === id);
    if (slot && slot.topicId) {
      const r = findTopic(slot.topicId);
      if (r && !r.topic.stages.mastered) {
         r.topic.stages.mastered = true;
         if (!r.topic.nextReviewDate) {
            r.topic.nextReviewDate = addDays(todayStr(), 1);
            r.topic.lastReviewDate = todayStr();
            state.heatmap[todayStr()] = (state.heatmap[todayStr()]||0) + 1;
            updateStreak();
         }
         showToast(`🏆 "${r.topic.name}" marked as Mastered!`, 'success');
      }
    }
  }
  
  saveState();
  renderDailySchedule();
}

function clearSchedule() {
  if (!confirm('Clear today\'s schedule? Topics added to folders will remain.')) return;
  state.dailySchedule = { date: null, items: [], completedIds: [] };
  saveState();
  renderDailySchedule();
  showToast('Schedule cleared', 'info');
}

function renderDailySchedule() {
  const section = document.getElementById('daily-schedule-section');
  const timeline = document.getElementById('schedule-timeline');
  const progressWrap = document.getElementById('schedule-progress-wrap');
  const progressFill = document.getElementById('schedule-progress-fill');
  const progressText = document.getElementById('schedule-progress-text');
  const dayTitle = document.getElementById('schedule-day-title');
  const dateBadge = document.getElementById('schedule-date-badge');
  const subtitle = document.getElementById('schedule-subtitle-text');

  if (!section || !timeline) return;

  const sched = state.dailySchedule;
  if (!sched || !sched.items || sched.items.length === 0) {
    section.style.display = 'none';
    return;
  }

  section.style.display = '';
  const completed = (sched.completedIds || []);
  const total = sched.items.length;
  const done = sched.items.filter(s => completed.includes(s.id)).length;
  const pct = total ? Math.round((done / total) * 100) : 0;

  if (dayTitle) dayTitle.textContent = sched.dayTitle || "Today's Study Schedule";
  if (dateBadge) dateBadge.textContent = sched.date || todayStr();
  if (subtitle) subtitle.textContent = `${total} study sessions · ${done} completed`;
  if (progressWrap) progressWrap.style.display = total > 0 ? '' : 'none';
  if (progressFill) progressFill.style.width = pct + '%';
  if (progressText) progressText.textContent = `${done} / ${total} done`;

  timeline.innerHTML = sched.items.map(slot => {
    const isDone = completed.includes(slot.id);
    const tagsHtml = slot.tags && slot.tags.length
      ? `<div class="slot-tags">${slot.tags.map(tg => `<span class="slot-tag">#${tg}</span>`).join('')}</div>` : '';
    return `
      <div class="schedule-slot ${isDone ? 'completed' : ''}" id="sslot-${slot.id}">
        <input type="checkbox" class="slot-done-cb" ${isDone ? 'checked' : ''} onchange="toggleScheduleSlot('${slot.id}')">
        <span class="slot-emoji">${slot.emoji || '📚'}</span>
        <span class="slot-time">${slot.time}</span>
        <div class="slot-body">
          <div class="slot-topic">${slot.topic}</div>
          <div class="slot-subject">${slot.subject} · ${slot.type}</div>
          ${tagsHtml}
        </div>
        <span class="slot-diff ${slot.difficulty || 'medium'}">${slot.difficulty || 'med'}</span>
      </div>`;
  }).join('');
}

// ─────────────────────────────────────────────
//  EXPOSE GLOBALS — must be after all function definitions
// ─────────────────────────────────────────────
window.saveState = saveState;
window.showView = showView;
window.navigateToFolder = navigateToFolder;
window.goBackFromSubfolder = goBackFromSubfolder;
window.goToTopic = goToTopic;
window.openPomodoro = openPomodoro;
window.closePomodoro = closePomodoro;
window.togglePomodoro = togglePomodoro;
window.resetPomodoro = resetPomodoro;
window.skipPomodoroPhase = skipPomodoroPhase;
window.toggleTheme = toggleTheme;
window.toggleTimer = toggleTimer;
window.resetDayTimer = resetDayTimer;
window.startRevisionSession = startRevisionSession;
window.showWeakAreas = showWeakAreas;
window.saveWhyStarted = saveWhyStarted;
window.toggleIslamic = toggleIslamic;
window.addReminder = addReminder;
window.deleteReminder = deleteReminder;
window.startWeekendSession = startWeekendSession;
window.closeWeekendSession = closeWeekendSession;
window.wkndRate = wkndRate;
window.wkndSkip = wkndSkip;
window.sessionReview = sessionReview;
window.doSearch = doSearch;
window.addPlannerTask = addPlannerTask;
window.toggleTask = toggleTask;
window.deleteTask = deleteTask;
window.openAddFolder = openAddFolder;
window.closeAddFolder = closeAddFolder;
window.saveAddFolder = saveAddFolder;
window.openEditFolder = openEditFolder;
window.deleteFolder = deleteFolder;
window.openAddSubfolder = openAddSubfolder;
window.closeAddSubfolder = closeAddSubfolder;
window.saveAddSubfolder = saveAddSubfolder;
window.openAddTopic = openAddTopic;
window.closeAddTopic = closeAddTopic;
window.saveAddTopic = saveAddTopic;
window.openMoveTopic = openMoveTopic;
window.closeMoveTopic = closeMoveTopic;
window.confirmMoveTopic = confirmMoveTopic;
window.openSettingsPanel = openSettingsPanel;
window.closeSettingsPanel = closeSettingsPanel;
window.exportBackup = exportBackup;
window.handleRestoreFile = handleRestoreFile;
window.closeRestore = closeRestore;
window.dangerClearData = dangerClearData;
window.openJsonImport = openJsonImport;
window.closeJsonImport = closeJsonImport;
window.selectMergeOpt = selectMergeOpt;
window.handleJsonDragOver = handleJsonDragOver;
window.handleJsonDrop = handleJsonDrop;
window.handleJsonFileSelect = handleJsonFileSelect;
window.importJson = importJson;
window.filterTopics = filterTopics;
window.setFilter = setFilter;
window.toggleTopic = toggleTopic;
window.toggleStage = toggleStage;
window.reviewTopic = reviewTopic;
window.toggleWeak = toggleWeak;
window.toggleStar = toggleStar;
window.saveTopic = saveTopic;
window.addLink = addLink;
window.removeLink = removeLink;
window.deleteTopic = deleteTopic;
window.copyPrompt = copyPrompt;

// Auth
window.openAuthModal = openAuthModal;
window.closeAuthModal = closeAuthModal;
window.signInWithGoogle = signInWithGoogle;
window.signOutUser = signOutUser;

// Schedule
window.openScheduleImport = openScheduleImport;
window.closeScheduleImport = closeScheduleImport;
window.handleSchedDragOver = handleSchedDragOver;
window.handleSchedDrop = handleSchedDrop;
window.handleSchedFileSelect = handleSchedFileSelect;
window.importScheduleToTrack = importScheduleToTrack;
window.toggleScheduleSlot = toggleScheduleSlot;
window.clearSchedule = clearSchedule;
window.copySchedulePrompt = copySchedulePrompt;

// ─────────────────────────────────────────────
//  INIT
// ─────────────────────────────────────────────
window.addEventListener('DOMContentLoaded', async () => {
  loadTheme();
  loadState();
  initTimer();
  updatePomodoroDisplay();
  renderHome();
  
  setInterval(() => {
    if (timerRunning) {
      state.studyTimer.todayElapsed = timerElapsed;
      saveState();
    }
  }, 30000);

  // Initialize Firebase using Canvas configuration
  await initFirebase();
});
</script>
</body>
</html>
