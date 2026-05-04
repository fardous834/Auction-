<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>LLFC Auction Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Rajdhani:wght@400;500;600;700&family=Oswald:wght@300;400;500;600;700&display=swap" rel="stylesheet"/>
<style>
  :root {
    --red: #D0021B;
    --red-deep: #8B0000;
    --red-bright: #FF1930;
    --white: #FFFFFF;
    --off-white: #F5F0F0;
    --grey: #E8E0E0;
    --dark: #1A0505;
    --card-shadow: 0 4px 20px rgba(208,2,27,0.15);
    --font-display: 'Bebas Neue', sans-serif;
    --font-body: 'Rajdhani', sans-serif;
    --font-head: 'Oswald', sans-serif;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body { background:#f0e8e8; font-family:var(--font-body); color:var(--dark); min-height:100vh; }

  /* ── NAV ── */
  #top-nav {
    background: var(--red-deep);
    background: linear-gradient(135deg, #8B0000 0%, #D0021B 60%, #8B0000 100%);
    padding: 0;
    display: flex;
    align-items: stretch;
    box-shadow: 0 4px 20px rgba(0,0,0,0.4);
    position: sticky; top:0; z-index:1000;
  }
  .nav-brand {
    display:flex; align-items:center; gap:12px; padding:12px 20px;
    border-right:2px solid rgba(255,255,255,0.15);
    flex:1;
  }
  #nav-logo-img { width:48px; height:48px; border-radius:50%; object-fit:cover; border:2px solid rgba(255,255,255,0.4); display:none; }
  #nav-logo-placeholder { width:48px; height:48px; border-radius:50%; background:rgba(255,255,255,0.15); border:2px dashed rgba(255,255,255,0.4); display:flex; align-items:center; justify-content:center; font-size:18px; }
  #nav-title { font-family:var(--font-display); font-size:22px; color:#fff; letter-spacing:2px; line-height:1.1; }
  #nav-subtitle { font-family:var(--font-body); font-size:11px; color:rgba(255,255,255,0.7); letter-spacing:3px; text-transform:uppercase; }
  .nav-tabs { display:flex; }
  .nav-tab {
    padding: 0 28px;
    background: transparent;
    border: none;
    color: rgba(255,255,255,0.65);
    font-family: var(--font-head);
    font-size: 14px; font-weight:600;
    letter-spacing: 2px; text-transform:uppercase;
    cursor: pointer; transition: all 0.2s;
    border-bottom: 3px solid transparent;
    position:relative;
  }
  .nav-tab:hover { color:#fff; background:rgba(255,255,255,0.08); }
  .nav-tab.active { color:#fff; border-bottom-color:#fff; background:rgba(0,0,0,0.15); }
  .nav-live-badge {
    background:#fff; color:var(--red); font-size:9px; font-weight:700;
    padding:2px 5px; border-radius:3px; letter-spacing:1px; margin-left:6px; vertical-align:middle;
    animation: pulse-badge 1.5s infinite;
  }
  @keyframes pulse-badge { 0%,100%{opacity:1} 50%{opacity:0.5} }

  /* ── MAIN SECTIONS ── */
  .section { display:none; padding:28px 24px; max-width:1400px; margin:0 auto; }
  .section.active { display:block; }

  /* ── CARDS ── */
  .card {
    background:#fff;
    border-radius:12px;
    box-shadow: var(--card-shadow);
    margin-bottom:24px;
    overflow:hidden;
  }
  .card-header {
    background: linear-gradient(135deg, var(--red) 0%, var(--red-deep) 100%);
    padding:14px 20px;
    display:flex; align-items:center; gap:10px;
  }
  .card-header h2 {
    font-family:var(--font-display); font-size:20px; color:#fff; letter-spacing:2px;
  }
  .card-header .icon { font-size:20px; }
  .card-body { padding:20px; }

  /* ── GRID ── */
  .grid-2 { display:grid; grid-template-columns:1fr 1fr; gap:20px; }
  .grid-3 { display:grid; grid-template-columns:repeat(3,1fr); gap:20px; }
  @media(max-width:900px){.grid-2,.grid-3{grid-template-columns:1fr;}}

  /* ── FORM ELEMENTS ── */
  .form-row { display:flex; gap:12px; flex-wrap:wrap; margin-bottom:14px; }
  .form-group { display:flex; flex-direction:column; gap:5px; flex:1; min-width:160px; }
  label { font-size:12px; font-weight:600; letter-spacing:1px; text-transform:uppercase; color:#888; }
  input[type=text], input[type=number], input[type=password], select, textarea {
    border:2px solid #eee; border-radius:8px; padding:10px 14px;
    font-family:var(--font-body); font-size:15px; color:var(--dark);
    transition:border 0.2s; background:#fff; outline:none;
  }
  input:focus, select:focus, textarea:focus { border-color:var(--red); }
  input[type=file] { padding:8px; font-size:13px; }

  /* ── BUTTONS ── */
  .btn {
    padding:10px 22px; border:none; border-radius:8px; cursor:pointer;
    font-family:var(--font-head); font-size:14px; font-weight:600;
    letter-spacing:1.5px; text-transform:uppercase; transition:all 0.2s;
  }
  .btn-red { background:linear-gradient(135deg,var(--red),var(--red-deep)); color:#fff; }
  .btn-red:hover { background:linear-gradient(135deg,var(--red-bright),var(--red)); transform:translateY(-1px); box-shadow:0 4px 12px rgba(208,2,27,0.4); }
  .btn-white { background:#fff; color:var(--red); border:2px solid var(--red); }
  .btn-white:hover { background:var(--red); color:#fff; }
  .btn-green { background:linear-gradient(135deg,#00a651,#007a3d); color:#fff; }
  .btn-green:hover { transform:translateY(-1px); box-shadow:0 4px 12px rgba(0,166,81,0.4); }
  .btn-grey { background:#eee; color:#555; }
  .btn-grey:hover { background:#ddd; }
  .btn-sm { padding:6px 14px; font-size:12px; }
  .btn-danger { background:linear-gradient(135deg,#ff4444,#cc0000); color:#fff; }

  /* ── TABLE ── */
  .table-wrap { overflow-x:auto; border-radius:8px; border:1px solid #eee; }
  table { width:100%; border-collapse:collapse; font-size:14px; }
  thead { background:linear-gradient(135deg,var(--red),var(--red-deep)); }
  thead th { color:#fff; padding:12px 14px; font-family:var(--font-head); font-weight:600; letter-spacing:1px; text-align:left; white-space:nowrap; }
  tbody tr { border-bottom:1px solid #f0e8e8; transition:background 0.15s; }
  tbody tr:hover { background:#fff5f5; }
  tbody td { padding:11px 14px; vertical-align:middle; }
  .badge {
    display:inline-block; padding:3px 10px; border-radius:20px; font-size:11px;
    font-weight:700; letter-spacing:1px; text-transform:uppercase;
  }
  .badge-available { background:#e8f5e9; color:#2e7d32; }
  .badge-sold { background:#ffebee; color:#c62828; }
  .badge-youth { background:#fff3e0; color:#e65100; }
  .badge-local { background:#e3f2fd; color:#1565c0; }
  .badge-invited { background:#f3e5f5; color:#6a1b9a; }

  /* ── ADMIN LOGIN ── */
  #admin-login {
    max-width:380px; margin:80px auto;
    background:#fff; border-radius:16px; padding:40px;
    box-shadow: 0 8px 40px rgba(208,2,27,0.2);
    text-align:center;
  }
  #admin-login h2 { font-family:var(--font-display); font-size:32px; color:var(--red); letter-spacing:3px; margin-bottom:8px; }
  #admin-login p { color:#999; font-size:13px; margin-bottom:28px; }
  #admin-login input { width:100%; margin-bottom:14px; }

  /* ── VIEWER ── */
  .viewer-header {
    background: linear-gradient(160deg, var(--red-deep) 0%, var(--red) 50%, #ff4060 100%);
    border-radius:16px; padding:32px; text-align:center;
    margin-bottom:28px; position:relative; overflow:hidden;
    box-shadow: 0 8px 40px rgba(208,2,27,0.35);
  }
  .viewer-header::before {
    content:''; position:absolute; top:-30%; left:-10%; width:120%; height:200%;
    background:url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.04'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
  }
  #viewer-logo-img { width:90px; height:90px; border-radius:50%; object-fit:cover; border:4px solid rgba(255,255,255,0.4); margin-bottom:12px; display:none; }
  #viewer-logo-placeholder { width:90px; height:90px; border-radius:50%; background:rgba(255,255,255,0.15); border:3px dashed rgba(255,255,255,0.4); display:inline-flex; align-items:center; justify-content:center; font-size:32px; margin-bottom:12px; }
  #viewer-tournament-name { font-family:var(--font-display); font-size:42px; color:#fff; letter-spacing:4px; text-shadow:0 2px 20px rgba(0,0,0,0.3); }
  #viewer-tournament-sub { color:rgba(255,255,255,0.7); letter-spacing:4px; text-transform:uppercase; font-size:13px; margin-top:4px; }

  /* ── CATEGORY TABS ── */
  .cat-tabs { display:flex; gap:0; margin-bottom:20px; border-radius:10px; overflow:hidden; box-shadow:0 2px 10px rgba(0,0,0,0.1); }
  .cat-tab { flex:1; padding:12px; border:none; cursor:pointer; font-family:var(--font-head); font-size:13px; font-weight:600; letter-spacing:1.5px; text-transform:uppercase; background:#fff; color:#aaa; transition:all 0.2s; }
  .cat-tab.active.youth { background:linear-gradient(135deg,#ff9800,#e65100); color:#fff; }
  .cat-tab.active.local { background:linear-gradient(135deg,#2196f3,#1565c0); color:#fff; }
  .cat-tab.active.invited { background:linear-gradient(135deg,#9c27b0,#6a1b9a); color:#fff; }
  .cat-tab:hover:not(.active) { background:#f5f5f5; color:#555; }
  .cat-panel { display:none; }
  .cat-panel.active { display:block; }

  /* ── PLAYER CARD (VIEWER) ── */
  .player-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(200px,1fr)); gap:14px; }
  .player-card {
    background:#fff; border-radius:10px; padding:16px; text-align:center;
    box-shadow:0 2px 12px rgba(0,0,0,0.08); position:relative;
    border-top:4px solid var(--red); transition:transform 0.2s,box-shadow 0.2s;
  }
  .player-card:hover { transform:translateY(-3px); box-shadow:0 6px 20px rgba(208,2,27,0.15); }
  .player-card.sold { border-top-color:#c62828; opacity:0.75; }
  .player-name { font-family:var(--font-head); font-size:16px; font-weight:600; margin-bottom:4px; }
  .player-club { font-size:12px; color:#888; margin-bottom:6px; }
  .player-price { font-family:var(--font-display); font-size:22px; color:var(--red); }
  .player-sold-info { font-size:11px; color:#c62828; font-weight:600; margin-top:4px; letter-spacing:0.5px; }
  .sold-ribbon {
    position:absolute; top:8px; right:-4px;
    background:var(--red); color:#fff; font-size:9px; font-weight:700;
    padding:2px 10px; letter-spacing:1px; border-radius:3px 0 0 3px;
  }
  .sold-ribbon::after { content:''; position:absolute; right:0; bottom:-4px; border-left:4px solid var(--red-deep); border-bottom:4px solid transparent; }

  /* ── TEAM CARDS ── */
  .team-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(280px,1fr)); gap:20px; }
  .team-card { background:#fff; border-radius:14px; overflow:hidden; box-shadow:var(--card-shadow); }
  .team-card-header { background:linear-gradient(135deg,var(--red-deep),var(--red)); padding:18px; display:flex; align-items:center; gap:14px; }
  .team-logo-wrap { width:56px; height:56px; border-radius:50%; border:3px solid rgba(255,255,255,0.4); overflow:hidden; background:rgba(255,255,255,0.1); flex-shrink:0; display:flex; align-items:center; justify-content:center; font-size:24px; }
  .team-logo-wrap img { width:100%; height:100%; object-fit:cover; }
  .team-info h3 { font-family:var(--font-display); font-size:20px; color:#fff; letter-spacing:1px; }
  .team-cap { font-size:12px; color:rgba(255,255,255,0.75); }
  .team-budget-bar { padding:14px 16px; background:#fff5f5; border-bottom:1px solid #f0e8e8; }
  .budget-label { font-size:11px; color:#888; font-weight:600; letter-spacing:1px; text-transform:uppercase; margin-bottom:6px; display:flex; justify-content:space-between; }
  .budget-track { background:#eee; border-radius:20px; height:8px; overflow:hidden; }
  .budget-fill { background:linear-gradient(90deg,var(--red),var(--red-bright)); height:100%; border-radius:20px; transition:width 0.5s; }
  .squad-tabs { display:flex; border-bottom:1px solid #eee; }
  .squad-tab { flex:1; padding:9px 4px; border:none; background:none; cursor:pointer; font-size:11px; font-family:var(--font-head); font-weight:600; letter-spacing:1px; text-transform:uppercase; color:#aaa; border-bottom:2px solid transparent; transition:all 0.15s; }
  .squad-tab.active { color:var(--red); border-bottom-color:var(--red); }
  .squad-list { padding:12px 16px; min-height:80px; }
  .squad-player { display:flex; justify-content:space-between; align-items:center; padding:7px 0; border-bottom:1px solid #f5f0f0; font-size:13px; }
  .squad-player:last-child { border-bottom:none; }
  .squad-player-name { font-weight:600; }
  .squad-player-price { color:var(--red); font-weight:700; font-family:var(--font-head); }
  .squad-empty { color:#ccc; font-size:12px; text-align:center; padding:14px 0; letter-spacing:1px; }

  /* ── SELL MODAL ── */
  .modal-bg { display:none; position:fixed; inset:0; background:rgba(26,5,5,0.75); z-index:2000; align-items:center; justify-content:center; }
  .modal-bg.open { display:flex; }
  .modal { background:#fff; border-radius:16px; padding:32px; max-width:480px; width:90%; position:relative; }
  .modal h3 { font-family:var(--font-display); font-size:26px; color:var(--red); letter-spacing:2px; margin-bottom:6px; }
  .modal p { color:#888; font-size:13px; margin-bottom:20px; }
  .modal-close { position:absolute; top:14px; right:18px; font-size:22px; cursor:pointer; color:#ccc; }
  .modal-close:hover { color:var(--red); }

  /* ── AUCTION LIVE TICKER ── */
  .ticker-bar {
    background:var(--dark); color:#fff; padding:8px 0; overflow:hidden;
    position:relative; margin-bottom:20px; border-radius:8px;
  }
  .ticker-inner { display:flex; align-items:center; white-space:nowrap; animation:ticker 30s linear infinite; }
  @keyframes ticker { from{transform:translateX(100%)} to{transform:translateX(-100%)} }
  .ticker-item { margin-right:40px; font-size:13px; letter-spacing:1px; }
  .ticker-item span { color:var(--red-bright); font-weight:700; }

  /* ── STATS ROW ── */
  .stats-row { display:grid; grid-template-columns:repeat(4,1fr); gap:16px; margin-bottom:24px; }
  @media(max-width:700px){ .stats-row{grid-template-columns:repeat(2,1fr);} }
  .stat-card { background:#fff; border-radius:12px; padding:18px; text-align:center; box-shadow:0 2px 12px rgba(208,2,27,0.1); }
  .stat-val { font-family:var(--font-display); font-size:36px; color:var(--red); letter-spacing:1px; }
  .stat-label { font-size:11px; text-transform:uppercase; letter-spacing:2px; color:#999; margin-top:2px; }

  /* ── SELL PANEL ── */
  .sell-player-row { display:flex; align-items:center; justify-content:space-between; padding:10px 14px; border-radius:8px; margin-bottom:8px; background:#fff; border:1px solid #eee; transition:all 0.15s; }
  .sell-player-row:hover { border-color:var(--red); }
  .sell-player-info { display:flex; flex-direction:column; gap:2px; }
  .sell-player-info strong { font-size:14px; }
  .sell-player-info small { color:#999; font-size:12px; }

  /* ── UPLOAD PREVIEW ── */
  .img-preview { width:70px; height:70px; border-radius:50%; object-fit:cover; border:3px solid var(--red); display:none; }
  .img-placeholder { width:70px; height:70px; border-radius:50%; background:#f5f0f0; border:2px dashed #ddd; display:flex; align-items:center; justify-content:center; font-size:28px; }

  /* ── MSG ── */
  .msg { padding:10px 16px; border-radius:8px; font-size:13px; font-weight:600; margin-top:10px; display:none; }
  .msg.success { background:#e8f5e9; color:#2e7d32; display:block; }
  .msg.error { background:#ffebee; color:#c62828; display:block; }

  /* ── SCROLLBAR ── */
  ::-webkit-scrollbar{width:6px;height:6px}
  ::-webkit-scrollbar-track{background:#f0e8e8}
  ::-webkit-scrollbar-thumb{background:var(--red);border-radius:3px}

  /* ── SECTION TITLE ── */
  .section-title { font-family:var(--font-display); font-size:28px; color:var(--red); letter-spacing:3px; margin-bottom:6px; }
  .section-divider { height:3px; background:linear-gradient(90deg,var(--red),transparent); margin-bottom:24px; border-radius:2px; }

  .empty-state { text-align:center; padding:40px; color:#ccc; font-size:14px; letter-spacing:1px; }
  .empty-state .icon { font-size:40px; display:block; margin-bottom:10px; }

  .delete-btn { background:none; border:none; cursor:pointer; color:#ddd; font-size:16px; transition:color 0.15s; }
  .delete-btn:hover { color:var(--red); }

  /* ── LOADING ── */
  .loading { text-align:center; padding:30px; color:#ccc; }
  .spinner { width:30px; height:30px; border:3px solid #eee; border-top-color:var(--red); border-radius:50%; animation:spin 0.7s linear infinite; margin:0 auto 10px; }
  @keyframes spin{to{transform:rotate(360deg)}}
</style>
</head>
<body>

<!-- ══════════════ TOP NAV ══════════════ -->
<nav id="top-nav">
  <div class="nav-brand">
    <img id="nav-logo-img" src="" alt="Logo"/>
    <div id="nav-logo-placeholder">🏆</div>
    <div>
      <div id="nav-title">LLFC AUCTION</div>
      <div id="nav-subtitle">Live Player Auction</div>
    </div>
  </div>
  <div class="nav-tabs">
    <button class="nav-tab active" onclick="showSection('viewer')">
      🔴 LIVE <span class="nav-live-badge">LIVE</span>
    </button>
    <button class="nav-tab" onclick="showSection('admin-gate')">⚙️ ADMIN</button>
  </div>
</nav>

<!-- ══════════════ ADMIN LOGIN ══════════════ -->
<div id="admin-gate" class="section">
  <div id="admin-login">
    <div style="font-size:52px;margin-bottom:10px;">🔐</div>
    <h2>ADMIN</h2>
    <p>Enter admin password to continue</p>
    <input type="password" id="admin-pass" placeholder="Password" style="width:100%;margin-bottom:14px;"/>
    <button class="btn btn-red" style="width:100%" onclick="checkAdminPass()">UNLOCK PANEL</button>
    <div id="login-msg" class="msg"></div>
  </div>
</div>

<!-- ══════════════ ADMIN PANEL ══════════════ -->
<div id="admin" class="section">
  <div class="section-title">⚙️ ADMIN CONTROL</div>
  <div class="section-divider"></div>

  <div class="grid-2">
    <!-- TOURNAMENT SETTINGS -->
    <div class="card">
      <div class="card-header"><span class="icon">🏆</span><h2>TOURNAMENT SETTINGS</h2></div>
      <div class="card-body">
        <div class="form-row" style="align-items:flex-start">
          <div style="display:flex;flex-direction:column;align-items:center;gap:8px">
            <img id="logo-preview-img" class="img-preview" src=""/>
            <div id="logo-preview-placeholder" class="img-placeholder">🏆</div>
            <input type="file" id="logo-file" accept="image/*" onchange="previewLogo(this)" style="font-size:11px;max-width:130px"/>
          </div>
          <div class="form-group" style="flex:1">
            <label>Tournament Name</label>
            <input type="text" id="tournament-name" placeholder="e.g. LLFC Premier Auction 2025"/>
            <label style="margin-top:10px">Sub Title</label>
            <input type="text" id="tournament-sub" placeholder="e.g. Season 1 · Sylhet"/>
          </div>
        </div>
        <button class="btn btn-red" onclick="saveTournament()">💾 SAVE SETTINGS</button>
        <div id="tournament-msg" class="msg"></div>
      </div>
    </div>

    <!-- SELL PLAYER -->
    <div class="card">
      <div class="card-header"><span class="icon">🔨</span><h2>SELL PLAYER</h2></div>
      <div class="card-body">
        <div class="form-row">
          <div class="form-group">
            <label>Select Player</label>
            <select id="sell-player-select"><option value="">-- Choose Player --</option></select>
          </div>
          <div class="form-group">
            <label>Sell To Team</label>
            <select id="sell-team-select"><option value="">-- Choose Team --</option></select>
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label>Final Sold Price (৳)</label>
            <input type="number" id="sell-price" placeholder="Enter sold price"/>
          </div>
        </div>
        <button class="btn btn-green" onclick="sellPlayer()">🔨 CONFIRM SALE</button>
        <button class="btn btn-grey" style="margin-left:8px" onclick="unsellPlayer()">↩ UNSELL</button>
        <div id="sell-msg" class="msg"></div>
      </div>
    </div>
  </div>

  <!-- TEAM REGISTRATION -->
  <div class="card">
    <div class="card-header"><span class="icon">👥</span><h2>TEAM REGISTRATION</h2></div>
    <div class="card-body">
      <div class="form-row">
        <div class="form-group">
          <label>Team Name</label>
          <input type="text" id="team-name" placeholder="Team name"/>
        </div>
        <div class="form-group">
          <label>Captain / Owner Name</label>
          <input type="text" id="team-cap" placeholder="Captain name"/>
        </div>
        <div class="form-group">
          <label>Total Budget (৳)</label>
          <input type="number" id="team-budget" placeholder="e.g. 50000"/>
        </div>
        <div class="form-group" style="max-width:130px">
          <label>Team Logo</label>
          <input type="file" id="team-logo" accept="image/*" onchange="previewTeamLogo(this)"/>
          <img id="team-logo-preview" class="img-preview" style="margin-top:6px;width:50px;height:50px" src=""/>
        </div>
      </div>
      <button class="btn btn-red" onclick="addTeam()">➕ REGISTER TEAM</button>
      <div id="team-msg" class="msg"></div>
      <div style="margin-top:18px">
        <div class="table-wrap">
          <table id="team-table">
            <thead><tr><th>Team</th><th>Captain</th><th>Budget</th><th>Remaining</th><th>Action</th></tr></thead>
            <tbody id="team-tbody"></tbody>
          </table>
        </div>
      </div>
    </div>
  </div>

  <!-- PLAYER REGISTRATION TABS -->
  <div class="card">
    <div class="card-header"><span class="icon">⚽</span><h2>PLAYER REGISTRATION</h2></div>
    <div class="card-body">
      <div class="cat-tabs">
        <button class="cat-tab youth active" onclick="switchRegTab('youth',this)">🟠 YOUTH</button>
        <button class="cat-tab local" onclick="switchRegTab('local',this)">🔵 LOCAL</button>
        <button class="cat-tab invited" onclick="switchRegTab('invited',this)">🟣 INVITED</button>
      </div>

      <!-- YOUTH REG -->
      <div id="reg-youth" class="cat-panel active">
        <div class="form-row">
          <div class="form-group"><label>Player Name</label><input type="text" id="youth-name" placeholder="Full name"/></div>
          <div class="form-group"><label>Base Price (৳)</label><input type="number" id="youth-price" placeholder="e.g. 2000"/></div>
          <div style="display:flex;align-items:flex-end">
            <button class="btn btn-red" onclick="addPlayer('youth')">➕ ADD YOUTH</button>
          </div>
        </div>
        <div id="youth-msg" class="msg"></div>
        <div class="table-wrap"><table><thead><tr><th>#</th><th>Name</th><th>Base Price</th><th>Status</th><th>Sold To</th><th>Sold Price</th><th>Del</th></tr></thead><tbody id="youth-tbody"></tbody></table></div>
      </div>

      <!-- LOCAL REG -->
      <div id="reg-local" class="cat-panel">
        <div class="form-row">
          <div class="form-group"><label>Player Name</label><input type="text" id="local-name" placeholder="Full name"/></div>
          <div class="form-group"><label>Base Price (৳)</label><input type="number" id="local-price" placeholder="e.g. 3000"/></div>
          <div style="display:flex;align-items:flex-end">
            <button class="btn btn-red" onclick="addPlayer('local')">➕ ADD LOCAL</button>
          </div>
        </div>
        <div id="local-msg" class="msg"></div>
        <div class="table-wrap"><table><thead><tr><th>#</th><th>Name</th><th>Base Price</th><th>Status</th><th>Sold To</th><th>Sold Price</th><th>Del</th></tr></thead><tbody id="local-tbody"></tbody></table></div>
      </div>

      <!-- INVITED REG -->
      <div id="reg-invited" class="cat-panel">
        <div class="form-row">
          <div class="form-group"><label>Player Name</label><input type="text" id="invited-name" placeholder="Full name"/></div>
          <div class="form-group"><label>Club Name</label><input type="text" id="invited-club" placeholder="Club/District"/></div>
          <div class="form-group"><label>Base Price (৳)</label><input type="number" id="invited-price" placeholder="e.g. 5000"/></div>
          <div style="display:flex;align-items:flex-end">
            <button class="btn btn-red" onclick="addPlayer('invited')">➕ ADD INVITED</button>
          </div>
        </div>
        <div id="invited-msg" class="msg"></div>
        <div class="table-wrap"><table><thead><tr><th>#</th><th>Name</th><th>Club</th><th>Base Price</th><th>Status</th><th>Sold To</th><th>Sold Price</th><th>Del</th></tr></thead><tbody id="invited-tbody"></tbody></table></div>
      </div>
    </div>
  </div>
</div>

<!-- ══════════════ VIEWER ══════════════ -->
<div id="viewer" class="section active">

  <!-- HEADER -->
  <div class="viewer-header">
    <img id="viewer-logo-img" src="" alt="Logo"/>
    <div id="viewer-logo-placeholder">🏆</div>
    <div id="viewer-tournament-name">LLFC AUCTION</div>
    <div id="viewer-tournament-sub">LIVE PLAYER AUCTION</div>
  </div>

  <!-- STATS -->
  <div class="stats-row" id="stats-row">
    <div class="stat-card"><div class="stat-val" id="stat-total">0</div><div class="stat-label">Total Players</div></div>
    <div class="stat-card"><div class="stat-val" id="stat-sold">0</div><div class="stat-label">Sold Players</div></div>
    <div class="stat-card"><div class="stat-val" id="stat-available">0</div><div class="stat-label">Available</div></div>
    <div class="stat-card"><div class="stat-val" id="stat-teams">0</div><div class="stat-label">Teams</div></div>
  </div>

  <!-- PLAYER LISTS -->
  <div class="section-title">📋 PLAYER POOL</div>
  <div class="section-divider"></div>
  <div class="cat-tabs">
    <button class="cat-tab youth active" onclick="switchViewTab('youth',this)">🟠 YOUTH PLAYERS</button>
    <button class="cat-tab local" onclick="switchViewTab('local',this)">🔵 LOCAL PLAYERS</button>
    <button class="cat-tab invited" onclick="switchViewTab('invited',this)">🟣 INVITED PLAYERS</button>
  </div>
  <div id="view-youth" class="cat-panel active"><div id="youth-player-grid" class="player-grid"></div></div>
  <div id="view-local" class="cat-panel"><div id="local-player-grid" class="player-grid"></div></div>
  <div id="view-invited" class="cat-panel"><div id="invited-player-grid" class="player-grid"></div></div>

  <!-- TEAMS -->
  <div class="section-title" style="margin-top:32px">🏟️ TEAMS & SQUADS</div>
  <div class="section-divider"></div>
  <div id="team-grid" class="team-grid"></div>
</div>

<!-- ══════════════ SELL MODAL ══════════════ -->
<div id="sell-modal" class="modal-bg">
  <div class="modal">
    <span class="modal-close" onclick="closeSellModal()">✕</span>
    <h3>🔨 SELL PLAYER</h3>
    <p id="modal-player-info">Player details here</p>
    <div class="form-group" style="margin-bottom:12px">
      <label>Sell To Team</label>
      <select id="modal-team-select"></select>
    </div>
    <div class="form-group" style="margin-bottom:18px">
      <label>Final Price (৳)</label>
      <input type="number" id="modal-price" placeholder="Enter bid amount"/>
    </div>
    <button class="btn btn-green" style="width:100%" onclick="confirmModalSell()">✅ CONFIRM SALE</button>
    <div id="modal-msg" class="msg"></div>
  </div>
</div>

<!-- ══════════════ FIREBASE ══════════════ -->
<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
  import { getDatabase, ref, set, get, push, remove, update, onValue }
    from "https://www.gstatic.com/firebasejs/11.0.1/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyAcTmairmtnca5JlULVFH8i7cfJsp2IXlo",
    authDomain: "sylhet-abf80.firebaseapp.com",
    databaseURL: "https://sylhet-abf80-default-rtdb.firebaseio.com",
    projectId: "sylhet-abf80",
    storageBucket: "sylhet-abf80.firebasestorage.app",
    messagingSenderId: "267629441375",
    appId: "1:267629441375:web:d3088b677b05931f5886ea",
    measurementId: "G-06996WM4E3"
  };

  const app = initializeApp(firebaseConfig);
  const db  = getDatabase(app);

  const ADMIN_PASSWORD = "llfc2025"; // Change this!

  /* ── expose to window ── */
  window._db  = db;
  window._ref = ref;
  window._set = set;
  window._get = get;
  window._push = push;
  window._remove = remove;
  window._update = update;
  window._onValue = onValue;

  /* ── REAL-TIME LISTENERS ── */
  onValue(ref(db,'tournament'), snap => {
    const d = snap.val() || {};
    document.getElementById('nav-title').textContent = d.name || 'LLFC AUCTION';
    document.getElementById('nav-subtitle').textContent = d.sub || 'Live Player Auction';
    document.getElementById('viewer-tournament-name').textContent = d.name || 'LLFC AUCTION';
    document.getElementById('viewer-tournament-sub').textContent = d.sub || 'LIVE PLAYER AUCTION';
    if(d.logo){
      document.getElementById('nav-logo-img').src = d.logo;
      document.getElementById('nav-logo-img').style.display='block';
      document.getElementById('nav-logo-placeholder').style.display='none';
      document.getElementById('viewer-logo-img').src = d.logo;
      document.getElementById('viewer-logo-img').style.display='block';
      document.getElementById('viewer-logo-placeholder').style.display='none';
      document.getElementById('logo-preview-img').src = d.logo;
      document.getElementById('logo-preview-img').style.display='block';
      document.getElementById('logo-preview-placeholder').style.display='none';
    }
    if(d.name) document.getElementById('tournament-name').value = d.name;
    if(d.sub)  document.getElementById('tournament-sub').value  = d.sub;
  });

  onValue(ref(db,'teams'), snap => {
    window._teams = snap.val() || {};
    renderTeamTable();
    renderTeamGrid();
    populateSellSelects();
    updateStats();
  });

  ['youth','local','invited'].forEach(cat => {
    onValue(ref(db,`players/${cat}`), snap => {
      window[`_${cat}`] = snap.val() || {};
      renderPlayerTable(cat);
      renderPlayerGrid(cat);
      populateSellSelects();
      updateStats();
    });
  });

  window._ADMIN_PASSWORD = ADMIN_PASSWORD;
</script>

<script>
/* ══════════════════════════════════════════
   NAVIGATION
══════════════════════════════════════════ */
function showSection(id){
  document.querySelectorAll('.section').forEach(s=>s.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t=>t.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  event.currentTarget.classList.add('active');
  if(id==='admin-gate' && sessionStorage.getItem('llfc_admin')){
    document.getElementById('admin-gate').classList.remove('active');
    document.getElementById('admin').classList.add('active');
  }
}

/* ══════════════════════════════════════════
   ADMIN AUTH
══════════════════════════════════════════ */
function checkAdminPass(){
  const pass = document.getElementById('admin-pass').value;
  const msg  = document.getElementById('login-msg');
  if(pass === window._ADMIN_PASSWORD){
    sessionStorage.setItem('llfc_admin','1');
    document.getElementById('admin-gate').classList.remove('active');
    document.getElementById('admin').classList.add('active');
    showMsg(msg,'✅ Access granted!','success');
  } else {
    showMsg(msg,'❌ Incorrect password','error');
  }
}
document.getElementById('admin-pass').addEventListener('keydown',e=>{ if(e.key==='Enter') checkAdminPass(); });

/* ══════════════════════════════════════════
   TABS
══════════════════════════════════════════ */
function switchRegTab(tab, el){
  document.querySelectorAll('#admin .cat-panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('#admin .cat-tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('reg-'+tab).classList.add('active');
  el.classList.add('active');
}
function switchViewTab(tab, el){
  document.querySelectorAll('#viewer .cat-panel').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('#viewer .cat-tab').forEach(t=>t.classList.remove('active'));
  document.getElementById('view-'+tab).classList.add('active');
  el.classList.add('active');
}

/* ══════════════════════════════════════════
   TOURNAMENT SETTINGS
══════════════════════════════════════════ */
let _logoBase64 = null;
function previewLogo(input){
  const file = input.files[0]; if(!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    _logoBase64 = e.target.result;
    document.getElementById('logo-preview-img').src = _logoBase64;
    document.getElementById('logo-preview-img').style.display='block';
    document.getElementById('logo-preview-placeholder').style.display='none';
  };
  reader.readAsDataURL(file);
}

function saveTournament(){
  const db=window._db, ref=window._ref, set=window._set;
  const name = document.getElementById('tournament-name').value.trim();
  const sub  = document.getElementById('tournament-sub').value.trim();
  const msg  = document.getElementById('tournament-msg');
  if(!name){ showMsg(msg,'⚠️ Tournament name required','error'); return; }
  const data = { name, sub: sub||'Live Player Auction' };
  if(_logoBase64) data.logo = _logoBase64;
  set(ref(db,'tournament'), data)
    .then(()=>showMsg(msg,'✅ Settings saved!','success'))
    .catch(e=>showMsg(msg,'❌ '+e.message,'error'));
}

/* ══════════════════════════════════════════
   TEAM LOGO
══════════════════════════════════════════ */
let _teamLogoBase64 = null;
function previewTeamLogo(input){
  const file = input.files[0]; if(!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    _teamLogoBase64 = e.target.result;
    const el = document.getElementById('team-logo-preview');
    el.src = _teamLogoBase64; el.style.display='block';
  };
  reader.readAsDataURL(file);
}

/* ══════════════════════════════════════════
   TEAMS
══════════════════════════════════════════ */
function addTeam(){
  const db=window._db, ref=window._ref, push=window._push;
  const name   = document.getElementById('team-name').value.trim();
  const cap    = document.getElementById('team-cap').value.trim();
  const budget = parseFloat(document.getElementById('team-budget').value)||0;
  const msg    = document.getElementById('team-msg');
  if(!name||!cap||!budget){ showMsg(msg,'⚠️ Fill all fields','error'); return; }
  const data = { name, cap, budget, remaining: budget };
  if(_teamLogoBase64) data.logo = _teamLogoBase64;
  push(ref(db,'teams'), data)
    .then(()=>{
      showMsg(msg,'✅ Team added!','success');
      document.getElementById('team-name').value='';
      document.getElementById('team-cap').value='';
      document.getElementById('team-budget').value='';
      _teamLogoBase64=null;
      document.getElementById('team-logo-preview').style.display='none';
    })
    .catch(e=>showMsg(msg,'❌ '+e.message,'error'));
}

function renderTeamTable(){
  const teams = window._teams||{};
  const tbody = document.getElementById('team-tbody');
  if(!Object.keys(teams).length){ tbody.innerHTML=`<tr><td colspan="5" class="empty-state">No teams yet</td></tr>`; return; }
  tbody.innerHTML = Object.entries(teams).map(([id,t])=>`
    <tr>
      <td><strong>${t.name}</strong></td>
      <td>${t.cap}</td>
      <td>৳${t.budget?.toLocaleString()}</td>
      <td style="color:${t.remaining<t.budget*0.2?'#c62828':'#2e7d32'}">৳${(t.remaining||t.budget).toLocaleString()}</td>
      <td><button class="btn btn-danger btn-sm" onclick="deleteTeam('${id}')">🗑️</button></td>
    </tr>`).join('');
}

function deleteTeam(id){
  if(!confirm('Delete this team?')) return;
  window._remove(window._ref(window._db,`teams/${id}`));
}

/* ══════════════════════════════════════════
   PLAYERS
══════════════════════════════════════════ */
function addPlayer(cat){
  const db=window._db, ref=window._ref, push=window._push;
  const name  = document.getElementById(`${cat}-name`).value.trim();
  const price = parseFloat(document.getElementById(`${cat}-price`).value)||0;
  const club  = cat==='invited' ? document.getElementById('invited-club').value.trim() : '';
  const msg   = document.getElementById(`${cat}-msg`);
  if(!name||!price){ showMsg(msg,'⚠️ Name & price required','error'); return; }
  if(cat==='invited'&&!club){ showMsg(msg,'⚠️ Club name required','error'); return; }
  const data = { name, basePrice: price, status:'available', cat };
  if(club) data.club = club;
  push(ref(db,`players/${cat}`), data)
    .then(()=>{
      showMsg(msg,'✅ Player added!','success');
      document.getElementById(`${cat}-name`).value='';
      document.getElementById(`${cat}-price`).value='';
      if(cat==='invited') document.getElementById('invited-club').value='';
    })
    .catch(e=>showMsg(msg,'❌ '+e.message,'error'));
}

function renderPlayerTable(cat){
  const players = window[`_${cat}`]||{};
  const tbody = document.getElementById(`${cat}-tbody`);
  const isInv = cat==='invited';
  if(!Object.keys(players).length){
    tbody.innerHTML=`<tr><td colspan="${isInv?8:7}" class="empty-state">No players yet</td></tr>`;
    return;
  }
  tbody.innerHTML = Object.entries(players).map(([id,p],i)=>`
    <tr>
      <td>${i+1}</td>
      <td><strong>${p.name}</strong></td>
      ${isInv?`<td>${p.club||'-'}</td>`:''}
      <td>৳${p.basePrice?.toLocaleString()}</td>
      <td><span class="badge badge-${p.status==='sold'?'sold':'available'}">${p.status||'available'}</span></td>
      <td>${p.soldTo||'-'}</td>
      <td>${p.soldPrice?'৳'+p.soldPrice.toLocaleString():'-'}</td>
      <td><button class="delete-btn" onclick="deletePlayer('${cat}','${id}')">🗑️</button></td>
    </tr>`).join('');
}

function deletePlayer(cat,id){
  if(!confirm('Delete this player?')) return;
  window._remove(window._ref(window._db,`players/${cat}/${id}`));
}

/* ══════════════════════════════════════════
   SELL PLAYER
══════════════════════════════════════════ */
function populateSellSelects(){
  const teams = window._teams||{};
  const allPlayers = [];
  ['youth','local','invited'].forEach(cat=>{
    const players = window[`_${cat}`]||{};
    Object.entries(players).forEach(([id,p])=>{
      allPlayers.push({id:`${cat}::${id}`, label:`[${cat.toUpperCase()}] ${p.name} (৳${p.basePrice})`, status:p.status});
    });
  });

  // Admin sell selects
  const pSel = document.getElementById('sell-player-select');
  const tSel = document.getElementById('sell-team-select');
  const pVal = pSel.value, tVal = tSel.value;
  pSel.innerHTML = '<option value="">-- Choose Player --</option>' +
    allPlayers.filter(p=>p.status!=='sold').map(p=>`<option value="${p.id}">${p.label}</option>`).join('');
  tSel.innerHTML = '<option value="">-- Choose Team --</option>' +
    Object.entries(teams).map(([id,t])=>`<option value="${id}">${t.name}</option>`).join('');
  if(pVal) pSel.value=pVal;
  if(tVal) tSel.value=tVal;

  // Modal
  const mSel = document.getElementById('modal-team-select');
  mSel.innerHTML = '<option value="">-- Choose Team --</option>' +
    Object.entries(teams).map(([id,t])=>`<option value="${id}">${t.name}</option>`).join('');
}

function sellPlayer(){
  const compound = document.getElementById('sell-player-select').value;
  const teamId   = document.getElementById('sell-team-select').value;
  const price    = parseFloat(document.getElementById('sell-price').value)||0;
  const msg      = document.getElementById('sell-msg');
  if(!compound||!teamId||!price){ showMsg(msg,'⚠️ Select player, team & price','error'); return; }
  _executeSell(compound, teamId, price, msg);
}

function _executeSell(compound, teamId, price, msg){
  const [cat, pid] = compound.split('::');
  const teams   = window._teams||{};
  const players = window[`_${cat}`]||{};
  const team    = teams[teamId];
  const player  = players[pid];
  if(!team||!player){ showMsg(msg,'❌ Invalid selection','error'); return; }
  if((team.remaining||team.budget) < price){
    showMsg(msg,`❌ Insufficient budget! Available: ৳${(team.remaining||team.budget).toLocaleString()}`,'error'); return;
  }
  const newRemaining = (team.remaining||team.budget) - price;
  const db=window._db, ref=window._ref, update=window._update;
  const updates = {};
  updates[`players/${cat}/${pid}/status`]    = 'sold';
  updates[`players/${cat}/${pid}/soldTo`]    = team.name;
  updates[`players/${cat}/${pid}/soldTeamId`]= teamId;
  updates[`players/${cat}/${pid}/soldPrice`] = price;
  updates[`teams/${teamId}/remaining`]       = newRemaining;
  update(ref(db), updates)
    .then(()=>showMsg(msg,'✅ Player sold successfully!','success'))
    .catch(e=>showMsg(msg,'❌ '+e.message,'error'));
}

function unsellPlayer(){
  const compound = document.getElementById('sell-player-select').value;
  const msg      = document.getElementById('sell-msg');
  if(!compound){ showMsg(msg,'⚠️ Select a player first','error'); return; }
  const [cat, pid] = compound.split('::');
  const player = (window[`_${cat}`]||{})[pid];
  if(!player||player.status!=='sold'){ showMsg(msg,'⚠️ Player is not sold','error'); return; }
  const teamId = player.soldTeamId;
  const price  = player.soldPrice||0;
  const team   = (window._teams||{})[teamId];
  const db=window._db, ref=window._ref, update=window._update;
  const updates = {};
  updates[`players/${cat}/${pid}/status`]    = 'available';
  updates[`players/${cat}/${pid}/soldTo`]    = null;
  updates[`players/${cat}/${pid}/soldTeamId`]= null;
  updates[`players/${cat}/${pid}/soldPrice`] = null;
  if(team) updates[`teams/${teamId}/remaining`] = (team.remaining||0) + price;
  update(ref(db), updates)
    .then(()=>showMsg(msg,'✅ Player unsold!','success'))
    .catch(e=>showMsg(msg,'❌ '+e.message,'error'));
}

/* ══════════════════════════════════════════
   VIEWER — PLAYER GRIDS
══════════════════════════════════════════ */
function renderPlayerGrid(cat){
  const players = window[`_${cat}`]||{};
  const grid = document.getElementById(`${cat}-player-grid`);
  const colorMap = {youth:'#e65100',local:'#1565c0',invited:'#6a1b9a'};
  const color = colorMap[cat];
  if(!Object.keys(players).length){
    grid.innerHTML=`<div class="empty-state" style="grid-column:1/-1"><span class="icon">👤</span>No ${cat} players registered yet</div>`;
    return;
  }
  grid.innerHTML = Object.entries(players).map(([id,p])=>`
    <div class="player-card ${p.status==='sold'?'sold':''}" style="border-top-color:${p.status==='sold'?'#c62828':color}">
      ${p.status==='sold'?`<div class="sold-ribbon">SOLD</div>`:''}
      <div style="font-size:36px;margin-bottom:8px">${cat==='youth'?'🟠':cat==='local'?'🔵':'🟣'}</div>
      <div class="player-name">${p.name}</div>
      ${p.club?`<div class="player-club">🏟️ ${p.club}</div>`:''}
      <div class="player-price">৳${p.basePrice?.toLocaleString()}</div>
      ${p.status==='sold'?`<div class="player-sold-info">→ ${p.soldTo} @ ৳${p.soldPrice?.toLocaleString()}</div>`:''}
      <div style="margin-top:8px"><span class="badge badge-${p.status==='sold'?'sold':'available'}">${p.status==='sold'?'SOLD':'AVAILABLE'}</span></div>
    </div>`).join('');
}

/* ══════════════════════════════════════════
   VIEWER — TEAM CARDS
══════════════════════════════════════════ */
function renderTeamGrid(){
  const teams = window._teams||{};
  const grid  = document.getElementById('team-grid');
  if(!Object.keys(teams).length){
    grid.innerHTML=`<div class="empty-state" style="grid-column:1/-1"><span class="icon">🏟️</span>No teams registered yet</div>`;
    return;
  }
  grid.innerHTML = Object.entries(teams).map(([id,t])=>{
    const allP = getAllPlayersByTeam(t.name);
    const youth=allP.filter(p=>p.cat==='youth');
    const local=allP.filter(p=>p.cat==='local');
    const invited=allP.filter(p=>p.cat==='invited');
    const spent = (t.budget||0)-(t.remaining!=null?t.remaining:t.budget||0);
    const pct   = t.budget ? Math.max(0,Math.min(100,Math.round(spent/t.budget*100))) : 0;
    return `
    <div class="team-card" id="teamcard-${id}">
      <div class="team-card-header">
        <div class="team-logo-wrap">
          ${t.logo?`<img src="${t.logo}" alt="logo"/>`:`🏆`}
        </div>
        <div class="team-info">
          <h3>${t.name}</h3>
          <div class="team-cap">👤 ${t.cap}</div>
          <div style="font-size:12px;color:rgba(255,255,255,0.6);margin-top:2px">Players: ${allP.length}</div>
        </div>
      </div>
      <div class="team-budget-bar">
        <div class="budget-label">
          <span>Budget Used</span>
          <span>৳${spent.toLocaleString()} / ৳${(t.budget||0).toLocaleString()}</span>
        </div>
        <div class="budget-track"><div class="budget-fill" style="width:${pct}%"></div></div>
        <div style="font-size:11px;color:#2e7d32;margin-top:4px;font-weight:700">
          ৳${(t.remaining!=null?t.remaining:t.budget||0).toLocaleString()} remaining
        </div>
      </div>
      <div class="squad-tabs">
        <button class="squad-tab active" onclick="switchSquadTab('${id}','youth',this)">🟠 Youth</button>
        <button class="squad-tab" onclick="switchSquadTab('${id}','local',this)">🔵 Local</button>
        <button class="squad-tab" onclick="switchSquadTab('${id}','invited',this)">🟣 Invited</button>
      </div>
      <div id="squad-${id}-youth" class="squad-list">
        ${renderSquadList(youth)}
      </div>
      <div id="squad-${id}-local" class="squad-list" style="display:none">
        ${renderSquadList(local)}
      </div>
      <div id="squad-${id}-invited" class="squad-list" style="display:none">
        ${renderSquadList(invited)}
      </div>
    </div>`;
  }).join('');
}

function renderSquadList(players){
  if(!players.length) return `<div class="squad-empty">— No players —</div>`;
  return players.map(p=>`
    <div class="squad-player">
      <div class="squad-player-name">${p.name}</div>
      <div class="squad-player-price">৳${p.soldPrice?.toLocaleString()}</div>
    </div>`).join('');
}

function switchSquadTab(teamId, cat, el){
  const card = document.getElementById(`teamcard-${teamId}`);
  card.querySelectorAll('.squad-tab').forEach(t=>t.classList.remove('active'));
  card.querySelectorAll('.squad-list').forEach(l=>l.style.display='none');
  el.classList.add('active');
  document.getElementById(`squad-${teamId}-${cat}`).style.display='block';
}

function getAllPlayersByTeam(teamName){
  const result=[];
  ['youth','local','invited'].forEach(cat=>{
    const players=window[`_${cat}`]||{};
    Object.values(players).forEach(p=>{
      if(p.soldTo===teamName) result.push({...p, cat});
    });
  });
  return result;
}

/* ══════════════════════════════════════════
   STATS
══════════════════════════════════════════ */
function updateStats(){
  let total=0, sold=0;
  ['youth','local','invited'].forEach(cat=>{
    const players=window[`_${cat}`]||{};
    Object.values(players).forEach(p=>{ total++; if(p.status==='sold') sold++; });
  });
  document.getElementById('stat-total').textContent = total;
  document.getElementById('stat-sold').textContent  = sold;
  document.getElementById('stat-available').textContent = total-sold;
  document.getElementById('stat-teams').textContent = Object.keys(window._teams||{}).length;
}

/* ══════════════════════════════════════════
   HELPERS
══════════════════════════════════════════ */
function showMsg(el, text, type){
  el.textContent = text;
  el.className = `msg ${type}`;
  setTimeout(()=>{ el.className='msg'; el.textContent=''; }, 4000);
}

function closeSellModal(){ document.getElementById('sell-modal').classList.remove('open'); }

/* init */
window._teams={};window._youth={};window._local={};window._invited={};
</script>
</body>
</html>
