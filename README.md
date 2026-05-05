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
  tbody tr { border-bottom:1px solid #f5f5f5; transition:all 0.2s; }
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

  .team-grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(340px,1fr)); gap:24px; margin:24px 0; }
  .team-card {
    background:linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border-radius:16px;
    box-shadow: var(--card-shadow);
    overflow:hidden;
    transition:all 0.3s;
    border: 2px solid var(--gold);
    cursor: pointer;
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

  .team-squad-section {
    background: rgba(208,2,27,0.05);
    border: 2px solid var(--gold);
    border-radius: 12px;
    padding: 16px;
    margin-top: 16px;
  }
  
  .squad-title {
    font-family: var(--font-head);
    font-size: 13px;
    color: var(--red);
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 12px;
    font-weight: 700;
  }

  .squad-player-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid rgba(208,2,27,0.1);
    font-size: 13px;
  }

  .squad-player-item:last-child {
    border-bottom: none;
  }

  .squad-player-name {
    color: var(--dark);
    font-weight: 600;
  }

  .squad-player-price {
    font-family: var(--font-display);
    color: var(--red);
    font-weight: 700;
  }

  .team-action-buttons {
    display: flex;
    gap: 12px;
    margin-top: 16px;
    flex-wrap: wrap;
  }

  .team-action-buttons .btn {
    flex: 1;
    padding: 10px 16px;
    font-size: 12px;
    min-width: 150px;
  }

  .player-list-wrapper {
    background: linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border-radius: 16px;
    overflow: hidden;
    border: 2px solid var(--gold);
    box-shadow: 0 8px 32px rgba(208,2,27,0.15);
  }

  .player-list-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
  }

  .player-list-table thead {
    background: linear-gradient(135deg, var(--red) 0%, var(--red-deep) 100%);
    border-bottom: 3px solid var(--gold);
  }

  .player-list-table thead th {
    color: #fff;
    padding: 16px;
    text-align: left;
    font-family: var(--font-head);
    font-weight: 700;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    font-size: 12px;
  }

  .player-list-table tbody tr {
    border-bottom: 1px solid #f0f0f0;
    transition: all 0.3s ease;
  }

  .player-list-table tbody tr:hover {
    background: linear-gradient(90deg, rgba(208,2,27,0.05) 0%, transparent 100%);
    box-shadow: inset 8px 0 0 var(--gold);
  }

  .player-list-table tbody td {
    padding: 16px;
    vertical-align: middle;
  }

  .player-name-cell {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .player-photo {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    object-fit: cover;
    border: 2px solid var(--gold);
  }

  .player-name-text {
    cursor: pointer;
    font-family: var(--font-display);
    font-size: 16px;
    color: var(--red);
    font-weight: 700;
    letter-spacing: 1px;
    transition: all 0.2s;
    text-transform: uppercase;
  }

  .player-name-text:hover {
    color: var(--red-bright);
    text-shadow: 0 2px 8px rgba(208,2,27,0.3);
  }

  .player-price {
    font-family: var(--font-display);
    font-size: 18px;
    color: var(--red);
    font-weight: 700;
    letter-spacing: 1px;
  }

  .player-club {
    color: #666;
    font-size: 13px;
    font-weight: 600;
  }

  .player-stats-preview {
    display: flex;
    gap: 6px;
  }

  .stat-mini {
    background: linear-gradient(135deg, var(--red), var(--red-deep));
    color: var(--gold);
    padding: 4px 8px;
    border-radius: 6px;
    font-size: 11px;
    font-weight: 700;
    text-align: center;
    min-width: 35px;
    border: 1px solid var(--gold);
  }

  .action-buttons {
    display: flex;
    gap: 8px;
  }

  .action-btn {
    padding: 8px 14px;
    font-size: 11px;
    font-weight: 700;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s;
    font-family: var(--font-head);
    letter-spacing: 1px;
    text-transform: uppercase;
  }

  .action-btn.download {
    background: linear-gradient(135deg, var(--gold), #c59e1b);
    color: #000;
  }

  .action-btn.download:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 16px rgba(212,175,55,0.4);
  }

  .category-badge {
    display: inline-block;
    padding: 6px 12px;
    border-radius: 8px;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 1px;
    text-transform: uppercase;
    border: 1px solid;
  }

  .category-badge.youth {
    background: #dbeafe;
    color: #1e40af;
    border-color: #1e40af;
  }

  .category-badge.local {
    background: #fee2e2;
    color: #991b1b;
    border-color: #991b1b;
  }

  .category-badge.invited {
    background: #fef3c7;
    color: #92400e;
    border-color: #92400e;
  }

  .sold-management-row {
    background: linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border: 2px solid var(--gold);
    border-radius: 12px;
    padding: 16px;
    margin-bottom: 16px;
    display: grid;
    grid-template-columns: 1.5fr 1fr 1fr 0.8fr 0.8fr;
    gap: 16px;
    align-items: center;
  }

  .sold-player-info h4 {
    color: var(--red);
    font-size: 16px;
    margin-bottom: 4px;
    letter-spacing: 1px;
  }

  .sold-player-info p {
    color: #666;
    font-size: 12px;
    margin: 2px 0;
  }

  .sold-input-group {
    display: flex;
    gap: 8px;
  }

  .sold-input-group input,
  .sold-input-group select {
    flex: 1;
    padding: 10px;
    border: 2px solid #ddd;
    border-radius: 8px;
    font-family: var(--font-body);
    font-size: 13px;
  }

  .sold-input-group input:focus,
  .sold-input-group select:focus {
    border-color: var(--red);
    outline: none;
  }

  .sold-action-btn {
    padding: 10px 12px;
    font-size: 10px;
    white-space: nowrap;
    margin: 0 2px;
  }

  .direct-sign-row {
    background: linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border: 2px solid var(--gold);
    border-radius: 12px;
    padding: 16px;
    margin-bottom: 16px;
    display: grid;
    grid-template-columns: 1.5fr 1.2fr 1.2fr auto;
    gap: 16px;
    align-items: center;
  }

  .direct-sign-player-info h4 {
    color: var(--red);
    font-size: 16px;
    margin-bottom: 4px;
    letter-spacing: 1px;
  }

  .direct-sign-player-info p {
    color: #666;
    font-size: 12px;
    margin: 2px 0;
  }

  .free-sign-badge {
    display: inline-block;
    background: linear-gradient(135deg, var(--gold), #c59e1b);
    color: #000;
    padding: 4px 12px;
    border-radius: 6px;
    font-size: 10px;
    font-weight: 700;
    margin-top: 4px;
  }

  .bid-history {
    background: #f8f7f7;
    border-radius: 12px;
    padding: 16px;
    margin-top: 16px;
    border-left: 4px solid var(--red);
  }

  .bid-history h4 {
    color: var(--red);
    font-size: 14px;
    margin-bottom: 12px;
    text-transform: uppercase;
    font-weight: 700;
  }

  .bid-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px 0;
    border-bottom: 1px solid #e8e0e0;
    font-size: 12px;
  }

  .bid-item:last-child {
    border-bottom: none;
  }

  .bid-team {
    color: var(--red);
    font-weight: 700;
  }

  .bid-price {
    font-family: var(--font-display);
    color: var(--dark);
    font-weight: 700;
  }

  .team-balance-sheet {
    background: #f8f7f7;
    border-radius: 16px;
    border: 2px solid var(--gold);
    padding: 24px;
    margin-bottom: 24px;
  }

  .balance-sheet-title {
    font-family: var(--font-display);
    font-size: 24px;
    color: var(--red);
    margin-bottom: 16px;
    text-align: center;
  }

  .team-requirement-box {
    background: linear-gradient(135deg, #ffffff 0%, #f8f7f7 100%);
    border: 2px solid var(--gold);
    border-radius: 12px;
    padding: 16px;
    margin-bottom: 16px;
  }

  .team-requirement-header {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 16px;
  }

  .team-requirement-name {
    font-family: var(--font-display);
    font-size: 18px;
    color: var(--red);
    font-weight: 700;
    flex: 1;
  }

  .requirement-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    border-radius: 8px;
    margin-bottom: 8px;
    font-size: 13px;
    font-weight: 600;
  }

  .requirement-item.pending {
    background: #ffebee;
    border-left: 4px solid var(--red);
    color: #c62828;
  }

  .requirement-item.met {
    background: #e8f5e9;
    border-left: 4px solid #4caf50;
    color: #2e7d32;
  }

  .requirement-progress {
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .requirement-bar {
    flex: 1;
    height: 6px;
    background: #e0e0e0;
    border-radius: 3px;
    overflow: hidden;
  }

  .requirement-bar-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--red), var(--red-deep));
    transition: width 0.3s ease;
  }

  @media(max-width:768px) {
    .player-list-table { font-size: 12px; }
    .player-list-table thead th { padding: 12px 8px; }
    .player-list-table tbody td { padding: 12px 8px; }
    .action-buttons { flex-direction: column; }
    .action-btn { padding: 6px 10px; font-size: 10px; }
    .sold-management-row { grid-template-columns: 1fr; }
    .direct-sign-row { grid-template-columns: 1fr; }
  }

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
    <button class="nav-tab" onclick="showSection('team-requirements')">REQUIREMENTS</button>
    <button class="nav-tab" onclick="showSection('balance-sheet')">BALANCE SHEET</button>
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

<!-- TEAM REQUIREMENTS SECTION -->
<div id="team-requirements" class="section">
  <div class="section-title">TEAM REQUIREMENTS STATUS</div>
  <div class="section-divider"></div>
  <p style="color:#666; margin-bottom:24px; font-size:14px;">⚠️ This section shows which teams haven't met the minimum player requirements in each category</p>
  <div id="requirements-content"></div>
</div>

<!-- BALANCE SHEET SECTION -->
<div id="balance-sheet" class="section">
  <div class="section-title">ALL TEAMS BALANCE SHEET</div>
  <div class="section-divider"></div>
  <button class="btn btn-gold" style="margin-bottom:24px;" onclick="downloadBalanceSheet()">📥 DOWNLOAD BALANCE SHEET (JPG)</button>
  <div id="balance-sheet-content"></div>
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
      <div class="card-header"><h2>BID RULES - PLAYER QUANTITY</h2></div>
      <div class="card-body">
        <div class="form-row">
          <div class="form-group">
            <label>Youth - Min Players</label>
            <input type="number" id="youth-min" placeholder="e.g. 2" min="0"/>
          </div>
          <div class="form-group">
            <label>Youth - Max Players</label>
            <input type="number" id="youth-max" placeholder="e.g. 5" min="0"/>
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label>Local - Min Players</label>
            <input type="number" id="local-min" placeholder="e.g. 3" min="0"/>
          </div>
          <div class="form-group">
            <label>Local - Max Players</label>
            <input type="number" id="local-max" placeholder="e.g. 6" min="0"/>
          </div>
        </div>
        <div class="form-row">
          <div class="form-group">
            <label>Invited - Min Players</label>
            <input type="number" id="invited-min" placeholder="e.g. 1" min="0"/>
          </div>
          <div class="form-group">
            <label>Invited - Max Players</label>
            <input type="number" id="invited-max" placeholder="e.g. 3" min="0"/>
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
          <thead><tr><th>#</th><th>Team</th><th>Captain</th><th>Budget</th><th>Spent</th><th>Remaining</th><th>Free Signs</th><th>Action</th></tr></thead>
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
    <div class="card-header"><h2>DIRECT SIGNING - FREE PLAYER (ONE PER TEAM)</h2></div>
    <div class="card-body">
      <p style="margin-bottom:16px; color:#666; font-size:13px;">⚠️ Each team gets 1 FREE direct signing without auction. Players already directly signed won't appear below.</p>
      <div id="direct-sign-management"></div>
    </div>
  </div>

  <div class="card">
    <div class="card-header"><h2>SOLD OUT MANAGEMENT - QUICK ASSIGN</h2></div>
    <div class="card-body">
      <p style="margin-bottom:16px; color:#666; font-size:13px;">Edit or delete player assignments. Players with direct sign won't appear here.</p>
      <div id="sold-management"></div>
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
    <div id="youth-list-view" class="player-list-wrapper"></div>
  </div>

  <div id="view-local" class="cat-panel">
    <div id="local-list-view" class="player-list-wrapper"></div>
  </div>

  <div id="view-invited" class="cat-panel">
    <div id="invited-list-view" class="player-list-wrapper"></div>
  </div>
</div>

<!-- PROFILE MODAL -->
<div id="profile-modal" class="modal-bg">
  <div class="modal">
    <span class="modal-close" onclick="closeModal()">✕</span>
    <div id="modal-content"></div>
  </div>
</div>

<!-- TEAM DETAIL MODAL -->
<div id="team-detail-modal" class="modal-bg">
  <div class="modal">
    <span class="modal-close" onclick="closeModal()">✕</span>
    <div id="team-detail-content"></div>
  </div>
</div>

<!-- EDIT SOLD PLAYER MODAL -->
<div id="edit-sold-modal" class="modal-bg">
  <div class="modal">
    <span class="modal-close" onclick="closeModal()">✕</span>
    <div id="edit-sold-content"></div>
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
window._bidHistory = {};
window._directSigns = {};
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

    onValue(ref(db,'bidHistory'), snap => {
      window._bidHistory = snap.val() || {};
    });

    onValue(ref(db,'directSigns'), snap => {
      window._directSigns = snap.val() || {};
      renderDirectSignManagement();
    });

    onValue(ref(db,'teams'), snap => {
      try {
        window._teams = snap.val() || {};
        renderTeamTable();
        renderTeamProfiles();
        renderBalanceSheetContent();
        renderTeamRequirements();
        updateStats();
        populateSelects();
      } catch(e) {
        console.error("Error processing teams:", e);
      }
    });

    ['youth','local','invited'].forEach(cat => {
      onValue(ref(db,`players/${cat}`), snap => {
        try {
          window[`_${cat}`] = snap.val() || {};
          renderPlayerTable(cat);
          renderPlayerList(cat);
          renderSoldManagement();
          renderDirectSignManagement();
          renderTeamRequirements();
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
        renderPlayerList('youth');
        renderPlayerList('local');
        renderPlayerList('invited');
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
  
  const data = { name, cap, budget, logo, remaining: budget, spent: 0, freeSigns: 1 };
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
    tbody.innerHTML=`<tr><td colspan="8" class="empty-state">No teams yet</td></tr>`; 
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
      <td><span style="background:#fef3c7; padding:4px 8px; border-radius:6px; font-size:12px; font-weight:700; color:#92400e;">${t.freeSigns || 0}</span></td>
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
    
    const squad = [];
    ['youth','local','invited'].forEach(cat=>{
      Object.entries(window[`_${cat}`]||{}).forEach(([pid,p])=>{
        if(p.soldTeamId===id) {
          squad.push({
            name: p.name,
            price: p.soldPrice || 0,
            cat: cat
          });
        }
      });
    });

    const directSign = (window._directSigns||{})[id];
    if(directSign) {
      squad.unshift({
        name: directSign.playerName,
        price: 0,
        cat: 'FREE',
        isFreeSign: true
      });
    }
    
    return `
    <div class="team-card" onclick="showTeamDetail('${id}')">
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
        <div class="team-stat">
          <div class="team-stat-label">Squad Size</div>
          <div class="team-stat-value">${squad.length} Players</div>
        </div>
        <div style="margin-top:16px; background:#f5f5f5; border-radius:8px; overflow:hidden;">
          <div style="height:6px; background:linear-gradient(90deg,var(--red),var(--red-deep)); width:${usedPercent}%;"></div>
        </div>
        <div style="text-align:center; font-size:11px; color:#999; margin-top:8px; font-weight:700;">${usedPercent}% USED</div>
      </div>
    </div>`;
  }).join('');
}

function showTeamDetail(teamId){
  const team = (window._teams||{})[teamId];
  if(!team) return;

  const squad = [];
  ['youth','local','invited'].forEach(cat=>{
    Object.entries(window[`_${cat}`]||{}).forEach(([pid,p])=>{
      if(p.soldTeamId===teamId) {
        squad.push({
          name: p.name,
          price: p.soldPrice || 0,
          cat: cat
        });
      }
    });
  });

  const directSign = (window._directSigns||{})[teamId];
  if(directSign) {
    squad.unshift({
      name: directSign.playerName,
      price: 0,
      cat: 'FREE',
      isFreeSign: true
    });
  }

  const bidRules = window._bidRules||{};
  
  const html = `
    <div style="padding:28px;">
      <h2 style="font-family:var(--font-display); color:var(--red); font-size:32px; letter-spacing:2px; margin-bottom:8px; display:flex; align-items:center; gap:12px;">
        ${team.logo ? `<img src="${team.logo}" style="width:60px; height:60px; border-radius:50%; object-fit:cover; border:2px solid var(--gold);"/>` : ''}
        ${team.name}
      </h2>
      <p style="color:#666; font-size:13px; margin-bottom:24px;">Captain: <strong>${team.cap}</strong></p>

      <div style="display:grid; grid-template-columns:repeat(auto-fit,minmax(180px,1fr)); gap:16px; margin-bottom:28px;">
        <div style="background:#f8f7f7; padding:16px; border-radius:12px; border:2px solid var(--gold);">
          <div style="color:#666; font-size:11px; font-weight:700; text-transform:uppercase; margin-bottom:8px;">Total Budget</div>
          <div style="font-family:var(--font-display); font-size:28px; color:var(--red);">${(team.budget||0).toLocaleString()}</div>
        </div>
        <div style="background:#f8f7f7; padding:16px; border-radius:12px; border:2px solid var(--gold);">
          <div style="color:#666; font-size:11px; font-weight:700; text-transform:uppercase; margin-bottom:8px;">Spent</div>
          <div style="font-family:var(--font-display); font-size:28px; color:#dc2626;">${(team.spent||0).toLocaleString()}</div>
        </div>
        <div style="background:#f8f7f7; padding:16px; border-radius:12px; border:2px solid var(--gold);">
          <div style="color:#666; font-size:11px; font-weight:700; text-transform:uppercase; margin-bottom:8px;">Remaining</div>
          <div style="font-family:var(--font-display); font-size:28px; color:#2e7d32;">${(team.remaining||0).toLocaleString()}</div>
        </div>
      </div>

      <div style="background:#f8f7f7; padding:16px; border-radius:12px; border:2px solid var(--gold); margin-bottom:28px;">
        <h4 style="color:var(--red); font-size:14px; margin-bottom:12px; text-transform:uppercase; font-weight:700;">📋 Squad (${squad.length} Players)</h4>
        ${squad.map((p,i)=>`
          <div style="display:flex; justify-content:space-between; align-items:center; padding:10px 0; border-bottom:1px solid #e8e0e0; font-size:13px;">
            <div style="display:flex; align-items:center; gap:8px;">
              <span style="background:${p.isFreeSign?'#fef3c7':p.cat==='youth'?'#dbeafe':p.cat==='local'?'#fee2e2':'#fef3c7'}; padding:4px 8px; border-radius:6px; font-size:10px; font-weight:700;">${p.isFreeSign?'FREE':p.cat.toUpperCase()[0]}</span>
              <span style="font-weight:600; color:var(--dark);">${p.name}</span>
            </div>
            <span style="font-family:var(--font-display); color:var(--red); font-weight:700;">${p.isFreeSign?'FREE':p.price.toLocaleString()}</span>
          </div>
        `).join('')}
      </div>

      <div style="background:#f8f7f7; padding:16px; border-radius:12px; border:2px solid var(--gold); margin-bottom:28px;">
        <h4 style="color:var(--red); font-size:14px; margin-bottom:12px; text-transform:uppercase; font-weight:700;">📊 Min/Max Requirements</h4>
        <div style="display:grid; grid-template-columns:repeat(3,1fr); gap:12px;">
          <div style="text-align:center;">
            <div style="color:#666; font-size:11px; font-weight:700; margin-bottom:6px;">YOUTH</div>
            <div style="font-family:var(--font-display); font-size:18px; color:var(--red);">${bidRules.youth?.min||0}-${bidRules.youth?.max||0}</div>
          </div>
          <div style="text-align:center;">
            <div style="color:#666; font-size:11px; font-weight:700; margin-bottom:6px;">LOCAL</div>
            <div style="font-family:var(--font-display); font-size:18px; color:var(--red);">${bidRules.local?.min||0}-${bidRules.local?.max||0}</div>
          </div>
          <div style="text-align:center;">
            <div style="color:#666; font-size:11px; font-weight:700; margin-bottom:6px;">INVITED</div>
            <div style="font-family:var(--font-display); font-size:18px; color:var(--red);">${bidRules.invited?.min||0}-${bidRules.invited?.max||0}</div>
          </div>
        </div>
      </div>

      <button class="btn btn-gold" onclick="downloadTeamProfile('${teamId}','${team.name}')" style="width:100%;">📥 DOWNLOAD TEAM PROFILE (JPG)</button>
    </div>
  `;

  const modal = document.getElementById('team-detail-modal');
  const content = document.getElementById('team-detail-content');
  if(content && modal) {
    content.innerHTML = html;
    modal.classList.add('open');
  }
}

function renderTeamRequirements(){
  const container = document.getElementById('requirements-content');
  if(!container) return;

  const teams = window._teams||{};
  const bidRules = window._bidRules||{};
  const teamsList = Object.entries(teams);
  
  if(!teamsList.length){
    container.innerHTML = `<div class="empty-state">No teams registered yet</div>`;
    return;
  }

  let html = '';

  teamsList.forEach(([id, t]) => {
    let youth = 0, local = 0, invited = 0;
    ['youth','local','invited'].forEach(cat=>{
      Object.entries(window[`_${cat}`]||{}).forEach(([pid,p])=>{
        if(p.soldTeamId === id) {
          if(cat === 'youth') youth++;
          else if(cat === 'local') local++;
          else invited++;
        }
      });
    });

    const directSign = (window._directSigns||{})[id];
    if(directSign) {
      if(directSign.category === 'youth') youth++;
      else if(directSign.category === 'local') local++;
      else invited++;
    }

    const youthMin = bidRules.youth?.min || 0;
    const localMin = bidRules.local?.min || 0;
    const invitedMin = bidRules.invited?.min || 0;

    const youthPending = youth < youthMin;
    const localPending = local < localMin;
    const invitedPending = invited < invitedMin;
    const hasWarning = youthPending || localPending || invitedPending;

    html += `
      <div class="team-requirement-box">
        <div class="team-requirement-header">
          ${t.logo ? `<img src="${t.logo}" style="width:50px; height:50px; border-radius:50%; object-fit:cover; border:2px solid var(--gold);"/>` : ''}
          <div class="team-requirement-name">${t.name}</div>
          <div style="font-size:12px; color:#999;">Captain: ${t.cap}</div>
        </div>
        
        ${hasWarning ? `
          <div class="requirement-item pending" style="margin-bottom:12px;">
            <strong>⚠️ TEAM HAS NOT MET MINIMUM REQUIREMENTS</strong>
          </div>
        ` : `
          <div class="requirement-item met" style="margin-bottom:12px;">
            <strong>✅ TEAM HAS MET ALL REQUIREMENTS</strong>
          </div>
        `}
        
        <div style="display:grid; grid-template-columns:repeat(3,1fr); gap:12px;">
          <div class="requirement-item ${youthPending ? 'pending' : 'met'}">
            <div style="display:flex; justify-content:space-between; align-items:center; width:100%; margin-bottom:6px;">
              <span>YOUTH</span>
              <span style="font-family:var(--font-display); font-size:16px; font-weight:700;">${youth}/${youthMin}</span>
            </div>
            <div class="requirement-bar">
              <div class="requirement-bar-fill" style="width:${Math.min(100, (youth/Math.max(1,youthMin))*100)}%"></div>
            </div>
            ${youthPending ? `<div style="font-size:11px; margin-top:6px; font-weight:700;">Need ${youthMin-youth} more</div>` : ''}
          </div>

          <div class="requirement-item ${localPending ? 'pending' : 'met'}">
            <div style="display:flex; justify-content:space-between; align-items:center; width:100%; margin-bottom:6px;">
              <span>LOCAL</span>
              <span style="font-family:var(--font-display); font-size:16px; font-weight:700;">${local}/${localMin}</span>
            </div>
            <div class="requirement-bar">
              <div class="requirement-bar-fill" style="width:${Math.min(100, (local/Math.max(1,localMin))*100)}%"></div>
            </div>
            ${localPending ? `<div style="font-size:11px; margin-top:6px; font-weight:700;">Need ${localMin-local} more</div>` : ''}
          </div>

          <div class="requirement-item ${invitedPending ? 'pending' : 'met'}">
            <div style="display:flex; justify-content:space-between; align-items:center; width:100%; margin-bottom:6px;">
              <span>INVITED</span>
              <span style="font-family:var(--font-display); font-size:16px; font-weight:700;">${invited}/${invitedMin}</span>
            </div>
            <div class="requirement-bar">
              <div class="requirement-bar-fill" style="width:${Math.min(100, (invited/Math.max(1,invitedMin))*100)}%"></div>
            </div>
            ${invitedPending ? `<div style="font-size:11px; margin-top:6px; font-weight:700;">Need ${invitedMin-invited} more</div>` : ''}
          </div>
        </div>
      </div>
    `;
  });

  container.innerHTML = html || `<div class="empty-state">No team data available</div>`;
}

function downloadTeamProfile(teamId, teamName){
  const team = (window._teams||{})[teamId];
  if(!team) return;
  
  const squad = [];
  ['youth','local','invited'].forEach(cat=>{
    Object.entries(window[`_${cat}`]||{}).forEach(([pid,p])=>{
      if(p.soldTeamId===teamId) {
        squad.push({
          name: p.name,
          price: p.soldPrice || 0,
          cat: cat
        });
      }
    });
  });

  const directSign = (window._directSigns||{})[teamId];
  if(directSign) {
    squad.unshift({
      name: directSign.playerName,
      price: 0,
      cat: 'FREE'
    });
  }

  const canvas = document.createElement('canvas');
  canvas.width = 1200;
  canvas.height = 1600;
  const ctx = canvas.getContext('2d');

  const bgGrad = ctx.createLinearGradient(0,0,1200,1600);
  bgGrad.addColorStop(0, '#0a0a0a');
  bgGrad.addColorStop(0.5, '#1a0a0a');
  bgGrad.addColorStop(1, '#0a0a0a');
  ctx.fillStyle = bgGrad;
  ctx.fillRect(0,0,1200,1600);

  const headerGrad = ctx.createLinearGradient(0,0,1200,180);
  headerGrad.addColorStop(0, '#D0021B');
  headerGrad.addColorStop(1, '#8B0000');
  ctx.fillStyle = headerGrad;
  ctx.fillRect(0,0,1200,180);

  ctx.fillStyle = '#D4AF37';
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 3;
  ctx.strokeRect(20,20,1160,1560);

  if(team.logo) {
    const img = new Image();
    img.crossOrigin = 'anonymous';
    img.onload = () => {
      ctx.drawImage(img, 1050, 40, 120, 120);
      drawTeamContent();
    };
    img.onerror = drawTeamContent;
    img.src = team.logo;
  } else {
    drawTeamContent();
  }

  function drawTeamContent() {
    ctx.fillStyle = '#ffffff';
    ctx.font = 'bold 64px Oswald';
    ctx.textAlign = 'left';
    ctx.fillText(teamName.toUpperCase(), 60, 120);

    ctx.font = 'bold 28px Oswald';
    ctx.fillStyle = '#D4AF37';
    ctx.fillText(`Captain: ${team.cap}`, 60, 160);

    ctx.fillStyle = 'rgba(255,255,255,0.05)';
    ctx.fillRect(60, 220, 1080, 160);
    ctx.strokeStyle = '#D4AF37';
    ctx.lineWidth = 2;
    ctx.strokeRect(60, 220, 1080, 160);

    const statBoxes = [
      { label: 'Total Budget', value: (team.budget||0).toLocaleString() },
      { label: 'Spent', value: (team.spent||0).toLocaleString() },
      { label: 'Remaining', value: (team.remaining||0).toLocaleString() }
    ];

    statBoxes.forEach((s, i) => {
      const x = 80 + (i * 360);
      ctx.fillStyle = '#D4AF37';
      ctx.font = 'bold 14px Oswald';
      ctx.textAlign = 'left';
      ctx.fillText(s.label.toUpperCase(), x, 250);
      ctx.font = 'bold 40px Oswald';
      ctx.fillStyle = '#ffffff';
      ctx.fillText(s.value + ' Coins', x, 310);
    });

    ctx.fillStyle = '#D4AF37';
    ctx.font = 'bold 36px Oswald';
    ctx.textAlign = 'center';
    ctx.fillText(`SQUAD - ${squad.length} PLAYERS`, 600, 440);

    let yPos = 500;
    const rowHeight = 80;

    squad.forEach((player, idx) => {
      const bgColor = idx % 2 === 0 ? 'rgba(255,255,255,0.08)' : 'rgba(208,2,27,0.05)';
      ctx.fillStyle = bgColor;
      ctx.fillRect(60, yPos - 50, 1080, 70);

      ctx.strokeStyle = '#D4AF37';
      ctx.lineWidth = 1;
      ctx.strokeRect(60, yPos - 50, 1080, 70);

      ctx.fillStyle = '#ffffff';
      ctx.font = 'bold 24px Oswald';
      ctx.textAlign = 'left';
      ctx.fillText(`${idx + 1}. ${player.name.toUpperCase()}`, 90, yPos - 15);

      ctx.fillStyle = '#D4AF37';
      ctx.font = 'bold 28px Oswald';
      ctx.textAlign = 'right';
      const priceText = player.cat === 'FREE' ? 'FREE SIGNING' : player.price.toLocaleString();
      ctx.fillText(priceText, 1110, yPos - 15);

      ctx.fillStyle = '#999';
      ctx.font = 'bold 14px Oswald';
      ctx.textAlign = 'left';
      ctx.fillText(`${player.cat.toUpperCase()}`, 90, yPos + 15);

      yPos += rowHeight;
    });

    ctx.fillStyle = '#D4AF37';
    ctx.font = 'bold 24px Oswald';
    ctx.textAlign = 'center';
    ctx.fillText('LLFC AUCTION 2026', 600, 1570);

    canvas.toBlob(blob=>{
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `${teamName}_Squad_Profile.jpg`;
      a.click();
      URL.revokeObjectURL(url);
    }, 'image/jpeg', 0.95);
  }
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

function renderPlayerList(cat){
  const players = window[`_${cat}`]||{};
  const container = document.getElementById(`${cat}-list-view`);
  if(!container) return;
  
  const availablePlayers = Object.entries(players).filter(([id,p]) => p.status !== 'sold');
  
  if(!availablePlayers.length){
    container.innerHTML = `<div class="empty-state">No available ${cat} players</div>`;
    return;
  }
  
  const rows = availablePlayers.map(([id,p],i)=>{
    const stats = (window._stats||{})[`${cat}::${id}`]||{};
    const serialNum = i+1;
    
    return `
      <tr>
        <td>
          <div class="player-name-cell">
            ${p.photo ? `<img src="${p.photo}" class="player-photo" alt="${p.name}"/>` : `<div style="width:40px;height:40px;border-radius:50%;background:#f0f0f0;border:2px solid var(--gold);"></div>`}
            <span class="player-name-text" onclick="showFullProfile('${cat}','${id}',${serialNum})">${p.name || 'UNKNOWN'}</span>
          </div>
        </td>
        <td><span class="category-badge ${cat}">${cat.toUpperCase()}</span></td>
        <td><span style="color:#666; font-weight:600;">${p.club || 'LLFC'}</span></td>
        <td><span class="player-price">${(p.basePrice||0).toLocaleString()}</span></td>
        <td>
          ${stats.matches ? `
            <div class="player-stats-preview">
              <div class="stat-mini" title="Matches">${stats.matches}</div>
              <div class="stat-mini" title="Wins">${stats.wins || 0}</div>
              <div class="stat-mini" title="Win %">${stats.ratio || 0}%</div>
            </div>
          ` : '<span style="color:#ccc; font-size:12px;">No stats</span>'}
        </td>
        <td>
          <div class="action-buttons">
            <button class="action-btn download" onclick="downloadCardImage('${cat}','${id}',${serialNum})">📥 DOWNLOAD</button>
          </div>
        </td>
      </tr>
    `;
  }).join('');
  
  container.innerHTML = `
    <table class="player-list-table">
      <thead>
        <tr>
          <th style="width:35%">PLAYER</th>
          <th style="width:12%">CATEGORY</th>
          <th style="width:15%">CLUB</th>
          <th style="width:12%">BASE PRICE</th>
          <th style="width:18%">STATS</th>
          <th style="width:8%">ACTION</th>
        </tr>
      </thead>
      <tbody>
        ${rows}
      </tbody>
    </table>
  `;
}

function downloadCardImage(cat, id, serial){
  const player = (window[`_${cat}`]||{})[id];
  const stats = (window._stats||{})[`${cat}::${id}`]||{};
  const tourney = window._tournamentData||{};
  
  const canvas = document.createElement('canvas');
  canvas.width = 1080;
  canvas.height = 1440;
  const ctx = canvas.getContext('2d');
  
  const bgGrad = ctx.createLinearGradient(0,0,1080,1440);
  bgGrad.addColorStop(0, '#0a0a0a');
  bgGrad.addColorStop(0.5, '#1a0a0a');
  bgGrad.addColorStop(1, '#0a0a0a');
  ctx.fillStyle = bgGrad;
  ctx.fillRect(0,0,1080,1440);
  
  ctx.fillStyle = '#D4AF37';
  ctx.fillRect(0,0,1080,16);
  ctx.fillRect(0,1424,1080,16);
  
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 4;
  ctx.strokeRect(20,20,1040,1400);
  
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
  
  ctx.fillStyle = '#ffffff';
  ctx.font = 'bold 64px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText((player.name||'UNKNOWN').toUpperCase(), 540, 590);
  
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
  
  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 32px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText(`BASE PRICE: ${(player.basePrice||0).toLocaleString()} COINS`, 540, 800);
  
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
  
  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 24px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText('LLFC AUCTION 2026', 540, 1420);
  
  canvas.toBlob(blob=>{
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `${player.name}_${cat}_${String(serial).padStart(2,'0')}.jpg`;
    a.click();
    URL.revokeObjectURL(url);
  }, 'image/jpeg', 0.95);
}

function showFullProfile(cat, id, serial){
  const player = (window[`_${cat}`]||{})[id];
  if(!player) return;
  
  const stats = (window._stats||{})[`${cat}::${id}`]||{};
  const tourney = window._tournamentData||{};
  
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
      
      <div style="padding:14px; background:var(--gold-light); border-radius:12px; margin-bottom:24px; color:var(--red); font-weight:700; font-size:13px; letter-spacing:1px;">AVAILABLE FOR AUCTION</div>
      
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
      
      <button class="btn btn-gold" onclick="downloadCardImage('${cat}','${id}', ${serial})" style="width:100%;">📥 DOWNLOAD JPG</button>
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
  document.getElementById('team-detail-modal')?.classList.remove('open');
  document.getElementById('edit-sold-modal')?.classList.remove('open');
}

function getAvailablePlayersForDirectSign(){
  const directSigns = window._directSigns||{};
  const alreadySignedIds = new Set();
  
  Object.values(directSigns).forEach(ds => {
    if(ds.playerId) alreadySignedIds.add(ds.playerId);
  });

  const allPlayers = [];
  ['youth','local','invited'].forEach(cat=>{
    Object.entries(window[`_${cat}`]||{}).forEach(([id,p])=>{
      const compId = `${cat}::${id}`;
      if(p.status !== 'sold' && !alreadySignedIds.has(compId)){
        allPlayers.push({
          compId: compId,
          name: p.name||'-',
          cat: cat,
          basePrice: p.basePrice||0,
          id: id
        });
      }
    });
  });

  return allPlayers;
}

function renderDirectSignManagement(){
  const container = document.getElementById('direct-sign-management');
  if(!container) return;
  
  const allPlayers = getAvailablePlayersForDirectSign();
  
  if(!allPlayers.length){
    container.innerHTML=`<div class="empty-state">All available players have direct signs or are sold</div>`;
    return;
  }

  const teams = window._teams||{};
  const teamOptions = Object.entries(teams).map(([id,t])=>`<option value="${id}">${t.name}</option>`).join('');

  container.innerHTML = allPlayers.map(p=>{
    return `
    <div class="direct-sign-row">
      <div class="direct-sign-player-info">
        <h4>${p.name}</h4>
        <p style="color: var(--red); font-weight: 700;">${p.cat.toUpperCase()} • Base: ${p.basePrice.toLocaleString()}</p>
      </div>
      <select id="team-direct-${p.compId}" onchange="">
        <option value="">Select Team</option>
        ${teamOptions}
      </select>
      <input type="text" id="reason-direct-${p.compId}" placeholder="Reason (optional)" style="flex:1;"/>
      <button class="btn btn-green sold-action-btn" onclick="saveDirectSign('${p.compId}')">SIGN NOW (FREE)</button>
    </div>`}).join('');
}

function saveDirectSign(compId){
  const [cat, id] = compId.split('::');
  const teamSelect = document.getElementById(`team-direct-${compId}`);
  const reasonInput = document.getElementById(`reason-direct-${compId}`);
  
  const teamId = teamSelect.value;
  const reason = reasonInput.value.trim() || 'Direct Signing';
  
  if(!teamId) { alert('Select team'); return; }
  
  const team = (window._teams||{})[teamId];
  if(!team) { alert('Team not found'); return; }

  if((team.freeSigns || 1) <= 0) { alert('This team has no free signings left'); return; }
  
  const player = (window[`_${cat}`]||{})[id];
  if(!player) { alert('Player not found'); return; }
  
  const updates = {};
  updates[`players/${cat}/${id}/status`] = 'sold';
  updates[`players/${cat}/${id}/soldTo`] = team.name;
  updates[`players/${cat}/${id}/soldPrice`] = 0;
  updates[`players/${cat}/${id}/soldTeamId`] = teamId;
  updates[`teams/${teamId}/freeSigns`] = (team.freeSigns || 1) - 1;
  updates[`directSigns/${teamId}`] = {
    playerId: compId,
    playerName: player.name,
    category: cat,
    reason: reason,
    timestamp: new Date().toLocaleString()
  };
  
  window._update(window._ref(window._db), updates)
    .then(()=>alert('Player directly signed to team!'))
    .catch(e=>alert('Error: '+e.message));
}

function renderSoldManagement(){
  const container = document.getElementById('sold-management');
  if(!container) return;
  
  const directSigns = window._directSigns||{};
  const directSignPlayerIds = new Set();
  Object.values(directSigns).forEach(ds => {
    if(ds.playerId) directSignPlayerIds.add(ds.playerId);
  });

  const allPlayers = [];
  let counter = 1;
  
  ['youth','local','invited'].forEach(cat=>{
    const players = window[`_${cat}`]||{};
    Object.entries(players).forEach(([id,p])=>{
      const compId = `${cat}::${id}`;
      if(p.status!=='sold' && !directSignPlayerIds.has(compId)){
        allPlayers.push({
          compId: compId,
          counter: counter++,
          name: p.name||'-',
          cat: cat,
          basePrice: p.basePrice||0,
          id: id
        });
      }
    });
  });

  if(!allPlayers.length){
    container.innerHTML=`<div class="empty-state">All available players are assigned or have direct signs!</div>`;
    return;
  }

  const teams = window._teams||{};
  const teamOptions = Object.entries(teams).map(([id,t])=>`<option value="${id}">${t.name}</option>`).join('');

  container.innerHTML = allPlayers.map(p=>`
    <div class="sold-management-row">
      <div class="sold-player-info">
        <h4>${p.name}</h4>
        <p style="color: var(--red); font-weight: 700;">${p.cat.toUpperCase()} • Base: ${p.basePrice.toLocaleString()}</p>
      </div>
      <div class="sold-input-group">
        <select id="team-${p.compId}" onchange="">
          <option value="">Select Team</option>
          ${teamOptions}
        </select>
      </div>
      <div class="sold-input-group">
        <input type="number" id="price-${p.compId}" placeholder="Sold Price" min="0"/>
      </div>
      <button class="btn btn-green sold-action-btn" onclick="saveSoldPlayer('${p.compId}')">SAVE</button>
      <button class="btn btn-danger sold-action-btn" onclick="deleteSoldAssignment('${p.compId}')" title="Delete this assignment">DELETE</button>
    </div>`).join('');
}

function saveSoldPlayer(compId){
  const [cat, id] = compId.split('::');
  const teamSelect = document.getElementById(`team-${compId}`);
  const priceInput = document.getElementById(`price-${compId}`);
  
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
    .then(()=>alert('Player marked as sold!'))
    .catch(e=>alert('Error: '+e.message));
}

function deleteSoldAssignment(compId){
  if(!confirm('Remove this sold assignment?')) return;
  
  const [cat, id] = compId.split('::');
  const player = (window[`_${cat}`]||{})[id];
  
  if(!player) return;

  const teamId = player.soldTeamId;
  const team = (window._teams||{})[teamId];
  
  if(!team) { alert('Team not found'); return; }
  
  const updates = {};
  updates[`players/${cat}/${id}/status`] = 'available';
  updates[`players/${cat}/${id}/soldTo`] = null;
  updates[`players/${cat}/${id}/soldPrice`] = null;
  updates[`players/${cat}/${id}/soldTeamId`] = null;
  updates[`teams/${teamId}/spent`] = Math.max(0, (team.spent || 0) - (player.soldPrice || 0));
  updates[`teams/${teamId}/remaining`] = (team.budget || 0) - Math.max(0, (team.spent || 0) - (player.soldPrice || 0));
  
  window._update(window._ref(window._db), updates)
    .then(()=>alert('Assignment removed!'))
    .catch(e=>alert('Error: '+e.message));
}

function renderBalanceSheetContent(){
  const container = document.getElementById('balance-sheet-content');
  if(!container) return;

  const teams = window._teams||{};
  const teamsList = Object.entries(teams);
  
  if(!teamsList.length){
    container.innerHTML = `<div class="empty-state">No teams to display</div>`;
    return;
  }

  let html = `<div class="team-balance-sheet">`;
  
  teamsList.forEach(([id, t]) => {
    const spent = t.spent || 0;
    const remaining = t.remaining || t.budget || 0;

    let youth = 0, local = 0, invited = 0;
    ['youth','local','invited'].forEach(cat=>{
      Object.entries(window[`_${cat}`]||{}).forEach(([pid,p])=>{
        if(p.soldTeamId === id) {
          if(cat === 'youth') youth++;
          else if(cat === 'local') local++;
          else invited++;
        }
      });
    });

    const directSign = (window._directSigns||{})[id];
    if(directSign) {
      if(directSign.category === 'youth') youth++;
      else if(directSign.category === 'local') local++;
      else invited++;
    }

    html += `
      <div style="background:#fff; border-radius:12px; border:2px solid var(--gold); padding:20px; margin-bottom:16px; display:grid; grid-template-columns:auto 1fr 1fr 1fr 1fr 1fr 1fr; gap:16px; align-items:center;">
        <div style="text-align:center;">
          ${t.logo ? `<img src="${t.logo}" style="width:60px; height:60px; border-radius:50%; object-fit:cover; border:2px solid var(--gold);"/>` : ''}
        </div>
        <div>
          <div style="font-family:var(--font-display); font-size:16px; color:var(--red); font-weight:700;">${t.name}</div>
          <div style="color:#666; font-size:11px;">Captain: ${t.cap}</div>
        </div>
        <div style="text-align:center;">
          <div style="color:#666; font-size:10px; text-transform:uppercase; font-weight:700; margin-bottom:4px;">Youth</div>
          <div style="font-family:var(--font-display); font-size:18px; color:var(--red);">${youth}</div>
        </div>
        <div style="text-align:center;">
          <div style="color:#666; font-size:10px; text-transform:uppercase; font-weight:700; margin-bottom:4px;">Local</div>
          <div style="font-family:var(--font-display); font-size:18px; color:var(--red);">${local}</div>
        </div>
        <div style="text-align:center;">
          <div style="color:#666; font-size:10px; text-transform:uppercase; font-weight:700; margin-bottom:4px;">Invited</div>
          <div style="font-family:var(--font-display); font-size:18px; color:var(--red);">${invited}</div>
        </div>
        <div style="text-align:center;">
          <div style="color:#666; font-size:10px; text-transform:uppercase; font-weight:700; margin-bottom:4px;">Spent</div>
          <div style="font-family:var(--font-display); font-size:16px; color:#dc2626;">${spent.toLocaleString()}</div>
        </div>
        <div style="text-align:center;">
          <div style="color:#666; font-size:10px; text-transform:uppercase; font-weight:700; margin-bottom:4px;">Remaining</div>
          <div style="font-family:var(--font-display); font-size:16px; color:#2e7d32;">${remaining.toLocaleString()}</div>
        </div>
      </div>
    `;
  });

  html += `</div>`;
  container.innerHTML = html;
}

function downloadBalanceSheet(){
  const teams = window._teams||{};
  const teamsList = Object.entries(teams);
  
  if(!teamsList.length){
    alert('No teams to download');
    return;
  }

  const canvas = document.createElement('canvas');
  canvas.width = 1600;
  canvas.height = Math.max(1000, 300 + (teamsList.length * 180));
  const ctx = canvas.getContext('2d');

  const bgGrad = ctx.createLinearGradient(0,0,1600,canvas.height);
  bgGrad.addColorStop(0, '#0a0a0a');
  bgGrad.addColorStop(0.5, '#1a0a0a');
  bgGrad.addColorStop(1, '#0a0a0a');
  ctx.fillStyle = bgGrad;
  ctx.fillRect(0,0,1600,canvas.height);

  ctx.fillStyle = '#D4AF37';
  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 4;
  ctx.strokeRect(20,20,1560,canvas.height-40);

  ctx.fillStyle = '#ffffff';
  ctx.font = 'bold 72px Oswald';
  ctx.textAlign = 'center';
  ctx.fillText('TEAMS BALANCE SHEET', 800, 100);

  ctx.fillStyle = '#D4AF37';
  ctx.font = 'bold 32px Oswald';
  ctx.fillText('LLFC AUCTION 2026', 800, 150);

  ctx.strokeStyle = '#D4AF37';
  ctx.lineWidth = 3;
  ctx.beginPath();
  ctx.moveTo(200, 180);
  ctx.lineTo(1400, 180);
  ctx.stroke();

  let yPos = 250;
  const rowHeight = 160;

  teamsList.forEach(([id, t], idx) => {
    const spent = t.spent || 0;
    const remaining = t.remaining || t.budget || 0;

    let youth = 0, local = 0, invited = 0;
    ['youth','local','invited'].forEach(cat=>{
      Object.entries(window[`_${cat}`]||{}).forEach(([pid,p])=>{
        if(p.soldTeamId === id) {
          if(cat === 'youth') youth++;
          else if(cat === 'local') local++;
          else invited++;
        }
      });
    });

    const directSign = (window._directSigns||{})[id];
    if(directSign) {
      if(directSign.category === 'youth') youth++;
      else if(directSign.category === 'local') local++;
      else invited++;
    }

    const bgColor = idx % 2 === 0 ? 'rgba(255,255,255,0.08)' : 'rgba(208,2,27,0.05)';
    ctx.fillStyle = bgColor;
    ctx.fillRect(40, yPos - 120, 1520, 140);

    ctx.strokeStyle = '#D4AF37';
    ctx.lineWidth = 2;
    ctx.strokeRect(40, yPos - 120, 1520, 140);

    if(t.logo) {
      try {
        const img = new Image();
        img.crossOrigin = 'anonymous';
        img.onload = () => {
          ctx.drawImage(img, 60, yPos - 110, 100, 100);
        };
        img.src = t.logo;
      } catch(e) {}
    }

    ctx.fillStyle = '#ffffff';
    ctx.font = 'bold 36px Oswald';
    ctx.textAlign = 'left';
    ctx.fillText(t.name.toUpperCase(), 180, yPos - 70);

    ctx.fillStyle = '#D4AF37';
    ctx.font = 'bold 18px Oswald';
    ctx.fillText(`Captain: ${t.cap}`, 180, yPos - 35);

    const statLabels = ['YOUTH', 'LOCAL', 'INVITED', 'SPENT', 'REMAINING'];
    const statValues = [youth, local, invited, spent.toLocaleString(), remaining.toLocaleString()];
    const statColors = ['#dbeafe', '#fee2e2', '#fef3c7', '#dc2626', '#2e7d32'];

    let xStatPos = 1000;
    statLabels.forEach((label, i) => {
      ctx.fillStyle = statColors[i];
      ctx.font = 'bold 18px Oswald';
      ctx.textAlign = 'center';
      ctx.fillText(statValues[i], xStatPos, yPos - 60);
      
      ctx.fillStyle = '#999';
      ctx.font = 'bold 12px Oswald';
      ctx.fillText(label, xStatPos, yPos - 30);
      
      xStatPos += 90;
    });

    yPos += rowHeight;
  });

  canvas.toBlob(blob=>{
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'Teams_Balance_Sheet.jpg';
    a.click();
    URL.revokeObjectURL(url);
  }, 'image/jpeg', 0.95);
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
  
  ['youth','local','invited'].forEach(cat=>{
    const table = document.querySelector(`#${cat}-list-view .player-list-table tbody`);
    if(!table) return;
    
    const rows = table.querySelectorAll('tr');
    rows.forEach(row=>{
      const playerNameEl = row.querySelector('.player-name-text');
      if(!playerNameEl) return;
      
      const playerName = playerNameEl.textContent.toLowerCase();
      const matchSearch = !searchText || playerName.includes(searchText);
      const matchCat = !category || cat === category;
      
      if(matchSearch && matchCat){
        row.style.display='';
      } else {
        row.style.display='none';
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