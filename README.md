<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>ResidenceIQ — Smart Society Management</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet" />
<style>
  :root {
    --bg: #0a0f1e;
    --surface: #111827;
    --surface2: #1a2236;
    --border: #1f2d45;
    --accent: #3b82f6;
    --accent2: #06b6d4;
    --gold: #f59e0b;
    --green: #10b981;
    --red: #ef4444;
    --purple: #8b5cf6;
    --text: #f1f5f9;
    --muted: #64748b;
    --muted2: #94a3b8;
    --font-head: 'Syne', sans-serif;
    --font-body: 'DM Sans', sans-serif;
    --radius: 16px;
    --shadow: 0 8px 32px rgba(0,0,0,0.4);
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--font-body);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── SCROLLBAR ── */
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--border); border-radius: 10px; }

  /* ── NOISE TEXTURE OVERLAY ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.035'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 9999;
    opacity: 0.4;
  }

  /* ── LAYOUT ── */
  .app { display: flex; min-height: 100vh; }

  /* ── SIDEBAR ── */
  .sidebar {
    width: 260px;
    background: var(--surface);
    border-right: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 0; left: 0; bottom: 0;
    z-index: 100;
    transition: transform 0.3s ease;
  }
  .sidebar-logo {
    padding: 28px 24px 20px;
    border-bottom: 1px solid var(--border);
  }
  .sidebar-logo .logo-icon {
    width: 42px; height: 42px;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 20px; margin-bottom: 12px;
  }
  .sidebar-logo h1 {
    font-family: var(--font-head);
    font-size: 18px; font-weight: 800;
    letter-spacing: -0.5px;
  }
  .sidebar-logo p { font-size: 11px; color: var(--muted); margin-top: 2px; }

  .sidebar-nav { flex: 1; padding: 16px 12px; overflow-y: auto; }
  .nav-section-label {
    font-size: 10px; font-weight: 600; letter-spacing: 1.5px;
    color: var(--muted); text-transform: uppercase;
    padding: 8px 12px 6px;
  }
  .nav-item {
    display: flex; align-items: center; gap: 12px;
    padding: 11px 14px; border-radius: 10px;
    cursor: pointer; transition: all 0.2s;
    font-size: 14px; font-weight: 500; color: var(--muted2);
    margin-bottom: 2px; position: relative;
  }
  .nav-item:hover { background: var(--surface2); color: var(--text); }
  .nav-item.active {
    background: linear-gradient(135deg, rgba(59,130,246,0.15), rgba(6,182,212,0.08));
    color: var(--accent);
    border: 1px solid rgba(59,130,246,0.2);
  }
  .nav-item .icon { font-size: 18px; width: 22px; text-align: center; }
  .nav-badge {
    margin-left: auto;
    background: var(--red);
    color: #fff;
    font-size: 10px; font-weight: 700;
    padding: 2px 7px; border-radius: 20px;
  }
  .sidebar-footer {
    padding: 16px 12px;
    border-top: 1px solid var(--border);
  }
  .user-card {
    display: flex; align-items: center; gap: 12px;
    padding: 10px 12px; border-radius: 10px;
    background: var(--surface2); cursor: pointer;
  }
  .user-avatar {
    width: 36px; height: 36px; border-radius: 10px;
    background: linear-gradient(135deg, var(--purple), var(--accent));
    display: flex; align-items: center; justify-content: center;
    font-weight: 700; font-size: 14px; flex-shrink: 0;
  }
  .user-info .name { font-size: 13px; font-weight: 600; }
  .user-info .role { font-size: 11px; color: var(--muted); }

  /* ── MAIN ── */
  .main {
    flex: 1;
    margin-left: 260px;
    min-height: 100vh;
    display: flex; flex-direction: column;
  }

  /* ── TOPBAR ── */
  .topbar {
    height: 64px;
    background: rgba(10,15,30,0.8);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; gap: 16px;
    padding: 0 28px;
    position: sticky; top: 0; z-index: 50;
  }
  .topbar-title {
    font-family: var(--font-head);
    font-size: 20px; font-weight: 700; flex: 1;
  }
  .topbar-search {
    display: flex; align-items: center; gap: 10px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px; padding: 8px 14px;
    width: 240px;
  }
  .topbar-search input {
    background: none; border: none; outline: none;
    color: var(--text); font-size: 13px;
    font-family: var(--font-body); width: 100%;
  }
  .topbar-search input::placeholder { color: var(--muted); }
  .topbar-btn {
    width: 38px; height: 38px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    cursor: pointer; font-size: 17px;
    transition: all 0.2s; position: relative;
  }
  .topbar-btn:hover { background: var(--surface2); border-color: var(--accent); }
  .notif-dot {
    position: absolute; top: 6px; right: 6px;
    width: 8px; height: 8px; border-radius: 50%;
    background: var(--red); border: 2px solid var(--bg);
  }
  .hamburger {
    display: none;
    background: none; border: none;
    color: var(--text); font-size: 22px;
    cursor: pointer; padding: 4px;
  }

  /* ── PAGE CONTENT ── */
  .page { display: none; padding: 28px; }
  .page.active { display: block; animation: fadeIn 0.3s ease; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }

  /* ── STAT CARDS ── */
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px; margin-bottom: 28px;
  }
  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 22px;
    position: relative; overflow: hidden;
    transition: transform 0.2s, border-color 0.2s;
  }
  .stat-card:hover { transform: translateY(-2px); border-color: var(--accent); }
  .stat-card::before {
    content: '';
    position: absolute; top: 0; left: 0; right: 0;
    height: 3px;
  }
  .stat-card.blue::before { background: linear-gradient(90deg, var(--accent), var(--accent2)); }
  .stat-card.green::before { background: linear-gradient(90deg, var(--green), #34d399); }
  .stat-card.gold::before { background: linear-gradient(90deg, var(--gold), #fcd34d); }
  .stat-card.red::before { background: linear-gradient(90deg, var(--red), #f87171); }
  .stat-icon {
    width: 44px; height: 44px; border-radius: 12px;
    display: flex; align-items: center; justify-content: center;
    font-size: 20px; margin-bottom: 14px;
  }
  .stat-card.blue .stat-icon { background: rgba(59,130,246,0.12); }
  .stat-card.green .stat-icon { background: rgba(16,185,129,0.12); }
  .stat-card.gold .stat-icon { background: rgba(245,158,11,0.12); }
  .stat-card.red .stat-icon { background: rgba(239,68,68,0.12); }
  .stat-value {
    font-family: var(--font-head);
    font-size: 32px; font-weight: 800;
    line-height: 1; margin-bottom: 4px;
  }
  .stat-label { font-size: 13px; color: var(--muted); }
  .stat-change {
    font-size: 11px; font-weight: 600;
    margin-top: 8px; display: flex; align-items: center; gap: 4px;
  }
  .stat-change.up { color: var(--green); }
  .stat-change.down { color: var(--red); }

  /* ── GRID LAYOUTS ── */
  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px; }
  .three-col { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 20px; margin-bottom: 20px; }
  .one-two { display: grid; grid-template-columns: 1fr 2fr; gap: 20px; margin-bottom: 20px; }

  /* ── CARD ── */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 22px;
  }
  .card-header {
    display: flex; align-items: center; justify-content: space-between;
    margin-bottom: 20px;
  }
  .card-title {
    font-family: var(--font-head);
    font-size: 15px; font-weight: 700;
  }
  .card-action {
    font-size: 12px; color: var(--accent);
    cursor: pointer; font-weight: 500;
  }
  .card-action:hover { text-decoration: underline; }

  /* ── ACTIVITY FEED ── */
  .activity-list { display: flex; flex-direction: column; gap: 12px; }
  .activity-item {
    display: flex; gap: 12px; align-items: flex-start;
    padding: 12px; border-radius: 10px;
    background: var(--surface2); transition: background 0.2s;
  }
  .activity-item:hover { background: var(--border); }
  .activity-dot {
    width: 36px; height: 36px; border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 16px; flex-shrink: 0;
  }
  .activity-dot.blue { background: rgba(59,130,246,0.15); }
  .activity-dot.green { background: rgba(16,185,129,0.15); }
  .activity-dot.gold { background: rgba(245,158,11,0.15); }
  .activity-dot.red { background: rgba(239,68,68,0.15); }
  .activity-dot.purple { background: rgba(139,92,246,0.15); }
  .activity-text { flex: 1; }
  .activity-text strong { font-size: 13px; font-weight: 600; }
  .activity-text p { font-size: 12px; color: var(--muted); margin-top: 2px; }
  .activity-time { font-size: 11px; color: var(--muted); flex-shrink: 0; margin-top: 2px; }

  /* ── QUICK ACTIONS ── */
  .quick-actions { display: grid; grid-template-columns: repeat(4,1fr); gap: 12px; }
  .qa-btn {
    display: flex; flex-direction: column; align-items: center; gap: 8px;
    padding: 18px 10px; border-radius: 12px;
    background: var(--surface2); border: 1px solid var(--border);
    cursor: pointer; transition: all 0.2s; text-align: center;
  }
  .qa-btn:hover { border-color: var(--accent); background: rgba(59,130,246,0.08); }
  .qa-btn .qa-icon { font-size: 24px; }
  .qa-btn span { font-size: 11px; font-weight: 500; color: var(--muted2); line-height: 1.3; }

  /* ── NOTICE BOARD ── */
  .notice-item {
    padding: 14px; border-radius: 10px;
    background: var(--surface2); margin-bottom: 10px;
    border-left: 3px solid var(--accent);
    cursor: pointer; transition: all 0.2s;
  }
  .notice-item:hover { background: var(--border); }
  .notice-item.urgent { border-left-color: var(--red); }
  .notice-item.event { border-left-color: var(--gold); }
  .notice-tag {
    font-size: 10px; font-weight: 700; letter-spacing: 1px;
    text-transform: uppercase; margin-bottom: 5px;
  }
  .notice-item .notice-tag { color: var(--accent); }
  .notice-item.urgent .notice-tag { color: var(--red); }
  .notice-item.event .notice-tag { color: var(--gold); }
  .notice-title { font-size: 13px; font-weight: 600; margin-bottom: 3px; }
  .notice-desc { font-size: 12px; color: var(--muted); }
  .notice-date { font-size: 11px; color: var(--muted); margin-top: 6px; }

  /* ── TABLES ── */
  .table-wrap { overflow-x: auto; }
  table { width: 100%; border-collapse: collapse; }
  thead th {
    text-align: left; font-size: 11px; font-weight: 600;
    letter-spacing: 1px; text-transform: uppercase; color: var(--muted);
    padding: 10px 14px; border-bottom: 1px solid var(--border);
  }
  tbody tr { transition: background 0.15s; cursor: pointer; }
  tbody tr:hover { background: var(--surface2); }
  tbody td { padding: 13px 14px; font-size: 13px; border-bottom: 1px solid rgba(31,45,69,0.5); }
  tbody tr:last-child td { border-bottom: none; }

  /* ── BADGES ── */
  .badge {
    display: inline-flex; align-items: center; gap: 4px;
    padding: 3px 10px; border-radius: 20px;
    font-size: 11px; font-weight: 600;
  }
  .badge-green { background: rgba(16,185,129,0.15); color: var(--green); }
  .badge-red { background: rgba(239,68,68,0.15); color: var(--red); }
  .badge-gold { background: rgba(245,158,11,0.15); color: var(--gold); }
  .badge-blue { background: rgba(59,130,246,0.15); color: var(--accent); }
  .badge-purple { background: rgba(139,92,246,0.15); color: var(--purple); }
  .badge-muted { background: var(--surface2); color: var(--muted2); }

  /* ── DONUT CHART (CSS) ── */
  .donut-wrap {
    display: flex; align-items: center; gap: 24px;
  }
  .donut {
    width: 120px; height: 120px; flex-shrink: 0;
    border-radius: 50%;
    background: conic-gradient(
      var(--green) 0% 68%,
      var(--red) 68% 80%,
      var(--gold) 80% 92%,
      var(--border) 92% 100%
    );
    display: flex; align-items: center; justify-content: center;
    position: relative;
  }
  .donut::after {
    content: ''; position: absolute;
    width: 72px; height: 72px;
    background: var(--surface);
    border-radius: 50%;
  }
  .donut-center {
    position: absolute; z-index: 1;
    text-align: center;
  }
  .donut-center .pct {
    font-family: var(--font-head);
    font-size: 20px; font-weight: 800;
  }
  .donut-center .lbl { font-size: 10px; color: var(--muted); }
  .donut-legend { display: flex; flex-direction: column; gap: 8px; }
  .legend-item { display: flex; align-items: center; gap: 8px; font-size: 12px; }
  .legend-dot { width: 10px; height: 10px; border-radius: 3px; flex-shrink: 0; }

  /* ── BAR CHART (CSS) ── */
  .bar-chart { display: flex; align-items: flex-end; gap: 10px; height: 120px; padding-top: 10px; }
  .bar-col { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 6px; }
  .bar-col .bar-val { font-size: 10px; color: var(--muted); }
  .bar {
    width: 100%; border-radius: 6px 6px 0 0;
    background: linear-gradient(180deg, var(--accent), rgba(59,130,246,0.3));
    transition: height 0.8s cubic-bezier(.4,0,.2,1);
    min-height: 4px;
  }
  .bar:hover { background: linear-gradient(180deg, var(--accent2), var(--accent)); }
  .bar-label { font-size: 10px; color: var(--muted); }

  /* ── FORMS ── */
  .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  .form-group { display: flex; flex-direction: column; gap: 6px; }
  .form-group.full { grid-column: 1/-1; }
  .form-label { font-size: 12px; font-weight: 600; color: var(--muted2); }
  .form-input, .form-select, .form-textarea {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 11px 14px;
    color: var(--text);
    font-family: var(--font-body);
    font-size: 14px;
    outline: none;
    transition: border-color 0.2s;
    width: 100%;
  }
  .form-input:focus, .form-select:focus, .form-textarea:focus {
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(59,130,246,0.1);
  }
  .form-textarea { resize: vertical; min-height: 90px; }
  .form-select { appearance: none; cursor: pointer; }
  option { background: var(--surface2); }

  /* ── BUTTONS ── */
  .btn {
    display: inline-flex; align-items: center; gap: 8px;
    padding: 10px 20px; border-radius: 10px;
    font-family: var(--font-body); font-size: 14px; font-weight: 600;
    cursor: pointer; transition: all 0.2s; border: none;
  }
  .btn-primary {
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    color: #fff;
  }
  .btn-primary:hover { opacity: 0.9; transform: translateY(-1px); box-shadow: 0 4px 16px rgba(59,130,246,0.3); }
  .btn-outline {
    background: none; border: 1px solid var(--border);
    color: var(--text);
  }
  .btn-outline:hover { border-color: var(--accent); color: var(--accent); }
  .btn-danger { background: rgba(239,68,68,0.15); color: var(--red); border: 1px solid rgba(239,68,68,0.3); }
  .btn-danger:hover { background: rgba(239,68,68,0.25); }
  .btn-sm { padding: 7px 14px; font-size: 12px; border-radius: 8px; }
  .btn-icon { padding: 8px; border-radius: 8px; }

  /* ── MAINTENANCE CARDS ── */
  .complaint-card {
    background: var(--surface2); border: 1px solid var(--border);
    border-radius: 12px; padding: 16px; margin-bottom: 12px;
    transition: all 0.2s; cursor: pointer;
  }
  .complaint-card:hover { border-color: var(--accent); }
  .complaint-head { display: flex; align-items: center; gap: 12px; margin-bottom: 8px; }
  .complaint-head .flat { font-size: 12px; font-weight: 700; color: var(--accent); }
  .complaint-title { font-size: 14px; font-weight: 600; flex: 1; }
  .complaint-desc { font-size: 12px; color: var(--muted); margin-bottom: 10px; line-height: 1.5; }
  .complaint-foot { display: flex; align-items: center; gap: 8px; }
  .complaint-foot .date { font-size: 11px; color: var(--muted); margin-left: auto; }
  .priority-dot {
    width: 8px; height: 8px; border-radius: 50%;
  }
  .priority-dot.high { background: var(--red); }
  .priority-dot.medium { background: var(--gold); }
  .priority-dot.low { background: var(--green); }

  /* ── VISITOR PASS ── */
  .visitor-card {
    background: var(--surface2); border: 1px solid var(--border);
    border-radius: 12px; padding: 16px;
    display: flex; align-items: center; gap: 14px;
    margin-bottom: 10px; transition: all 0.2s; cursor: pointer;
  }
  .visitor-card:hover { border-color: var(--accent2); }
  .visitor-avatar {
    width: 44px; height: 44px; border-radius: 12px;
    background: linear-gradient(135deg, #334155, #1e293b);
    display: flex; align-items: center; justify-content: center;
    font-size: 18px; flex-shrink: 0;
  }
  .visitor-info { flex: 1; }
  .visitor-info .vname { font-size: 14px; font-weight: 600; }
  .visitor-info .vmeta { font-size: 12px; color: var(--muted); margin-top: 2px; }
  .visitor-actions { display: flex; gap: 8px; }

  /* ── AMENITY BOOKING ── */
  .amenity-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: 14px; }
  .amenity-card {
    background: var(--surface2); border: 1px solid var(--border);
    border-radius: 12px; padding: 18px; text-align: center;
    cursor: pointer; transition: all 0.2s;
  }
  .amenity-card:hover { border-color: var(--accent); transform: translateY(-2px); }
  .amenity-card.booked { border-color: var(--green); background: rgba(16,185,129,0.06); }
  .amenity-icon { font-size: 32px; margin-bottom: 10px; }
  .amenity-name { font-size: 13px; font-weight: 700; margin-bottom: 4px; }
  .amenity-avail { font-size: 11px; color: var(--muted); }
  .amenity-slot {
    margin-top: 10px; padding: 6px 12px;
    border-radius: 20px; font-size: 11px; font-weight: 600;
    background: rgba(59,130,246,0.1); color: var(--accent);
    display: inline-block;
  }

  /* ── POLL ── */
  .poll-option {
    margin-bottom: 10px;
  }
  .poll-label { display: flex; justify-content: space-between; font-size: 13px; margin-bottom: 5px; }
  .poll-bar-bg {
    height: 8px; border-radius: 20px; background: var(--surface2); overflow: hidden;
  }
  .poll-bar-fill {
    height: 100%; border-radius: 20px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    transition: width 1s ease;
  }

  /* ── PAYMENT TABLE ── */
  .payment-row { display: flex; align-items: center; gap: 12px; padding: 12px; border-radius: 10px; background: var(--surface2); margin-bottom: 8px; }
  .payment-row:hover { background: var(--border); }
  .payment-icon { width: 36px; height: 36px; border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 16px; flex-shrink: 0; }
  .payment-info { flex: 1; }
  .payment-info .ptitle { font-size: 13px; font-weight: 600; }
  .payment-info .pmeta { font-size: 11px; color: var(--muted); }
  .payment-amount { font-family: var(--font-head); font-size: 16px; font-weight: 700; text-align: right; }
  .payment-amount .pamount { display: block; }
  .payment-amount .pdate { font-size: 11px; color: var(--muted); font-family: var(--font-body); font-weight: 400
