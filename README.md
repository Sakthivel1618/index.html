<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RFM Analytics Dashboard — Chennai</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0;}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#0f1117;color:#e2e8f0;font-size:13px;}
.wrap{max-width:1400px;margin:0 auto;padding:1.25rem;}

/* TOP BAR */
.topbar{display:flex;align-items:center;justify-content:space-between;margin-bottom:1.25rem;flex-wrap:wrap;gap:10px;}
.logo{display:flex;align-items:center;gap:10px;}
.logo h1{font-size:18px;font-weight:700;color:#fff;letter-spacing:-.3px;}
.logo span{font-size:11px;color:#64748b;background:#1e2533;padding:2px 8px;border-radius:4px;border:1px solid #2d3748;}
.topright{display:flex;align-items:center;gap:10px;flex-wrap:wrap;}
.rpill{font-size:11px;color:#94a3b8;background:#1e2533;border:1px solid #2d3748;border-radius:20px;padding:5px 12px;display:flex;align-items:center;gap:5px;}
.dot{width:7px;height:7px;background:#22c55e;border-radius:50%;flex-shrink:0;animation:pulse 2s infinite;}
@keyframes pulse{0%,100%{opacity:1;}50%{opacity:.3;}}

/* FILTER BAR */
.filterbar{display:flex;align-items:center;gap:10px;margin-bottom:1.25rem;flex-wrap:wrap;padding:10px 14px;background:#1a1f2e;border:1px solid #2d3748;border-radius:10px;}
.filter-group{display:flex;align-items:center;gap:6px;}
.filter-label{font-size:10px;color:#64748b;text-transform:uppercase;letter-spacing:.05em;white-space:nowrap;}
.filter-tabs{display:flex;background:#0f1117;border:1px solid #2d3748;border-radius:6px;overflow:hidden;}
.ftab{padding:5px 14px;font-size:12px;font-weight:500;cursor:pointer;border:none;background:transparent;color:#64748b;transition:all .15s;}
.ftab.active{background:#2563EB;color:#fff;}
select{background:#0f1117;border:1px solid #2d3748;color:#e2e8f0;border-radius:6px;padding:5px 10px;font-size:12px;cursor:pointer;outline:none;}
.divider{width:1px;height:20px;background:#2d3748;}

/* KPI CARDS */
.kpigrid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:10px;margin-bottom:1.25rem;}
.kpi{background:#1a1f2e;border:1px solid #2d3748;border-radius:10px;padding:14px 16px;position:relative;overflow:hidden;}
.kpi::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;}
.kpi.blue::before{background:linear-gradient(90deg,#2563EB,#60a5fa);}
.kpi.green::before{background:linear-gradient(90deg,#16a34a,#4ade80);}
.kpi.amber::before{background:linear-gradient(90deg,#d97706,#fbbf24);}
.kpi.purple::before{background:linear-gradient(90deg,#7c3aed,#a78bfa);}
.kpi.rose::before{background:linear-gradient(90deg,#e11d48,#fb7185);}
.kpi.cyan::before{background:linear-gradient(90deg,#0891b2,#22d3ee);}
.kpi.teal::before{background:linear-gradient(90deg,#0d9488,#2dd4bf);}
.kl{font-size:10px;color:#64748b;text-transform:uppercase;letter-spacing:.06em;margin-bottom:5px;}
.kv{font-size:22px;font-weight:700;color:#f1f5f9;line-height:1.1;}
.ks{font-size:10px;color:#64748b;margin-top:3px;}
.kbadge{display:inline-flex;align-items:center;gap:3px;font-size:10px;font-weight:600;padding:1px 6px;border-radius:3px;margin-top:3px;}
.kbadge.up{background:rgba(34,197,94,.15);color:#4ade80;}
.kbadge.dn{background:rgba(239,68,68,.15);color:#f87171;}

/* SECTION HEADERS */
.sec{display:flex;align-items:center;justify-content:space-between;margin-bottom:10px;margin-top:1.25rem;}
.sec h2{font-size:13px;font-weight:600;color:#cbd5e1;letter-spacing:.02em;}
.sec-line{flex:1;height:1px;background:#1e2533;margin-left:10px;}

/* CHART GRID */
.g2{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px;}
.g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px;margin-bottom:12px;}
.g13{display:grid;grid-template-columns:1fr 3fr;gap:12px;margin-bottom:12px;}
.gfull{margin-bottom:12px;}
.card{background:#1a1f2e;border:1px solid #2d3748;border-radius:10px;padding:1rem 1.15rem;}
.card.full{grid-column:1/-1;}
.ctitle{font-size:12px;font-weight:600;color:#cbd5e1;margin-bottom:4px;}
.csub{font-size:10px;color:#475569;margin-bottom:10px;}
.chart-wrap{position:relative;}

/* LEGEND */
.lgnd{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:8px;}
.le{font-size:10px;color:#64748b;display:flex;align-items:center;gap:4px;}
.ld{width:8px;height:8px;border-radius:2px;flex-shrink:0;}

/* HORIZONTAL BARS */
.brow{display:flex;align-items:center;gap:8px;margin-bottom:7px;}
.blbl{font-size:11px;color:#94a3b8;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.blbl.w80{width:80px;}
.blbl.w110{width:110px;}
.btrk{flex:1;background:#0f1117;border-radius:3px;height:7px;}
.bfil{height:7px;border-radius:3px;transition:width .5s ease;}
.bval{text-align:right;font-size:11px;color:#e2e8f0;font-weight:500;white-space:nowrap;}
.bval.w60{width:60px;}
.bval.w80{width:80px;}

/* TABLE */
.tcard{background:#1a1f2e;border:1px solid #2d3748;border-radius:10px;padding:1rem 1.15rem;margin-bottom:12px;overflow-x:auto;}
table{width:100%;border-collapse:collapse;font-size:11px;min-width:700px;}
th{text-align:left;padding:7px 10px;color:#475569;font-weight:600;border-bottom:1px solid #2d3748;font-size:10px;text-transform:uppercase;letter-spacing:.06em;white-space:nowrap;}
td{padding:7px 10px;color:#cbd5e1;border-bottom:1px solid #1e2533;}
tr:last-child td{border-bottom:none;}
tr:hover td{background:#1e2533;}
.badge{display:inline-block;padding:2px 7px;border-radius:4px;font-size:10px;font-weight:600;}
.bn{background:rgba(37,99,235,.2);color:#60a5fa;}
.br{background:rgba(22,163,74,.2);color:#4ade80;}
.brc{background:rgba(217,119,6,.2);color:#fbbf24;}

/* INSIGHTS */
.insight-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:10px;margin-bottom:12px;}
.insight{background:#1a1f2e;border:1px solid #2d3748;border-radius:10px;padding:12px 14px;}
.insight-icon{font-size:20px;margin-bottom:6px;}
.insight-title{font-size:11px;font-weight:600;color:#94a3b8;margin-bottom:4px;}
.insight-val{font-size:16px;font-weight:700;color:#f1f5f9;}
.insight-desc{font-size:10px;color:#475569;margin-top:3px;line-height:1.4;}

/* LOADING/ERROR */
.loading{text-align:center;padding:4rem;color:#475569;font-size:14px;}
.err{background:#2d1515;border:1px solid #7f1d1d;border-radius:10px;padding:1rem 1.25rem;color:#fca5a5;font-size:13px;line-height:1.6;margin-bottom:1rem;}

/* WEEK PILLS */
.week-picker{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:10px;}
.wpill{padding:3px 10px;border-radius:4px;font-size:11px;cursor:pointer;border:1px solid #2d3748;background:#0f1117;color:#64748b;transition:all .15s;}
.wpill.active{background:#2563EB;color:#fff;border-color:#2563EB;}

@media(max-width:768px){.g2,.g3,.g13{grid-template-columns:1fr;}.card.full{grid-column:auto;}}
</style>
</head>
<body>
<div class="wrap">

<div class="topbar">
  <div class="logo">
    <h1>📊 RFM Analytics</h1>
    <span>Chennai · KPN Fresh</span>
  </div>
  <div class="topright">
    <div class="rpill"><span class="dot"></span><span id="rl">Daily refresh at 8 AM</span></div>
    <div class="rpill">🕐 <span id="lu">Loading…</span></div>
  </div>
</div>

<!-- FILTER BAR -->
<div class="filterbar">
  <div class="filter-group">
    <span class="filter-label">View</span>
    <div class="filter-tabs">
      <button class="ftab active" onclick="switchView('month')">Monthly</button>
      <button class="ftab" onclick="switchView('week')">Weekly</button>
    </div>
  </div>
  <div class="divider"></div>
  <div class="filter-group">
    <span class="filter-label">Period</span>
    <select id="yearFilter" onchange="applyFilters()">
      <option value="all">All Years</option>
      <option value="2024">2024</option>
      <option value="2025">2025</option>
      <option value="2026">2026</option>
    </select>
  </div>
  <div class="filter-group" id="monthFilterGroup">
    <span class="filter-label">From</span>
    <select id="fromFilter" onchange="applyFilters()"><option value="">All</option></select>
    <span class="filter-label">To</span>
    <select id="toFilter" onchange="applyFilters()"><option value="">All</option></select>
  </div>
  <div class="divider"></div>
  <div class="filter-group">
    <span class="filter-label">Manager</span>
    <select id="cmFilter" onchange="applyFilters()">
      <option value="all">All Managers</option>
    </select>
  </div>
  <div style="margin-left:auto;font-size:11px;color:#475569;" id="rowcount">—</div>
</div>

<div id="content"><div class="loading">⏳ Fetching live data from Google Sheet…</div></div>
</div>

<script>
var SHEET_ID='10XNFBfJKW4gs_ra9CNtPUA5GXi9tb6xbnf6PnFgDSLI';
var GID='1815598507';
var URLS=[
  'https://docs.google.com/spreadsheets/d/'+SHEET_ID+'/export?format=csv&gid='+GID+'&t=',
  'https://docs.google.com/spreadsheets/d/'+SHEET_ID+'/gviz/tq?tqx=out:csv&gid='+GID+'&t=',
  'https://docs.google.com/spreadsheets/d/'+SHEET_ID+'/gviz/tq?tqx=out:csv&sheet=RFM+-+E&t='
];
var allData=[],activeView='month',myCharts={};
var COLORS={New:'#2563EB',Retained:'#16a34a',Reactivated:'#d97706',
  FMCG:'#2563EB',Fruits:'#e11d48',Staples:'#d97706',Vegetables:'#16a34a',
  Consumables:'#7c3aed',KPN:'#0891b2',Marketing:'#db2777'};
var CMS_COLORS=['#2563EB','#16a34a','#d97706','#7c3aed','#0891b2'];

function n(r,f){return parseFloat(r[f])||0;}
function fmtL(v){v=Math.round(v);if(v>=10000000)return '₹'+(v/10000000).toFixed(2)+'Cr';if(v>=100000)return '₹'+(v/100000).toFixed(2)+'L';if(v>=1000)return '₹'+(v/1000).toFixed(1)+'K';return '₹'+v;}
function numL(v){v=Math.round(v);if(v>=10000000)return (v/10000000).toFixed(2)+'Cr';if(v>=100000)return (v/100000).toFixed(2)+'L';if(v>=1000)return (v/1000).toFixed(1)+'K';return v.toLocaleString('en-IN');}
function fmt(v){return Math.round(v).toLocaleString('en-IN');}
function pct(a,b){return b>0?(a/b*100).toFixed(1)+'%':'—';}
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
  var ts=Date.now(),txt=null,errors=[];
  for(var i=0;i<URLS.length;i++){
    try{
      var r=await fetch(URLS[i]+ts);
      if(!r.ok)throw new Error('HTTP '+r.status);
      var t=await r.text();
      if(t.length<200||t.trim().startsWith('<')||t.indexOf('rolling_period')===-1)throw new Error('Bad response');
      txt=t;break;
    }catch(e){errors.push(e.message);}
  }
  if(!txt){
    document.getElementById('content').innerHTML='<div class="err"><strong>⚠️ Could not load data.</strong><br>'+errors.join('<br>')+'</div>';
    return;
  }
  var rows=parseCSV(txt);
  var hdrs=rows[0].map(function(h){return h.replace(/"/g,'').trim().toLowerCase();});
  allData=rows.slice(1).filter(function(r){return r.some(function(c){return c.trim();});})
    .map(function(row){var obj={};hdrs.forEach(function(h,i){obj[h]=(row[i]||'').replace(/"/g,'').trim();});return obj;});
  
  document.getElementById('lu').textContent='Updated '+new Date().toLocaleTimeString('en-IN',{hour:'2-digit',minute:'2-digit'});
  populateFilters();
  applyFilters();
  scheduleNext();
}

function populateFilters(){
  var cms=[...new Set(allData.map(function(r){return r['cluster_manager'];}))].filter(Boolean).sort();
  var sel=document.getElementById('cmFilter');
  cms.forEach(function(c){var o=document.createElement('option');o.value=c;o.textContent=c;sel.appendChild(o);});
  
  var mPeriods=[...new Set(allData.filter(function(r){return (r['rolling_period']||'').startsWith('RM_');}).map(function(r){return r['rolling_period'];}))].sort();
  var wPeriods=[...new Set(allData.filter(function(r){return (r['rolling_period']||'').startsWith('RW_');}).map(function(r){return r['rolling_period'];}))].sort();
  window._mPeriods=mPeriods;
  window._wPeriods=wPeriods;
  updatePeriodFilters();
}

function updatePeriodFilters(){
  var periods=activeView==='month'?window._mPeriods:window._wPeriods;
  if(!periods)return;
  var yr=document.getElementById('yearFilter').value;
  var filtered=yr==='all'?periods:periods.filter(function(p){return p.indexOf(yr)>-1;});
  var from=document.getElementById('fromFilter');
  var to=document.getElementById('toFilter');
  from.innerHTML='<option value="">Earliest</option>';
  to.innerHTML='<option value="">Latest</option>';
  filtered.forEach(function(p){
    var lbl=p.replace('RM_','').replace('RW_','');
    var o1=document.createElement('option');o1.value=p;o1.textContent=lbl;from.appendChild(o1);
    var o2=document.createElement('option');o2.value=p;o2.textContent=lbl;to.appendChild(o2);
  });
}

function switchView(v){
  activeView=v;
  document.querySelectorAll('.ftab').forEach(function(t,i){t.classList.toggle('active',(v==='month'&&i===0)||(v==='week'&&i===1));});
  updatePeriodFilters();
  applyFilters();
}

function applyFilters(){
  updatePeriodFilters();
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
  if(!data.length){
    document.getElementById('content').innerHTML='<div class="err">No data matches the selected filters.</div>';
    return;
  }
  buildDashboard(data);
}

function scheduleNext(){
  var now=new Date(),next=new Date(now);
  next.setHours(8,0,0,0);
  if(now>=next)next.setDate(next.getDate()+1);
  var ms=next-now,hrs=Math.floor(ms/3600000),mins=Math.floor((ms%3600000)/60000);
  document.getElementById('rl').textContent='Next refresh in '+hrs+'h '+mins+'m';
  setTimeout(function(){loadData();},ms);
}

function destroyCharts(){Object.values(myCharts).forEach(function(c){try{c.destroy();}catch(e){}});myCharts={};}

function buildDashboard(data){
  destroyCharts();

  /* ── AGGREGATIONS ── */
  var totC=0,totNew=0,totRet=0,totReact=0,totSales=0,totNOB=0;
  var totFMCG=0,totFruits=0,totStaples=0,totVeg=0,totCons=0,totKPN=0,totMkt=0;
  var totNewSales=0,totRetSales=0,totReactSales=0;
  data.forEach(function(r){
    totC+=n(r,'total_customers');totNew+=n(r,'new_customers');
    totRet+=n(r,'retained_customers');totReact+=n(r,'reactivated_customers');
    totSales+=n(r,'total_overall_sales');totNOB+=n(r,'total_overall_nob');
    totFMCG+=n(r,'total_fmcg_sales');totFruits+=n(r,'total_fruits_sales');
    totStaples+=n(r,'total_staples_sales');totVeg+=n(r,'total_vegetables_sales');
    totCons+=n(r,'total_consumables_sales');totKPN+=n(r,'total_kpn_services_sales');
    totMkt+=n(r,'total_marketing_scheme_sales');
    totNewSales+=n(r,'new_overall_sales');totRetSales+=n(r,'retained_overall_sales');
    totReactSales+=n(r,'reactivated_overall_sales');
  });
  var retRate=totC>0?(totRet/totC*100):0;
  var avgSPC=totC>0?totSales/totC:0;
  var avgNOB=totC>0?totNOB/totC:0;

  /* ── PERIOD MAP ── */
  var periods=[...new Set(data.map(function(r){return r['rolling_period'];}))].sort();
  var pMap={};
  data.forEach(function(r){
    var p=r['rolling_period'];
    if(!pMap[p])pMap[p]={new:0,ret:0,react:0,sales:0,nob:0,newSales:0,retSales:0,reactSales:0,
      fmcg:0,fruits:0,staples:0,veg:0,cons:0,kpn:0,mkt:0};
    pMap[p].new+=n(r,'new_customers');pMap[p].ret+=n(r,'retained_customers');
    pMap[p].react+=n(r,'reactivated_customers');pMap[p].sales+=n(r,'total_overall_sales');
    pMap[p].nob+=n(r,'total_overall_nob');
    pMap[p].newSales+=n(r,'new_overall_sales');pMap[p].retSales+=n(r,'retained_overall_sales');
    pMap[p].reactSales+=n(r,'reactivated_overall_sales');
    pMap[p].fmcg+=n(r,'total_fmcg_sales');pMap[p].fruits+=n(r,'total_fruits_sales');
    pMap[p].staples+=n(r,'total_staples_sales');pMap[p].veg+=n(r,'total_vegetables_sales');
    pMap[p].cons+=n(r,'total_consumables_sales');pMap[p].kpn+=n(r,'total_kpn_services_sales');
    pMap[p].mkt+=n(r,'total_marketing_scheme_sales');
  });

  /* ── CM MAP ── */
  var cmMap={};
  data.forEach(function(r){
    var cm=r['cluster_manager']||'Unknown';
    if(!cmMap[cm])cmMap[cm]={new:0,ret:0,react:0,tot:0,sales:0,nob:0,newSales:0,retSales:0,reactSales:0,
      fmcg:0,fruits:0,staples:0,veg:0};
    cmMap[cm].new+=n(r,'new_customers');cmMap[cm].ret+=n(r,'retained_customers');
    cmMap[cm].react+=n(r,'reactivated_customers');cmMap[cm].tot+=n(r,'total_customers');
    cmMap[cm].sales+=n(r,'total_overall_sales');cmMap[cm].nob+=n(r,'total_overall_nob');
    cmMap[cm].newSales+=n(r,'new_overall_sales');cmMap[cm].retSales+=n(r,'retained_overall_sales');
    cmMap[cm].reactSales+=n(r,'reactivated_overall_sales');
    cmMap[cm].fmcg+=n(r,'total_fmcg_sales');cmMap[cm].fruits+=n(r,'total_fruits_sales');
    cmMap[cm].staples+=n(r,'total_staples_sales');cmMap[cm].veg+=n(r,'total_vegetables_sales');
  });
  var cms=Object.keys(cmMap).sort(function(a,b){return cmMap[b].sales-cmMap[a].sales;});
  var maxCMSales=cms.length?Math.max.apply(null,cms.map(function(c){return cmMap[c].sales;})):1;
  var maxCMCust=cms.length?Math.max.apply(null,cms.map(function(c){return cmMap[c].tot;})):1;

  var catVals={FMCG:totFMCG,Fruits:totFruits,Staples:totStaples,Vegetables:totVeg,Consumables:totCons,'KPN Svc':totKPN,Marketing:totMkt};
  var sortedCats=Object.entries(catVals).sort(function(a,b){return b[1]-a[1];});
  var maxCat=sortedCats.length?sortedCats[0][1]:1;

  var lbl=periods.map(function(p){return p.replace('RM_','').replace('RW_','');});

  /* ── PERIOD-OVER-PERIOD CHANGE ── */
  var lastP=periods[periods.length-1],prevP=periods[periods.length-2];
  var salesChg=prevP&&pMap[prevP].sales>0?((pMap[lastP].sales-pMap[prevP].sales)/pMap[prevP].sales*100):0;
  var custChg=prevP&&(pMap[prevP].new+pMap[prevP].ret+pMap[prevP].react)>0?
    ((pMap[lastP].new+pMap[lastP].ret+pMap[lastP].react-(pMap[prevP].new+pMap[prevP].ret+pMap[prevP].react))/(pMap[prevP].new+pMap[prevP].ret+pMap[prevP].react)*100):0;
  var lastRetRate=pMap[lastP]?(pMap[lastP].ret/(pMap[lastP].new+pMap[lastP].ret+pMap[lastP].react)*100):0;

  /* ══ HTML ══ */
  var h='';

  /* KPIs */
  h+='<div class="kpigrid">';
  h+=kpi('blue','Total Customers',numL(totC),'All segments',custChg,true);
  h+=kpi('green','Total Sales',fmtL(totSales),'All periods',salesChg,true);
  h+=kpi('cyan','Orders (NOB)',numL(totNOB),'Number of bills',0,false);
  h+=kpi('amber','New Customers',numL(totNew),pct(totNew,totC)+' of total',0,false);
  h+=kpi('teal','Retained',numL(totRet),retRate.toFixed(1)+'% retention rate',0,false);
  h+=kpi('purple','Reactivated',numL(totReact),pct(totReact,totC)+' of total',0,false);
  h+=kpi('rose','Avg ₹/Customer',fmtL(avgSPC),'Blended avg',0,false);
  h+='</div>';

  /* ── INSIGHTS ROW ── */
  h+='<div class="sec"><h2>📌 Key Insights</h2><div class="sec-line"></div></div>';
  h+='<div class="insight-grid">';
  h+=insightCard('🏆','Best Performer',cms[0]||'—','Highest total sales: '+fmtL(cmMap[cms[0]]?cmMap[cms[0]].sales:0));
  h+=insightCard('📈','Latest Period',lastP?lastP.replace('RM_','').replace('RW_',''):'—','Sales: '+fmtL(pMap[lastP]?pMap[lastP].sales:0)+' | Customers: '+numL(pMap[lastP]?(pMap[lastP].new+pMap[lastP].ret+pMap[lastP].react):0));
  h+=insightCard('🔄','Retention Rate',retRate.toFixed(1)+'%','Latest: '+lastRetRate.toFixed(1)+'% | Target >50%');
  h+=insightCard('💰','Revenue per Order',fmtL(totNOB>0?totSales/totNOB:0),'Avg basket value across all bills');
  h+=insightCard('🆕','New vs Retained',pct(totNew,totC)+' vs '+pct(totRet,totC),'New customer acquisition share');
  h+=insightCard('📦','Top Category',sortedCats[0]?sortedCats[0][0]:'—','Sales: '+fmtL(sortedCats[0]?sortedCats[0][1]:0));
  h+='</div>';

  /* ── TREND CHARTS ── */
  h+='<div class="sec"><h2>📈 Trend Analysis</h2><div class="sec-line"></div></div>';
  h+='<div class="g2">';
  h+='<div class="card"><div class="ctitle">Sales Trend by Period</div><div class="csub">Total overall sales across all periods</div><div class="lgnd"><span class="le"><span class="ld" style="background:#2563EB"></span>Sales (₹)</span><span class="le"><span class="ld" style="background:#22d3ee"></span>Customers</span></div><div class="chart-wrap" style="height:200px;"><canvas id="salesTrend"></canvas></div></div>';
  h+='<div class="card"><div class="ctitle">Customer Segment Trend</div><div class="csub">New / Retained / Reactivated over time</div><div class="lgnd"><span class="le"><span class="ld" style="background:#2563EB"></span>New</span><span class="le"><span class="ld" style="background:#16a34a"></span>Retained</span><span class="le"><span class="ld" style="background:#d97706"></span>Reactivated</span></div><div class="chart-wrap" style="height:200px;"><canvas id="segTrend"></canvas></div></div>';
  h+='</div>';

  h+='<div class="g2">';
  h+='<div class="card"><div class="ctitle">Sales by Segment (New / Retained / Reactivated)</div><div class="csub">Revenue contribution by customer type</div><div class="lgnd"><span class="le"><span class="ld" style="background:#2563EB"></span>New</span><span class="le"><span class="ld" style="background:#16a34a"></span>Retained</span><span class="le"><span class="ld" style="background:#d97706"></span>Reactivated</span></div><div class="chart-wrap" style="height:200px;"><canvas id="salesSegTrend"></canvas></div></div>';
  h+='<div class="card"><div class="ctitle">Retention Rate Trend</div><div class="csub">% of retained customers per period</div><div class="lgnd"><span class="le"><span class="ld" style="background:#4ade80"></span>Retention %</span></div><div class="chart-wrap" style="height:200px;"><canvas id="retTrend"></canvas></div></div>';
  h+='</div>';

  /* ── SEGMENT & CATEGORY ── */
  h+='<div class="sec"><h2>🎯 Segment & Category Analysis</h2><div class="sec-line"></div></div>';
  h+='<div class="g13">';
  h+='<div class="card"><div class="ctitle">Customer Mix</div><div class="csub">Overall segment split</div><div class="chart-wrap" style="height:180px;"><canvas id="donut"></canvas></div>';
  h+='<div style="margin-top:10px;">';
  h+='<div class="brow"><div class="blbl w80">New</div><div class="btrk"><div class="bfil" style="width:'+pct(totNew,totC)+';background:#2563EB;"></div></div><div class="bval w60">'+pct(totNew,totC)+'</div></div>';
  h+='<div class="brow"><div class="blbl w80">Retained</div><div class="btrk"><div class="bfil" style="width:'+pct(totRet,totC)+';background:#16a34a;"></div></div><div class="bval w60">'+pct(totRet,totC)+'</div></div>';
  h+='<div class="brow"><div class="blbl w80">Reactivated</div><div class="btrk"><div class="bfil" style="width:'+pct(totReact,totC)+';background:#d97706;"></div></div><div class="bval w60">'+pct(totReact,totC)+'</div></div>';
  h+='</div></div>';
  h+='<div class="card"><div class="ctitle">Category-wise Sales Breakdown</div><div class="csub">Revenue by product category with trend</div><div class="chart-wrap" style="height:220px;"><canvas id="catChart"></canvas></div></div>';
  h+='</div>';

  /* ── CLUSTER MANAGER ── */
  h+='<div class="sec"><h2>👥 Cluster Manager Performance</h2><div class="sec-line"></div></div>';
  h+='<div class="g2">';
  h+='<div class="card"><div class="ctitle">Sales by Cluster Manager</div><div class="csub">Total revenue comparison</div>';
  h+=cms.map(function(c,i){return '<div class="brow"><div class="blbl w110" title="'+c+'">'+c+'</div><div class="btrk"><div class="bfil" style="width:'+(maxCMSales>0?(cmMap[c].sales/maxCMSales*100).toFixed(1):0)+'%;background:'+CMS_COLORS[i%5]+';"></div></div><div class="bval w80">'+fmtL(cmMap[c].sales)+'</div></div>';}).join('');
  h+='</div>';
  h+='<div class="card"><div class="ctitle">Customers by Cluster Manager</div><div class="csub">Total customer base comparison</div>';
  h+=cms.map(function(c,i){return '<div class="brow"><div class="blbl w110" title="'+c+'">'+c+'</div><div class="btrk"><div class="bfil" style="width:'+(maxCMCust>0?(cmMap[c].tot/maxCMCust*100).toFixed(1):0)+'%;background:'+CMS_COLORS[i%5]+';"></div></div><div class="bval w80">'+numL(cmMap[c].tot)+'</div></div>';}).join('');
  h+='</div>';
  h+='</div>';

  h+='<div class="g2">';
  h+='<div class="card"><div class="ctitle">Manager: New vs Retained vs Reactivated</div><div class="csub">Customer segment breakdown per manager</div><div class="lgnd"><span class="le"><span class="ld" style="background:#2563EB"></span>New</span><span class="le"><span class="ld" style="background:#16a34a"></span>Retained</span><span class="le"><span class="ld" style="background:#d97706"></span>Reactivated</span></div><div class="chart-wrap" style="height:200px;"><canvas id="cmSegBar"></canvas></div></div>';
  h+='<div class="card"><div class="ctitle">Manager: Sales Split (New / Retained / Reactivated)</div><div class="csub">Revenue by segment per manager</div><div class="lgnd"><span class="le"><span class="ld" style="background:#2563EB"></span>New Sales</span><span class="le"><span class="ld" style="background:#16a34a"></span>Retained Sales</span><span class="le"><span class="ld" style="background:#d97706"></span>Reactivated Sales</span></div><div class="chart-wrap" style="height:200px;"><canvas id="cmSalesBar"></canvas></div></div>';
  h+='</div>';

  /* ── CATEGORY TREND ── */
  h+='<div class="sec"><h2>📦 Category Sales Trend</h2><div class="sec-line"></div></div>';
  h+='<div class="gfull"><div class="card"><div class="ctitle">Category-wise Sales Trend Over Time</div><div class="csub">FMCG, Fruits, Staples, Vegetables trend across all periods</div><div class="lgnd"><span class="le"><span class="ld" style="background:#2563EB"></span>FMCG</span><span class="le"><span class="ld" style="background:#e11d48"></span>Fruits</span><span class="le"><span class="ld" style="background:#d97706"></span>Staples</span><span class="le"><span class="ld" style="background:#16a34a"></span>Vegetables</span><span class="le"><span class="ld" style="background:#7c3aed"></span>Consumables</span></div><div class="chart-wrap" style="height:220px;"><canvas id="catTrend"></canvas></div></div></div>';

  /* ── DETAIL TABLE ── */
  h+='<div class="sec"><h2>📋 Cluster Manager Detail Table</h2><div class="sec-line"></div></div>';
  h+='<div class="tcard"><table>';
  h+='<thead><tr><th>Manager</th><th style="text-align:right">New</th><th style="text-align:right">Retained</th><th style="text-align:right">Reactivated</th><th style="text-align:right">Total Customers</th><th style="text-align:right">Retention %</th><th style="text-align:right">Total Sales</th><th style="text-align:right">NOB</th><th style="text-align:right">₹/Customer</th><th style="text-align:right">New Sales</th><th style="text-align:right">Retained Sales</th></tr></thead><tbody>';
  h+=cms.map(function(c){var d=cmMap[c];var rr=d.tot>0?(d.ret/d.tot*100).toFixed(1):0;
    return '<tr><td><strong>'+c+'</strong></td>'
    +'<td style="text-align:right"><span class="badge bn">'+fmt(d.new)+'</span></td>'
    +'<td style="text-align:right"><span class="badge br">'+fmt(d.ret)+'</span></td>'
    +'<td style="text-align:right"><span class="badge brc">'+fmt(d.react)+'</span></td>'
    +'<td style="text-align:right">'+fmt(d.tot)+'</td>'
    +'<td style="text-align:right"><span style="color:'+(rr>50?'#4ade80':rr>30?'#fbbf24':'#f87171')+'">'+rr+'%</span></td>'
    +'<td style="text-align:right">'+fmtL(d.sales)+'</td>'
    +'<td style="text-align:right">'+fmt(d.nob)+'</td>'
    +'<td style="text-align:right">'+(d.tot>0?fmtL(d.sales/d.tot):'—')+'</td>'
    +'<td style="text-align:right">'+fmtL(d.newSales)+'</td>'
    +'<td style="text-align:right">'+fmtL(d.retSales)+'</td>'
    +'</tr>';}).join('');
  h+='</tbody></table></div>';

  /* ── PERIOD TABLE ── */
  h+='<div class="sec"><h2>📅 Period-wise Summary</h2><div class="sec-line"></div></div>';
  h+='<div class="tcard"><table>';
  h+='<thead><tr><th>Period</th><th style="text-align:right">New</th><th style="text-align:right">Retained</th><th style="text-align:right">Reactivated</th><th style="text-align:right">Total</th><th style="text-align:right">Retention %</th><th style="text-align:right">Sales</th><th style="text-align:right">NOB</th><th style="text-align:right">FMCG</th><th style="text-align:right">Fruits</th><th style="text-align:right">Staples</th><th style="text-align:right">Vegetables</th></tr></thead><tbody>';
  h+=periods.slice(-20).map(function(p){var d=pMap[p];var tot=d.new+d.ret+d.react;var rr=tot>0?(d.ret/tot*100).toFixed(1):0;
    return '<tr><td><strong>'+p.replace('RM_','').replace('RW_','')+'</strong></td>'
    +'<td style="text-align:right"><span class="badge bn">'+numL(d.new)+'</span></td>'
    +'<td style="text-align:right"><span class="badge br">'+numL(d.ret)+'</span></td>'
    +'<td style="text-align:right"><span class="badge brc">'+numL(d.react)+'</span></td>'
    +'<td style="text-align:right">'+numL(tot)+'</td>'
    +'<td style="text-align:right"><span style="color:'+(rr>50?'#4ade80':rr>30?'#fbbf24':'#f87171')+'">'+rr+'%</span></td>'
    +'<td style="text-align:right">'+fmtL(d.sales)+'</td>'
    +'<td style="text-align:right">'+numL(d.nob)+'</td>'
    +'<td style="text-align:right">'+fmtL(d.fmcg)+'</td>'
    +'<td style="text-align:right">'+fmtL(d.fruits)+'</td>'
    +'<td style="text-align:right">'+fmtL(d.staples)+'</td>'
    +'<td style="text-align:right">'+fmtL(d.veg)+'</td>'
    +'</tr>';}).join('');
  h+='</tbody></table></div>';

  document.getElementById('content').innerHTML=h;

  /* ══ CHARTS ══ */
  var co={responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},
    scales:{x:{ticks:{autoSkip:true,maxRotation:45,font:{size:9},color:'#475569'},grid:{color:'#1e2533'}},
      y:{ticks:{font:{size:9},color:'#475569'},grid:{color:'#1e2533'}}}};
  var dark={plugins:{legend:{display:false}},responsive:true,maintainAspectRatio:false};

  // Sales Trend
  var st=document.getElementById('salesTrend');
  if(st)myCharts.st=new Chart(st,{type:'line',data:{labels:lbl,datasets:[
    {label:'Sales',data:periods.map(function(p){return Math.round(pMap[p].sales);}),borderColor:'#2563EB',backgroundColor:'rgba(37,99,235,0.12)',fill:true,tension:0.35,yAxisID:'y',borderWidth:2,pointRadius:2,pointBackgroundColor:'#2563EB'},
    {label:'Customers',data:periods.map(function(p){return Math.round(pMap[p].new+pMap[p].ret+pMap[p].react);}),borderColor:'#22d3ee',fill:false,tension:0.35,yAxisID:'y1',borderDash:[4,3],borderWidth:2,pointRadius:2,pointBackgroundColor:'#22d3ee'}
  ]},options:{...co,scales:{x:co.scales.x,y:{...co.scales.y,ticks:{...co.scales.y.ticks,callback:function(v){return fmtL(v);}}},y1:{position:'right',ticks:{font:{size:9},color:'#475569',callback:function(v){return numL(v);}},grid:{display:false}}}}});

  // Segment Trend (stacked bar)
  var segT=document.getElementById('segTrend');
  if(segT)myCharts.segT=new Chart(segT,{type:'bar',data:{labels:lbl,datasets:[
    {label:'New',data:periods.map(function(p){return Math.round(pMap[p].new);}),backgroundColor:'#2563EB'},
    {label:'Retained',data:periods.map(function(p){return Math.round(pMap[p].ret);}),backgroundColor:'#16a34a'},
    {label:'Reactivated',data:periods.map(function(p){return Math.round(pMap[p].react);}),backgroundColor:'#d97706'}
  ]},options:{...co,scales:{x:{...co.scales.x,stacked:true},y:{...co.scales.y,stacked:true,ticks:{...co.scales.y.ticks,callback:function(v){return numL(v);}}}}}});

  // Sales by Segment Trend
  var sst=document.getElementById('salesSegTrend');
  if(sst)myCharts.sst=new Chart(sst,{type:'bar',data:{labels:lbl,datasets:[
    {label:'New Sales',data:periods.map(function(p){return Math.round(pMap[p].newSales);}),backgroundColor:'rgba(37,99,235,0.8)'},
    {label:'Retained Sales',data:periods.map(function(p){return Math.round(pMap[p].retSales);}),backgroundColor:'rgba(22,163,74,0.8)'},
    {label:'Reactivated Sales',data:periods.map(function(p){return Math.round(pMap[p].reactSales);}),backgroundColor:'rgba(217,119,6,0.8)'}
  ]},options:{...co,scales:{x:{...co.scales.x,stacked:true},y:{...co.scales.y,stacked:true,ticks:{...co.scales.y.ticks,callback:function(v){return fmtL(v);}}}}}});

  // Retention Rate Trend
  var retT=document.getElementById('retTrend');
  if(retT)myCharts.retT=new Chart(retT,{type:'line',data:{labels:lbl,datasets:[
    {label:'Retention %',data:periods.map(function(p){var tot=pMap[p].new+pMap[p].ret+pMap[p].react;return tot>0?parseFloat((pMap[p].ret/tot*100).toFixed(1)):0;}),
    borderColor:'#4ade80',backgroundColor:'rgba(74,222,128,0.1)',fill:true,tension:0.35,borderWidth:2,pointRadius:2,pointBackgroundColor:'#4ade80'}
  ]},options:{...co,scales:{x:co.scales.x,y:{...co.scales.y,min:0,max:100,ticks:{...co.scales.y.ticks,callback:function(v){return v+'%';}}}}}});

  // Donut
  var dn=document.getElementById('donut');
  if(dn)myCharts.dn=new Chart(dn,{type:'doughnut',data:{labels:['New','Retained','Reactivated'],datasets:[{data:[Math.round(totNew),Math.round(totRet),Math.round(totReact)],backgroundColor:['#2563EB','#16a34a','#d97706'],borderWidth:0,hoverOffset:4}]},
  options:{...dark,plugins:{legend:{display:false},tooltip:{callbacks:{label:function(c){return c.label+': '+fmt(c.raw)+' ('+pct(c.raw,totC)+')';}}}}}});

  // Category Bar
  var cb=document.getElementById('catChart');
  if(cb)myCharts.cb=new Chart(cb,{type:'bar',data:{labels:sortedCats.map(function(e){return e[0];}),
    datasets:[{data:sortedCats.map(function(e){return Math.round(e[1]);}),
    backgroundColor:['#2563EB','#e11d48','#d97706','#16a34a','#7c3aed','#0891b2','#db2777'],borderRadius:4}]},
  options:{...co,plugins:{legend:{display:false}},scales:{x:co.scales.x,y:{...co.scales.y,ticks:{...co.scales.y.ticks,callback:function(v){return fmtL(v);}}}}}});

  // CM Segment Bar
  var cmSeg=document.getElementById('cmSegBar');
  if(cmSeg)myCharts.cmSeg=new Chart(cmSeg,{type:'bar',data:{labels:cms,datasets:[
    {label:'New',data:cms.map(function(c){return Math.round(cmMap[c].new);}),backgroundColor:'#2563EB'},
    {label:'Retained',data:cms.map(function(c){return Math.round(cmMap[c].ret);}),backgroundColor:'#16a34a'},
    {label:'Reactivated',data:cms.map(function(c){return Math.round(cmMap[c].react);}),backgroundColor:'#d97706'}
  ]},options:{...co,scales:{x:{...co.scales.x,stacked:true},y:{...co.scales.y,stacked:true,ticks:{...co.scales.y.ticks,callback:function(v){return numL(v);}}}}}});

  // CM Sales Bar
  var cmSales=document.getElementById('cmSalesBar');
  if(cmSales)myCharts.cmSales=new Chart(cmSales,{type:'bar',data:{labels:cms,datasets:[
    {label:'New Sales',data:cms.map(function(c){return Math.round(cmMap[c].newSales);}),backgroundColor:'rgba(37,99,235,0.85)'},
    {label:'Retained Sales',data:cms.map(function(c){return Math.round(cmMap[c].retSales);}),backgroundColor:'rgba(22,163,74,0.85)'},
    {label:'Reactivated Sales',data:cms.map(function(c){return Math.round(cmMap[c].reactSales);}),backgroundColor:'rgba(217,119,6,0.85)'}
  ]},options:{...co,scales:{x:{...co.scales.x,stacked:true},y:{...co.scales.y,stacked:true,ticks:{...co.scales.y.ticks,callback:function(v){return fmtL(v);}}}}}});

  // Category Trend
  var ctT=document.getElementById('catTrend');
  if(ctT)myCharts.ctT=new Chart(ctT,{type:'line',data:{labels:lbl,datasets:[
    {label:'FMCG',data:periods.map(function(p){return Math.round(pMap[p].fmcg);}),borderColor:'#2563EB',backgroundColor:'transparent',tension:0.35,borderWidth:2,pointRadius:1},
    {label:'Fruits',data:periods.map(function(p){return Math.round(pMap[p].fruits);}),borderColor:'#e11d48',backgroundColor:'transparent',tension:0.35,borderWidth:2,pointRadius:1},
    {label:'Staples',data:periods.map(function(p){return Math.round(pMap[p].staples);}),borderColor:'#d97706',backgroundColor:'transparent',tension:0.35,borderWidth:2,pointRadius:1},
    {label:'Vegetables',data:periods.map(function(p){return Math.round(pMap[p].veg);}),borderColor:'#16a34a',backgroundColor:'transparent',tension:0.35,borderWidth:2,pointRadius:1},
    {label:'Consumables',data:periods.map(function(p){return Math.round(pMap[p].cons);}),borderColor:'#7c3aed',backgroundColor:'transparent',tension:0.35,borderWidth:2,pointRadius:1}
  ]},options:{...co,scales:{x:co.scales.x,y:{...co.scales.y,ticks:{...co.scales.y.ticks,callback:function(v){return fmtL(v);}}}}}});
}

function kpi(cls,label,val,sub,chg,showChg){
  var badge='';
  if(showChg&&chg!==0){
    var up=chg>0;
    badge='<div class="kbadge '+(up?'up':'dn')+'">'+(up?'↑':'↓')+Math.abs(chg).toFixed(1)+'% vs prev</div>';
  }
  return '<div class="kpi '+cls+'"><div class="kl">'+label+'</div><div class="kv">'+val+'</div><div class="ks">'+sub+'</div>'+badge+'</div>';
}
function insightCard(icon,title,val,desc){
  return '<div class="insight"><div class="insight-icon">'+icon+'</div><div class="insight-title">'+title+'</div><div class="insight-val">'+val+'</div><div class="insight-desc">'+desc+'</div></div>';
}

loadData();
</script>
</body>
</html>
