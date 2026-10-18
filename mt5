<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Headway Terminal</title>
<style>
  :root{
    --bg:#f4f6f8; --panel:#ffffff; --line:#e3e7ec; --text:#1a2332; --muted:#6b7785;
    --gold:#d4a017; --gold-soft:#f7edd3; --green:#1f9d55; --red:#d64545; --blue:#1f4e79;
  }
  *{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',system-ui,-apple-system,sans-serif}
  body{background:var(--bg);color:var(--text)}
  header{background:var(--panel);border-bottom:1px solid var(--line);padding:14px 22px;display:flex;align-items:center;gap:18px;flex-wrap:wrap}
  .brand{font-weight:700;font-size:18px;letter-spacing:.5px}
  .brand span{color:var(--gold)}
  .tabs{display:flex;gap:6px;flex-wrap:wrap;margin-left:auto}
  .tab{padding:7px 14px;border-radius:8px;border:1px solid var(--line);background:#fafbfc;cursor:pointer;font-size:13px;font-weight:600;color:var(--muted)}
  .tab.active{background:var(--gold-soft);border-color:var(--gold);color:var(--text)}
  .equity{margin-left:18px;font-size:13px;color:var(--muted)}
  .equity b{color:var(--text);font-size:15px}
  main{padding:18px 22px 40px;max-width:1200px;margin:0 auto}
  .grid{display:grid;grid-template-columns:1.4fr 1fr;gap:16px}
  @media(max-width:900px){.grid{grid-template-columns:1fr}}
  .card{background:var(--panel);border:1px solid var(--line);border-radius:12px;padding:16px}
  .card h2{font-size:14px;color:var(--muted);font-weight:600;margin-bottom:10px;text-transform:uppercase;letter-spacing:.8px}
  canvas{width:100%;height:320px;background:#fbfcfd;border-radius:8px;border:1px solid var(--line)}
  .stats{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-top:14px}
  @media(max-width:700px){.stats{grid-template-columns:repeat(2,1fr)}}
  .stat{background:#fafbfc;border:1px solid var(--line);border-radius:8px;padding:10px}
  .stat .l{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.6px}
  .stat .v{font-size:17px;font-weight:700;margin-top:3px}
  .pos{color:var(--green)} .neg{color:var(--red)}
  table{width:100%;border-collapse:collapse;font-size:13px;margin-top:8px}
  th,td{text-align:left;padding:8px 6px;border-bottom:1px solid var(--line)}
  th{color:var(--muted);font-weight:600;font-size:11px;text-transform:uppercase;letter-spacing:.6px}
  .disclaimer{margin-top:22px;font-size:11px;color:var(--muted);line-height:1.6;border-top:1px solid var(--line);padding-top:12px}
  .wm{position:fixed;right:14px;bottom:12px;font-size:10px;color:#c5cad3;pointer-events:none;z-index:5}
</style>
</head>
<body>
<header>
  <div class="brand">HEADWAY<span>TERMINAL</span></div>
  <div class="tabs" id="tabs"></div>
  <div class="equity">Equity <b id="equityVal">$0.00</b> <span id="equityPnl"></span></div>
</header>
<main>
  <div class="grid">
    <div class="card">
      <h2>Gold / XAUUSD</h2>
      <canvas id="goldChart" width="900" height="320"></canvas>
      <div class="stats" id="goldStats"></div>
    </div>
    <div class="card">
      <h2>Bitcoin / BTCUSD</h2>
      <canvas id="btcChart" width="900" height="320"></canvas>
      <div class="stats" id="btcStats"></div>
    </div>
  </div>
  <div class="card" style="margin-top:16px">
    <h2>Open Positions</h2>
    <table>
      <thead><tr><th>Account</th><th>Symbol</th><th>Side</th><th>Lots</th><th>Entry</th><th>PnL</th></tr></thead>
      <tbody id="posBody"></tbody>
    </table>
  </div>
  <div class="disclaimer">
    DISCLAIMER: This is a personal, non-commercial demo project created for educational and portfolio-display purposes only.
    It is not affiliated with, endorsed by, or connected to Headway or any other broker.
    All trading data, prices, and figures shown are simulated and fictional.
    The author does not claim ownership of any third-party trademarks or branding used herein.
    Nothing herein constitutes financial, investment, or trading advice. Trading involves substantial risk of loss.
  </div>
</main>
<div class="wm">personal demo · not affiliated with Headway</div>
<script>
const ACCOUNTS = [
  {id:"10000001",name:"Client",ccy:"USD",balance:1000,   startEquity:1000,   goldLots:0.10, goldSide:"buy",  goldEntry:2650.0, btcLots:0.02, btcSide:"buy",  btcEntry:61000},
  {id:"10000002",name:"Client",ccy:"USD",balance:6500,   startEquity:6500,   goldLots:0.50, goldSide:"buy",  goldEntry:2640.0, btcLots:0.10, btcSide:"sell", btcEntry:62500},
  {id:"10000003",name:"Client",ccy:"USD",balance:500,    startEquity:500,    goldLots:0.05, goldSide:"sell", goldEntry:2680.0, btcLots:0.01, btcSide:"buy",  btcEntry:60500},
  {id:"10000004",name:"Client",ccy:"USD",balance:4000,   startEquity:4000,   goldLots:0.30, goldSide:"buy",  goldEntry:2635.0, btcLots:0.05, btcSide:"buy",  btcEntry:61200},
  {id:"10000005",name:"Client",ccy:"USD",balance:4234,   startEquity:4234,   goldLots:0.20, goldSide:"sell", goldEntry:2670.0, btcLots:0.03, btcSide:"sell", btcEntry:63000},
  {id:"10000006",name:"Client",ccy:"EUR",balance:3000,   startEquity:3000,   goldLots:0.15, goldSide:"buy",  goldEntry:2645.0, btcLots:0.02, btcSide:"buy",  btcEntry:60800},
  {id:"10000007",name:"Client",ccy:"USD",balance:1000,   startEquity:1000,   goldLots:0.08, goldSide:"sell", goldEntry:2665.0, btcLots:0.01, btcSide:"sell", btcEntry:62000}
];

let state = ACCOUNTS.map(a => ({...a, goldPx:a.goldEntry, btcPx:a.btcEntry, t:0}));
let active = 0;
let goldHist = [ ];

function randn(){let u=0,v=0;while(!u)u=Math.random();while(!v)v=Math.random();return Math.sqrt(-2*Math.log(u))*Math.cos(2*Math.PI*v)}

function tick(){
  for(const s of state){
    s.t++;
    const gDrift = Math.sin(s.t/40)*0.15 + randn()*0.35;
    const bDrift = Math.sin(s.t/55)*40 + randn()*90;
    s.goldPx = Math.max(2400, Math.min(2900, s.goldPx + gDrift));
    s.btcPx  = Math.max(50000, Math.min(75000, s.btcPx + bDrift));
    if(s.t % 2 === 0){ goldHist.push(s.goldPx); btcHist.push(s.btcPx); }
    if(goldHist.length > 300){ goldHist.shift(); btcHist.shift(); }
  }
  render();
}

function pnl(s){
  const gMult = s.goldSide==="buy"?1:-1;
  const bMult = s.btcSide==="buy"?1:-1;
  const g = (s.goldPx - s.goldEntry) * s.goldLots * 100 * gMult;
  const b = (s.btcPx - s.btcEntry) * s.btcLots * 1 * bMult;
  return g + b;
}

function render(){
  const tabs = document.getElementById("tabs");
  tabs.innerHTML = "";
  state.forEach((s,i)=>{
    const t = document.createElement("div");
    t.className = "tab" + (i===active?" active":"");
    t.textContent = s.id;
    t.onclick = ()=>{active=i; render();};
    tabs.appendChild(t);
  });
  const s = state ;
  const p = pnl(s);
  const eq = s.startEquity + p;
  document.getElementById("equityVal").textContent = (s.ccy==="EUR"?"€":"$") + eq.toLocaleString(undefined,{minimumFractionDigits:2,maximumFractionDigits:2});
  const ep = document.getElementById("equityPnl");
  ep.textContent = (p>=0?"+":"") + p.toFixed(2);
  ep.className = p>=0?"pos":"neg";
  drawChart("goldChart", goldHist, 2400, 2900, "#d4a017");
  drawChart("btcChart", btcHist, 50000, 75000, "#1f4e79");
  document.getElementById("goldStats").innerHTML = statBlock("Gold", s.goldEntry, s.goldPx, s.goldLots, s.goldSide);
  document.getElementById("btcStats").innerHTML = statBlock("BTC", s.btcEntry, s.btcPx, s.btcLots, s.btcSide);
  const tb = document.getElementById("posBody");
  tb.innerHTML = "";
  state.forEach((x,i)=>{
    const pp = pnl(x);
    const tr = document.createElement("tr");
    tr.innerHTML = `<td>${x.id}</td><td>XAUUSD / BTCUSD</td><td class="${x.goldSide==="buy"?"pos":"neg"}">${x.goldSide.toUpperCase()} / ${x.btcSide.toUpperCase()}</td><td>${x.goldLots} / ${x.btcLots}</td><td>${x.goldEntry} / ${x.btcEntry}</td><td class="${pp>=0?"pos":"neg"}">${pp>=0?"+":""}${pp.toFixed(2)}</td>`;
    tb.appendChild(tr);
  });
}

function statBlock(label, entry, px, lots, side){
  const diff = px - entry;
  const sign = diff>=0?"+":"";
  const cls = diff>=0?"pos":"neg";
  return `<div class="stat"><div class="l">Entry</div><div class="v">${entry}</div></div>
  <div class="stat"><div class="l">Live</div><div class="v ${cls}">${px.toFixed(label==="Gold"?2:0)}</div></div>
  <div class="stat"><div class="l">Lots / Side</div><div class="v">${lots} ${side.toUpperCase()}</div></div>
  <div class="stat"><div class="l">Unrealized</div><div class="v ${cls}">${sign}${(diff*lots*(label==="Gold"?100:1)).toFixed(2)}</div></div>`;
}

function drawChart(id, data, lo, hi, color){
  const c = document.getElementById(id);
  const ctx = c.getContext("2d");
  const w = c.width, h = c.height;
  ctx.clearRect(0,0,w,h);
  ctx.strokeStyle = "#e3e7ec"; ctx.lineWidth = 1;
  for(let i=1;i<5;i++){const y = h*i/5; ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(w,y); ctx.stroke();}
  if(data.length < 2) return;
  ctx.beginPath();
  data.forEach((v,i)=>{
    const x = i/(data.length-1)*w;
    const y = h - ((v-lo)/(hi-lo))*h;
    if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y);
  });
  ctx.strokeStyle = color; ctx.lineWidth = 2; ctx.stroke();
  const last = data ;
  const y = h - ((last-lo)/(hi-lo))*h;
  ctx.beginPath(); ctx.arc(w-2, y, 3.5, 0, Math.PI*2); ctx.fillStyle = color; ctx.fill();
}

setInterval(tick, 1000);
tick();
</script>
</body>
</html>
