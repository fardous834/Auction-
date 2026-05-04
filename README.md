<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>LLFC Auction Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Rajdhani:wght@400;500;600;700&family=Oswald:wght@300;400;500;600;700&family=Playfair+Display:wght@700;900&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet"/>
<style>
  :root {
    --red: #D0021B;
    --red-deep: #8B0000;
    --red-bright: #FF1930;
    --gold: #D4AF37;
    --gold-light: #F5E6C8;
    --white: #FFFFFF;
    --off-white: #F5F0F0;
    --grey: #E8E0E0;
    --dark: #1A0505;
    --black-bg: #0a0a0a;
    --card-shadow: 0 8px 32px rgba(208,2,27,0.2);
    --card-shadow-hover: 0 16px 48px rgba(208,2,27,0.35);
    --font-display: 'Bebas Neue', sans-serif;
    --font-body: 'Rajdhani', sans-serif;
    --font-head: 'Oswald', sans-serif;
    --font-modern: 'Inter', sans-serif;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body { background:#0f0f0f; font-family:var(--font-body); color:var(--dark); min-height:100vh; }

  #top-nav {
    background: linear-gradient(135deg, #1a1a1a 0%, #2d1b1b 50%, #1a1a1a 100%);
    padding: 0;
    display: flex;
    align-items: stretch;
    box-shadow: 0 8px 32px rgba(0,0,0,0.6);
    position: sticky; top:0; z-index:1000;
    border-bottom: 3px solid var(--gold);
  }
  .nav-brand {
    display:flex; align-items:center; gap:16px; padding:12px 24px;
    border-right:2px solid rgba(255,255,255,0.1);
    flex:1;
  }
  #nav-logo-img { width:52px; height:52px; border-radius:8px; object-fit:cover; border:2px solid var(--gold); display:none; }
  #nav-logo-placeholder { width:52px; height:52px; border-radius:8px; background:linear-gradient(135deg,var(--red),var(--gold)); border:2px solid var(--gold); display:flex; align-items:center; justify-content:center; font-size:24px; }
  #nav-title { font-family:var(--font-display); font-size:24px; color:#fff; letter-spacing:3px; line-height:1.1; }
  #nav-subtitle { font-family:var(--font-body); font-size:11px; color:rgba(255,255,255,0.6); letter-spacing:2px; text-transform:uppercase; }
  .nav-tabs { display:flex; flex-wrap:wrap; }
  .nav-tab {
    padding: 0 20px;
    background: transparent;
    border: none;
    color: rgba(255,255,255,0.6);
    font-family: var(--font-head);
    font-size: 11px; font-weight:600;
    letter-spacing: 2px; text-transform:uppercase;
    cursor: pointer; transition: all 0.3s;
    border-bottom: 3px solid transparent;
  }
  .nav-tab:hover { color:#fff; background:rgba(255,255,255,0.05); }
  .nav-tab.active { color:var(--gold); border-bottom-color:var(--gold); background:rgba(212,175,55,0.08); }

  .section { display:none; padding:32px 28px; max-width:1600px; margin:0 auto; }
  .section.active { display:block; }

  .card {
    background:linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border-radius:16px;
    box-shadow: var(--card-shadow);
    margin-bottom:24px;
    overflow:hidden;
    transition: all 0.3s ease;
    border: 2px solid var(--gold);
  }
  .card:hover { box-shadow: var(--card-shadow-hover); transform: translateY(-2px); }
  .card-header {
    background: linear-gradient(135deg, var(--red) 0%, var(--red-deep) 100%);
    padding:18px 24px;
    display:flex; align-items:center; gap:12px;
    border-bottom: 3px solid var(--gold);
  }
  .card-header h2 {
    font-family:var(--font-display); font-size:20px; color:#fff; letter-spacing:2px;
  }
  .card-body { padding:24px; }

  .grid-2 { display:grid; grid-template-columns:1fr 1fr; gap:24px; }
  .grid-3 { display:grid; grid-template-columns:repeat(3,1fr); gap:24px; }
  @media(max-width:900px){.grid-2{grid-template-columns:1fr;} .grid-3{grid-template-columns:1fr;}}

  .form-row { display:flex; gap:16px; flex-wrap:wrap; margin-bottom:18px; }
  .form-group { display:flex; flex-direction:column; gap:8px; flex:1; min-width:160px; }
  label { font-size:12px; font-weight:700; letter-spacing:1.5px; text-transform:uppercase; color:#666; }
  input[type=text], input[type=number], input[type=password], input[type=url], select, textarea {
    border:2px solid #ddd; border-radius:10px; padding:12px 16px;
    font-family:var(--font-body); font-size:15px; color:var(--dark);
    transition:all 0.3s; background:#fff; outline:none;
  }
  input:focus, select:focus, textarea:focus { 
    border-color:var(--red); 
    box-shadow: 0 0 0 3px rgba(208,2,27,0.1);
  }

  .btn {
    padding:12px 26px; border:none; border-radius:10px; cursor:pointer;
    font-family:var(--font-head); font-size:14px; font-weight:700;
    letter-spacing:1.5px; text-transform:uppercase; transition:all 0.3s;
  }
  .btn-red { background:linear-gradient(135deg,var(--red),var(--red-deep)); color:#fff; }
  .btn-red:hover { background:linear-gradient(135deg,var(--red-bright),var(--red)); transform:translateY(-2px); box-shadow:0 8px 20px rgba(208,2,27,0.4); }
  .btn-green { background:linear-gradient(135deg,#00a651,#007a3d); color:#fff; }
  .btn-green:hover { transform:translateY(-2px); box-shadow:0 8px 20px rgba(0,166,81,0.4); }
  .btn-gold { background:linear-gradient(135deg,var(--gold),#c59e1b); color:#000; font-weight:700; }
  .btn-gold:hover { transform:translateY(-2px); box-shadow:0 8px 20px rgba(212,175,55,0.4); }
  .btn-sm { padding:8px 16px; font-size:12px; }
  .btn-danger { background:linear-gradient(135deg,#ff4444,#cc0000); color:#fff; }

  .table-wrap { overflow-x:auto; border-radius:12px; border:2px solid var(--gold); margin:16px 0; background:#fff; }
  table { width:100%; border-collapse:collapse; font-size:13px; }
  thead { background:linear-gradient(135deg,var(--red),var(--red-deep)); }
  thead th { color:#fff; padding:14px 12px; font-family:var(--font-head); font-weight:700; letter-spacing:1px; text-align:left; white-space:nowrap; font-size:11px; }
  tbody tr { border-bottom:1px solid #f5f5f5; transition:all 0.2s; cursor:pointer; }
  tbody tr:hover { background:#fff9f9; }
  tbody td { padding:12px; vertical-align:middle; }
  .badge {
    display:inline-block; padding:4px 10px; border-radius:20px; font-size:10px;
    font-weight:700; letter-spacing:0.5px; text-transform:uppercase;
  }
  .badge-available { background:#e8f5e9; color:#2e7d32; }
  .badge-sold { background:#ffebee; color:#c62828; }
  .badge-youth { background:#dbeafe; color:#1e40af; }
  .badge-local { background:#fee2e2; color:#991b1b; }
  .badge-invited { background:#fef3c7; color:#92400e; }

  #admin-login, #moderator-login {
    max-width:420px; margin:100px auto;
    background:linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border-radius:20px; padding:48px;
    box-shadow: 0 16px 60px rgba(208,2,27,0.25);
    text-align:center;
    border: 3px solid var(--gold);
  }
  #admin-login h2, #moderator-login h2 { font-family:var(--font-display); font-size:36px; color:var(--red); letter-spacing:3px; margin-bottom:12px; }

  .viewer-header {
    background: linear-gradient(160deg, #1a1a1a 0%, #2d1b1b 50%, #1a0a0a 100%);
    border-radius:20px; padding:48px; text-align:center;
    margin-bottom:40px; position:relative; overflow:hidden;
    box-shadow: 0 12px 48px rgba(0,0,0,0.5);
    border: 3px solid var(--gold);
  }
  .viewer-header-inner { display:flex; align-items:center; justify-content:center; gap:32px; flex-wrap:wrap; }
  #viewer-logo { width:100px; height:100px; border-radius:12px; object-fit:cover; border:3px solid var(--gold); }
  #viewer-logo-placeholder { width:100px; height:100px; border-radius:12px; background:linear-gradient(135deg,var(--red),var(--gold)); display:flex; align-items:center; justify-content:center; font-size:48px; }
  #viewer-tournament-name { font-family:var(--font-display); font-size:52px; color:#fff; letter-spacing:4px; text-shadow:0 4px 20px rgba(0,0,0,0.5); }
  #viewer-tournament-sub { color:rgba(255,255,255,0.8); letter-spacing:2px; text-transform:uppercase; font-size:13px; margin-top:8px; }

  .cat-tabs { display:flex; gap:0; margin-bottom:28px; border-radius:12px; overflow:hidden; box-shadow:0 4px 12px rgba(0,0,0,0.1); background:#fff; border: 2px solid var(--gold); }
  .cat-tab { flex:1; padding:14px; border:none; cursor:pointer; font-family:var(--font-head); font-size:13px; font-weight:700; letter-spacing:1.5px; text-transform:uppercase; background:#fff; color:#aaa; transition:all 0.3s; }
  .cat-tab.active { background:linear-gradient(135deg,var(--red),var(--red-deep)); color:#fff; }
  .cat-tab:hover:not(.active) { background:#f5f5f5; color:#555; }
  .cat-panel { display:none; }
  .cat-panel.active { display:block; }

  .modal-bg { display:none; position:fixed; inset:0; background:rgba(0,0,0,0.85); z-index:2000; align-items:center; justify-content:center; padding:20px; overflow-y:auto; }
  .modal-bg.open { display:flex; }
  .modal { background:linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%); border-radius:20px; padding:40px; max-width:900px; width:100%; position:relative; box-shadow:0 20px 60px rgba(0,0,0,0.3); border: 3px solid var(--gold); max-height:90vh; overflow-y:auto; }
  .modal-close { position:absolute; top:20px; right:24px; font-size:28px; cursor:pointer; color:#ccc; transition:all 0.2s; }
  .modal-close:hover { color:var(--red); transform:rotate(90deg); }

  .msg { padding:14px 18px; border-radius:10px; font-size:13px; font-weight:700; margin-top:12px; display:none; border-left:4px solid; }
  .msg.success { background:#e8f5e9; color:#2e7d32; display:block; border-left-color:#4caf50; }
  .msg.error { background:#ffebee; color:#c62828; display:block; border-left-color:#f44336; }

  .section-title { font-family:var(--font-display); font-size:36px; color:var(--red); letter-spacing:3px; margin-bottom:8px; }
  .section-divider { height:4px; background:linear-gradient(90deg,var(--gold),transparent); margin-bottom:32px; border-radius:2px; width:200px; }

  .empty-state { text-align:center; padding:60px 40px; color:#ccc; font-size:15px; letter-spacing:1px; }
  .delete-btn { background:none; border:none; cursor:pointer; color:#ddd; font-size:16px; transition:color 0.2s; padding:6px; }
  .delete-btn:hover { color:var(--red); }

  .logo-preview { width:100px; height:100px; border-radius:12px; border:3px solid var(--gold); object-fit:cover; margin-top:10px; display:none; }

  /* PLAYER CARD */
  .player-card-container { display:grid; grid-template-columns:repeat(auto-fill,minmax(360px,1fr)); gap:28px; margin:24px 0; }
  .player-card { 
    cursor:pointer; 
    transition:all 0.3s;
    position:relative;
    background: linear-gradient(135deg, #0a0a0a 0%, #1a0a0a 50%, #0a0a0a 100%);
    border: 3px solid var(--gold);
    border-radius: 12px;
    padding: 0;
    min-height: 820px;
    display: flex;
    flex-direction: column;
    overflow: hidden;
    box-shadow: 0 10px 40px rgba(0,0,0,0.4);
  }
  .player-card:hover { transform:scale(1.01); box-shadow: 0 15px 50px rgba(212,175,55,0.35); }

  .card-tournament-header {
    background: linear-gradient(135deg, var(--red) 0%, var(--red-deep) 100%);
    padding: 14px;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    border-bottom: 3px solid var(--gold);
  }
  .card-tournament-logo { width:36px; height:36px; border-radius:6px; object-fit:cover; border:2px solid var(--gold); display:none; }
  .card-tournament-logo-placeholder { width:36px; height:36px; border-radius:6px; background:linear-gradient(135deg,var(--gold),#c59e1b); display:flex; align-items:center; justify-content:center; font-size:18px; flex-shrink:0; font-weight:bold; }
  .card-tournament-name { font-family:var(--font-display); font-size:13px; color:var(--gold); letter-spacing:2px; text-transform:uppercase; margin: 0; font-weight:700; }

  .card-category-badge { text-align: center; padding: 14px; background: rgba(255,255,255,0.02); }
  .card-cat-big-badge {
    display: inline-block;
    background: linear-gradient(135deg, var(--red), var(--red-deep));
    color: var(--gold);
    padding: 10px 28px;
    border-radius: 10px;
    font-size: 16px;
    font-weight: 700;
    letter-spacing: 3px;
    text-transform: uppercase;
    border: 2px solid var(--gold);
    font-family: var(--font-head);
    box-shadow: 0 6px 20px rgba(208,2,27,0.4);
  }

  .card-serial-big {
    font-family: var(--font-display);
    font-size: 64px;
    color: var(--gold);
    text-align: center;
    padding: 8px 0;
    border-bottom: 2px solid var(--gold);
    font-weight: 700;
    letter-spacing: 3px;
  }

  .card-player-section {
    background: rgba(255,255,255,0.05);
    border: 2px solid var(--gold);
    border-radius: 8px;
    padding: 14px;
    margin: 12px 14px 0 14px;
    text-align: center;
    flex-grow: 0;
  }
  .card-player-label { font-size:9px; color:var(--gold); text-transform:uppercase; letter-spacing:1.5px; margin-bottom:4px; font-weight:700; }
  .card-player-photo { width:56px; height:56px; border-radius:50%; object-fit:cover; border:2px solid var(--gold); margin:6px auto 6px auto; display:none; }
  .card-player-name { font-family:var(--font-display); font-size:24px; color:#fff; letter-spacing:1px; font-weight:700; text-transform:uppercase; margin: 0; line-height:1.2; }
  .card-base-price-small { font-size:11px; color:var(--gold); margin-top:6px; font-weight:700; }

  .card-club-section {
    background: rgba(255,255,255,0.05);
    border: 2px solid var(--gold);
    border-radius: 8px;
    padding: 12px;
    margin: 10px 14px 0 14px;
    text-align: center;
  }
  .card-club-label { font-size:9px; color:var(--gold); text-transform:uppercase; letter-spacing:1.5px; margin-bottom:3px; font-weight:700; }
  .card-club-name { font-size:14px; color:#fff; font-weight:700; }

  .card-base-price-section { text-align: center; padding: 0 14px; margin-bottom: 10px; }
  .card-base-price { font-family: var(--font-display); font-size: 13px; color: var(--gold); font-weight: 700; letter-spacing: 2px; }

  .card-description-box {
    background: #000000;
    border: 2px solid var(--gold);
    border-radius: 8px;
    padding: 12px;
    margin: 10px 14px 0 14px;
    min-height: 70px;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: inset 0 2px 8px rgba(0,0,0,0.8);
  }
  .card-description-text {
    font-size: 10px;
    color: #ffffff;
    line-height: 1.4;
    text-align: center;
    letter-spacing: 0.5px;
    font-weight: 700;
  }

  .card-stats {
    background: rgba(255,255,255,0.05);
    border: 3px solid var(--gold);
    border-radius: 8px;
    padding: 12px;
    margin: 10px 14px 0 14px;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    box-shadow: 0 6px 20px rgba(212,175,55,0.2);
  }
  .stats-label { font-size:11px; color:var(--gold); text-transform:uppercase; letter-spacing:2px; margin-bottom:10px; font-weight:700; text-align:center; font-family:var(--font-head); }
  .stats-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:8px; flex-grow:1; }
  .stat-badge {
    background: linear-gradient(135deg, var(--red), var(--red-deep));
    border: 2px solid var(--gold);
    border-radius: 8px;
    padding: 8px 6px;
    text-align: center;
    box-shadow: 0 6px 16px rgba(208,2,27,0.35);
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .stat-value { font-family:var(--font-display); font-size:18px; color:var(--gold); font-weight:700; }
  .stat-key { font-size:8px; color:rgba(255,255,255,0.95); text-transform:uppercase; letter-spacing:0.5px; margin-top:2px; font-weight:700; }

  .card-action-buttons {
    display: flex;
    gap: 8px;
    padding: 10px 14px 12px 14px;
    flex-wrap: wrap;
    justify-content: center;
  }
  .card-action-btn {
    padding: 8px 14px;
    font-size: 11px;
    flex: 1;
    min-width: 80px;
  }

  /* MODERN BID LIST */
  .bid-list-container { margin-top: 20px; }
  .bid-item {
    background: linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border: 2px solid var(--gold);
    border-radius: 12px;
    padding: 16px;
    margin-bottom: 12px;
    display: grid;
    grid-template-columns: 60px 1fr auto;
    gap: 16px;
    align-items: center;
    transition: all 0.3s;
  }
  .bid-item:hover { box-shadow: 0 8px 20px rgba(208,2,27,0.2); transform: translateX(4px); }
  .bid-item-num { 
    background: linear-gradient(135deg, var(--red), var(--red-deep));
    color: var(--gold);
    width: 50px;
    height: 50px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 700;
    font-size: 18px;
    border: 2px solid var(--gold);
  }
  .bid-item-info h4 { color: var(--red); font-size: 16px; margin-bottom: 4px; letter-spacing: 1px; }
  .bid-item-info p { color: #666; font-size: 12px; margin: 2px 0; }
  .bid-item-actions { display: flex; gap: 8px; flex-wrap: wrap; }

  .search-filter-section {
    background:linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border-radius:16px;
    padding:24px;
    margin-bottom:32px;
    border: 2px solid var(--gold);
  }
  .search-filter-section h3 { font-family:var(--font-display); font-size:18px; color:var(--red); margin-bottom:16px; }
  .search-filter-row { display:flex; gap:16px; flex-wrap:wrap; margin-bottom:16px; }
  .search-filter-row input, .search-filter-row select { flex:1; min-width:150px; }

  .auction-timer {
    background: linear-gradient(135deg, var(--red) 0%, var(--red-deep) 100%);
    border-radius:16px;
    padding:24px;
    margin-bottom:32px;
    text-align:center;
    border: 3px solid var(--gold);
    box-shadow: 0 8px 32px rgba(208,2,27,0.3);
  }
  .auction-timer h3 { font-family:var(--font-display); font-size:18px; color:var(--gold); margin-bottom:12px; }
  .timer-display { font-family:var(--font-display); font-size:48px; color:var(--gold); letter-spacing:2px; margin:16px 0; }
  .timer-controls { display:flex; gap:12px; justify-content:center; flex-wrap:wrap; }
  .timer-btn { padding:10px 20px; background:rgba(255,255,255,0.2); color:#fff; border:2px solid var(--gold); border-radius:8px; cursor:pointer; font-family:var(--font-head); font-weight:700; transition:all 0.3s; }
  .timer-btn:hover { background:rgba(255,255,255,0.3); }

  .list-view { display:none; }
  .list-view.active { display:block; }

  .team-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(300px,1fr)); gap:24px; margin:24px 0; }
  .team-card {
    background:linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border-radius:16px;
    box-shadow: var(--card-shadow);
    overflow:hidden;
    transition:all 0.3s;
    border: 2px solid var(--gold);
    cursor:pointer;
  }
  .team-card:hover { 
    box-shadow: var(--card-shadow-hover);
    transform: translateY(-4px);
  }
  .team-card-header {
    background: linear-gradient(135deg, var(--red) 0%, var(--red-deep) 100%);
    padding:24px;
    text-align:center;
    position:relative;
    height:140px;
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    border-bottom: 3px solid var(--gold);
  }
  .team-logo { width:70px; height:70px; border-radius:50%; border:3px solid var(--gold); object-fit:cover; margin-bottom:8px; }
  .team-card h3 { color:#fff; font-family:var(--font-display); font-size:22px; letter-spacing:2px; }
  .team-card-body { padding:24px; }
  .team-stat { display:flex; justify-content:space-between; align-items:center; padding:12px 0; border-bottom:1px solid #f0f0f0; }
  .team-stat:last-child { border-bottom:none; }
  .team-stat-label { color:#666; font-size:12px; font-weight:700; text-transform:uppercase; }
  .team-stat-value { font-family:var(--font-display); font-size:24px; color:var(--red); }

  ::-webkit-scrollbar{width:8px;height:8px}
  ::-webkit-scrollbar-track{background:#f0e8e8}
  ::-webkit-scrollbar-thumb{background:var(--red);border-radius:4px}
  ::-webkit-scrollbar-thumb:hover{background:var(--red-deep)}
</style>
</head>
<body>

<nav id="top-nav">
  <div class="nav-brand">
    <div id="nav-logo-placeholder"></div>
    <img id="nav-logo-img" src="" alt="logo"/>
    <div>
      <div id="nav-title">LLFC AUCTION</div>
      <div id="nav-subtitle">Live Player Auction</div>
    </div>
  </div>
  <div class="nav-tabs">
    <button class="nav-tab active" onclick="showSection('viewer')">LIVE AUCTION</button>
    <button class="nav-tab" onclick="showSection('teams-section')">TEAMS</button>
    <button class="nav-tab" onclick="showSection('admin-gate')">ADMIN</button>
    <button class="nav-tab" onclick="showSection('moderator-gate')">STATS</button>
  </div>
</nav>

<!-- TEAMS SECTION -->
<div id="teams-section" class="section">
  <div class="section-title">TEAM PROFILES</div>
  <div class="section-divider"></div>
  <div class="team-grid" id="team-profiles-grid"></div>
</div>

<!-- ADMIN LOGIN -->
<div id="admin-gate" class="section">
  <div id="admin-login">
    <h2>ADMIN ACCESS</h2>
    <p>Enter admin password to continue</p>
    <input type="password" id="admin-pass" placeholder="Password" style="width:100%;margin-bottom:16px;"/>
    <button class="btn btn-red" style="width:100%" onclick="checkAdminPass()">UNLOCK PANEL</button>
    <div id="login-msg" class="msg"></div>
  </div>
</div>

<!-- ADMIN PANEL -->
<div id="admin" class="section">
  <div class="section-title">ADMIN CONTROL</div>
  <div class="section-divider"></div>

  <div class="grid-2">
    <div class="card">
      <div class="card-header"><h2>TOURNAMENT SETTINGS</h2></div>
      <div class="card-body">
        <div class="form-row">
          <div class="form-group">
            <label>Tournament Name</label>
            <input type="text" id="tournament-name" placeholder="e.g. LLFC Premier Auction 2025"/>
          </div>
          <div class="form-group">
            <label>Sub Title</label>
            <input type="text" id="tournament-sub" placeholder="e.g. Season 1 - Sylhet"/>
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label>Tournament Logo URL</label>
            <input type="url" id="tournament-logo" placeholder="https://example.com/logo.png"/>
            <img id="tournament-logo-preview" class="logo-preview" alt="Logo preview"/>
          </div>
        </div>
        <button class="btn btn-red" style="margin-top:16px; width:100%" onclick="saveTournament()">SAVE SETTINGS</button>
        <div id="tournament-msg" class="msg"></div>
      </div>
    </div>

    <div class="card">
      <div class="card-header"><h2>BID RULES</h2></div>
      <div class="card-body">
        <div class="form-row">
          <div class="form-group">
            <label>Youth - Min Price</label>
            <input type="number" id="youth-min" placeholder="e.g. 1000" min="0"/>
          </div>
          <div class="form-group">
            <label>Youth - Max Price</label>
            <input type="number" id="youth-max" placeholder="e.g. 10000" min="0"/>
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label>Local - Min Price</label>
            <input type="number" id="local-min" placeholder="e.g. 2000" min="0"/>
          </div>
          <div class="form-group">
            <label>Local - Max Price</label>
            <input type="number" id="local-max" placeholder="e.g. 20000" min="0"/>
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label>Invited - Min Price</label>
            <input type="number" id="invited-min" placeholder="e.g. 3000" min="0"/>
          </div>
          <div class="form-group">
            <label>Invited - Max Price</label>
            <input type="number" id="invited-max" placeholder="e.g. 50000" min="0"/>
          </div>
        </div>
        <button class="btn btn-red" style="margin-top:16px; width:100%" onclick="saveBidRules()">SAVE BID RULES</button>
        <div id="bid-rules-msg" class="msg"></div>
      </div>
    </div>
  </div>

  <div class="card">
    <div class="card-header"><h2>TEAM REGISTRATION</h2></div>
    <div class="card-body">
      <div class="form-row">
        <div class="form-group">
          <label>Team Name</label>
          <input type="text" id="team-name" placeholder="Team name"/>
        </div>
        <div class="form-group">
          <label>Captain</label>
          <input type="text" id="team-cap" placeholder="Captain name"/>
        </div>
        <div class="form-group">
          <label>Total Budget (Coins)</label>
          <input type="number" id="team-budget" placeholder="e.g. 50000"/>
        </div>
        <div class="form-group">
          <label>Team Logo URL</label>
          <input type="url" id="team-logo" placeholder="https://example.com/team-logo.png"/>
        </div>
      </div>
      <button class="btn btn-red" style="margin-top:16px; width:100%" onclick="addTeam()">REGISTER TEAM</button>
      <div id="team-msg" class="msg"></div>
    </div>
  </div>

  <div class="card">
    <div class="card-header"><h2>REGISTERED TEAMS</h2></div>
    <div class="card-body">
      <div class="table-wrap">
        <table id="team-table">
          <thead><tr><th>#</th><th>Team</th><th>Captain</th><th>Budget</th><th>Spent</th><th>Remaining</th><th>Action</th></tr></thead>
          <tbody id="team-tbody"></tbody>
        </table>
      </div>
    </div>
  </div>

  <div class="card">
    <div class="card-header"><h2>PLAYER REGISTRATION</h2></div>
    <div class="card-body">
      <div class="cat-tabs">
        <button class="cat-tab active" onclick="switchRegTab('youth',this)">YOUTH</button>
        <button class="cat-tab" onclick="switchRegTab('local',this)">LOCAL</button>
        <button class="cat-tab" onclick="switchRegTab('invited',this)">INVITED</button>
      </div>

      <div id="reg-youth" class="cat-panel active">
        <div class="form-row">
          <div class="form-group"><label>Player Name</label><input type="text" id="youth-name" placeholder="Full name"/></div>
          <div class="form-group"><label>Base Price (Coins)</label><input type="number" id="youth-price" placeholder="e.g. 2000"/></div>
          <div class="form-group" style="max-width:100px"><label>Photo</label><input type="file" id="youth-photo" accept="image/*"/></div>
          <div style="display:flex;align-items:flex-end"><button class="btn btn-red btn-sm" onclick="addPlayer('youth')">ADD PLAYER</button></div>
        </div>
        <div id="youth-msg" class="msg"></div>
        <div class="table-wrap"><table><thead><tr><th>#</th><th>Name</th><th>Base Price</th><th>Status</th><th>Sold To</th><th>Sold Price</th><th>Action</th></tr></thead><tbody id="youth-tbody"></tbody></table></div>
      </div>

      <div id="reg-local" class="cat-panel">
        <div class="form-row">
          <div class="form-group"><label>Player Name</label><input type="text" id="local-name" placeholder="Full name"/></div>
          <div class="form-group"><label>Base Price (Coins)</label><input type="number" id="local-price" placeholder="e.g. 3000"/></div>
          <div class="form-group" style="max-width:100px"><label>Photo</label><input type="file" id="local-photo" accept="image/*"/></div>
          <div style="display:flex;align-items:flex-end"><button class="btn btn-red btn-sm" onclick="addPlayer('local')">ADD PLAYER</button></div>
        </div>
        <div id="local-msg" class="msg"></div>
        <div class="table-wrap"><table><thead><tr><th>#</th><th>Name</th><th>Base Price</th><th>Status</th><th>Sold To</th><th>Sold Price</th><th>Action</th></tr></thead><tbody id="local-tbody"></tbody></table></div>
      </div>

      <div id="reg-invited" class="cat-panel">
        <div class="form-row">
          <div class="form-group"><label>Player Name</label><input type="text" id="invited-name" placeholder="Full name"/></div>
          <div class="form-group"><label>Club Name</label><input type="text" id="invited-club" placeholder="Club/District"/></div>
          <div class="form-group"><label>Base Price (Coins)</label><input type="number" id="invited-price" placeholder="e.g. 5000"/></div>
          <div class="form-group" style="max-width:100px"><label>Photo</label><input type="file" id="invited-photo" accept="image/*"/></div>
          <div style="display:flex;align-items:flex-end"><button class="btn btn-red btn-sm" onclick="addPlayer('invited')">ADD PLAYER</button></div>
        </div>
        <div id="invited-msg" class="msg"></div>
        <div class="table-wrap"><table><thead><tr><th>#</th><th>Name</th><th>Club</th><th>Base Price</th><th>Status</th><th>Sold To</th><th>Sold Price</th><th>Action</th></tr></thead><tbody id="invited-tbody"></tbody></table></div>
      </div>
    </div>
  </div>

  <div class="card">
    <div class="card-header"><h2>SOLD OUT MANAGEMENT</h2></div>
    <div class="card-body">
      <div class="bid-list-container" id="sold-list"></div>
    </div>
  </div>
</div>

<!-- MODERATOR LOGIN -->
<div id="moderator-gate" class="section">
  <div id="moderator-login">
    <h2>STATS MANAGER</h2>
    <p>Enter moderator password</p>
    <input type="password" id="moderator-pass" placeholder="Password" style="width:100%;margin-bottom:16px;"/>
    <button class="btn btn-red" style="width:100%" onclick="checkModeratorPass()">UNLOCK STATS</button>
    <div id="moderator-login-msg" class="msg"></div>
  </div>
</div>

<!-- MODERATOR PANEL -->
<div id="moderator" class="section">
  <div class="section-title">PLAYER STATS MANAGEMENT</div>
  <div class="section-divider"></div>

  <div class="card">
    <div class="card-header"><h2>ADD/UPDATE PLAYER STATS</h2></div>
    <div class="card-body">
      <div class="form-row">
        <div class="form-group">
          <label>Select Player</label>
          <select id="stats-player-select" onchange="loadPlayerStats()"><option value="">-- Choose Player --</option></select>
        </div>
      </div>

      <div id="stats-form-section" style="display:none">
        <div class="form-row">
          <div class="form-group"><label>COBEG Rank</label><input type="text" id="stats-rank" placeholder="Elite, A, B, C"/></div>
          <div class="form-group"><label>Matches Played</label><input type="number" id="stats-matches" placeholder="0" min="0"/></div>
          <div class="form-group"><label>Wins</label><input type="number" id="stats-wins" placeholder="0" min="0"/></div>
          <div class="form-group"><label>Draws</label><input type="number" id="stats-draws" placeholder="0" min="0"/></div>
          <div class="form-group"><label>MOTM</label><input type="number" id="stats-motm" placeholder="0" min="0"/></div>
          <div class="form-group"><label>Win Ratio (%)</label><input type="number" id="stats-ratio" placeholder="0" min="0" max="100" step="0.1"/></div>
        </div>
        <button class="btn btn-green" onclick="savePlayerStats()">SAVE STATS</button>
        <div id="stats-msg" class="msg"></div>
      </div>
    </div>
  </div>

  <div class="card">
    <div class="card-header"><h2>ALL PLAYER STATS</h2></div>
    <div class="card-body">
      <div class="table-wrap">
        <table id="stats-table">
          <thead>
            <tr><th>#</th><th>Name</th><th>Cat</th><th>Rank</th><th>Matches</th><th>Wins</th><th>Draws</th><th>MOTM</th><th>Win %</th></tr>
          </thead>
          <tbody id="stats-tbody"></tbody>
        </table>
      </div>
    </div>
  </div>
</div>

<!-- VIEWER HOME -->
<div id="viewer" class="section active">
  <div class="viewer-header">
    <div class="viewer-header-inner">
      <div id="viewer-logo-placeholder"></div>
      <img id="viewer-logo" src="" alt="logo"/>
      <div>
        <div id="viewer-tournament-name">LLFC AUCTION</div>
        <div id="viewer-tournament-sub">LIVE PLAYER AUCTION</div>
      </div>
    </div>
  </div>

  <div style="display:grid; grid-template-columns:repeat(auto-fit, minmax(200px,1fr)); gap:16px; margin-bottom:32px;">
    <div class="card" style="margin-bottom:0; border-color:var(--gold);"><div class="card-body" style="text-align:center"><div style="font-size:36px; color:var(--red); font-family:var(--font-display);" id="stat-total">0</div><div style="font-size:11px; color:#999; text-transform:uppercase; letter-spacing:1.5px; margin-top:8px; font-weight:700;">TOTAL PLAYERS</div></div></div>
    <div class="card" style="margin-bottom:0; border-color:var(--gold);"><div class="card-body" style="text-align:center"><div style="font-size:36px; color:var(--red); font-family:var(--font-display);" id="stat-sold">0</div><div style="font-size:11px; color:#999; text-transform:uppercase; letter-spacing:1.5px; margin-top:8px; font-weight:700;">SOLD</div></div></div>
    <div class="card" style="margin-bottom:0; border-color:var(--gold);"><div class="card-body" style="text-align:center"><div style="font-size:36px; color:var(--red); font-family:var(--font-display);" id="stat-available">0</div><div style="font-size:11px; color:#999; text-transform:uppercase; letter-spacing:1.5px; margin-top:8px; font-weight:700;">AVAILABLE</div></div></div>
    <div class="card" style="margin-bottom:0; border-color:var(--gold);"><div class="card-body" style="text-align:center"><div style="font-size:36px; color:var(--red); font-family:var(--font-display);" id="stat-teams">0</div><div style="font-size:11px; color:#999; text-transform:uppercase; letter-spacing:1.5px; margin-top:8px; font-weight:700;">TEAMS</div></div></div>
  </div>

  <div class="auction-timer">
    <h3>LIVE AUCTION TIMER</h3>
    <div class="timer-display" id="timer-display">00:00:00</div>
    <div class="timer-controls">
      <button class="timer-btn" onclick="startTimer()">START</button>
      <button class="timer-btn" onclick="pauseTimer()">PAUSE</button>
      <button class="timer-btn" onclick="resetTimer()">RESET</button>
      <input type="number" id="timer-input" placeholder="Minutes" style="width:100px; padding:8px; border:2px solid var(--gold); border-radius:8px;" min="1" max="999" value="30"/>
    </div>
  </div>

  <div class="search-filter-section">
    <h3>SEARCH & FILTER PLAYERS</h3>
    <div class="search-filter-row">
      <input type="text" id="search-player" placeholder="Search by player name..." onkeyup="filterPlayers()"/>
      <select id="filter-category" onchange="filterPlayers()">
        <option value="">All Categories</option>
        <option value="youth">Youth</option>
        <option value="local">Local</option>
        <option value="invited">Invited</option>
      </select>
      <select id="filter-status" onchange="filterPlayers()">
        <option value="">All Status</option>
        <option value="available">Available</option>
        <option value="sold">Sold</option>
      </select>
      <select id="filter-team" onchange="filterPlayers()">
        <option value="">All Teams</option>
      </select>
    </div>
  </div>

  <div class="section-title">PLAYER POOL</div>
  <div class="section-divider"></div>

  <div class="cat-tabs">
    <button class="cat-tab active" onclick="switchViewTab('youth',this)">YOUTH</button>
    <button class="cat-tab" onclick="switchViewTab('local',this)">LOCAL</button>
    <button class="cat-tab" onclick="switchViewTab('invited',this)">INVITED</button>
  </div>

  <div id="view-youth" class="cat-panel active">
    <div id="youth-card-view" class="player-card-container"></div>
  </div>

  <div id="view-local" class="cat-panel">
    <div id="local-card-view" class="player-card-container"></div>
  </div>

  <div id="view-invited" class="cat-panel">
    <div id="invited-card-view" class="player-card-container"></div>
  </div>
</div>

<!-- PROFILE MODAL -->
<div id="profile-modal" class="modal-bg">
  <div class="modal">
    <span class="modal-close" onclick="closeModal()">✕</span>
    <div id="modal-content"></div>
  </div>
</div>

<script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>

<script>
window._teams={};
window._youth={};
window._local={};
window._invited={};
window._stats={};
window._bidRules={};
window._tournamentData={};
window._db = null;
window._ref = null;
window._set = null;
window._get = null;
window._push = null;
window._remove = null;
window._update = null;
window._onValue = null;
window._ADMIN_PASSWORD = "Fardous";
window._MODERATOR_PASSWORD = "Fardous";
window.currentViewTab = 'youth';
window.timerInterval = null;
window.timerSeconds = 0;

async function initializeFirebase() {
  try {
    const { initializeApp } = await import("https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js");
    const { getDatabase, ref, set, get, push, remove, update, onValue } = await import("https://www.gstatic.com/firebasejs/11.0.1/firebase-database.js");

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
    const db = getDatabase(app);

    window._db = db;
    window._ref = ref;
    window._set = set;
    window._get = get;
    window._push = push;
    window._remove = remove;
    window._update = update;
    window._onValue = onValue;

    console.log("✅ Firebase initialized");

    onValue(ref(db,'tournament'), snap => {
      try {
        const d = snap.val() || {};
        const titleEl = document.getElementById('nav-title');
        const subEl = document.getElementById('nav-subtitle');
        const viewerNameEl = document.getElementById('viewer-tournament-name');
        const viewerSubEl = document.getElementById('viewer-tournament-sub');

        if(titleEl) titleEl.textContent = d.name || 'LLFC AUCTION';
        if(subEl) subEl.textContent = d.sub || 'Live Player Auction';
        if(viewerNameEl) viewerNameEl.textContent = d.name || 'LLFC AUCTION';
        if(viewerSubEl) viewerSubEl.textContent = d.sub || 'LIVE PLAYER AUCTION';

        if(d.logo) {
          const navImg = document.getElementById('nav-logo-img');
          const viewerImg = document.getElementById('viewer-logo');
          if(navImg) { navImg.src = d.logo; navImg.style.display = 'block'; }
          if(viewerImg) { viewerImg.src = d.logo; viewerImg.style.display = 'block'; }
          const navPlaceholder = document.getElementById('nav-logo-placeholder');
          const viewerPlaceholder = document.getElementById('viewer-logo-placeholder');
          if(navPlaceholder) navPlaceholder.style.display = 'none';
          if(viewerPlaceholder) viewerPlaceholder.style.display = 'none';
        }
        window._tournamentData = d;
      } catch(e) {
        console.error("Error updating tournament data:", e);
      }
    });

    onValue(ref(db,'bidRules'), snap => {
      window._bidRules = snap.val() || {};
    });

    onValue(ref(db,'teams'), snap => {
      try {
        window._teams = snap.val() || {};
        renderTeamTable();
        renderTeamProfiles();
        updateStats();
        populateSelects();
        populateTeamFilter();
      } catch(e) {
        console.error("Error processing teams:", e);
      }
    });

    ['youth','local','invited'].forEach(cat => {
      onValue(ref(db,`players/${cat}`), snap => {
        try {
          window[`_${cat}`] = snap.val() || {};
          renderPlayerTable(cat);
          renderPlayerCards(cat);
          renderSoldList();
          updateStats();
          populateSelects();
          renderStatsTable();
        } catch(e) {
          console.error(`Error processing ${cat} players:`, e);
        }
      });
    });

    onValue(ref(db,'stats'), snap => {
      try {
        window._stats = snap.val() || {};
        renderPlayerCards('youth');
        renderPlayerCards('local');
        renderPlayerCards('invited');
        renderStatsTable();
      } catch(e) {
        console.error("Error processing stats:", e);
      }
    });

  } catch(e) {
    console.error("Firebase initialization error:", e);
    alert("Database connection failed. Check internet.");
  }
}

document.addEventListener('DOMContentLoaded', initializeFirebase);

function showSection(id){
  try {
    document.querySelectorAll('.section').forEach(s=> { if(s) s.classList.remove('active'); });
    document.querySelectorAll('.nav-tab').forEach(t=> { if(t) t.classList.remove('active'); });
    
    const targetSection = document.getElementById(id);
    if(targetSection) targetSection.classList.add('active');
    if(event && event.currentTarget) event.currentTarget.classList.add('active');
    
    if(id==='admin-gate' && sessionStorage.getItem('llfc_admin')){
      document.getElementById('admin-gate')?.classList.remove('active');
      document.getElementById('admin')?.classList.add('active');
    }
    if(id==='moderator-gate' && sessionStorage.getItem('llfc_moderator')){
      document.getElementById('moderator-gate')?.classList.remove('active');
      document.getElementById('moderator')?.classList.add('active');
    }
  } catch(e) {
    console.error("Error in showSection:", e);
  }
}

function checkAdminPass(){
  const passEl = document.getElementById('admin-pass');
  const msgEl = document.getElementById('login-msg');
  if(!passEl || !msgEl) return;

  if(passEl.value === window._ADMIN_PASSWORD){
    sessionStorage.setItem('llfc_admin','1');
    document.getElementById('admin-gate')?.classList.remove('active');
    document.getElementById('admin')?.classList.add('active');
    showMsg(msgEl,'Access granted!','success');
  } else {
    showMsg(msgEl,'Incorrect password','error');
  }
  passEl.value = '';
}

function checkModeratorPass(){
  const passEl = document.getElementById('moderator-pass');
  const msgEl = document.getElementById('moderator-login-msg');
  if(!passEl || !msgEl) return;

  if(passEl.value === window._MODERATOR_PASSWORD){
    sessionStorage.setItem('llfc_moderator','1');
    document.getElementById('moderator-gate')?.classList.remove('active');
    document.getElementById('moderator')?.classList.add('active');
    showMsg(msgEl,'Access granted!','success');
  } else {
    showMsg(msgEl,'Incorrect password','error');
  }
  passEl.value = '';
}

document.addEventListener('DOMContentLoaded', () => {
  const adminPass = document.getElementById('admin-pass');
  const modPass = document.getElementById('moderator-pass');
  if(adminPass) adminPass.addEventListener('keydown', e => { if(e.key==='Enter') checkAdminPass(); });
  if(modPass) modPass.addEventListener('keydown', e => { if(e.key==='Enter') checkModeratorPass(); });

  const tourLogo = document.getElementById('tournament-logo');
  if(tourLogo) tourLogo.addEventListener('change', () => {
    const preview = document.getElementById('tournament-logo-preview');
    if(preview && tourLogo.value) {
      preview.src = tourLogo.value;
      preview.style.display = 'block';
    }
  });
});

function saveTournament(){
  if(!window._db) { alert('Database not initialized'); return; }
  const nameEl = document.getElementById('tournament-name');
  const subEl = document.getElementById('tournament-sub');
  const logoEl = document.getElementById('tournament-logo');
  const msgEl = document.getElementById('tournament-msg');

  const name = nameEl.value.trim();
  const sub = subEl.value.trim() || 'Live Player Auction';
  const logo = logoEl.value.trim() || '';
  
  if(!name){ showMsg(msgEl,'Tournament name required','error'); return; }
  
  window._set(window._ref(window._db,'tournament'), { name, sub, logo })
    .then(()=>showMsg(msgEl,'Settings saved!','success'))
    .catch(e=>showMsg(msgEl,e.message,'error'));
}

function saveBidRules(){
  if(!window._db) { alert('Database not initialized'); return; }
  
  const data = {
    youth: {
      min: parseInt(document.getElementById('youth-min').value) || 0,
      max: parseInt(document.getElementById('youth-max').value) || 0
    },
    local: {
      min: parseInt(document.getElementById('local-min').value) || 0,
      max: parseInt(document.getElementById('local-max').value) || 0
    },
    invited: {
      min: parseInt(document.getElementById('invited-min').value) || 0,
      max: parseInt(document.getElementById('invited-max').value) || 0
    }
  };

  const msgEl = document.getElementById('bid-rules-msg');
  window._set(window._ref(window._db,'bidRules'), data)
    .then(()=>showMsg(msgEl,'Bid rules saved!','success'))
    .catch(e=>showMsg(msgEl,e.message,'error'));
}

function addTeam(){
  if(!window._db) { alert('Database not initialized'); return; }
  const nameEl = document.getElementById('team-name');
  const capEl = document.getElementById('team-cap');
  const budgetEl = document.getElementById('team-budget');
  const logoEl = document.getElementById('team-logo');
  const msgEl = document.getElementById('team-msg');

  const name = nameEl.value.trim();
  const cap = capEl.value.trim();
  const budget = parseFloat(budgetEl.value)||0;
  const logo = logoEl.value.trim() || '';
  
  if(!name||!cap||!budget){ showMsg(msgEl,'Fill all fields','error'); return; }
  
  const data = { name, cap, budget, logo, remaining: budget, spent: 0 };
  window._push(window._ref(window._db,'teams'), data)
    .then(()=>{
      showMsg(msgEl,'Team added!','success');
      nameEl.value=''; capEl.value=''; budgetEl.value=''; logoEl.value='';
    })
    .catch(e=>showMsg(msgEl,e.message,'error'));
}

function renderTeamTable(){
  const teams = window._teams||{};
  const tbody = document.getElementById('team-tbody');
  if(!tbody) return;
  
  if(!Object.keys(teams).length){ 
    tbody.innerHTML=`<tr><td colspan="7" class="empty-state">No teams yet</td></tr>`; 
    return; 
  }
  
  tbody.innerHTML = Object.entries(teams).map(([id,t],i)=>{
    const spent = (t.spent || 0);
    const remaining = (t.remaining || t.budget || 0);
    return `
    <tr>
      <td>${i+1}</td>
      <td><strong>${t.name || '-'}</strong></td>
      <td>${t.cap || '-'}</td>
      <td>${(t.budget||0).toLocaleString()}</td>
      <td>${spent.toLocaleString()}</td>
      <td style="color:${remaining<(t.budget||0)*0.2?'#c62828':'#2e7d32'};font-weight:700">${remaining.toLocaleString()}</td>
      <td><button class="btn btn-danger btn-sm" onclick="deleteTeam('${id}')">DELETE</button></td>
    </tr>`;
  }).join('');
}

function renderTeamProfiles(){
  const teams = window._teams||{};
  const grid = document.getElementById('team-profiles-grid');
  if(!grid) return;
  
  if(!Object.keys(teams).length){
    grid.innerHTML = `<div class="empty-state" style="grid-column:1/-1">No teams registered yet</div>`;
    return;
  }
  
  grid.innerHTML = Object.entries(teams).map(([id,t])=>{
    const spent = (t.spent || 0);
    const remaining = (t.remaining || t.budget || 0);
    const usedPercent = Math.round((spent/(t.budget||1))*100);
    return `
    <div class="team-card">
      <div class="team-card-header">
        ${t.logo ? `<img src="${t.logo}" class="team-logo" alt="logo"/>` : ''}
        <h3>${t.name || 'Team'}</h3>
      </div>
      <div class="team-card-body">
        <div class="team-stat">
          <div class="team-stat-label">Captain</div>
          <div style="color:var(--dark);font-weight:700;">${t.cap || '-'}</div>
        </div>
        <div class="team-stat">
          <div class="team-stat-label">Total Budget</div>
          <div class="team-stat-value">${(t.budget||0).toLocaleString()} Coins</div>
        </div>
        <div class="team-stat">
          <div class="team-stat-label">Spent</div>
          <div style="color:#dc2626;font-weight:700;font-size:18px;">${spent.toLocaleString()} Coins</div>
        </div>
        <div class="team-stat">
          <div class="team-stat-label">Remaining</div>
          <div class="team-stat-value" style="color:${remaining<(t.budget||0)*0.2?'#dc2626':'#2e7d32'}">${remaining.toLocaleString()} Coins</div>
        </div>
        <div style="margin-top:16px; background:#f5f5f5; border-radius:8px; overflow:hidden;">
          <div style="height:6px; background:linear-gradient(90deg,var(--red),var(--red-deep)); width:${usedPercent}%;"></div>
        </div>
        <div style="text-align:center; font-size:11px; color:#999; margin-top:8px; font-weight:700;">${usedPercent}% USED</div>
      </div>
    </div>`;
  }).join('');
}

function deleteTeam(id){
  if(!confirm('Delete this team?')) return;
  window._remove(window._ref(window._db,`teams/${id}`))
    .catch(e => console.error("Error deleting team:", e));
}

function addPlayer(cat){
  if(!window._db) { alert('Database not initialized'); return; }
  const nameEl = document.getElementById(`${cat}-name`);
  const priceEl = document.getElementById(`${cat}-price`);
  const clubEl = cat==='invited' ? document.getElementById('invited-club') : null;
  const photoEl = document.getElementById(`${cat}-photo`);
  const msgEl = document.getElementById(`${cat}-msg`);

  const name = nameEl.value.trim();
  const price = parseFloat(priceEl.value)||0;
  const club = clubEl ? clubEl.value.trim() : (cat === 'invited' ? 'INVITED' : 'LLFC');

  if(!name||!price){ showMsg(msgEl,'Name & price required','error'); return; }
  if(cat==='invited'&&!clubEl.value.trim()){ showMsg(msgEl,'Club name required','error'); return; }

  const data = { name, basePrice: price, status:'available', cat, club };

  if(photoEl.files[0]){
    const reader = new FileReader();
    reader.onload = e => {
      data.photo = e.target.result;
      window._push(window._ref(window._db,`players/${cat}`), data)
        .then(()=>{ showMsg(msgEl,'Player added!','success'); clearPlayerForm(cat); })
        .catch(e=>showMsg(msgEl,e.message,'error'));
    };
    reader.readAsDataURL(photoEl.files[0]);
  } else {
    window._push(window._ref(window._db,`players/${cat}`), data)
      .then(()=>{ showMsg(msgEl,'Player added!','success'); clearPlayerForm(cat); })
      .catch(e=>showMsg(msgEl,e.message,'error'));
  }
}

function clearPlayerForm(cat){
  document.getElementById(`${cat}-name`).value='';
  document.getElementById(`${cat}-price`).value='';
  document.getElementById(`${cat}-photo`).value='';
  if(cat==='invited') document.getElementById('invited-club').value='';
}

function renderPlayerTable(cat){
  const players = window[`_${cat}`]||{};
  const tbody = document.getElementById(`${cat}-tbody`);
  if(!tbody) return;
  
  if(!Object.keys(players).length){
    tbody.innerHTML=`<tr><td colspan="7" class="empty-state">No players</td></tr>`;
    return;
  }
  
  tbody.innerHTML = Object.entries(players).map(([id,p],i)=>`
    <tr>
      <td>${i+1}</td>
      <td><strong>${p.name || '-'}</strong></td>
      <td>${(p.basePrice||0).toLocaleString()}</td>
      <td><span class="badge badge-${p.status||'available'}">${p.status||'available'}</span></td>
      <td>${p.soldTo||'-'}</td>
      <td>${p.soldPrice?p.soldPrice.toLocaleString():'-'}</td>
      <td><button class="delete-btn" onclick="deletePlayer('${cat}','${id}')">DELETE</button></td>
    </tr>`).join('');
}

function deletePlayer(cat,id){
  if(!confirm('Delete this player?')) return;
  window._remove(window._ref(window._db,`players/${cat}/${id}`))
    .catch(e => console.error("Error:", e));
}

function renderPlayerCards(cat){
  const players = window[`_${cat}`]||{};
  const container = document.getElementById(`${cat}-card-view`);
  if(!container) return;
  
  if(!Object.keys(players).length){
    container.innerHTML = `<div class="empty-state" style="grid-column:1/-1; padding:60px;">No ${cat} players</div>`;
    return;
  }
  
  container.innerHTML = Object.entries(players).map(([id,p],i)=>{
    const stats = (window._stats||{})[`${cat}::${id}`]||{};
    const tourney = window._tournamentData||{};
    const serialNum = i+1;
    
    return `
    <div class="player-card">
      <div class="card-tournament-header">
        ${tourney.logo ? `<img src="${tourney.logo}" class="card-tournament-logo" alt="logo"/>` : `<div class="card-tournament-logo-placeholder">🏆</div>`}
        <div class="card-tournament-name">${tourney.name || 'LLFC'}</div>
      </div>
      
      <div class="card-category-badge">
        <div class="card-cat-big-badge">${cat.toUpperCase()}</div>
      </div>
      
      <div class="card-serial-big">#${String(serialNum).padStart(2,'0')}</div>
      
      <div class="card-player-section">
        <div class="card-player-label">PLAYER NAME</div>
        ${p.photo ? `<img src="${p.photo}" class="card-player-photo" alt="${p.name}"/>` : ''}
        <div class="card-player-name">${p.name || 'UNKNOWN'}</div>
        <div class="card-base-price-small">Base Price: ${(p.basePrice||0).toLocaleString()} Coins</div>
      </div>

      <div class="card-club-section">
        <div class="card-club-label">CLUB NAME</div>
        <div class="card-club-name">${p.club || 'LLFC'}</div>
      </div>

      <div class="card-base-price-section">
        <div class="card-base-price">BASE PRICE: ${(p.basePrice||0).toLocaleString()} COINS</div>
      </div>

      <div class="card-description-box">
        <div class="card-description-text">${cat === 'invited' ? 'A highly skilled player known for consistent performances. His dedication and impact make him stand out among the best in the community.' : 'One of the most talented players of LLFC. A rising star who continues to impress with exceptional skill and great potential.'}</div>
      </div>
      
      <div class="card-stats">
        <div class="stats-label">COBEG STATS</div>
        <div class="stats-grid">
          <div class="stat-badge">
            <div class="stat-value">${stats.rank || '-'}</div>
            <div class="stat-key">RANK</div>
          </div>
          <div class="stat-badge">
            <div class="stat-value">${stats.matches || 0}</div>
            <div class="stat-key">MATCHES</div>
          </div>
          <div class="stat-badge">
            <div class="stat-value">${stats.wins || 0}</div>
            <div class="stat-key">WINS</div>
          </div>
          <div class="stat-badge">
            <div class="stat-value">${stats.draws || 0}</div>
            <div class="stat-key">DRAWS</div>
          </div>
          <div class="stat-badge">
            <div class="stat-value">${stats.motm || 0}</div>
            <div class="stat-key">MOTM</div>
          </div>
          <div class="stat-badge">
            <div class="stat-value">${stats.ratio || 0}%</div>
            <div class="stat-key">WIN %</div>
          </div>
        </div>
      </div>

      <div class="card-action-buttons">
        <button class="btn btn-gold card-action-btn" onclick="showFullProfile('${cat}','${id}',${serialNum})">VIEW</button>
        <button class="btn btn-red card-action-btn" onclick="downloadCardImage('${cat}','${id}',${serialNum})">DOWNLOAD</button>
      </div>
    </div>`;
  }).join('');
}

function downloadCardImage(cat, id, serial){
  const player = (window[`_${cat}`]||{})[id];
  const stats = (window._stats||{})[`${cat}::${id}`]||{};
  const tourney = window._tournamentData||{};
  
  const canvas = document.createElement('canvas');
  canvas.width = 1080;
  canvas.height = 1440;
  const ctx = canvas.getContext('2d');
  
  // BACKGROUND GRADIENT
  const bgGrad = ctx.createLinearGradient(0,0,1080,1440);
  bgGrad.addColorStop(0, '#0a0a0a');
  bgGrad.addColorStop(0.5, '#1a0a0a');
  bgGrad.addColorStop(1, '#0a0a0a');
  ctx.fillStyle = bgGrad;
  ctx.fillRect(0,0,1080,1440);
  
  // GOLD BORDERS
  ctx.fillStyle = '#D4AF37';
  ctx.fillRect(0,0,1080,16);
  ctx.fillRect(0,1424,1080,16);
  
  // OUTER FRAME
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 4;
  ctx.strokeRect(20,20,1040,1400);
  
  // TOURNAMENT HEADER
  const headerGrad = ctx.createLinearGradient(0,40,0,160);
  headerGrad.addColorStop(0, '#D0021B');
  headerGrad.addColorStop(1, '#8B0000');
  ctx.fillStyle = headerGrad;
  ctx.fillRect(0,40,1080,140);
  
  ctx.fillStyle = '#ffffff';
  ctx.font = 'bold 54px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText(tourney.name || 'LLFC AUCTION', 540, 100);
  
  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 28px Oswald';
  ctx.fillText('LIVE PLAYER AUCTION', 540, 150);
  
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 3;
  ctx.beginPath();
  ctx.moveTo(0, 190);
  ctx.lineTo(1080, 190);
  ctx.stroke();
  
  // CATEGORY BADGE
  const badgeGrad = ctx.createLinearGradient(240,220,240,320);
  badgeGrad.addColorStop(0, '#D0021B');
  badgeGrad.addColorStop(1, '#8B0000');
  ctx.fillStyle = badgeGrad;
  ctx.fillRect(240,220,600,100);
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 3;
  ctx.strokeRect(240,220,600,100);
  
  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 72px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText(cat.toUpperCase(), 540, 300);
  
  // SERIAL NUMBER
  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 140px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText(`#${String(serial).padStart(2,'0')}`, 540, 470);
  
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 3;
  ctx.beginPath();
  ctx.moveTo(100, 500);
  ctx.lineTo(980, 500);
  ctx.stroke();
  
  // PLAYER NAME
  ctx.fillStyle = '#ffffff';
  ctx.font = 'bold 64px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText((player.name||'UNKNOWN').toUpperCase(), 540, 590);
  
  // CLUB NAME
  ctx.fillStyle = 'rgba(255,255,255,0.05)';
  ctx.fillRect(120,630,840,100);
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 2;
  ctx.strokeRect(120,630,840,100);
  
  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 24px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText('CLUB NAME', 540, 665);
  ctx.font = 'bold 40px Oswald';
  ctx.fillStyle = '#ffffff';
  ctx.fillText(player.club || 'LLFC', 540, 715);
  
  // BASE PRICE
  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 32px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText(`BASE PRICE: ${(player.basePrice||0).toLocaleString()} COINS`, 540, 800);
  
  // DESCRIPTION
  ctx.fillStyle = '#000000';
  ctx.fillRect(120,840,840,140);
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 3;
  ctx.strokeRect(120,840,840,140);
  
  ctx.fillStyle = '#ffffff';
  ctx.font = 'bold 18px Oswald';
  ctx.textAlign = 'center';
  
  const description = cat === 'invited' ? 'A highly skilled player known for consistent performances. His dedication and impact make him stand out among the best in the community.' : 'One of the most talented players of LLFC. A rising star who continues to impress with exceptional skill and great potential.';
  
  const maxWidth = 800;
  const words = description.split(' ');
  let line = '';
  let y = 880;
  const lineHeight = 32;
  
  words.forEach(word => {
    const testLine = line + (line ? ' ' : '') + word;
    const metrics = ctx.measureText(testLine);
    if(metrics.width > maxWidth && line) {
      ctx.fillText(line, 540, y);
      line = word;
      y += lineHeight;
    } else {
      line = testLine;
    }
  });
  if(line) ctx.fillText(line, 540, y);
  
  // STATS SECTION - 3x2 GRID (CENTERED)
  const statsBoxH = 380;
  const statsBoxTop = 970;
  ctx.fillStyle = 'rgba(255,255,255,0.05)';
  ctx.fillRect(80,statsBoxTop,920,statsBoxH);
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 3;
  ctx.strokeRect(80,statsBoxTop,920,statsBoxH);
  
  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 32px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText('COBEG STATS', 540, 1015);
  
  // 6 stat badges - 3x2 grid - CENTERED
  const statBadges = [
    { label: 'RANK', value: stats.rank || '-' },
    { label: 'MATCHES', value: stats.matches || 0 },
    { label: 'WINS', value: stats.wins || 0 },
    { label: 'DRAWS', value: stats.draws || 0 },
    { label: 'MOTM', value: stats.motm || 0 },
    { label: 'WIN %', value: (stats.ratio || 0) + '%' }
  ];
  
  const badgeW = 180;
  const badgeH = 110;
  const gap = 25;
  const totalWidth = (3 * badgeW) + (2 * gap);
  const startX = (1080 - totalWidth) / 2;
  const startY = 1055;
  
  statBadges.forEach((stat, idx) => {
    const col = idx % 3;
    const row = Math.floor(idx / 3);
    const x = startX + (col * (badgeW + gap));
    const y = startY + (row * (badgeH + gap));
    
    const badgeGr = ctx.createLinearGradient(x, y, x, y + badgeH);
    badgeGr.addColorStop(0, '#D0021B');
    badgeGr.addColorStop(1, '#8B0000');
    ctx.fillStyle = badgeGr;
    ctx.fillRect(x, y, badgeW, badgeH);
    
    ctx.strokeStyle = '#D4AF37';
    ctx.lineWidth = 2;
    ctx.strokeRect(x, y, badgeW, badgeH);
    
    ctx.fillStyle = '#D4AF37';
    ctx.font = 'bold 36px Oswald';
    ctx.textAlign = 'center';
    ctx.fillText(stat.value, x + badgeW/2, y + 60);
    
    ctx.font = 'bold 14px Oswald';
    ctx.fillStyle = 'rgba(255,255,255,0.95)';
    ctx.fillText(stat.label, x + badgeW/2, y + 98);
  });
  
  // FOOTER
  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 24px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText('LLFC AUCTION 2026', 540, 1420);
  
  // DOWNLOAD
  canvas.toBlob(blob=>{
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `${player.name}_${cat}_${String(serial).padStart(2,'0')}.jpg`;
    a.click();
    URL.revokeObjectURL(url);
  }, 'image/jpeg', 0.95);
}

function markSoldModal(cat, id){
  if(!sessionStorage.getItem('llfc_admin')){
    alert('Only admin can mark players as sold!');
    return;
  }

  const player = (window[`_${cat}`]||{})[id];
  if(!player) return;
  
  const teams = window._teams||{};
  const teamOptions = Object.entries(teams).map(([tid,t])=>`<option value="${tid}">${t.name}</option>`).join('');
  
  const modal = document.getElementById('profile-modal');
  const modalContent = document.getElementById('modal-content');
  
  const html = `
    <div style="text-align:center; padding:28px;">
      <h3 style="font-family:var(--font-display); color:var(--red); font-size:24px; letter-spacing:2px; margin-bottom:20px;">
        MARK AS SOLD
      </h3>
    </div>
    <div style="padding:28px;">
      <div class="form-row">
        <div class="form-group">
          <label>Select Team</label>
          <select id="sold-team-select">
            <option value="">-- Choose Team --</option>
            ${teamOptions}
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Sold Price (Coins)</label>
          <input type="number" id="sold-price-input" placeholder="e.g. 5000" min="0"/>
        </div>
      </div>
      <div style="display:flex; gap:12px; margin-top:24px;">
        <button class="btn btn-red" style="flex:1;" onclick="confirmSold('${cat}','${id}')">CONFIRM SOLD</button>
        <button class="btn btn-danger" style="flex:1;" onclick="closeModal()">CANCEL</button>
      </div>
    </div>
  `;
  
  if(modalContent && modal){
    modalContent.innerHTML = html;
    modal.classList.add('open');
  }
}

function confirmSold(cat, id){
  const teamSelect = document.getElementById('sold-team-select');
  const priceInput = document.getElementById('sold-price-input');
  
  const teamId = teamSelect.value;
  const soldPrice = parseFloat(priceInput.value);
  
  if(!teamId || !soldPrice) { alert('Select team and enter price'); return; }
  
  const team = (window._teams||{})[teamId];
  if(!team) { alert('Team not found'); return; }
  
  const updates = {};
  updates[`players/${cat}/${id}/status`] = 'sold';
  updates[`players/${cat}/${id}/soldTo`] = team.name;
  updates[`players/${cat}/${id}/soldPrice`] = soldPrice;
  updates[`players/${cat}/${id}/soldTeamId`] = teamId;
  updates[`teams/${teamId}/spent`] = (team.spent || 0) + soldPrice;
  updates[`teams/${teamId}/remaining`] = (team.remaining || team.budget || 0) - soldPrice;
  
  window._update(window._ref(window._db), updates)
    .then(()=>{ closeModal(); alert('Player marked as sold!'); })
    .catch(e=>alert('Error: '+e.message));
}

function showFullProfile(cat, id, serial){
  const player = (window[`_${cat}`]||{})[id];
  if(!player) return;
  
  const stats = (window._stats||{})[`${cat}::${id}`]||{};
  const tourney = window._tournamentData||{};
  const isAdmin = sessionStorage.getItem('llfc_admin');
  
  const html = `
    <div style="text-align:center; padding:28px;">
      <h3 style="font-family:var(--font-display); color:var(--red); font-size:28px; letter-spacing:2px; margin-bottom:8px;">
        PLAYER PROFILE
      </h3>
      <div style="margin-bottom:24px; font-size:12px; color:#999;">
        #${String(serial).padStart(2,'0')} - ${tourney.name||'LLFC'} - ${tourney.sub||'Auction 2026'}
      </div>
    </div>
    <div style="text-align:center; padding:28px;">
      ${player.photo?`<img src="${player.photo}" style="width:180px; height:180px; border-radius:50%; object-fit:cover; border:4px solid var(--gold); margin-bottom:24px;"/>`:''}
      <div style="font-family:var(--font-display); font-size:36px; color:var(--red); letter-spacing:2px; margin-bottom:12px;">${player.name||'Unknown'}</div>
      <div style="font-size:14px; color:#666; margin-bottom:20px;">
        <span class="badge" style="background:var(--gold-light);color:var(--red);margin:4px;font-weight:700;">${cat.toUpperCase()}</span>
        ${player.club?`<span class="badge" style="background:var(--grey);color:var(--dark);margin:4px;">${player.club}</span>`:''}
      </div>
      <div style="font-family:var(--font-display); font-size:32px; color:var(--red); margin-bottom:28px;">${(player.basePrice||0).toLocaleString()} Coins</div>
      
      ${player.status==='sold'?`
        <div style="background:linear-gradient(135deg,var(--red),var(--red-deep)); padding:20px; border-radius:12px; margin-bottom:24px; color:#fff; border:2px solid var(--gold);">
          <div style="font-size:12px; color:rgba(255,255,255,0.9); text-transform:uppercase; letter-spacing:1px; margin-bottom:8px; font-weight:700;">Sold Information</div>
          <div style="font-family:var(--font-head); font-size:20px; margin-bottom:8px; font-weight:700;">${player.soldTo||'-'}</div>
          <div style="font-family:var(--font-display); font-size:28px; color:var(--gold);">${(player.soldPrice||0).toLocaleString()} Coins</div>
        </div>
      `:'<div style="padding:14px; background:var(--gold-light); border-radius:12px; margin-bottom:24px; color:var(--red); font-weight:700; font-size:13px; letter-spacing:1px;">AVAILABLE FOR AUCTION</div>'}
      
      ${Object.keys(stats).length>0?`
        <div style="background:#f8f7f7; padding:24px; border-radius:12px; margin-bottom:24px; border:2px solid var(--gold);">
          <div style="font-family:var(--font-head); font-size:16px; color:var(--dark); letter-spacing:1px; margin-bottom:16px; text-transform:uppercase; font-weight:700;">COBEG STATS</div>
          <div style="display:grid; grid-template-columns:repeat(3,1fr); gap:14px;">
            ${stats.rank?`<div style="background:#fff; padding:14px; border-radius:8px; border:1px solid var(--gold);"><div style="font-family:var(--font-display); font-size:24px; color:var(--red);">${stats.rank}</div><div style="font-size:10px; color:#999; text-transform:uppercase; letter-spacing:1px; margin-top:6px; font-weight:700;">RANK</div></div>`:''}
            <div style="background:#fff; padding:14px; border-radius:8px; border:1px solid var(--gold);"><div style="font-family:var(--font-display); font-size:24px; color:var(--red);">${stats.matches||0}</div><div style="font-size:10px; color:#999; text-transform:uppercase; letter-spacing:1px; margin-top:6px; font-weight:700;">MATCHES</div></div>
            <div style="background:#fff; padding:14px; border-radius:8px; border:1px solid var(--gold);"><div style="font-family:var(--font-display); font-size:24px; color:var(--red);">${stats.wins||0}</div><div style="font-size:10px; color:#999; text-transform:uppercase; letter-spacing:1px; margin-top:6px; font-weight:700;">WINS</div></div>
            <div style="background:#fff; padding:14px; border-radius:8px; border:1px solid var(--gold);"><div style="font-family:var(--font-display); font-size:24px; color:var(--red);">${stats.draws||0}</div><div style="font-size:10px; color:#999; text-transform:uppercase; letter-spacing:1px; margin-top:6px; font-weight:700;">DRAWS</div></div>
            <div style="background:#fff; padding:14px; border-radius:8px; border:1px solid var(--gold);"><div style="font-family:var(--font-display); font-size:24px; color:var(--red);">${stats.motm||0}</div><div style="font-size:10px; color:#999; text-transform:uppercase; letter-spacing:1px; margin-top:6px; font-weight:700;">MOTM</div></div>
            <div style="background:#fff; padding:14px; border-radius:8px; border:1px solid var(--gold);"><div style="font-family:var(--font-display); font-size:24px; color:var(--red);">${stats.ratio||0}%</div><div style="font-size:10px; color:#999; text-transform:uppercase; letter-spacing:1px; margin-top:6px; font-weight:700;">WIN %</div></div>
          </div>
        </div>
      `:''}
      
      <div style="display: flex; gap: 12px; flex-wrap: wrap; justify-content: center;">
        <button class="btn btn-gold" onclick="downloadCardImage('${cat}','${id}', ${serial})">📥 DOWNLOAD JPG</button>
        ${isAdmin && player.status !== 'sold' ? `<button class="btn btn-green" onclick="markSoldModal('${cat}','${id}')">✓ MARK SOLD</button>` : ''}
      </div>
    </div>
  `;
  
  const modalContent = document.getElementById('modal-content');
  const modalBg = document.getElementById('profile-modal');
  if(modalContent && modalBg) {
    modalContent.innerHTML = html;
    modalBg.classList.add('open');
  }
}

function closeModal(){
  document.getElementById('profile-modal')?.classList.remove('open');
}

function renderSoldList(){
  const container = document.getElementById('sold-list');
  if(!container) return;
  
  const allPlayers = [];
  let counter = 1;
  
  ['youth','local','invited'].forEach(cat=>{
    const players = window[`_${cat}`]||{};
    Object.entries(players).forEach(([id,p])=>{
      if(p.status==='sold'){
        allPlayers.push({
          compId: `${cat}::${id}`,
          counter: counter++,
          name: p.name||'-',
          cat: cat,
          basePrice: p.basePrice||0,
          soldTo: p.soldTo||'-',
          soldPrice: p.soldPrice||0,
          soldTeamId: p.soldTeamId||''
        });
      }
    });
  });

  if(!allPlayers.length){
    container.innerHTML=`<div class="empty-state">No sold players yet</div>`;
    return;
  }

  const teams = window._teams||{};
  const isAdmin = sessionStorage.getItem('llfc_admin');

  container.innerHTML = allPlayers.map(p=>`
    <div class="bid-item">
      <div class="bid-item-num">${p.counter}</div>
      <div class="bid-item-info">
        <h4>${p.name}</h4>
        <p style="color: var(--red); font-weight: 700;">${p.cat.toUpperCase()} • Base: ${p.basePrice.toLocaleString()}</p>
        <p><strong>${p.soldTo}</strong> • Sold: ${p.soldPrice.toLocaleString()} Coins</p>
      </div>
      <div class="bid-item-actions">
        ${isAdmin ? `
          <select style="padding:6px 8px; font-size:11px; border-radius:6px; border:1px solid #ddd; cursor:pointer;" onchange="updateSoldTeam('${p.compId}',this.value)">
            <option value="">Change Team</option>
            ${Object.entries(teams).map(([id,t])=>`<option value="${id}">${t.name}</option>`).join('')}
          </select>
          <input type="number" style="padding:6px 8px; font-size:11px; border-radius:6px; border:1px solid #ddd; width:80px;" placeholder="Price" value="${p.soldPrice}" onchange="updateSoldPrice('${p.compId}',this.value)"/>
          <button class="btn btn-danger btn-sm" onclick="unsellPlayer('${p.compId}')">UNDO</button>
        ` : ''}
      </div>
    </div>`).join('');
}

function updateSoldTeam(compId, teamId){
  if(!window._db || !window._update) return;
  const [cat, pid] = compId.split('::');
  const teams = window._teams||{};
  const team = teams[teamId];
  if(!team) return;
  
  const updates = {};
  updates[`players/${cat}/${pid}/soldTo`] = team.name;
  updates[`players/${cat}/${pid}/soldTeamId`] = teamId;
  window._update(window._ref(window._db), updates).catch(e=>console.error('Error:', e));
}

function updateSoldPrice(compId, price){
  if(!window._db || !window._update) return;
  const [cat, pid] = compId.split('::');
  const updates = {};
  updates[`players/${cat}/${pid}/soldPrice`] = parseFloat(price)||0;
  window._update(window._ref(window._db), updates).catch(e=>console.error('Error:', e));
}

function unsellPlayer(compId){
  if(!confirm('Unsell this player?')) return;
  if(!window._db || !window._update) return;
  
  const [cat, pid] = compId.split('::');
  const updates = {};
  updates[`players/${cat}/${pid}/status`]     = 'available';
  updates[`players/${cat}/${pid}/soldTo`]     = null;
  updates[`players/${cat}/${pid}/soldTeamId`] = null;
  updates[`players/${cat}/${pid}/soldPrice`]  = null;
  window._update(window._ref(window._db), updates).catch(e=>console.error('Error:', e));
}

function updateStats(){
  let total=0, sold=0;
  ['youth','local','invited'].forEach(cat=>{
    Object.values(window[`_${cat}`]||{}).forEach(p=>{ 
      total++; 
      if(p.status==='sold') sold++; 
    });
  });
  
  const totalEl = document.getElementById('stat-total');
  const soldEl = document.getElementById('stat-sold');
  const availEl = document.getElementById('stat-available');
  const teamsEl = document.getElementById('stat-teams');

  if(totalEl) totalEl.textContent = total;
  if(soldEl) soldEl.textContent = sold;
  if(availEl) availEl.textContent = total-sold;
  if(teamsEl) teamsEl.textContent = Object.keys(window._teams||{}).length;
}

function populateSelects(){
  const allPlayers = [];
  ['youth','local','invited'].forEach(cat=>{
    Object.entries(window[`_${cat}`]||{}).forEach(([id,p])=>{
      allPlayers.push({
        id: `${cat}::${id}`,
        label: `[${cat.toUpperCase()}] ${p.name||'Unknown'}`
      });
    });
  });
  
  const sel = document.getElementById('stats-player-select');
  if(sel){
    const val = sel.value;
    sel.innerHTML = '<option value="">-- Choose Player --</option>' +
      allPlayers.map(p=>`<option value="${p.id}">${p.label}</option>`).join('');
    if(val) sel.value = val;
  }
}

function populateTeamFilter(){
  const teams = window._teams||{};
  const sel = document.getElementById('filter-team');
  if(sel){
    sel.innerHTML = '<option value="">All Teams</option>' +
      Object.entries(teams).map(([id,t])=>`<option value="${t.name}">${t.name}</option>`).join('');
  }
}

function loadPlayerStats(){
  const compId = document.getElementById('stats-player-select')?.value;
  const formSection = document.getElementById('stats-form-section');
  
  if(!compId){
    if(formSection) formSection.style.display = 'none';
    return;
  }
  
  if(formSection) formSection.style.display = 'block';
  const playerStats = (window._stats||{})[compId]||{};
  
  ['rank', 'matches', 'wins', 'draws', 'motm', 'ratio'].forEach(field => {
    const el = document.getElementById(`stats-${field}`);
    if(el) el.value = playerStats[field] || '';
  });
}

function savePlayerStats(){
  if(!window._db) { alert('Database not initialized'); return; }
  
  const compIdEl = document.getElementById('stats-player-select');
  const msgEl = document.getElementById('stats-msg');

  const compId = compIdEl.value;
  if(!compId){ showMsg(msgEl,'Select a player first','error'); return; }
  
  const data = {
    rank: (document.getElementById('stats-rank')?.value||'').trim(),
    matches: parseInt(document.getElementById('stats-matches')?.value)||0,
    wins: parseInt(document.getElementById('stats-wins')?.value)||0,
    draws: parseInt(document.getElementById('stats-draws')?.value)||0,
    motm: parseInt(document.getElementById('stats-motm')?.value)||0,
    ratio: parseFloat(document.getElementById('stats-ratio')?.value)||0
  };
  
  window._set(window._ref(window._db,`stats/${compId}`), data)
    .then(()=>showMsg(msgEl,'Stats saved!','success'))
    .catch(e=>showMsg(msgEl,e.message,'error'));
}

function renderStatsTable(){
  const tbody = document.getElementById('stats-tbody');
  if(!tbody) return;
  
  const stats = window._stats||{};
  const rows = [];
  let counter = 1;
  
  ['youth','local','invited'].forEach(cat=>{
    Object.entries(window[`_${cat}`]||{}).forEach(([id,p])=>{
      const playerStats = stats[`${cat}::${id}`];
      if(playerStats){
        rows.push({
          counter: counter++,
          name: p.name||'-',
          cat: cat,
          stats: playerStats
        });
      }
    });
  });
  
  if(!rows.length){
    tbody.innerHTML = `<tr><td colspan="9" class="empty-state">No stats yet</td></tr>`;
    return;
  }
  
  tbody.innerHTML = rows.map(r=>`
    <tr>
      <td>${r.counter}</td>
      <td><strong>${r.name}</strong></td>
      <td><span class="badge badge-${r.cat}">${r.cat.toUpperCase().charAt(0)}</span></td>
      <td>${r.stats.rank||'-'}</td>
      <td>${r.stats.matches||0}</td>
      <td>${r.stats.wins||0}</td>
      <td>${r.stats.draws||0}</td>
      <td>${r.stats.motm||0}</td>
      <td>${r.stats.ratio||0}%</td>
    </tr>`).join('');
}

function switchRegTab(tab, el){
  document.querySelectorAll('#admin .cat-panel').forEach(p => { if(p) p.classList.remove('active'); });
  document.querySelectorAll('#admin .cat-tab').forEach(t => { if(t) t.classList.remove('active'); });
  const panel = document.getElementById('reg-'+tab);
  if(panel) panel.classList.add('active');
  if(el) el.classList.add('active');
}

function switchViewTab(tab, el){
  document.querySelectorAll('#viewer .cat-panel').forEach(p => { if(p) p.classList.remove('active'); });
  document.querySelectorAll('#viewer .cat-tab').forEach(t => { if(t) t.classList.remove('active'); });
  const panel = document.getElementById('view-'+tab);
  if(panel) panel.classList.add('active');
  if(el) el.classList.add('active');
  window.currentViewTab = tab;
}

function filterPlayers(){
  const searchText = document.getElementById('search-player')?.value.toLowerCase()||'';
  const category = document.getElementById('filter-category')?.value||'';
  const status = document.getElementById('filter-status')?.value||'';
  const teamName = document.getElementById('filter-team')?.value||'';
  
  ['youth','local','invited'].forEach(cat=>{
    const cards = document.querySelectorAll(`#${cat}-card-view .player-card`);
    cards.forEach((card,idx)=>{
      const player = (window[`_${cat}`]||{})[Object.keys(window[`_${cat}`]||{})[idx]];
      if(!player) { card.style.display='none'; return; }
      
      const matchSearch = !searchText || player.name?.toLowerCase().includes(searchText);
      const matchCat = !category || cat === category;
      const matchStatus = !status || player.status === status;
      const matchTeam = !teamName || player.soldTo === teamName;
      
      if(matchSearch && matchCat && matchStatus && matchTeam){
        card.style.display='';
      } else {
        card.style.display='none';
      }
    });
  });
}

function startTimer(){
  if(window.timerInterval) return;
  const input = document.getElementById('timer-input');
  if(!window.timerSeconds && input.value) {
    window.timerSeconds = parseInt(input.value) * 60;
  }
  window.timerInterval = setInterval(()=>{
    window.timerSeconds--;
    updateTimerDisplay();
    if(window.timerSeconds <= 0) pauseTimer();
  }, 1000);
}

function pauseTimer(){
  if(window.timerInterval) {
    clearInterval(window.timerInterval);
    window.timerInterval = null;
  }
}

function resetTimer(){
  pauseTimer();
  window.timerSeconds = 0;
  updateTimerDisplay();
}

function updateTimerDisplay(){
  const hrs = Math.floor(window.timerSeconds / 3600);
  const mins = Math.floor((window.timerSeconds % 3600) / 60);
  const secs = window.timerSeconds % 60;
  const display = document.getElementById('timer-display');
  if(display) {
    display.textContent = String(hrs).padStart(2,'0') + ':' + String(mins).padStart(2,'0') + ':' + String(secs).padStart(2,'0');
  }
}

function showMsg(el, text, type){
  if(!el) return;
  el.textContent = text;
  el.className = `msg ${type}`;
  setTimeout(()=>{ 
    el.className='msg'; 
    el.textContent=''; 
  }, 4000);
}
</script>

</body>
</html>