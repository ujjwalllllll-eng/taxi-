<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>YatraCab — Owner Dashboard</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Baloo+2:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet"/>
<style>
:root{
  --s:#FF6B00;--sl:#FF8C38;--sp:#FFF3E8;
  --g:#138808;--gl:#1aaa0a;
  --navy:#0A1628;--navy2:#12233F;--steel:#1E3A5F;
  --muted:#8A9BB0;--border:rgba(255,255,255,0.09);
  --card:rgba(18,35,63,0.9);--gold:#F5C842;--sur:#0E1F38;
  --red:#E53935;--blue:#1565C0;
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;overflow:hidden}
body{font-family:'DM Sans',sans-serif;background:var(--navy);color:#fff;display:flex;height:100vh}
body::before{content:'';position:fixed;inset:0;background:radial-gradient(ellipse 60% 40% at 80% 0%,rgba(255,107,0,0.12),transparent 60%),radial-gradient(ellipse 50% 40% at -5% 90%,rgba(19,136,8,0.08),transparent 60%);pointer-events:none;z-index:0}

/* SIDEBAR */
.sidebar{width:220px;flex-shrink:0;background:var(--navy2);border-right:1px solid var(--border);display:flex;flex-direction:column;z-index:2;position:relative}
.sb-logo{padding:18px 18px 12px;font-family:'Baloo 2',cursive;font-size:20px;font-weight:800;border-bottom:1px solid var(--border)}
.sb-logo span{color:var(--s)}
.sb-logo em{color:var(--gl);font-style:normal}
.sb-logo small{display:block;font-size:10px;font-family:'DM Sans',sans-serif;font-weight:400;color:var(--muted);letter-spacing:0.05em;text-transform:uppercase;margin-top:2px}
.sb-nav{flex:1;overflow-y:auto;padding:10px 8px}
.sb-section{font-size:10px;color:var(--muted);text-transform:uppercase;letter-spacing:0.08em;padding:10px 8px 4px}
.nav-item{display:flex;align-items:center;gap:9px;padding:9px 10px;border-radius:9px;cursor:pointer;font-size:13px;color:rgba(255,255,255,0.7);transition:all 0.15s;margin-bottom:2px;white-space:nowrap}
.nav-item:hover{background:rgba(255,255,255,0.06);color:#fff}
.nav-item.active{background:rgba(255,107,0,0.18);color:var(--sl);font-weight:500}
.nav-item .ni{font-size:14px;width:18px;text-align:center}
.nav-badge{margin-left:auto;background:var(--s);color:#fff;font-size:9px;font-weight:700;padding:2px 6px;border-radius:10px}
.sb-footer{padding:12px;border-top:1px solid var(--border)}
.owner-row{display:flex;align-items:center;gap:9px}
.owner-av{width:34px;height:34px;border-radius:50%;background:linear-gradient(135deg,var(--s),#FF3D00);display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:700;flex-shrink:0}
.owner-name{font-size:13px;font-weight:500}
.owner-role{font-size:11px;color:var(--muted)}

/* MAIN */
.main{flex:1;display:flex;flex-direction:column;overflow:hidden;z-index:1;position:relative}
.topbar{height:54px;flex-shrink:0;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;padding:0 20px;background:rgba(10,22,40,0.7);backdrop-filter:blur(10px)}
.topbar-title{font-family:'Baloo 2',cursive;font-size:18px;font-weight:700}
.topbar-right{display:flex;align-items:center;gap:10px}
.tb-pill{background:rgba(255,255,255,0.06);border:1px solid var(--border);border-radius:8px;padding:5px 11px;font-size:12px;cursor:pointer}
.tb-pill.live{border-color:rgba(26,170,10,0.4);color:var(--gl)}
.tb-pill.live::before{content:'● ';font-size:10px}
.tb-notif{position:relative;cursor:pointer}
.notif-dot{position:absolute;top:-2px;right:-2px;width:8px;height:8px;background:var(--s);border-radius:50%;border:2px solid var(--navy)}
.content{flex:1;overflow-y:auto;overflow-x:hidden;padding:18px 20px}

/* TABS */
.page{display:none}
.page.active{display:block}

/* METRIC CARDS */
.metrics{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:12px;margin-bottom:18px}
.mc{background:var(--card);border:1px solid var(--border);border-radius:13px;padding:14px 16px;position:relative;overflow:hidden}
.mc::before{content:'';position:absolute;top:0;left:0;right:0;height:3px;border-radius:13px 13px 0 0}
.mc.orange::before{background:linear-gradient(90deg,var(--s),var(--sl))}
.mc.green::before{background:linear-gradient(90deg,var(--g),var(--gl))}
.mc.blue::before{background:linear-gradient(90deg,#1565C0,#1976D2)}
.mc.gold::before{background:linear-gradient(90deg,#c79400,var(--gold))}
.mc.red::before{background:linear-gradient(90deg,var(--red),#EF5350)}
.mc-label{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:0.05em;margin-bottom:4px}
.mc-val{font-family:'Baloo 2',cursive;font-size:24px;font-weight:700}
.mc-val.orange{color:var(--sl)}
.mc-val.green{color:var(--gl)}
.mc-val.blue{color:#42A5F5}
.mc-val.gold{color:var(--gold)}
.mc-val.red{color:#EF5350}
.mc-delta{font-size:11px;margin-top:3px;color:var(--muted)}
.mc-delta.up{color:var(--gl)}
.mc-delta.dn{color:#EF5350}

/* CARDS */
.card{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:16px;margin-bottom:14px}
.card-head{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px}
.card-title{font-family:'Baloo 2',cursive;font-size:15px;font-weight:700;display:flex;align-items:center;gap:7px}
.card-title .ci{background:rgba(255,107,0,0.15);border-radius:7px;width:26px;height:26px;display:flex;align-items:center;justify-content:center;font-size:13px}
.card-action{font-size:12px;color:var(--sl);cursor:pointer;background:rgba(255,107,0,0.1);border:1px solid rgba(255,107,0,0.25);padding:4px 11px;border-radius:7px}
.grid2{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.grid3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px}

/* TABLE */
table{width:100%;border-collapse:collapse;font-size:13px}
th{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:0.05em;font-weight:500;padding:0 10px 8px;text-align:left}
td{padding:10px 10px;border-top:1px solid rgba(255,255,255,0.05);vertical-align:middle}
tr:hover td{background:rgba(255,255,255,0.03)}
.status-badge{display:inline-block;padding:3px 9px;border-radius:20px;font-size:11px;font-weight:600;white-space:nowrap}
.s-active{background:rgba(26,170,10,0.15);color:var(--gl)}
.s-idle{background:rgba(245,200,66,0.15);color:var(--gold)}
.s-offline{background:rgba(255,255,255,0.07);color:var(--muted)}
.s-trip{background:rgba(255,107,0,0.15);color:var(--sl)}
.s-done{background:rgba(26,170,10,0.1);color:var(--gl)}
.s-cancel{background:rgba(229,57,53,0.12);color:#EF5350}
.s-pending{background:rgba(245,200,66,0.12);color:var(--gold)}

/* DRIVER ROW */
.dr-av{width:32px;height:32px;border-radius:50%;background:linear-gradient(135deg,var(--s),#FF3D00);display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:700;flex-shrink:0}
.dr-av.green{background:linear-gradient(135deg,var(--g),var(--gl))}
.dr-av.blue{background:linear-gradient(135deg,#1565C0,#1976D2)}
.dr-info{font-size:13px;font-weight:500}
.dr-sub{font-size:11px;color:var(--muted)}

/* CHART */
.chart-wrap{position:relative;width:100%}

/* LIVE MAP */
.live-map{background:rgba(255,255,255,0.03);border-radius:10px;height:220px;display:flex;align-items:center;justify-content:center;border:1px dashed rgba(255,255,255,0.1);position:relative;overflow:hidden}
.road-h{position:absolute;left:10%;right:10%;top:50%;height:3px;background:rgba(255,255,255,0.08);border-radius:2px}
.road-v{position:absolute;top:10%;bottom:10%;left:50%;width:3px;background:rgba(255,255,255,0.08);border-radius:2px}
.map-car{position:absolute;font-size:18px;animation:float 3s ease-in-out infinite}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-6px)}}
.map-pin{position:absolute;font-size:14px}
.map-overlay{position:absolute;top:8px;left:8px;background:rgba(10,22,40,0.85);border:1px solid var(--border);border-radius:8px;padding:6px 10px;font-size:11px}
.map-dot{width:6px;height:6px;background:var(--gl);border-radius:50%;display:inline-block;animation:pulse 1.5s infinite;margin-right:4px}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:0.3}}

/* FORMS */
input,select,textarea{background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.1);border-radius:9px;padding:9px 12px;color:#fff;font-family:'DM Sans',sans-serif;font-size:13px;outline:none;transition:border-color 0.2s;width:100%}
input:focus,select:focus{border-color:var(--s)}
select option{background:#1a2e50}
label{font-size:11px;color:var(--muted);display:block;margin-bottom:4px;text-transform:uppercase;letter-spacing:0.04em}
.field{margin-bottom:12px}
.btn{padding:9px 18px;border-radius:9px;border:none;cursor:pointer;font-family:'DM Sans',sans-serif;font-weight:500;font-size:13px}
.btn-primary{background:var(--s);color:#fff}
.btn-secondary{background:rgba(255,255,255,0.07);color:#fff;border:1px solid var(--border)}
.btn-danger{background:rgba(229,57,53,0.2);color:#EF5350;border:1px solid rgba(229,57,53,0.3)}
.btn-success{background:rgba(26,170,10,0.2);color:var(--gl);border:1px solid rgba(26,170,10,0.3)}

/* TOGGLE */
.toggle{position:relative;width:38px;height:21px;flex-shrink:0}
.toggle input{opacity:0;width:0;height:0}
.toggle-slider{position:absolute;inset:0;background:rgba(255,255,255,0.15);border-radius:21px;cursor:pointer;transition:0.2s}
.toggle-slider::before{content:'';position:absolute;width:15px;height:15px;left:3px;top:3px;background:#fff;border-radius:50%;transition:0.2s}
.toggle input:checked + .toggle-slider{background:var(--gl)}
.toggle input:checked + .toggle-slider::before{transform:translateX(17px)}

/* SCROLLBAR */
::-webkit-scrollbar{width:4px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:rgba(255,255,255,0.1);border-radius:4px}

/* ALERT */
.alert{padding:10px 14px;border-radius:9px;font-size:12px;margin-bottom:10px;display:flex;align-items:center;gap:8px}
.alert-warn{background:rgba(245,200,66,0.1);border:1px solid rgba(245,200,66,0.25);color:var(--gold)}
.alert-success{background:rgba(26,170,10,0.1);border:1px solid rgba(26,170,10,0.25);color:var(--gl)}
.alert-danger{background:rgba(229,57,53,0.1);border:1px solid rgba(229,57,53,0.25);color:#EF5350}

/* PROGRESS */
.prog-bar{height:6px;background:rgba(255,255,255,0.07);border-radius:3px;overflow:hidden;margin-top:6px}
.prog-fill{height:100%;border-radius:3px;transition:width 0.5s}

/* STAR */
.stars{color:var(--gold);font-size:12px}

/* SCROLLABLE TABLE */
.tbl-wrap{overflow-x:auto}

@media(max-width:800px){
  .sidebar{display:none}
  .grid2,.grid3{grid-template-columns:1fr}
}
</style>
</head>
<body>

<!-- SIDEBAR -->
<div class="sidebar">
  <div class="sb-logo">
    <span>Yatra</span><em>Cab</em> 🚖
    <small>Owner Control Panel</small>
  </div>
  <nav class="sb-nav">
    <div class="sb-section">Overview</div>
    <div class="nav-item active" onclick="nav(this,'dashboard')"><span class="ni">📊</span> Dashboard</div>
    <div class="nav-item" onclick="nav(this,'live')"><span class="ni">🗺️</span> Live Map <span class="nav-badge">12</span></div>
    <div class="sb-section">Operations</div>
    <div class="nav-item" onclick="nav(this,'bookings')"><span class="ni">📋</span> All Bookings <span class="nav-badge">3</span></div>
    <div class="nav-item" onclick="nav(this,'drivers')"><span class="ni">👨‍✈️</span> Drivers</div>
    <div class="nav-item" onclick="nav(this,'vehicles')"><span class="ni">🚗</span> Fleet</div>
    <div class="nav-item" onclick="nav(this,'customers')"><span class="ni">👥</span> Customers</div>
    <div class="sb-section">Finance</div>
    <div class="nav-item" onclick="nav(this,'revenue')"><span class="ni">💰</span> Revenue</div>
    <div class="nav-item" onclick="nav(this,'payouts')"><span class="ni">💸</span> Payouts</div>
    <div class="sb-section">Control</div>
    <div class="nav-item" onclick="nav(this,'pricing')"><span class="ni">🏷️</span> Pricing</div>
    <div class="nav-item" onclick="nav(this,'promos')"><span class="ni">🎟️</span> Promos</div>
    <div class="nav-item" onclick="nav(this,'alerts')"><span class="ni">🔔</span> Alerts <span class="nav-badge" style="background:var(--red)">5</span></div>
    <div class="nav-item" onclick="nav(this,'settings')"><span class="ni">⚙️</span> Settings</div>
  </nav>
  <div class="sb-footer">
    <div class="owner-row">
      <div class="owner-av">RP</div>
      <div>
        <div class="owner-name">Rajesh Patel</div>
        <div class="owner-role">Owner · Super Admin</div>
      </div>
    </div>
  </div>
</div>

<!-- MAIN -->
<div class="main">
  <div class="topbar">
    <div class="topbar-title" id="page-title">Dashboard Overview</div>
    <div class="topbar-right">
      <div class="tb-pill live">847 drivers online</div>
      <div class="tb-pill">📍 Surat, Gujarat</div>
      <div class="tb-pill" id="clock">--:--</div>
      <div class="tb-notif" onclick="nav(document.querySelector('[onclick*=alerts]'),'alerts')">
        🔔<div class="notif-dot"></div>
      </div>
    </div>
  </div>

  <div class="content">

    <!-- DASHBOARD PAGE -->
    <div class="page active" id="page-dashboard">
      <div class="metrics">
        <div class="mc orange"><div class="mc-label">Today's Revenue</div><div class="mc-val orange">₹1,24,800</div><div class="mc-delta up">↑ 18% vs yesterday</div></div>
        <div class="mc green"><div class="mc-label">Active Trips</div><div class="mc-val green">47</div><div class="mc-delta up">↑ 12 from 1hr ago</div></div>
        <div class="mc blue"><div class="mc-label">Drivers Online</div><div class="mc-val blue">312</div><div class="mc-delta">of 420 total</div></div>
        <div class="mc gold"><div class="mc-label">Bookings Today</div><div class="mc-val gold">284</div><div class="mc-delta up">↑ 34 from yesterday</div></div>
        <div class="mc red"><div class="mc-label">Cancellations</div><div class="mc-val red">19</div><div class="mc-delta dn">↑ 3 from yesterday</div></div>
        <div class="mc green"><div class="mc-label">Avg Rating</div><div class="mc-val green">4.82</div><div class="mc-delta up">↑ 0.04 this week</div></div>
      </div>

      <div class="grid2">
        <div class="card">
          <div class="card-head">
            <div class="card-title"><div class="ci">📈</div> Revenue — Last 7 Days</div>
          </div>
          <div class="chart-wrap" style="height:180px"><canvas id="revChart" role="img" aria-label="Revenue last 7 days">Revenue trend over 7 days.</canvas></div>
        </div>
        <div class="card">
          <div class="card-head">
            <div class="card-title"><div class="ci">🚕</div> Trips by Vehicle</div>
          </div>
          <div class="chart-wrap" style="height:180px"><canvas id="vehicleChart" role="img" aria-label="Trips by vehicle type">Trips by vehicle type pie chart.</canvas></div>
        </div>
      </div>

      <div class="grid2">
        <div class="card">
          <div class="card-head">
            <div class="card-title"><div class="ci">⚡</div> Live Activity</div>
            <div class="card-action">Refresh</div>
          </div>
          <div style="display:flex;flex-direction:column;gap:8px;">
            <div class="alert alert-success"><span>✅</span> Booking YC-9242 confirmed — Sedan · Surat→Ahmedabad · ₹3,650</div>
            <div class="alert alert-warn"><span>⚠️</span> Driver #D-041 idle for 40 min — Adajan area</div>
            <div class="alert alert-danger"><span>❌</span> Booking YC-9238 cancelled by customer</div>
            <div class="alert alert-success"><span>✅</span> Payout ₹12,400 processed — 8 drivers</div>
            <div class="alert alert-warn"><span>⚠️</span> Vehicle GJ05-AB-1234 service due in 200 km</div>
          </div>
        </div>
        <div class="card">
          <div class="card-head">
            <div class="card-title"><div class="ci">🏆</div> Top Routes Today</div>
          </div>
          <div style="display:flex;flex-direction:column;gap:10px">
            <div id="routes-list"></div>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-head">
          <div class="card-title"><div class="ci">👨‍✈️</div> Top Drivers Today</div>
          <div class="card-action" onclick="nav(document.querySelector('[onclick*=drivers]'),'drivers')">View All →</div>
        </div>
        <div class="tbl-wrap">
          <table>
            <thead><tr><th>Driver</th><th>Trips</th><th>Revenue</th><th>Rating</th><th>Status</th><th>Action</th></tr></thead>
            <tbody id="top-drivers"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- LIVE MAP PAGE -->
    <div class="page" id="page-live">
      <div class="metrics">
        <div class="mc green"><div class="mc-label">On Trip</div><div class="mc-val green">47</div></div>
        <div class="mc gold"><div class="mc-label">Available</div><div class="mc-val gold">265</div></div>
        <div class="mc orange"><div class="mc-label">Heading to Pickup</div><div class="mc-val orange">38</div></div>
        <div class="mc red"><div class="mc-label">Offline</div><div class="mc-val red">108</div></div>
      </div>
      <div class="card">
        <div class="card-head">
          <div class="card-title"><div class="ci">🗺️</div> Live Fleet Map — Surat</div>
          <div class="card-action">Full Screen</div>
        </div>
        <div class="live-map">
          <div class="road-h"></div>
          <div class="road-v"></div>
          <div class="map-car" style="left:20%;top:25%;animation-delay:0s">🚕</div>
          <div class="map-car" style="left:60%;top:20%;animation-delay:0.4s">🚖</div>
          <div class="map-car" style="left:40%;top:55%;animation-delay:0.8s">🚗</div>
          <div class="map-car" style="left:70%;top:60%;animation-delay:1.2s">🚕</div>
          <div class="map-car" style="left:15%;top:65%;animation-delay:0.6s">🚙</div>
          <div class="map-car" style="left:80%;top:35%;animation-delay:1.5s">🚕</div>
          <div class="map-car" style="left:50%;top:40%;animation-delay:0.2s">🚖</div>
          <div class="map-pin" style="left:30%;top:30%">📍</div>
          <div class="map-pin" style="left:65%;top:50%">📍</div>
          <div class="map-overlay"><span class="map-dot"></span>Live · 12 active trips in view</div>
        </div>
        <div style="margin-top:14px;display:flex;gap:10px;flex-wrap:wrap">
          <span style="font-size:12px;color:var(--muted)">Filter:</span>
          <button class="btn btn-primary" style="font-size:11px;padding:5px 12px">All</button>
          <button class="btn btn-secondary" style="font-size:11px;padding:5px 12px">On Trip</button>
          <button class="btn btn-secondary" style="font-size:11px;padding:5px 12px">Available</button>
          <button class="btn btn-secondary" style="font-size:11px;padding:5px 12px">Offline</button>
          <button class="btn btn-secondary" style="font-size:11px;padding:5px 12px">SOS Alert</button>
        </div>
      </div>
      <div class="card">
        <div class="card-head"><div class="card-title"><div class="ci">🚨</div> SOS & Emergency</div></div>
        <div class="alert alert-danger" style="margin-bottom:8px"><span>🆘</span> No active SOS alerts — all drivers safe</div>
        <div style="display:flex;gap:8px;flex-wrap:wrap">
          <button class="btn btn-danger">📢 Broadcast Alert to All Drivers</button>
          <button class="btn btn-secondary">📞 Emergency Coordinator</button>
          <button class="btn btn-secondary">🚔 Contact Police</button>
        </div>
      </div>
    </div>

    <!-- BOOKINGS PAGE -->
    <div class="page" id="page-bookings">
      <div class="metrics">
        <div class="mc orange"><div class="mc-label">Total Today</div><div class="mc-val orange">284</div></div>
        <div class="mc green"><div class="mc-label">Completed</div><div class="mc-val green">218</div></div>
        <div class="mc gold"><div class="mc-label">In Progress</div><div class="mc-val gold">47</div></div>
        <div class="mc red"><div class="mc-label">Cancelled</div><div class="mc-val red">19</div></div>
      </div>
      <div class="card">
        <div class="card-head">
          <div class="card-title"><div class="ci">📋</div> All Bookings</div>
          <div style="display:flex;gap:8px">
            <select style="width:auto;font-size:12px;padding:5px 10px"><option>All Status</option><option>Active</option><option>Completed</option><option>Cancelled</option></select>
            <input type="text" placeholder="Search booking ID..." style="width:180px;font-size:12px;padding:5px 10px"/>
          </div>
        </div>
        <div class="tbl-wrap">
          <table>
            <thead><tr><th>Booking ID</th><th>Customer</th><th>Route</th><th>Driver</th><th>Vehicle</th><th>Fare</th><th>Status</th><th>Actions</th></tr></thead>
            <tbody id="bookings-table"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- DRIVERS PAGE -->
    <div class="page" id="page-drivers">
      <div class="metrics">
        <div class="mc blue"><div class="mc-label">Total Drivers</div><div class="mc-val blue">420</div></div>
        <div class="mc green"><div class="mc-label">Online Now</div><div class="mc-val green">312</div></div>
        <div class="mc orange"><div class="mc-label">Verified</div><div class="mc-val orange">398</div></div>
        <div class="mc red"><div class="mc-label">Suspended</div><div class="mc-val red">7</div></div>
      </div>
      <div class="card">
        <div class="card-head">
          <div class="card-title"><div class="ci">👨‍✈️</div> Driver Management</div>
          <button class="btn btn-primary" style="font-size:12px">+ Add Driver</button>
        </div>
        <div class="tbl-wrap">
          <table>
            <thead><tr><th>Driver</th><th>Phone</th><th>Vehicle</th><th>Trips</th><th>Rating</th><th>Earnings (month)</th><th>Status</th><th>Actions</th></tr></thead>
            <tbody id="drivers-table"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- VEHICLES PAGE -->
    <div class="page" id="page-vehicles">
      <div class="metrics">
        <div class="mc blue"><div class="mc-label">Total Fleet</div><div class="mc-val blue">156</div></div>
        <div class="mc green"><div class="mc-label">Active</div><div class="mc-val green">124</div></div>
        <div class="mc gold"><div class="mc-label">Maintenance</div><div class="mc-val gold">18</div></div>
        <div class="mc red"><div class="mc-label">Inactive</div><div class="mc-val red">14</div></div>
      </div>
      <div class="card">
        <div class="card-head">
          <div class="card-title"><div class="ci">🚗</div> Fleet Management</div>
          <button class="btn btn-primary" style="font-size:12px">+ Add Vehicle</button>
        </div>
        <div class="tbl-wrap">
          <table>
            <thead><tr><th>Vehicle</th><th>Type</th><th>Driver</th><th>Plate No.</th><th>Km Run</th><th>Insurance</th><th>Service Due</th><th>Status</th></tr></thead>
            <tbody id="vehicles-table"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- CUSTOMERS PAGE -->
    <div class="page" id="page-customers">
      <div class="metrics">
        <div class="mc blue"><div class="mc-label">Total Users</div><div class="mc-val blue">12,480</div></div>
        <div class="mc green"><div class="mc-label">Active (30d)</div><div class="mc-val green">3,240</div></div>
        <div class="mc gold"><div class="mc-label">New This Month</div><div class="mc-val gold">842</div></div>
        <div class="mc orange"><div class="mc-label">Avg Trips/User</div><div class="mc-val orange">4.2</div></div>
      </div>
      <div class="card">
        <div class="card-head">
          <div class="card-title"><div class="ci">👥</div> Customer List</div>
          <input type="text" placeholder="Search customer..." style="width:200px;font-size:12px;padding:5px 10px"/>
        </div>
        <div class="tbl-wrap">
          <table>
            <thead><tr><th>Customer</th><th>Phone</th><th>Total Trips</th><th>Total Spent</th><th>Last Ride</th><th>Rating Given</th><th>Actions</th></tr></thead>
            <tbody id="customers-table"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- REVENUE PAGE -->
    <div class="page" id="page-revenue">
      <div class="metrics">
        <div class="mc orange"><div class="mc-label">Today</div><div class="mc-val orange">₹1,24,800</div></div>
        <div class="mc green"><div class="mc-label">This Week</div><div class="mc-val green">₹7,84,200</div></div>
        <div class="mc blue"><div class="mc-label">This Month</div><div class="mc-val blue">₹28,40,000</div></div>
        <div class="mc gold"><div class="mc-label">Platform Commission</div><div class="mc-val gold">₹4,26,000</div></div>
      </div>
      <div class="grid2">
        <div class="card">
          <div class="card-head"><div class="card-title"><div class="ci">📈</div> Monthly Revenue</div></div>
          <div class="chart-wrap" style="height:200px"><canvas id="monthChart" role="img" aria-label="Monthly revenue">Monthly revenue bar chart.</canvas></div>
        </div>
        <div class="card">
          <div class="card-head"><div class="card-title"><div class="ci">🧾</div> Revenue Breakdown</div></div>
          <div style="display:flex;flex-direction:column;gap:8px;font-size:13px">
            <div id="rev-breakdown"></div>
          </div>
        </div>
      </div>
      <div class="card">
        <div class="card-head"><div class="card-title"><div class="ci">📥</div> Export Reports</div></div>
        <div style="display:flex;gap:10px;flex-wrap:wrap">
          <button class="btn btn-secondary" onclick="alert('Downloading PDF...')">📄 Daily Report PDF</button>
          <button class="btn btn-secondary" onclick="alert('Downloading Excel...')">📊 Weekly Excel</button>
          <button class="btn btn-secondary" onclick="alert('Downloading CSV...')">📋 Monthly CSV</button>
          <button class="btn btn-secondary" onclick="alert('Sending to email...')">📧 Email GST Report</button>
        </div>
      </div>
    </div>

    <!-- PAYOUTS PAGE -->
    <div class="page" id="page-payouts">
      <div class="metrics">
        <div class="mc orange"><div class="mc-label">Pending Payouts</div><div class="mc-val orange">₹84,200</div></div>
        <div class="mc green"><div class="mc-label">Paid This Week</div><div class="mc-val green">₹3,48,000</div></div>
        <div class="mc blue"><div class="mc-label">Drivers Pending</div><div class="mc-val blue">38</div></div>
      </div>
      <div class="card">
        <div class="card-head">
          <div class="card-title"><div class="ci">💸</div> Driver Payouts</div>
          <button class="btn btn-success" onclick="alert('Processing all payouts...')">✅ Pay All Pending</button>
        </div>
        <div class="tbl-wrap">
          <table>
            <thead><tr><th>Driver</th><th>Trips</th><th>Gross</th><th>Commission (15%)</th><th>Net Payout</th><th>Method</th><th>Status</th><th>Action</th></tr></thead>
            <tbody id="payouts-table"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- PRICING PAGE -->
    <div class="page" id="page-pricing">
      <div class="card">
        <div class="card-head">
          <div class="card-title"><div class="ci">🏷️</div> Fare Pricing Control</div>
          <button class="btn btn-primary" onclick="alert('Pricing saved!')">💾 Save Changes</button>
        </div>
        <div id="pricing-grid"></div>
      </div>
      <div class="card">
        <div class="card-head"><div class="card-title"><div class="ci">🌙</div> Surge & Special Rates</div></div>
        <div class="grid3" id="surge-grid"></div>
      </div>
    </div>

    <!-- PROMOS PAGE -->
    <div class="page" id="page-promos">
      <div class="card">
        <div class="card-head">
          <div class="card-title"><div class="ci">🎟️</div> Promo Codes</div>
          <button class="btn btn-primary" onclick="alert('Create promo form...')">+ New Promo</button>
        </div>
        <div class="tbl-wrap">
          <table>
            <thead><tr><th>Code</th><th>Discount</th><th>Used / Limit</th><th>Expiry</th><th>Revenue Impact</th><th>Status</th><th>Actions</th></tr></thead>
            <tbody id="promos-table"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- ALERTS PAGE -->
    <div class="page" id="page-alerts">
      <div class="card">
        <div class="card-head"><div class="card-title"><div class="ci">🔔</div> System Alerts & Notifications</div></div>
        <div id="alerts-list"></div>
      </div>
      <div class="card">
        <div class="card-head"><div class="card-title"><div class="ci">📢</div> Send Broadcast</div></div>
        <div class="grid2">
          <div>
            <div class="field"><label>Target Audience</label>
              <select><option>All Drivers</option><option>Online Drivers</option><option>All Customers</option><option>Premium Customers</option><option>Everyone</option></select>
            </div>
            <div class="field"><label>Message</label>
              <textarea rows="3" placeholder="Type your broadcast message..."></textarea>
            </div>
          </div>
          <div>
            <div class="field"><label>Channel</label>
              <div style="display:flex;flex-direction:column;gap:8px;margin-top:4px">
                <label style="display:flex;align-items:center;gap:8px;text-transform:none;letter-spacing:0"><label class="toggle"><input type="checkbox" checked/><span class="toggle-slider"></span></label> Push Notification</label>
                <label style="display:flex;align-items:center;gap:8px;text-transform:none;letter-spacing:0"><label class="toggle"><input type="checkbox" checked/><span class="toggle-slider"></span></label> SMS</label>
                <label style="display:flex;align-items:center;gap:8px;text-transform:none;letter-spacing:0"><label class="toggle"><input type="checkbox"/><span class="toggle-slider"></span></label> Email</label>
                <label style="display:flex;align-items:center;gap:8px;text-transform:none;letter-spacing:0"><label class="toggle"><input type="checkbox"/><span class="toggle-slider"></span></label> WhatsApp</label>
              </div>
            </div>
            <button class="btn btn-primary" style="width:100%;margin-top:10px" onclick="alert('Broadcast sent!')">📢 Send Now</button>
          </div>
        </div>
      </div>
    </div>

    <!-- SETTINGS PAGE -->
    <div class="page" id="page-settings">
      <div class="grid2">
        <div class="card">
          <div class="card-head"><div class="card-title"><div class="ci">🏢</div> Business Info</div></div>
          <div class="field"><label>Company Name</label><input value="YatraCab Pvt. Ltd."/></div>
          <div class="field"><label>GST Number</label><input value="24AABCY1234Z1Z5"/></div>
          <div class="field"><label>Primary City</label><input value="Surat, Gujarat"/></div>
          <div class="field"><label>Support Phone</label><input value="+91 98765 43210"/></div>
          <button class="btn btn-primary" onclick="alert('Saved!')">Save</button>
        </div>
        <div class="card">
          <div class="card-head"><div class="card-title"><div class="ci">🔐</div> Access Control</div></div>
          <div style="display:flex;flex-direction:column;gap:10px;font-size:13px">
            <div style="display:flex;justify-content:space-between;align-items:center">Two-factor auth <label class="toggle"><input type="checkbox" checked/><span class="toggle-slider"></span></label></div>
            <div style="display:flex;justify-content:space-between;align-items:center">Driver GPS tracking <label class="toggle"><input type="checkbox" checked/><span class="toggle-slider"></span></label></div>
            <div style="display:flex;justify-content:space-between;align-items:center">Allow cash payments <label class="toggle"><input type="checkbox" checked/><span class="toggle-slider"></span></label></div>
            <div style="display:flex;justify-content:space-between;align-items:center">Allow cancellations <label class="toggle"><input type="checkbox" checked/><span class="toggle-slider"></span></label></div>
            <div style="display:flex;justify-content:space-between;align-items:center">Surge pricing auto <label class="toggle"><input type="checkbox" checked/><span class="toggle-slider"></span></label></div>
            <div style="display:flex;justify-content:space-between;align-items:center">New driver approval auto <label class="toggle"><input type="checkbox"/><span class="toggle-slider"></span></label></div>
            <div style="display:flex;justify-content:space-between;align-items:center">Night mode routes <label class="toggle"><input type="checkbox" checked/><span class="toggle-slider"></span></label></div>
          </div>
        </div>
        <div class="card">
          <div class="card-head"><div class="card-title"><div class="ci">💳</div> Payment Gateway</div></div>
          <div class="field"><label>Razorpay Key</label><input type="password" value="rzp_live_xxxxxxxxxxxxx"/></div>
          <div class="field"><label>Paytm MID</label><input type="password" value="SURAT00001234567"/></div>
          <div class="field"><label>Settlement Account</label><input value="SBI — 0042XXXXXXXX"/></div>
          <button class="btn btn-primary" onclick="alert('Saved!')">Save</button>
        </div>
        <div class="card">
          <div class="card-head"><div class="card-title"><div class="ci">📱</div> App Config</div></div>
          <div class="field"><label>App Version (Customer)</label><input value="v3.2.1"/></div>
          <div class="field"><label>App Version (Driver)</label><input value="v2.8.4"/></div>
          <div class="field"><label>Force Update Below</label><input value="v3.0.0"/></div>
          <div class="field"><label>Maintenance Mode</label>
            <div style="display:flex;align-items:center;gap:10px;margin-top:6px">
              <label class="toggle"><input type="checkbox"/><span class="toggle-slider"></span></label>
              <span style="font-size:12px;color:var(--muted)">Off — App is live</span>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div><!-- /content -->
</div><!-- /main -->

<script>
const titles={dashboard:'Dashboard Overview',live:'Live Fleet Map',bookings:'All Bookings',drivers:'Driver Management',vehicles:'Fleet Management',customers:'Customer Database',revenue:'Revenue & Analytics',payouts:'Driver Payouts',pricing:'Pricing Control',promos:'Promo Codes',alerts:'Alerts & Broadcast',settings:'System Settings'};

function nav(el,id){
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  if(el) el.classList.add('active');
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById('page-'+id).classList.add('active');
  document.getElementById('page-title').textContent=titles[id]||id;
}

// Clock
setInterval(()=>{
  const n=new Date();
  document.getElementById('clock').textContent=n.toLocaleTimeString('en-IN',{hour:'2-digit',minute:'2-digit'});
},1000);

// ── Charts ──
function initCharts(){
  const revCtx=document.getElementById('revChart').getContext('2d');
  new Chart(revCtx,{type:'line',data:{labels:['Mon','Tue','Wed','Thu','Fri','Sat','Sun'],datasets:[{label:'Revenue',data:[98000,112000,87000,134000,118000,156000,124800],borderColor:'#FF6B00',backgroundColor:'rgba(255,107,0,0.08)',borderWidth:2,pointRadius:3,tension:0.4,fill:true}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{ticks:{color:'#8A9BB0',font:{size:10}},grid:{color:'rgba(255,255,255,0.04)'}},y:{ticks:{color:'#8A9BB0',font:{size:10},callback:v=>'₹'+(v/1000).toFixed(0)+'k'},grid:{color:'rgba(255,255,255,0.04)'}}}}});

  const vcCtx=document.getElementById('vehicleChart').getContext('2d');
  new Chart(vcCtx,{type:'doughnut',data:{labels:['Sedan','Mini','SUV','Premium','Outstation'],datasets:[{data:[42,28,15,8,7],backgroundColor:['#FF6B00','#F5C842','#1565C0','#7B1FA2','#138808'],borderWidth:0}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},cutout:'65%'}});

  const mCtx=document.getElementById('monthChart').getContext('2d');
  new Chart(mCtx,{type:'bar',data:{labels:['Nov','Dec','Jan','Feb','Mar','Apr'],datasets:[{label:'Revenue',data:[1820000,2340000,1980000,2120000,2640000,2840000],backgroundColor:'rgba(255,107,0,0.7)',borderRadius:5}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{ticks:{color:'#8A9BB0',font:{size:10}},grid:{display:false}},y:{ticks:{color:'#8A9BB0',font:{size:10},callback:v=>'₹'+(v/100000).toFixed(1)+'L'},grid:{color:'rgba(255,255,255,0.04)'}}}}});
}

// ── Data ──
const drivers=[
  {id:'D-001',name:'Ramesh Patel',phone:'98765 43210',vehicle:'Swift Dzire (White)',plate:'GJ05-BE-1234',trips:47,rating:4.9,earned:18400,status:'trip'},
  {id:'D-002',name:'Suresh Shah',phone:'97654 32109',vehicle:'Wagon R (Silver)',plate:'GJ05-CD-2345',trips:39,rating:4.7,earned:14200,status:'active'},
  {id:'D-003',name:'Dinesh Desai',phone:'96543 21098',vehicle:'Toyota Etios',plate:'GJ05-EF-3456',trips:31,rating:4.8,earned:12800,status:'idle'},
  {id:'D-004',name:'Hitesh Modi',phone:'95432 10987',vehicle:'Honda Amaze',plate:'GJ05-GH-4567',trips:28,rating:4.6,earned:10400,status:'active'},
  {id:'D-005',name:'Mukesh Joshi',phone:'94321 09876',vehicle:'Maruti Ciaz',plate:'GJ05-IJ-5678',trips:22,rating:4.5,earned:8600,status:'offline'},
  {id:'D-006',name:'Naresh Trivedi',phone:'93210 98765',vehicle:'Innova Crysta',plate:'GJ05-KL-6789',trips:18,rating:4.9,earned:16200,status:'trip'},
];
const bookings=[
  {id:'YC-9242',customer:'Priya Mehta',route:'Surat → Ahmedabad',driver:'Ramesh Patel',vehicle:'Sedan',fare:3650,status:'trip'},
  {id:'YC-9241',customer:'Amit Shah',route:'Surat → Mumbai',driver:'Naresh Trivedi',vehicle:'Innova',fare:4800,status:'trip'},
  {id:'YC-9240',customer:'Ritu Jain',route:'Ring Road → Airport',driver:'Suresh Shah',vehicle:'Mini',fare:420,status:'done'},
  {id:'YC-9239',customer:'Vijay Patel',route:'Adajan → Sarthana',driver:'Dinesh Desai',vehicle:'AC Cab',fare:280,status:'done'},
  {id:'YC-9238',customer:'Kavya Nair',route:'Station → VR Mall',driver:'—',vehicle:'—',fare:150,status:'cancel'},
  {id:'YC-9237',customer:'Rohan Kumar',route:'Surat → Vadodara',driver:'Hitesh Modi',vehicle:'Sedan',fare:1900,status:'done'},
  {id:'YC-9236',customer:'Sneha Rao',route:'City Light → Dumas',driver:'Mukesh Joshi',vehicle:'SUV',fare:380,status:'pending'},
];
const vehicles=[
  {make:'Swift Dzire (White)',type:'Sedan',driver:'Ramesh Patel',plate:'GJ05-BE-1234',km:48200,insure:'Mar 2026',service:'800 km away',status:'active'},
  {make:'Wagon R (Silver)',type:'Mini',driver:'Suresh Shah',plate:'GJ05-CD-2345',km:62100,insure:'Jun 2026',service:'2,100 km',status:'active'},
  {make:'Innova Crysta',type:'SUV',driver:'Naresh Trivedi',plate:'GJ05-KL-6789',km:38900,insure:'Jan 2027',service:'4,500 km',status:'active'},
  {make:'Honda Amaze',type:'Sedan',driver:'Hitesh Modi',plate:'GJ05-GH-4567',km:55400,insure:'Sep 2025',service:'200 km away',status:'maintenance'},
  {make:'Maruti Ciaz',type:'AC Cab',driver:'Mukesh Joshi',plate:'GJ05-IJ-5678',km:71000,insure:'Dec 2025',service:'Due now',status:'maintenance'},
  {make:'Toyota Etios',type:'Sedan',driver:'Unassigned',plate:'GJ05-MN-7890',km:29800,insure:'Jul 2026',service:'3,200 km',status:'active'},
];
const customers=[
  {name:'Priya Mehta',phone:'91234 56789',trips:24,spent:48200,last:'Today',rating:'4.8'},
  {name:'Amit Shah',phone:'92345 67890',trips:18,spent:36400,last:'Yesterday',rating:'5.0'},
  {name:'Ritu Jain',phone:'93456 78901',trips:31,spent:52800,last:'Today',rating:'4.6'},
  {name:'Vijay Patel',phone:'94567 89012',trips:12,spent:22400,last:'2 days ago',rating:'4.9'},
  {name:'Rohan Kumar',phone:'95678 90123',trips:8,spent:14800,last:'1 week ago',rating:'4.7'},
];
const promos=[
  {code:'YATRA50',disc:'50% off base',used:1240,limit:5000,expiry:'31 May 2025',impact:'₹1,24,000',status:'active'},
  {code:'NEWUSER',disc:'₹100 off 1st',used:842,limit:'-',expiry:'Ongoing',impact:'₹84,200',status:'active'},
  {code:'AIRPORT30',disc:'30% off airport',used:340,limit:1000,expiry:'30 Apr 2025',impact:'₹22,800',status:'active'},
  {code:'MONSOON25',disc:'25% all trips',used:0,limit:2000,expiry:'1 Jun 2025',impact:'—',status:'pending'},
  {code:'SUMMER20',disc:'20% off',used:2100,limit:2100,expiry:'15 Apr 2025',impact:'₹38,400',status:'s-cancel'},
];
const alertsData=[
  {type:'danger',icon:'❌',msg:'Driver D-018 has not moved for 2 hours — possible breakdown',time:'2 min ago'},
  {type:'warn',icon:'⚠️',msg:'Vehicle GJ05-IJ-5678 service overdue — restrict until serviced',time:'15 min ago'},
  {type:'danger',icon:'🚨',msg:'5 consecutive cancellations by Driver D-031 — flag for review',time:'32 min ago'},
  {type:'warn',icon:'⚠️',msg:'High demand in Athwa Gate area — consider surge pricing',time:'1 hr ago'},
  {type:'success',icon:'✅',msg:'Payout batch of ₹1,24,000 to 48 drivers completed successfully',time:'2 hr ago'},
  {type:'warn',icon:'⚠️',msg:'Customer complaint — Booking YC-9195 reported rude driver',time:'3 hr ago'},
];

const stMap={trip:'s-trip',active:'s-active',idle:'s-idle',offline:'s-offline',done:'s-done',cancel:'s-cancel',pending:'s-pending',maintenance:'s-idle'};
const stLabel={trip:'On Trip',active:'Available',idle:'Idle',offline:'Offline',done:'Completed',cancel:'Cancelled',pending:'Pending',maintenance:'Maintenance'};

function populate(){
  // Top drivers
  const td=document.getElementById('top-drivers');
  drivers.slice(0,4).forEach(d=>{
    td.innerHTML+=`<tr>
      <td><div style="display:flex;align-items:center;gap:8px"><div class="dr-av">${d.name[0]}</div><div><div class="dr-info">${d.name}</div><div class="dr-sub">${d.id}</div></div></div></td>
      <td>${d.trips}</td>
      <td style="color:var(--sl);font-weight:600">₹${d.earned.toLocaleString('en-IN')}</td>
      <td><span class="stars">★</span> ${d.rating}</td>
      <td><span class="status-badge ${stMap[d.status]}">${stLabel[d.status]}</span></td>
      <td><button class="btn btn-secondary" style="font-size:11px;padding:4px 10px" onclick="alert('Viewing ${d.name}...')">View</button></td>
    </tr>`;
  });

  // Routes
  const routes=[{r:'Surat → Ahmedabad',n:38,rev:138700},{r:'Surat → Mumbai',n:24,rev:115200},{r:'City Loop',n:96,rev:38400},{r:'Airport Transfer',n:18,rev:32400},{r:'Surat → Vadodara',n:21,rev:39900}];
  const rl=document.getElementById('routes-list');
  routes.forEach((r,i)=>{
    const pct=Math.round(r.n/96*100);
    rl.innerHTML+=`<div style="margin-bottom:12px"><div style="display:flex;justify-content:space-between;font-size:13px;margin-bottom:4px"><span>${r.r}</span><span style="color:var(--sl);font-weight:600">₹${r.rev.toLocaleString('en-IN')}</span></div>
    <div style="display:flex;align-items:center;gap:8px"><div class="prog-bar" style="flex:1"><div class="prog-fill" style="width:${pct}%;background:${i===0?'var(--s)':i===2?'var(--gl)':'#1976D2'}"></div></div><span style="font-size:11px;color:var(--muted);width:24px">${r.n}</span></div></div>`;
  });

  // Bookings
  const bt=document.getElementById('bookings-table');
  bookings.forEach(b=>{
    bt.innerHTML+=`<tr>
      <td style="font-family:monospace;font-size:12px;color:var(--sl)">${b.id}</td>
      <td>${b.customer}</td>
      <td style="font-size:12px">${b.route}</td>
      <td style="font-size:12px">${b.driver}</td>
      <td style="font-size:12px">${b.vehicle}</td>
      <td style="color:var(--sl);font-weight:600">₹${b.fare}</td>
      <td><span class="status-badge ${stMap[b.status]}">${stLabel[b.status]}</span></td>
      <td><button class="btn btn-secondary" style="font-size:11px;padding:3px 9px" onclick="alert('Viewing booking ${b.id}')">View</button></td>
    </tr>`;
  });

  // Drivers
  const dtt=document.getElementById('drivers-table');
  drivers.forEach(d=>{
    dtt.innerHTML+=`<tr>
      <td><div style="display:flex;align-items:center;gap:8px"><div class="dr-av">${d.name[0]}</div><div><div class="dr-info">${d.name}</div><div class="dr-sub">${d.id}</div></div></div></td>
      <td style="font-size:12px">${d.phone}</td>
      <td style="font-size:12px">${d.vehicle}</td>
      <td>${d.trips}</td>
      <td><span class="stars">★</span> ${d.rating}</td>
      <td style="color:var(--sl);font-weight:600">₹${d.earned.toLocaleString('en-IN')}</td>
      <td><span class="status-badge ${stMap[d.status]}">${stLabel[d.status]}</span></td>
      <td style="display:flex;gap:5px;padding:8px 10px">
        <button class="btn btn-secondary" style="font-size:11px;padding:3px 8px" onclick="alert('Editing ${d.name}')">Edit</button>
        <button class="btn btn-danger" style="font-size:11px;padding:3px 8px" onclick="if(confirm('Suspend ${d.name}?'))alert('Suspended!')">Suspend</button>
      </td>
    </tr>`;
  });

  // Vehicles
  const vt=document.getElementById('vehicles-table');
  vehicles.forEach(v=>{
    const s=v.status==='active'?'s-active':'s-idle';
    vt.innerHTML+=`<tr>
      <td style="font-size:13px;font-weight:500">${v.make}</td>
      <td><span class="status-badge" style="background:rgba(21,101,192,0.15);color:#42A5F5">${v.type}</span></td>
      <td style="font-size:12px">${v.driver}</td>
      <td style="font-family:monospace;font-size:12px">${v.plate}</td>
      <td style="font-size:12px">${v.km.toLocaleString('en-IN')} km</td>
      <td style="font-size:12px">${v.insure}</td>
      <td style="font-size:12px;color:${v.service.includes('Due')||v.service.includes('200')?'#EF5350':'var(--muted)'}">${v.service}</td>
      <td><span class="status-badge ${s}">${stLabel[v.status]}</span></td>
    </tr>`;
  });

  // Customers
  const ct=document.getElementById('customers-table');
  customers.forEach(c=>{
    ct.innerHTML+=`<tr>
      <td><div style="display:flex;align-items:center;gap:8px"><div class="dr-av blue">${c.name[0]}</div><span>${c.name}</span></div></td>
      <td style="font-size:12px">${c.phone}</td>
      <td>${c.trips}</td>
      <td style="color:var(--sl);font-weight:600">₹${c.spent.toLocaleString('en-IN')}</td>
      <td style="font-size:12px;color:var(--muted)">${c.last}</td>
      <td><span class="stars">★</span> ${c.rating}</td>
      <td><button class="btn btn-secondary" style="font-size:11px;padding:3px 9px" onclick="alert('Viewing ${c.name}')">View</button></td>
    </tr>`;
  });

  // Payouts
  const pt=document.getElementById('payouts-table');
  drivers.forEach(d=>{
    const comm=Math.round(d.earned*0.15);
    const net=d.earned-comm;
    pt.innerHTML+=`<tr>
      <td><div style="display:flex;align-items:center;gap:8px"><div class="dr-av">${d.name[0]}</div><span style="font-size:13px">${d.name}</span></div></td>
      <td>${d.trips}</td>
      <td>₹${d.earned.toLocaleString('en-IN')}</td>
      <td style="color:#EF5350">₹${comm.toLocaleString('en-IN')}</td>
      <td style="color:var(--gl);font-weight:600">₹${net.toLocaleString('en-IN')}</td>
      <td style="font-size:12px">UPI</td>
      <td><span class="status-badge s-pending">Pending</span></td>
      <td><button class="btn btn-success" style="font-size:11px;padding:4px 10px" onclick="this.textContent='Paid ✓';this.className='btn btn-secondary';this.disabled=true">Pay Now</button></td>
    </tr>`;
  });

  // Revenue breakdown
  const items=[{l:'Sedan Trips',v:'₹48,200',p:39},{l:'Mini/Hatchback',v:'₹22,400',p:18},{l:'SUV & Premium',v:'₹28,600',p:23},{l:'Outstation',v:'₹18,200',p:15},{l:'Airport',v:'₹7,400',p:6}];
  const rb=document.getElementById('rev-breakdown');
  items.forEach(it=>{
    rb.innerHTML+=`<div style="margin-bottom:10px"><div style="display:flex;justify-content:space-between;font-size:13px;margin-bottom:4px"><span style="color:var(--muted)">${it.l}</span><span style="color:#fff;font-weight:500">${it.v}</span></div>
    <div class="prog-bar"><div class="prog-fill" style="width:${it.p}%;background:var(--s)"></div></div></div>`;
  });

  // Pricing
  const ptypes=[{n:'Mini Taxi',emoji:'🚕',base:9,min:80},{n:'Sedan Taxi',emoji:'🚖',base:12,min:100},{n:'AC Cab',emoji:'🚗',base:15,min:120},{n:'SUV',emoji:'🚘',base:18,min:150},{n:'Premium',emoji:'🚙',base:22,min:200},{n:'Outstation',emoji:'🚕',base:14,min:500}];
  const pg=document.getElementById('pricing-grid');
  pg.innerHTML='<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:12px">';
  ptypes.forEach(p=>{
    pg.innerHTML+=`<div style="background:rgba(255,255,255,0.04);border:1px solid var(--border);border-radius:10px;padding:12px">
      <div style="font-size:15px;margin-bottom:8px">${p.emoji} <strong style="font-size:13px">${p.n}</strong></div>
      <div class="field"><label>Per km (₹)</label><input type="number" value="${p.base}" min="1"/></div>
      <div class="field"><label>Base min fare (₹)</label><input type="number" value="${p.min}" min="50"/></div>
    </div>`;
  });
  pg.innerHTML+='</div>';

  // Surge
  const surges=[{l:'Night (10pm–6am)',val:'1.5x',on:true},{l:'Rain / Storm',val:'2.0x',on:true},{l:'Peak hours',val:'1.3x',on:true},{l:'Festival days',val:'1.8x',on:false},{l:'Holiday surge',val:'2.5x',on:false},{l:'Airport peak',val:'1.4x',on:true}];
  const sg=document.getElementById('surge-grid');
  surges.forEach(s=>{
    sg.innerHTML+=`<div style="background:rgba(255,255,255,0.04);border:1px solid var(--border);border-radius:10px;padding:12px">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
        <span style="font-size:13px;font-weight:500">${s.l}</span>
        <label class="toggle"><input type="checkbox" ${s.on?'checked':''}/><span class="toggle-slider"></span></label>
      </div>
      <div class="field"><label>Multiplier</label><input value="${s.val}"/></div>
    </div>`;
  });

  // Promos
  const pmt=document.getElementById('promos-table');
  promos.forEach(p=>{
    const s=p.status==='active'?'s-active':p.status==='pending'?'s-pending':'s-cancel';
    const sl=p.status==='active'?'Active':p.status==='pending'?'Scheduled':'Expired';
    pmt.innerHTML+=`<tr>
      <td style="font-family:monospace;font-weight:700;color:var(--gold)">${p.code}</td>
      <td style="font-size:12px">${p.disc}</td>
      <td style="font-size:12px">${p.used} / ${p.limit}</td>
      <td style="font-size:12px;color:var(--muted)">${p.expiry}</td>
      <td style="color:#EF5350;font-size:12px">${p.impact}</td>
      <td><span class="status-badge ${s}">${sl}</span></td>
      <td style="display:flex;gap:5px;padding:8px 10px">
        <button class="btn btn-secondary" style="font-size:11px;padding:3px 8px" onclick="alert('Edit promo ${p.code}')">Edit</button>
        <button class="btn btn-danger" style="font-size:11px;padding:3px 8px" onclick="alert('Deactivated!')">Stop</button>
      </td>
    </tr>`;
  });

  // Alerts
  const al=document.getElementById('alerts-list');
  alertsData.forEach(a=>{
    al.innerHTML+=`<div class="alert alert-${a.type}" style="justify-content:space-between;margin-bottom:8px">
      <div style="display:flex;align-items:center;gap:8px"><span>${a.icon}</span><span>${a.msg}</span></div>
      <div style="display:flex;align-items:center;gap:10px;flex-shrink:0">
        <span style="font-size:11px;color:var(--muted)">${a.time}</span>
        <button class="btn btn-secondary" style="font-size:11px;padding:3px 9px" onclick="this.parentElement.parentElement.remove()">Dismiss</button>
      </div>
    </div>`;
  });
}

document.addEventListener('DOMContentLoaded',()=>{
  populate();
  setTimeout(initCharts,100);
});
</script>
</body>
</html>  
