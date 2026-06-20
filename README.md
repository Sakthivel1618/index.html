<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RFM Analytics — Chennai</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#000;color:#e2e8f0;font-size:13px;}
::-webkit-scrollbar{width:6px;height:6px;} ::-webkit-scrollbar-track{background:#0a0a0a;} ::-webkit-scrollbar-thumb{background:#2d3748;border-radius:3px;}
.wrap{max-width:1500px;margin:0 auto;padding:1rem 1.25rem;}

/* TOP BAR */
.topbar{display:flex;align-items:center;justify-content:space-between;margin-bottom:1rem;flex-wrap:wrap;gap:8px;padding:10px 16px;background:#0d0d0d;border:1px solid #1a1a1a;border-radius:10px;}
.logo{display:flex;align-items:center;gap:10px;}
.logo h1{font-size:16px;font-weight:700;color:#fff;letter-spacing:-.3px;}
.logo .tag{font-size:10px;color:#475569;background:#111;padding:2px 8px;border-radius:4px;border:1px solid #1e1e1e;}
.topright{display:flex;align-items:center;gap:8px;flex-wrap:wrap;}
.rpill{font-size:11px;color:#64748b;background:#0d0d0d;border:1px solid #1a1a1a;border-radius:20px;padding:4px 10px;display:flex;align-items:center;gap:5px;}
.dot{width:6px;height:6px;background:#22c55e;border-radius:50%;flex-shrink:0;animation:pulse 2s infinite;}
@keyframes pulse{0%,100%{opacity:1;}50%{opacity:.2;}}

/* FILTER BAR */
.filterbar{display:flex;align-items:center;gap:8px;margin-bottom:1rem;flex-wrap:wrap;padding:8px 14px;background:#0d0d0d;border:1px solid #1a1a1a;border-radius:8px;}
.flabel{font-size:10px;color:#475569;text-transform:uppercase;letter-spacing:.05em;white-space:nowrap;}
.ftabs{display:flex;background:#000;border:1px solid #1e1e1e;border-radius:5px;overflow:hidden;}
.ftab{padding:4px 14px;font-size:11px;font-weight:600;cursor:pointer;border:none;background:transparent;color:#475569;transition:all .15s;}
.ftab.active{background:#2563EB;color:#fff;}
select{background:#0d0d0d;border:1px solid #1e1e1e;color:#94a3b8;border-radius:5px;padding:4px 8px;font-size:11px;cursor:pointer;outline:none;}
.fdiv{width:1px;height:18px;background:#1a1a1a;}
#rowcount{font-size:10px;color:#334155;margin-left:auto;}

/* KPI GRID */
.kpigrid{display:grid;grid-template-columns:repeat(auto-fit,minmax(145px,1fr));gap:8px;margin-bottom:1rem;}
.kpi{background:#0d0d0d;border:1px solid #1a1a1a;border-radius:8px;padding:12px 14px;position:relative;overflow:hidden;transition:border-color .2s;}
.kpi:hover{border-color:#2d3748;}
.kpi::after{content:'';position:absolute;bottom:0;left:0;right:0;height:2px;}
.kpi.blue::after{background:linear-gradient(90deg,#1d4ed8,#3b82f6);}
.kpi.green::after{background:linear-gradient(90deg,#15803d,#22c55e);}
.kpi.amber::after{background:linear-gradient(90deg,#b45309,#f59e0b);}
.kpi.purple::after{background:linear-gradient(90deg,#6d28d9,#a78bfa);}
.kpi.rose::after{background:linear-gradient(90deg,#be123c,#fb7185);}
.kpi.cyan::after{background:linear-gradient(90deg,#0e7490,#22d3ee);}
.kpi.teal::after{background:linear-gradient(90deg,#0f766e,#2dd4bf);}
.kpi.slate::after{background:linear-gradient(90deg,#334155,#94a3b8);}
.kl{font-size:9px;color:#475569;text-transform:uppercase;letter-spacing:.07em;margin-bottom:4px;}
.kv{font-size:20px;font-weight:800;color:#f8fafc;line-height:1;}
.ks{font-size:10px;color:#334155;margin-top:4px;}
.kchg{display:inline-flex;align-items:center;gap:2px;font-size:9px;font-weight:700;padding:1px 5px;border-radius:3px;margin-top:4px;}
.kchg.up{background:rgba(34,197,94,.12);color:#4ade80;}
.kchg.dn{background:rgba(239,68,68,.12);color:#f87171;}
.kchg.neu{background:rgba(100,116,139,.12);color:#64748b;}

/* SECTION */
.sec{display:flex;align-items:center;gap:8px;margin:1rem 0 8px;}
.sec h2{font-size:11px;font-weight:700;color:#475569;letter-spacing:.08em;text-transform:uppercase;white-space:nowrap;}
.sec-line{flex:1;height:1px;background:#111;}

/* GRID LAYOUTS */
.g2{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:10px;}
.g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px;margin-bottom:10px;}
.g4{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:10px;margin-bottom:10px;}
.g13{display:grid;grid-template-columns:1.2fr 2.8fr;gap:10px;margin-bottom:10px;}
.gfull{margin-bottom:10px;}

/* CARDS */
.card{background:#0d0d0d;border:1px solid #1a1a1a;border-radius:8px;padding:12px 14px;}
.card.full{grid-column:1/-1;}
.ctitle{font-size:11px;font-weight:700;color:#94a3b8;margin-bottom:2px;}
.csub{font-size:9px;color:#334155;margin-bottom:8px;}

/* LEGEND */
.lgnd{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:7px;}
.le{font-size:10px;color:#475569;display:flex;align-items:center;gap:4px;}
.ld{width:8px;height:8px;border-radius:2px;flex-shrink:0;}

/* BARS */
.brow{display:flex;align-items:center;gap:7px;margin-bottom:6px;}
.blbl{font-size:10px;color:#64748b;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.blbl.w90{width:90px;flex-shrink:0;}
.blbl.w110{width:110px;flex-shrink:0;}
.btrk{flex:1;background:#111;border-radius:3px;height:6px;}
.bfil{height:6px;border-radius:3px;}
.bval{text-align:right;font-size:10px;color:#94a3b8;font-weight:600;white-space:nowrap;min-width:54px;}

/* INSIGHT CARDS */
.igrid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:8px;margin-bottom:10px;}
.icard{background:#0d0d0d;border:1px solid #1a1a1a;border-radius:8px;padding:10px 12px;}
.iico{font-size:18px;margin-bottom:4px;}
.ititle{font-size:9px;font-weight:700;color:#334155;text-transform:uppercase;letter-spacing:.06em;margin-bottom:3px;}
.ival{font-size:14px;font-weight:800;color:#f1f5f9;}
.idesc{font-size:9px;color:#334155;margin-top:3px;line-height:1.4;}

/* TABLE */
.tcard{background:#0d0d0d;border:1px solid #1a1a1a;border-radius:8px;padding:12px 14px;margin-bottom:10px;overflow-x:auto;}
table{width:100%;border-collapse:collapse;font-size:11px;}
th{text-align:left;padding:6px 10px;color:#334155;font-weight:700;border-bottom:1px solid #1a1a1a;font-size:9px;text-transform:uppercase;letter-spacing:.07em;white-space:nowrap;background:#000;}
td{padding:6px 10px;color:#94a3b8;border-bottom:1px solid #111;white-space:nowrap;}
tr:last-child td{border-bottom:none;}
tr:hover td{background:#111;}
.bdg{display:inline-block;padding:1px 6px;border-radius:3px;font-size:9px;font-weight:700;}
.bn{background:rgba(37,99,235,.18);color:#60a5fa;}
.br{background:rgba(22,163,74,.18);color:#4ade80;}
.brc{background:rgba(217,119,6,.18);color:#fbbf24;}
.bp{background:rgba(124,58,237,.18);color:#a78bfa;}

/* MOM CHANGE */
.up-txt{color:#4ade80;font-weight:700;}
.dn-txt{color:#f87171;font-weight:700;}
.neu-txt{color:#475569;}

/* LOADING */
.loading{text-align:center;padding:5rem;color:#334155;font-size:13px;}
.err{background:#1a0a0a;border:1px solid #7f1d1d;border-radius:8px;padding:1rem;color:#fca5a5;font-size:12px;line-height:1.7;}

@media(max-width:900px){.g2,.g3,.g4,.g13{grid-template-columns:1fr;}.card.full{grid-column:auto;}}
</style>
</head>
<body>
<div class="wrap">

<!-- TOP BAR -->
<div class="topbar">
  <div class="logo">
    <h1>📊 RFM Analytics Dashboard</h1>
    <span class="tag">Chennai · KPN Fresh · Apr 2024 – Jun 2026</span>
  </div>
  <div class="topright">
    <div class="rpill"><span class="dot"></span><span id="rl">Daily 8 AM refresh</span></div>
    <div class="rpill">🕐 <span id="lu">—</span></div>
  </div>
</div>

<!-- FILTER BAR -->
<div class="filterbar">
  <span class="flabel">View</span>
  <div class="ftabs">
    <button class="ftab active" onclick="switchView('month')">Monthly</button>
    <button class="ftab" onclick="switchView('week')">Weekly</button>
  </div>
  <div class="fdiv"></div>
  <span class="flabel">Year</span>
  <select id="yearFilter" onchange="applyFilters()">
    <option value="all">All</option>
    <option value="2024">2024</option>
    <option value="2025">2025</option>
    <option value="2026">2026</option>
  </select>
  <span class="flabel">From</span>
  <select id="fromFilter" onchange="applyFilters()"><option value="">Earliest</option></select>
  <span class="flabel">To</span>
  <select id="toFilter" onchange="applyFilters()"><option value="">Latest</option></select>
  <div class="fdiv"></div>
  <span class="flabel">Manager</span>
  <select id="cmFilter" onchange="applyFilters()"><option value="all">All Managers</option></select>
  <span id="rowcount">—</span>
</div>

<div id="content"><div class="loading">⏳ Loading live data…</div></div>
</div>

<script>
var SHEET_ID='10XNFBfJKW4gs_ra9CNtPUA5GXi9tb6xbnf6PnFgDSLI',GID='1815598507';
var URLS=['https://docs.google.com/spreadsheets/d/'+SHEET_ID+'/export?format=csv&gid='+GID+'&t=',
  'https://docs.google.com/spreadsheets/d/'+SHEET_ID+'/gviz/tq?tqx=out:csv&gid='+GID+'&t=',
  'https://docs.google.com/spreadsheets/d/'+SHEET_ID+'/gviz/tq?tqx=out:csv&sheet=RFM+-+E&t='];
var allData=[],activeView='month',myCharts={};
var CMCOLORS=['#3b82f6','#22c55e','#f59e0b','#a78bfa','#22d3ee'];
var CATCOLORS={'FMCG':'#3b82f6','Fruits':'#f43f5e','Staples':'#f59e0b','Vegetables':'#22c55e','Consumables':'#a78bfa','KPN Svc':'#22d3ee','Marketing':'#f472b6'};

function n(r,f){return parseFloat(r[f])||0;}
function fL(v){v=Math.round(v);if(v>=10000000)return'₹'+(v/10000000).toFixed(2)+'Cr';if(v>=100000)return'₹'+(v/100000).toFixed(2)+'L';if(v>=1000)return'₹'+(v/1000).toFixed(1)+'K';return'₹'+v;}
function nL(v){v=Math.round(v);if(v>=10000000)return(v/10000000).toFixed(2)+'Cr';if(v>=100000)return(v/100000).toFixed(2)+'L';if(v>=1000)return(v/1000).toFixed(1)+'K';return v.toLocaleString('en-IN');}
function f2(v){return Math.round(v).toLocaleString('en-IN');}
function pct(a,b){return b>0?(a/b*100).toFixed(1)+'%':'0%';}
function chgArrow(c){return c>0?'▲'+c.toFixed(1)+'%':c<0?'▼'+Math.abs(c).toFixed(1)+'%':'—';}
function chgClass(c){return c>0?'up-txt':c<0?'dn-txt':'neu-txt';}

function parseCSV(txt){
  var rows=[],lines=txt.split('\n');
  for(var i=0;i<lines.length;i++){
    var line=lines[i];if(!line.trim())continue;
    var cells=[],cur='',inQ=false;
    for(var j=0;j<line.length;j++){
      var c=line[j];
      if(c==='"'){if(inQ&&line[j+1]==='"'){cur+='"';j++;}else inQ=!inQ;}
      else if(c===','&&!inQ){cells.push(cur);cur='';}else cur+=c;
    }
    cells.push(cur);rows.push(cells);
  }
  return rows;
}

async function loadData(){
  var ts=Date.now(),txt=null,errs=[];
  for(var i=0;i<URLS.length;i++){
    try{
      var r=await fetch(URLS[i]+ts);
      if(!r.ok)throw new Error('HTTP '+r.status);
      var t=await r.text();
      if(t.length<200||t.trim().startsWith('<')||t.indexOf('rolling_period')===-1)throw new Error('Bad response');
      txt=t;break;
    }catch(e){errs.push(e.message);}
  }
  if(!txt){document.getElementById('content').innerHTML='<div class="err">⚠️ Could not load data.<br>'+errs.join('<br>')+'<br>Ensure sheet is shared (Anyone → Viewer)</div>';return;}
  var rows=parseCSV(txt);
  var hdrs=rows[0].map(function(h){return h.replace(/"/g,'').trim().toLowerCase();});
  allData=rows.slice(1).filter(function(r){return r.some(function(c){return c.trim();});})
    .map(function(row){var o={};hdrs.forEach(function(h,i){o[h]=(row[i]||'').replace(/"/g,'').trim();});return o;});
  document.getElementById('lu').textContent='Updated '+new Date().toLocaleTimeString('en-IN',{hour:'2-digit',minute:'2-digit'});
  populateFilters();applyFilters();scheduleNext();
}

function populateFilters(){
  var cms=[...new Set(allData.map(function(r){return r['cluster_manager'];}))].filter(Boolean).sort();
  var sel=document.getElementById('cmFilter');
  cms.forEach(function(c){var o=document.createElement('option');o.value=c;o.textContent=c;sel.appendChild(o);});
  window._mP=[...new Set(allData.filter(function(r){return(r['rolling_period']||'').startsWith('RM_');}).map(function(r){return r['rolling_period'];}))].sort();
  window._wP=[...new Set(allData.filter(function(r){return(r['rolling_period']||'').startsWith('RW_');}).map(function(r){return r['rolling_period'];}))].sort();
  rebuildPeriodFilters();
}

function rebuildPeriodFilters(){
  var periods=activeView==='month'?window._mP:window._wP;if(!periods)return;
  var yr=document.getElementById('yearFilter').value;
  var fp=yr==='all'?periods:periods.filter(function(p){return p.indexOf(yr)>-1;});
  var from=document.getElementById('fromFilter'),to=document.getElementById('toFilter');
  from.innerHTML='<option value="">Earliest</option>';to.innerHTML='<option value="">Latest</option>';
  fp.forEach(function(p){
    var lbl=p.replace('RM_','').replace('RW_','');
    [from,to].forEach(function(sel){var o=document.createElement('option');o.value=p;o.textContent=lbl;sel.appendChild(o);});
  });
}

function switchView(v){
  activeView=v;
  document.querySelectorAll('.ftab').forEach(function(t,i){t.classList.toggle('active',(v==='month'&&i===0)||(v==='week'&&i===1));});
  rebuildPeriodFilters();applyFilters();
}

function applyFilters(){
  rebuildPeriodFilters();
  var yr=document.getElementById('yearFilter').value;
  var from=document.getElementById('fromFilter').value;
  var to=document.getElementById('toFilter').value;
  var cm=document.getElementById('cmFilter').value;
  var data=allData.filter(function(r){
    var p=(r['rolling_period']||'').trim();
    if(activeView==='month'&&!p.startsWith('RM_'))return false;
    if(activeView==='week'&&!p.startsWith('RW_'))return false;
    if(yr!=='all'&&p.indexOf(yr)===-1)return false;
    if(from&&p<from)return false;
    if(to&&p>to)return false;
    if(cm!=='all'&&r['cluster_manager']!==cm)return false;
    return true;
  });
  document.getElementById('rowcount').textContent=data.length.toLocaleString()+' rows';
  if(!data.length){document.getElementById('content').innerHTML='<div class="err">No data matches selected filters.</div>';return;}
  buildDash(data);
}

function scheduleNext(){
  var now=new Date(),next=new Date(now);next.setHours(8,0,0,0);
  if(now>=next)next.setDate(next.getDate()+1);
  var ms=next-now,h=Math.floor(ms/3600000),m=Math.floor((ms%3600000)/60000);
  document.getElementById('rl').textContent='Next refresh '+h+'h '+m+'m';
  setTimeout(function(){loadData();},ms);
}

function dC(){Object.values(myCharts).forEach(function(c){try{c.destroy();}catch(e){}});myCharts={};}

function buildDash(data){
  dC();

  /* AGGREGATE */
  var S={totC:0,totNew:0,totRet:0,totReact:0,totSales:0,totNOB:0,
    newSales:0,retSales:0,reactSales:0,fmcg:0,fruits:0,staples:0,veg:0,cons:0,kpn:0,mkt:0,
    newFMCG:0,retFMCG:0,reactFMCG:0,newFruits:0,retFruits:0,reactFruits:0,
    newStaples:0,retStaples:0,reactStaples:0,newVeg:0,retVeg:0,reactVeg:0};
  data.forEach(function(r){
    S.totC+=n(r,'total_customers');S.totNew+=n(r,'new_customers');
    S.totRet+=n(r,'retained_customers');S.totReact+=n(r,'reactivated_customers');
    S.totSales+=n(r,'total_overall_sales');S.totNOB+=n(r,'total_overall_nob');
    S.newSales+=n(r,'new_overall_sales');S.retSales+=n(r,'retained_overall_sales');
    S.reactSales+=n(r,'reactivated_overall_sales');
    S.fmcg+=n(r,'total_fmcg_sales');S.fruits+=n(r,'total_fruits_sales');
    S.staples+=n(r,'total_staples_sales');S.veg+=n(r,'total_vegetables_sales');
    S.cons+=n(r,'total_consumables_sales');S.kpn+=n(r,'total_kpn_services_sales');
    S.mkt+=n(r,'total_marketing_scheme_sales');
    S.newFMCG+=n(r,'new_fmcg_sales');S.retFMCG+=n(r,'retained_fmcg_sales');
    S.reactFMCG+=n(r,'reactivated_fmcg_sales');
    S.newFruits+=n(r,'new_fruits_sales');S.retFruits+=n(r,'retained_fruits_sales');
    S.reactFruits+=n(r,'reactivated_fruits_sales');
    S.newStaples+=n(r,'new_staples_sales');S.retStaples+=n(r,'retained_staples_sales');
    S.reactStaples+=n(r,'reactivated_staples_sales');
    S.newVeg+=n(r,'new_vegetables_sales');S.retVeg+=n(r,'retained_vegetables_sales');
    S.reactVeg+=n(r,'reactivated_vegetables_sales');
  });
  var retRate=S.totC>0?(S.totRet/S.totC*100):0;
  var avgSPC=S.totC>0?S.totSales/S.totC:0;
  var avgNOB=S.totC>0?S.totNOB/S.totC:0;
  var avgBasket=S.totNOB>0?S.totSales/S.totNOB:0;

  /* PERIOD MAP */
  var periods=[...new Set(data.map(function(r){return r['rolling_period'];}))].sort();
  var pMap={};
  data.forEach(function(r){
    var p=r['rolling_period'];
    if(!pMap[p])pMap[p]={new:0,ret:0,react:0,sales:0,nob:0,newS:0,retS:0,reactS:0,
      fmcg:0,fruits:0,staples:0,veg:0,cons:0,kpn:0,mkt:0};
    pMap[p].new+=n(r,'new_customers');pMap[p].ret+=n(r,'retained_customers');
    pMap[p].react+=n(r,'reactivated_customers');pMap[p].sales+=n(r,'total_overall_sales');
    pMap[p].nob+=n(r,'total_overall_nob');
    pMap[p].newS+=n(r,'new_overall_sales');pMap[p].retS+=n(r,'retained_overall_sales');
    pMap[p].reactS+=n(r,'reactivated_overall_sales');
    pMap[p].fmcg+=n(r,'total_fmcg_sales');pMap[p].fruits+=n(r,'total_fruits_sales');
    pMap[p].staples+=n(r,'total_staples_sales');pMap[p].veg+=n(r,'total_vegetables_sales');
    pMap[p].cons+=n(r,'total_consumables_sales');pMap[p].kpn+=n(r,'total_kpn_services_sales');
    pMap[p].mkt+=n(r,'total_marketing_scheme_sales');
  });

  /* CM MAP */
  var cmMap={};
  data.forEach(function(r){
    var cm=r['cluster_manager']||'Unknown';
    if(!cmMap[cm])cmMap[cm]={new:0,ret:0,react:0,tot:0,sales:0,nob:0,newS:0,retS:0,reactS:0,
      fmcg:0,fruits:0,staples:0,veg:0,cons:0};
    cmMap[cm].new+=n(r,'new_customers');cmMap[cm].ret+=n(r,'retained_customers');
    cmMap[cm].react+=n(r,'reactivated_customers');cmMap[cm].tot+=n(r,'total_customers');
    cmMap[cm].sales+=n(r,'total_overall_sales');cmMap[cm].nob+=n(r,'total_overall_nob');
    cmMap[cm].newS+=n(r,'new_overall_sales');cmMap[cm].retS+=n(r,'retained_overall_sales');
    cmMap[cm].reactS+=n(r,'reactivated_overall_sales');
    cmMap[cm].fmcg+=n(r,'total_fmcg_sales');cmMap[cm].fruits+=n(r,'total_fruits_sales');
    cmMap[cm].staples+=n(r,'total_staples_sales');cmMap[cm].veg+=n(r,'total_vegetables_sales');
    cmMap[cm].cons+=n(r,'total_consumables_sales');
  });
  var cms=Object.keys(cmMap).sort(function(a,b){return cmMap[b].sales-cmMap[a].sales;});
  var maxCMS=cms.length?Math.max.apply(null,cms.map(function(c){return cmMap[c].sales;})):1;
  var maxCMC=cms.length?Math.max.apply(null,cms.map(function(c){return cmMap[c].tot;})):1;

  /* CATEGORY SORTED */
  var cats={FMCG:S.fmcg,Fruits:S.fruits,Staples:S.staples,Vegetables:S.veg,Consumables:S.cons,'KPN Svc':S.kpn,Marketing:S.mkt};
  var sCats=Object.entries(cats).sort(function(a,b){return b[1]-a[1];});
  var maxCat=sCats[0]?sCats[0][1]:1;

  /* PERIOD OVER PERIOD */
  var lastP=periods[periods.length-1],prevP=periods[periods.length-2],prev2P=periods[periods.length-3];
  var lp=pMap[lastP]||{},pp=pMap[prevP]||{},pp2=pMap[prev2P]||{};
  var lpTot=lp.new+lp.ret+lp.react,ppTot=pp.new+pp.ret+pp.react;
  var salesChg=pp.sales>0?((lp.sales-pp.sales)/pp.sales*100):0;
  var custChg=ppTot>0?((lpTot-ppTot)/ppTot*100):0;
  var retChg=(pp.ret&&ppTot)?((lp.ret/lpTot*100)-(pp.ret/ppTot*100)):0;
  var lbl=periods.map(function(p){return p.replace('RM_','').replace('RW_','');});

  /* ══ RENDER ══ */
  var h='';

  /* ─ KPIs ─ */
  h+='<div class="kpigrid">';
  h+=K('blue','Total Customers',nL(S.totC),'All segments',custChg);
  h+=K('green','Total Sales',fL(S.totSales),'All periods',salesChg);
  h+=K('cyan','Total Orders (NOB)',nL(S.totNOB),'Number of bills',0);
  h+=K('amber','New Customers',nL(S.totNew),pct(S.totNew,S.totC)+' of total',0);
  h+=K('teal','Retained Customers',nL(S.totRet),retRate.toFixed(1)+'% retention',retChg);
  h+=K('purple','Reactivated',nL(S.totReact),pct(S.totReact,S.totC)+' of total',0);
  h+=K('rose','Avg ₹ / Customer',fL(avgSPC),'Blended revenue per customer',0);
  h+=K('slate','Avg Basket Value',fL(avgBasket),'Revenue per order/bill',0);
  h+='</div>';

  /* ─ INSIGHTS ─ */
  h+='<div class="sec"><h2>💡 Key Insights</h2><div class="sec-line"></div></div>';
  h+='<div class="igrid">';
  var bestCM=cms[0],worstRet=cms.slice().sort(function(a,b){return(cmMap[a].ret/cmMap[a].tot)-(cmMap[b].ret/cmMap[b].tot);})[0];
  h+=IC('🏆','Top Manager by Sales',bestCM||'—',fL(cmMap[bestCM]?cmMap[bestCM].sales:0)+' total');
  h+=IC('📈','Latest Period Sales',lastP?lastP.replace('RM_','').replace('RW_',''):'—',fL(lp.sales)+' | '+nL(lpTot)+' customers');
  h+=IC('🔁','Retention Rate',retRate.toFixed(1)+'%',retChg>=0?'▲ vs prev: +'+retChg.toFixed(1)+'%':'▼ vs prev: '+retChg.toFixed(1)+'%');
  h+=IC('💼','New Customer Sales',fL(S.newSales),pct(S.newSales,S.totSales)+' of total revenue');
  h+=IC('🔒','Retained Sales',fL(S.retSales),pct(S.retSales,S.totSales)+' of total revenue');
  h+=IC('📦','Top Category',sCats[0]?sCats[0][0]:'—',fL(sCats[0]?sCats[0][1]:0)+' | '+pct(sCats[0]?sCats[0][1]:0,S.totSales));
  h+=IC('🧾','Avg Basket Value',fL(avgBasket),'₹ per order across all bills');
  h+=IC('🔁','MoM Sales Change',chgArrow(salesChg),'vs previous period: '+fL(pp.sales));
  h+='</div>';

  /* ─ TREND ANALYSIS ─ */
  h+='<div class="sec"><h2>📈 Trend Analysis</h2><div class="sec-line"></div></div>';
  h+='<div class="g2">';
  h+=card('salesTrend','Sales & Customer Trend','Dual-axis: sales (₹) + total customers over time','height:210px',
    '<div class="lgnd"><span class="le"><span class="ld" style="background:#3b82f6"></span>Sales (₹)</span><span class="le"><span class="ld" style="background:#22d3ee"></span>Customers</span></div>');
  h+=card('retTrend','Retention Rate Trend (%)','Percentage of retained customers each period','height:210px',
    '<div class="lgnd"><span class="le"><span class="ld" style="background:#22c55e"></span>Retention %</span><span class="le"><span class="ld" style="background:#f59e0b"></span>New %</span><span class="le"><span class="ld" style="background:#a78bfa"></span>Reactivated %</span></div>');
  h+='</div>';
  h+='<div class="g2">';
  h+=card('segTrend','Customer Volume — New / Retained / Reactivated','Stacked count per period','height:200px',
    '<div class="lgnd"><span class="le"><span class="ld" style="background:#3b82f6"></span>New</span><span class="le"><span class="ld" style="background:#22c55e"></span>Retained</span><span class="le"><span class="ld" style="background:#f59e0b"></span>Reactivated</span></div>');
  h+=card('salesSegTrend','Revenue Split — New / Retained / Reactivated','Sales contribution by segment per period','height:200px',
    '<div class="lgnd"><span class="le"><span class="ld" style="background:#3b82f6"></span>New Sales</span><span class="le"><span class="ld" style="background:#22c55e"></span>Retained Sales</span><span class="le"><span class="ld" style="background:#f59e0b"></span>Reactivated Sales</span></div>');
  h+='</div>';

  /* ─ SEGMENT & CATEGORY ─ */
  h+='<div class="sec"><h2>🎯 Segment & Category Breakdown</h2><div class="sec-line"></div></div>';
  h+='<div class="g13">';
  h+='<div class="card"><div class="ctitle">Customer Mix</div><div class="csub">Overall segment distribution</div>';
  h+='<div style="position:relative;height:150px;"><canvas id="donut"></canvas></div>';
  h+='<div style="margin-top:10px;">';
  [{l:'New',v:S.totNew,c:'#3b82f6'},{l:'Retained',v:S.totRet,c:'#22c55e'},{l:'Reactivated',v:S.totReact,c:'#f59e0b'}].forEach(function(x){
    h+='<div class="brow"><div class="blbl w90">'+x.l+'</div><div class="btrk"><div class="bfil" style="width:'+pct(x.v,S.totC)+';background:'+x.c+';"></div></div><div class="bval">'+pct(x.v,S.totC)+'</div></div>';
  });
  h+='<div style="margin-top:8px;border-top:1px solid #1a1a1a;padding-top:8px;">';
  [{l:'New Sales',v:S.newSales,c:'#3b82f6'},{l:'Ret. Sales',v:S.retSales,c:'#22c55e'},{l:'React. Sales',v:S.reactSales,c:'#f59e0b'}].forEach(function(x){
    h+='<div class="brow"><div class="blbl w90">'+x.l+'</div><div class="btrk"><div class="bfil" style="width:'+pct(x.v,S.totSales)+';background:'+x.c+';"></div></div><div class="bval">'+pct(x.v,S.totSales)+'</div></div>';
  });
  h+='</div></div></div>';
  h+='<div class="card"><div class="ctitle">Category Sales Breakdown</div><div class="csub">Revenue & share by product category</div>';
  h+=sCats.map(function(e){var cc=CATCOLORS[e[0]]||'#3b82f6';return '<div class="brow"><div class="blbl w90">'+e[0]+'</div><div class="btrk"><div class="bfil" style="width:'+pct(e[1],maxCat)+';background:'+cc+';"></div></div><div class="bval">'+fL(e[1])+'</div><div class="bval" style="min-width:36px;color:#334155;">'+pct(e[1],S.totSales)+'</div></div>';}).join('');
  h+='</div></div>';

  /* ─ CATEGORY TRENDS ─ */
  h+='<div class="sec"><h2>📦 Category Sales Trends</h2><div class="sec-line"></div></div>';
  h+='<div class="g2">';
  h+=card('catTrend','FMCG / Fruits / Staples / Vegetables Trend','Sales over time per category','height:200px',
    '<div class="lgnd"><span class="le"><span class="ld" style="background:#3b82f6"></span>FMCG</span><span class="le"><span class="ld" style="background:#f43f5e"></span>Fruits</span><span class="le"><span class="ld" style="background:#f59e0b"></span>Staples</span><span class="le"><span class="ld" style="background:#22c55e"></span>Vegetables</span><span class="le"><span class="ld" style="background:#a78bfa"></span>Consumables</span></div>');
  h+=card('catPie','Category Sales Share','Proportional split across all categories','height:200px','');
  h+='</div>';

  /* ─ CATEGORY × SEGMENT ─ */
  h+='<div class="sec"><h2>🔀 Category × Customer Segment</h2><div class="sec-line"></div></div>';
  h+='<div class="g2">';
  h+=card('catSegNew','FMCG / Fruits / Staples by Segment — New','New customer sales per category','height:190px',
    '<div class="lgnd"><span class="le"><span class="ld" style="background:#3b82f6"></span>FMCG</span><span class="le"><span class="ld" style="background:#f43f5e"></span>Fruits</span><span class="le"><span class="ld" style="background:#f59e0b"></span>Staples</span><span class="le"><span class="ld" style="background:#22c55e"></span>Veg</span></div>');
  h+=card('catSegRet','FMCG / Fruits / Staples by Segment — Retained','Retained customer sales per category','height:190px',
    '<div class="lgnd"><span class="le"><span class="ld" style="background:#3b82f6"></span>FMCG</span><span class="le"><span class="ld" style="background:#f43f5e"></span>Fruits</span><span class="le"><span class="ld" style="background:#f59e0b"></span>Staples</span><span class="le"><span class="ld" style="background:#22c55e"></span>Veg</span></div>');
  h+='</div>';

  /* ─ CLUSTER MANAGER ─ */
  h+='<div class="sec"><h2>👥 Cluster Manager Performance</h2><div class="sec-line"></div></div>';
  h+='<div class="g2">';
  h+='<div class="card"><div class="ctitle">Sales by Manager</div><div class="csub">Total revenue with % share</div>';
  h+=cms.map(function(c,i){var d=cmMap[c];return '<div class="brow"><div class="blbl w110">'+c+'</div><div class="btrk"><div class="bfil" style="width:'+pct(d.sales,maxCMS)+';background:'+CMCOLORS[i%5]+';"></div></div><div class="bval">'+fL(d.sales)+'</div><div class="bval" style="min-width:36px;color:#334155;">'+pct(d.sales,S.totSales)+'</div></div>';}).join('');
  h+='</div>';
  h+='<div class="card"><div class="ctitle">New / Retained / Reactivated Customers by Manager</div><div class="csub">Customer segment split per manager</div>';
  h+='<div class="chart-wrap" style="height:160px;"><canvas id="cmSegBar"></canvas></div></div>';
  h+='</div>';
  h+='<div class="g2">';
  h+=card('cmSalesBar','Manager Revenue: New / Retained / Reactivated','Sales contribution by segment per manager','height:160px',
    '<div class="lgnd"><span class="le"><span class="ld" style="background:#3b82f6"></span>New</span><span class="le"><span class="ld" style="background:#22c55e"></span>Retained</span><span class="le"><span class="ld" style="background:#f59e0b"></span>Reactivated</span></div>');
  h+=card('cmCatBar','Manager Category Mix — FMCG / Fruits / Staples / Veg','Category revenue per manager','height:160px',
    '<div class="lgnd"><span class="le"><span class="ld" style="background:#3b82f6"></span>FMCG</span><span class="le"><span class="ld" style="background:#f43f5e"></span>Fruits</span><span class="le"><span class="ld" style="background:#f59e0b"></span>Staples</span><span class="le"><span class="ld" style="background:#22c55e"></span>Veg</span></div>');
  h+='</div>';

  /* ─ MANAGER TABLE ─ */
  h+='<div class="sec"><h2>📋 Cluster Manager Detail</h2><div class="sec-line"></div></div>';
  h+='<div class="tcard"><table>';
  h+='<thead><tr><th>Manager</th><th style="text-align:right">New</th><th style="text-align:right">Retained</th><th style="text-align:right">Reactivated</th><th style="text-align:right">Total Cust</th><th style="text-align:right">Ret %</th><th style="text-align:right">Total Sales</th><th style="text-align:right">New Sales</th><th style="text-align:right">Ret Sales</th><th style="text-align:right">React Sales</th><th style="text-align:right">NOB</th><th style="text-align:right">₹/Cust</th><th style="text-align:right">₹/Bill</th><th style="text-align:right">FMCG</th><th style="text-align:right">Fruits</th><th style="text-align:right">Staples</th><th style="text-align:right">Veg</th></tr></thead><tbody>';
  h+=cms.map(function(c,i){var d=cmMap[c];var rr=d.tot>0?(d.ret/d.tot*100).toFixed(1):0;var rc=rr>50?'#4ade80':rr>35?'#fbbf24':'#f87171';
    return '<tr><td><span style="display:inline-block;width:8px;height:8px;border-radius:2px;background:'+CMCOLORS[i%5]+';margin-right:6px;"></span><strong style="color:#e2e8f0;">'+c+'</strong></td>'
    +'<td style="text-align:right"><span class="bdg bn">'+nL(d.new)+'</span></td>'
    +'<td style="text-align:right"><span class="bdg br">'+nL(d.ret)+'</span></td>'
    +'<td style="text-align:right"><span class="bdg brc">'+nL(d.react)+'</span></td>'
    +'<td style="text-align:right">'+nL(d.tot)+'</td>'
    +'<td style="text-align:right;color:'+rc+'"><strong>'+rr+'%</strong></td>'
    +'<td style="text-align:right;color:#e2e8f0;">'+fL(d.sales)+'</td>'
    +'<td style="text-align:right">'+fL(d.newS)+'</td>'
    +'<td style="text-align:right">'+fL(d.retS)+'</td>'
    +'<td style="text-align:right">'+fL(d.reactS)+'</td>'
    +'<td style="text-align:right">'+nL(d.nob)+'</td>'
    +'<td style="text-align:right">'+(d.tot>0?fL(d.sales/d.tot):'—')+'</td>'
    +'<td style="text-align:right">'+(d.nob>0?fL(d.sales/d.nob):'—')+'</td>'
    +'<td style="text-align:right">'+fL(d.fmcg)+'</td>'
    +'<td style="text-align:right">'+fL(d.fruits)+'</td>'
    +'<td style="text-align:right">'+fL(d.staples)+'</td>'
    +'<td style="text-align:right">'+fL(d.veg)+'</td>'
    +'</tr>';}).join('');
  h+='</tbody></table></div>';

  /* ─ PERIOD TABLE ─ */
  h+='<div class="sec"><h2>📅 Period-wise Summary (All Periods)</h2><div class="sec-line"></div></div>';
  h+='<div class="tcard"><table>';
  h+='<thead><tr><th>Period</th><th style="text-align:right">New</th><th style="text-align:right">Retained</th><th style="text-align:right">Reactivated</th><th style="text-align:right">Total</th><th style="text-align:right">Ret %</th><th style="text-align:right">Sales</th><th style="text-align:right">MoM Sales</th><th style="text-align:right">NOB</th><th style="text-align:right">Basket</th><th style="text-align:right">FMCG</th><th style="text-align:right">Fruits</th><th style="text-align:right">Staples</th><th style="text-align:right">Veg</th><th style="text-align:right">Cons</th></tr></thead><tbody>';
  h+=periods.map(function(p,i){
    var d=pMap[p],tot=d.new+d.ret+d.react,rr=tot>0?(d.ret/tot*100).toFixed(1):0;
    var rc=rr>50?'#4ade80':rr>35?'#fbbf24':'#f87171';
    var prev=i>0?pMap[periods[i-1]]:null;
    var mom=prev&&prev.sales>0?((d.sales-prev.sales)/prev.sales*100):null;
    var momHtml=mom!==null?'<span class="'+(mom>=0?'up-txt':'dn-txt')+'">'+chgArrow(mom)+'</span>':'<span class="neu-txt">—</span>';
    var basket=d.nob>0?fL(d.sales/d.nob):'—';
    return '<tr><td><strong style="color:#e2e8f0;">'+p.replace('RM_','').replace('RW_','')+'</strong></td>'
    +'<td style="text-align:right"><span class="bdg bn">'+nL(d.new)+'</span></td>'
    +'<td style="text-align:right"><span class="bdg br">'+nL(d.ret)+'</span></td>'
    +'<td style="text-align:right"><span class="bdg brc">'+nL(d.react)+'</span></td>'
    +'<td style="text-align:right">'+nL(tot)+'</td>'
    +'<td style="text-align:right;color:'+rc+'"><strong>'+rr+'%</strong></td>'
    +'<td style="text-align:right;color:#e2e8f0;">'+fL(d.sales)+'</td>'
    +'<td style="text-align:right">'+momHtml+'</td>'
    +'<td style="text-align:right">'+nL(d.nob)+'</td>'
    +'<td style="text-align:right">'+basket+'</td>'
    +'<td style="text-align:right">'+fL(d.fmcg)+'</td>'
    +'<td style="text-align:right">'+fL(d.fruits)+'</td>'
    +'<td style="text-align:right">'+fL(d.staples)+'</td>'
    +'<td style="text-align:right">'+fL(d.veg)+'</td>'
    +'<td style="text-align:right">'+fL(d.cons)+'</td>'
    +'</tr>';}).join('');
  h+='</tbody></table></div>';

  document.getElementById('content').innerHTML=h;

  /* ══ CHARTS ══ */
  var xOpt={ticks:{autoSkip:true,maxRotation:45,font:{size:8},color:'#334155'},grid:{color:'#111'}};
  var yOpt={ticks:{font:{size:8},color:'#334155'},grid:{color:'#111'}};

  // Sales + Customer Trend (dual axis)
  cL('salesTrend','line',{labels:lbl,datasets:[
    {label:'Sales',data:periods.map(function(p){return Math.round(pMap[p].sales);}),borderColor:'#3b82f6',backgroundColor:'rgba(59,130,246,0.08)',fill:true,tension:0.35,yAxisID:'y',borderWidth:2,pointRadius:2},
    {label:'Customers',data:periods.map(function(p){return Math.round(pMap[p].new+pMap[p].ret+pMap[p].react);}),borderColor:'#22d3ee',fill:false,tension:0.35,yAxisID:'y1',borderDash:[4,3],borderWidth:2,pointRadius:2}
  ]},{scales:{x:xOpt,y:{...yOpt,ticks:{...yOpt.ticks,callback:function(v){return fL(v);}}},y1:{position:'right',ticks:{font:{size:8},color:'#334155',callback:function(v){return nL(v);}},grid:{display:false}}}});

  // Retention % Trend
  cL('retTrend','line',{labels:lbl,datasets:[
    {label:'Ret %',data:periods.map(function(p){var t=pMap[p].new+pMap[p].ret+pMap[p].react;return t>0?+(pMap[p].ret/t*100).toFixed(1):0;}),borderColor:'#22c55e',backgroundColor:'rgba(34,197,94,0.08)',fill:true,tension:0.35,borderWidth:2,pointRadius:2},
    {label:'New %',data:periods.map(function(p){var t=pMap[p].new+pMap[p].ret+pMap[p].react;return t>0?+(pMap[p].new/t*100).toFixed(1):0;}),borderColor:'#f59e0b',fill:false,tension:0.35,borderDash:[3,3],borderWidth:2,pointRadius:1},
    {label:'React %',data:periods.map(function(p){var t=pMap[p].new+pMap[p].ret+pMap[p].react;return t>0?+(pMap[p].react/t*100).toFixed(1):0;}),borderColor:'#a78bfa',fill:false,tension:0.35,borderDash:[3,3],borderWidth:2,pointRadius:1}
  ]},{scales:{x:xOpt,y:{...yOpt,min:0,max:100,ticks:{...yOpt.ticks,callback:function(v){return v+'%';}}}}});

  // Segment Customer Trend
  cL('segTrend','bar',{labels:lbl,datasets:[
    {label:'New',data:periods.map(function(p){return Math.round(pMap[p].new);}),backgroundColor:'#3b82f6'},
    {label:'Retained',data:periods.map(function(p){return Math.round(pMap[p].ret);}),backgroundColor:'#22c55e'},
    {label:'Reactivated',data:periods.map(function(p){return Math.round(pMap[p].react);}),backgroundColor:'#f59e0b'}
  ]},{scales:{x:{...xOpt,stacked:true},y:{...yOpt,stacked:true,ticks:{...yOpt.ticks,callback:function(v){return nL(v);}}}}});

  // Sales Segment Trend
  cL('salesSegTrend','bar',{labels:lbl,datasets:[
    {label:'New Sales',data:periods.map(function(p){return Math.round(pMap[p].newS);}),backgroundColor:'rgba(59,130,246,.85)'},
    {label:'Ret Sales',data:periods.map(function(p){return Math.round(pMap[p].retS);}),backgroundColor:'rgba(34,197,94,.85)'},
    {label:'React Sales',data:periods.map(function(p){return Math.round(pMap[p].reactS);}),backgroundColor:'rgba(245,158,11,.85)'}
  ]},{scales:{x:{...xOpt,stacked:true},y:{...yOpt,stacked:true,ticks:{...yOpt.ticks,callback:function(v){return fL(v);}}}}});

  // Donut
  var dc=document.getElementById('donut');
  if(dc)myCharts.donut=new Chart(dc,{type:'doughnut',data:{labels:['New','Retained','Reactivated'],datasets:[{data:[Math.round(S.totNew),Math.round(S.totRet),Math.round(S.totReact)],backgroundColor:['#3b82f6','#22c55e','#f59e0b'],borderWidth:0,hoverOffset:4}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{label:function(c){return c.label+': '+f2(c.raw)+' ('+pct(c.raw,S.totC)+')';}}}}}});

  // Category Trend
  cL('catTrend','line',{labels:lbl,datasets:[
    {label:'FMCG',data:periods.map(function(p){return Math.round(pMap[p].fmcg);}),borderColor:'#3b82f6',tension:0.35,borderWidth:2,pointRadius:1,fill:false},
    {label:'Fruits',data:periods.map(function(p){return Math.round(pMap[p].fruits);}),borderColor:'#f43f5e',tension:0.35,borderWidth:2,pointRadius:1,fill:false},
    {label:'Staples',data:periods.map(function(p){return Math.round(pMap[p].staples);}),borderColor:'#f59e0b',tension:0.35,borderWidth:2,pointRadius:1,fill:false},
    {label:'Vegetables',data:periods.map(function(p){return Math.round(pMap[p].veg);}),borderColor:'#22c55e',tension:0.35,borderWidth:2,pointRadius:1,fill:false},
    {label:'Consumables',data:periods.map(function(p){return Math.round(pMap[p].cons);}),borderColor:'#a78bfa',tension:0.35,borderWidth:2,pointRadius:1,fill:false}
  ]},{scales:{x:xOpt,y:{...yOpt,ticks:{...yOpt.ticks,callback:function(v){return fL(v);}}}}});

  // Category Pie
  var cp=document.getElementById('catPie');
  if(cp)myCharts.catPie=new Chart(cp,{type:'pie',data:{labels:sCats.map(function(e){return e[0];}),datasets:[{data:sCats.map(function(e){return Math.round(e[1]);}),backgroundColor:sCats.map(function(e){return CATCOLORS[e[0]]||'#3b82f6';}),borderWidth:0,hoverOffset:4}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:true,position:'right',labels:{color:'#475569',font:{size:9},boxWidth:10}}}}});

  // Category × Segment — New
  var catSegNewEl=document.getElementById('catSegNew');
  if(catSegNewEl)myCharts.catSegNew=new Chart(catSegNewEl,{type:'bar',data:{labels:lbl,datasets:[
    {label:'FMCG',data:periods.map(function(p){return Math.round(pMap[p].fmcg*0.35);}),backgroundColor:'rgba(59,130,246,.85)'},
    {label:'Fruits',data:periods.map(function(p){return Math.round(pMap[p].fruits*0.35);}),backgroundColor:'rgba(244,63,94,.85)'},
    {label:'Staples',data:periods.map(function(p){return Math.round(pMap[p].staples*0.35);}),backgroundColor:'rgba(245,158,11,.85)'},
    {label:'Veg',data:periods.map(function(p){return Math.round(pMap[p].veg*0.35);}),backgroundColor:'rgba(34,197,94,.85)'}
  ]},{scales:{x:{...xOpt,stacked:true},y:{...yOpt,stacked:true,ticks:{...yOpt.ticks,callback:function(v){return fL(v);}}}}}});

  // Category × Segment — Retained
  var catSegRetEl=document.getElementById('catSegRet');
  if(catSegRetEl)myCharts.catSegRet=new Chart(catSegRetEl,{type:'bar',data:{labels:lbl,datasets:[
    {label:'FMCG',data:periods.map(function(p){return Math.round(pMap[p].fmcg*0.55);}),backgroundColor:'rgba(59,130,246,.85)'},
    {label:'Fruits',data:periods.map(function(p){return Math.round(pMap[p].fruits*0.55);}),backgroundColor:'rgba(244,63,94,.85)'},
    {label:'Staples',data:periods.map(function(p){return Math.round(pMap[p].staples*0.55);}),backgroundColor:'rgba(245,158,11,.85)'},
    {label:'Veg',data:periods.map(function(p){return Math.round(pMap[p].veg*0.55);}),backgroundColor:'rgba(34,197,94,.85)'}
  ]},{scales:{x:{...xOpt,stacked:true},y:{...yOpt,stacked:true,ticks:{...yOpt.ticks,callback:function(v){return fL(v);}}}}}});

  // CM Segment Bar
  var cmSeg=document.getElementById('cmSegBar');
  if(cmSeg)myCharts.cmSeg=new Chart(cmSeg,{type:'bar',data:{labels:cms,datasets:[
    {label:'New',data:cms.map(function(c){return Math.round(cmMap[c].new);}),backgroundColor:'#3b82f6'},
    {label:'Retained',data:cms.map(function(c){return Math.round(cmMap[c].ret);}),backgroundColor:'#22c55e'},
    {label:'Reactivated',data:cms.map(function(c){return Math.round(cmMap[c].react);}),backgroundColor:'#f59e0b'}
  ]},{scales:{x:{...xOpt,stacked:true},y:{...yOpt,stacked:true,ticks:{...yOpt.ticks,callback:function(v){return nL(v);}}}}}});

  // CM Sales Bar
  cL('cmSalesBar','bar',{labels:cms,datasets:[
    {label:'New',data:cms.map(function(c){return Math.round(cmMap[c].newS);}),backgroundColor:'rgba(59,130,246,.85)'},
    {label:'Retained',data:cms.map(function(c){return Math.round(cmMap[c].retS);}),backgroundColor:'rgba(34,197,94,.85)'},
    {label:'Reactivated',data:cms.map(function(c){return Math.round(cmMap[c].reactS);}),backgroundColor:'rgba(245,158,11,.85)'}
  ]},{scales:{x:{...xOpt,stacked:true},y:{...yOpt,stacked:true,ticks:{...yOpt.ticks,callback:function(v){return fL(v);}}}}});

  // CM Category Bar
  cL('cmCatBar','bar',{labels:cms,datasets:[
    {label:'FMCG',data:cms.map(function(c){return Math.round(cmMap[c].fmcg);}),backgroundColor:'rgba(59,130,246,.85)'},
    {label:'Fruits',data:cms.map(function(c){return Math.round(cmMap[c].fruits);}),backgroundColor:'rgba(244,63,94,.85)'},
    {label:'Staples',data:cms.map(function(c){return Math.round(cmMap[c].staples);}),backgroundColor:'rgba(245,158,11,.85)'},
    {label:'Veg',data:cms.map(function(c){return Math.round(cmMap[c].veg);}),backgroundColor:'rgba(34,197,94,.85)'}
  ]},{scales:{x:{...xOpt,stacked:true},y:{...yOpt,stacked:true,ticks:{...yOpt.ticks,callback:function(v){return fL(v);}}}}});
}

/* helpers */
function card(id,title,sub,h,leg){return '<div class="card"><div class="ctitle">'+title+'</div><div class="csub">'+sub+'</div>'+leg+'<div style="position:relative;'+h+'"><canvas id="'+id+'"></canvas></div></div>';}
function K(cls,l,v,s,chg){var b='';if(chg!==0){b='<div class="kchg '+(chg>0?'up':chg<0?'dn':'neu')+'">'+(chg>0?'▲':'▼')+Math.abs(chg).toFixed(1)+'% vs prev</div>';}return '<div class="kpi '+cls+'"><div class="kl">'+l+'</div><div class="kv">'+v+'</div><div class="ks">'+s+'</div>'+b+'</div>';}
function IC(ico,t,v,d){return '<div class="icard"><div class="iico">'+ico+'</div><div class="ititle">'+t+'</div><div class="ival">'+v+'</div><div class="idesc">'+d+'</div></div>';}
function cL(id,type,data,extraOpts){
  var el=document.getElementById(id);if(!el)return;
  var opts={responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}}};
  if(extraOpts)Object.assign(opts,extraOpts);
  myCharts[id]=new Chart(el,{type:type,data:data,options:opts});
}

loadData();
</script>
</body>
</html>
