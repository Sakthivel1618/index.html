<!DOCTYPE html>
<html lang="en">
<head> 
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RFM Dashboard — Chennai</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #f5f5f5; color: #1a1a1a; font-size: 14px; }
  .wrap { max-width: 1200px; margin: 0 auto; padding: 1.5rem; }
  .top-bar { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1.5rem; flex-wrap: wrap; gap: 10px; }
  h1 { font-size: 20px; font-weight: 600; color: #111; }
  .rpill { font-size: 12px; color: #666; background: #fff; border: 1px solid #e0e0e0; border-radius: 20px; padding: 5px 14px; display: flex; align-items: center; gap: 6px; }
  .dot { width: 8px; height: 8px; background: #22c55e; border-radius: 50%; flex-shrink: 0; animation: pulse 2s infinite; }
  @keyframes pulse { 0%,100%{opacity:1;} 50%{opacity:.4;} }
  .tabs { display: flex; background: #fff; border: 1px solid #e0e0e0; border-radius: 8px; overflow: hidden; width: fit-content; margin-bottom: 1.5rem; }
  .tab { padding: 8px 20px; font-size: 13px; font-weight: 500; cursor: pointer; border: none; background: transparent; color: #888; transition: all .15s; }
  .tab.active { background: #185FA5; color: #fff; }
  .mgrid { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 12px; margin-bottom: 1.5rem; }
  .metric { background: #fff; border-radius: 10px; padding: 14px 16px; border: 1px solid #e8e8e8; }
  .ml { font-size: 10px; color: #888; text-transform: uppercase; letter-spacing: .06em; margin-bottom: 4px; }
  .mv { font-size: 22px; font-weight: 700; line-height: 1.1; color: #111; }
  .ms { font-size: 11px; color: #999; margin-top: 3px; }
  .mblue { background: #EBF4FF; border-color: #BDD7F5; } .mblue .mv { color: #185FA5; }
  .mgreen { background: #EDFAF2; border-color: #AADEC2; } .mgreen .mv { color: #15803d; }
  .mamber { background: #FFFBEB; border-color: #FCD34D; } .mamber .mv { color: #92400e; }
  .g2 { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-bottom: 1.5rem; }
  .card { background: #fff; border: 1px solid #e8e8e8; border-radius: 12px; padding: 1.25rem; }
  .ct { font-size: 13px; font-weight: 600; color: #222; margin-bottom: 10px; }
  .lgnd { display: flex; flex-wrap: wrap; gap: 10px; margin-bottom: 10px; }
  .le { font-size: 11px; color: #666; display: flex; align-items: center; gap: 5px; }
  .ld { width: 10px; height: 10px; border-radius: 2px; flex-shrink: 0; }
  .full { grid-column: 1 / -1; }
  .brow { display: flex; align-items: center; gap: 8px; margin-bottom: 8px; }
  .blbl { width: 100px; font-size: 11px; color: #666; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .btrk { flex: 1; background: #f0f0f0; border-radius: 4px; height: 8px; }
  .bfil { height: 8px; border-radius: 4px; transition: width .4s ease; }
  .bval { width: 62px; text-align: right; font-size: 11px; color: #333; font-weight: 500; }
  .tcard { background: #fff; border: 1px solid #e8e8e8; border-radius: 12px; padding: 1.25rem; margin-bottom: 1.5rem; overflow-x: auto; }
  table { width: 100%; border-collapse: collapse; font-size: 12px; min-width: 600px; }
  th { text-align: left; padding: 8px 10px; color: #888; font-weight: 600; border-bottom: 1px solid #f0f0f0; font-size: 10px; text-transform: uppercase; letter-spacing: .06em; }
  td { padding: 8px 10px; color: #222; border-bottom: 1px solid #f5f5f5; }
  tr:last-child td { border-bottom: none; }
  tr:hover td { background: #fafafa; }
  .badge { display: inline-block; padding: 2px 8px; border-radius: 4px; font-size: 10px; font-weight: 600; }
  .bn { background: #EBF4FF; color: #185FA5; }
  .br { background: #EDFAF2; color: #15803d; }
  .brc { background: #FFFBEB; color: #92400e; }
  .loading { text-align: center; padding: 3rem; color: #888; font-size: 14px; }
  .err { background: #FEF2F2; border: 1px solid #FECACA; border-radius: 10px; padding: 1rem 1.25rem; color: #991B1B; font-size: 13px; line-height: 1.6; margin-bottom: 1rem; }
  @media (max-width: 640px) { .g2 { grid-template-columns: 1fr; } .full { grid-column: auto; } }
</style>
</head>
<body>
<div class="wrap">
  <div class="top-bar">
    <h1>📊 RFM Analytics — Chennai</h1>
    <div class="rpill"><span class="dot"></span><span id="rl">Refreshes daily at 8 AM</span><span style="margin:0 4px;color:#ccc;">·</span><span id="lu">Loading…</span></div>
  </div>
  <div class="tabs">
    <button class="tab active" onclick="switchTab('month')">Monthly (RM_)</button>
    <button class="tab" onclick="switchTab('week')">Weekly (RW_)</button>
  </div>
  <div id="content"><div class="loading">⏳ Fetching data from Google Sheet…</div></div>
</div>

<script>
var SHEET_ID = '10XNFBfJKW4gs_ra9CNtPUA5GXi9tb6xbnf6PnFgDSLI';
var GID = '1815598507';

// Try multiple URL strategies to get the correct "RFM - E" tab
var URLS = [
  'https://docs.google.com/spreadsheets/d/' + SHEET_ID + '/export?format=csv&gid=' + GID + '&t=',
  'https://docs.google.com/spreadsheets/d/' + SHEET_ID + '/gviz/tq?tqx=out:csv&gid=' + GID + '&t=',
  'https://docs.google.com/spreadsheets/d/' + SHEET_ID + '/gviz/tq?tqx=out:csv&sheet=RFM+-+E&t='
];

var allData = [], activeTab = 'month', myCharts = {};

function n(r, f) { return parseFloat(r[f]) || 0; }
function fmtL(v) { v=Math.round(v); if(v>=10000000) return '₹'+(v/10000000).toFixed(2)+'Cr'; if(v>=100000) return '₹'+(v/100000).toFixed(2)+'L'; if(v>=1000) return '₹'+(v/1000).toFixed(0)+'K'; return '₹'+v; }
function numL(v) { v=Math.round(v); if(v>=10000000) return (v/10000000).toFixed(2)+'Cr'; if(v>=100000) return (v/100000).toFixed(2)+'L'; if(v>=1000) return (v/1000).toFixed(1)+'K'; return v.toLocaleString('en-IN'); }
function fmt(v) { return Math.round(v).toLocaleString('en-IN'); }

function parseCSV(txt) {
  var rows=[], lines=txt.split('\n');
  for(var i=0;i<lines.length;i++){
    var line=lines[i]; if(!line.trim()) continue;
    var cells=[], cur='', inQ=false;
    for(var j=0;j<line.length;j++){
      var c=line[j];
      if(c==='"'){if(inQ&&line[j+1]==='"'){cur+='"';j++;}else inQ=!inQ;}
      else if(c===','&&!inQ){cells.push(cur);cur='';}
      else cur+=c;
    }
    cells.push(cur); rows.push(cells);
  }
  return rows;
}

async function loadData() {
  var ts = Date.now();
  var txt = null, errors = [];
  for(var i=0; i<URLS.length; i++){
    try {
      var r = await fetch(URLS[i] + ts);
      if(!r.ok) throw new Error('HTTP '+r.status);
      var t = await r.text();
      if(t.length < 200) throw new Error('Response too short');
      if(t.trim().startsWith('<')) throw new Error('Got HTML not CSV');
      // Validate it has our expected header
      if(t.indexOf('rolling_period') === -1) throw new Error('rolling_period column not found');
      txt = t;
      break;
    } catch(e) { errors.push('URL'+(i+1)+': '+e.message); }
  }

  if(!txt) {
    document.getElementById('content').innerHTML = '<div class="err"><strong>⚠️ Could not load data.</strong><br>'+errors.join('<br>')+'<br><br>Ensure Google Sheet is shared: <em>Anyone with link → Viewer</em></div>';
    return;
  }

  try {
    var rows = parseCSV(txt);
    if(rows.length < 2) throw new Error('No data rows found');
    var hdrs = rows[0].map(function(h){ return h.replace(/"/g,'').trim().toLowerCase(); });
    allData = rows.slice(1)
      .filter(function(r){ return r.some(function(c){ return c.trim(); }); })
      .map(function(row){
        var obj={};
        hdrs.forEach(function(h,i){ obj[h]=(row[i]||'').replace(/"/g,'').trim(); });
        return obj;
      });
    document.getElementById('lu').textContent = 'Updated '+new Date().toLocaleTimeString('en-IN',{hour:'2-digit',minute:'2-digit'});
    render();
    scheduleNext();
  } catch(e) {
    document.getElementById('content').innerHTML = '<div class="err"><strong>Parse error:</strong> '+e.message+'</div>';
  }
}

function scheduleNext() {
  var now=new Date(), next=new Date(now);
  next.setHours(8,0,0,0);
  if(now>=next) next.setDate(next.getDate()+1);
  var ms=next-now;
  var hrs=Math.floor(ms/3600000), mins=Math.floor((ms%3600000)/60000);
  document.getElementById('rl').textContent='Next refresh in '+hrs+'h '+mins+'m (8:00 AM)';
  setTimeout(function(){ loadData(); }, ms);
}

function switchTab(tab) {
  activeTab=tab;
  document.querySelectorAll('.tab').forEach(function(t,i){
    t.classList.toggle('active',(tab==='month'&&i===0)||(tab==='week'&&i===1));
  });
  Object.values(myCharts).forEach(function(c){ try{c.destroy();}catch(e){} });
  myCharts={};
  render();
}

function render() {
  var data=allData.filter(function(r){
    var p=(r['rolling_period']||'').trim();
    return activeTab==='month' ? p.startsWith('RM_') : p.startsWith('RW_');
  });
  if(!data.length){
    var sample=[...new Set(allData.slice(0,10).map(function(r){return r['rolling_period'];}))].join(', ');
    document.getElementById('content').innerHTML='<div class="err">No '+(activeTab==='month'?'monthly (RM_)':'weekly (RW_)')+' rows found.<br><small>Periods in sheet: '+sample+'</small></div>';
    return;
  }
  buildUI(data);
}

function buildUI(data) {
  var totC=0,totNew=0,totRet=0,totReact=0,totSales=0,totNOB=0;
  data.forEach(function(r){
    totC+=n(r,'total_customers'); totNew+=n(r,'new_customers');
    totRet+=n(r,'retained_customers'); totReact+=n(r,'reactivated_customers');
    totSales+=n(r,'total_overall_sales'); totNOB+=n(r,'total_overall_nob');
  });
  var retRate=totC>0?(totRet/totC*100):0;
  var avgSPC=totC>0?totSales/totC:0;

  var periods=[...new Set(data.map(function(r){return r['rolling_period'];}))].sort();
  var pMap={};
  data.forEach(function(r){
    var p=r['rolling_period'];
    if(!pMap[p]) pMap[p]={new:0,retained:0,reactivated:0,sales:0};
    pMap[p].new+=n(r,'new_customers');
    pMap[p].retained+=n(r,'retained_customers');
    pMap[p].reactivated+=n(r,'reactivated_customers');
    pMap[p].sales+=n(r,'total_overall_sales');
  });

  var cmMap={};
  data.forEach(function(r){
    var cm=r['cluster_manager']||'Unknown';
    if(!cmMap[cm]) cmMap[cm]={new:0,ret:0,react:0,tot:0,sales:0,nob:0};
    cmMap[cm].new+=n(r,'new_customers'); cmMap[cm].ret+=n(r,'retained_customers');
    cmMap[cm].react+=n(r,'reactivated_customers'); cmMap[cm].tot+=n(r,'total_customers');
    cmMap[cm].sales+=n(r,'total_overall_sales'); cmMap[cm].nob+=n(r,'total_overall_nob');
  });
  var cms=Object.keys(cmMap).sort(function(a,b){return cmMap[b].sales-cmMap[a].sales;});
  var maxCM=cms.length?Math.max.apply(null,cms.map(function(c){return cmMap[c].sales;})):1;

  var catKeys={FMCG:'total_fmcg_sales',Fruits:'total_fruits_sales',Staples:'total_staples_sales',Vegetables:'total_vegetables_sales',Consumables:'total_consumables_sales','KPN Services':'total_kpn_services_sales',Marketing:'total_marketing_scheme_sales'};
  var catVals={};
  Object.keys(catKeys).forEach(function(k){catVals[k]=data.reduce(function(s,r){return s+n(r,catKeys[k]);},0);});
  var sortedCats=Object.entries(catVals).sort(function(a,b){return b[1]-a[1];});
  var maxCat=sortedCats.length?sortedCats[0][1]:1;

  var lbl=periods.map(function(p){return p.replace('RM_','').replace('RW_','');});

  var h='';
  h+='<div class="mgrid">';
  h+='<div class="metric mblue"><div class="ml">Total Customers</div><div class="mv">'+numL(totC)+'</div><div class="ms">All segments</div></div>';
  h+='<div class="metric mgreen"><div class="ml">Total Sales</div><div class="mv">'+fmtL(totSales)+'</div><div class="ms">All periods</div></div>';
  h+='<div class="metric"><div class="ml">Orders (NOB)</div><div class="mv">'+numL(totNOB)+'</div><div class="ms">Number of bills</div></div>';
  h+='<div class="metric mamber"><div class="ml">New Customers</div><div class="mv">'+numL(totNew)+'</div><div class="ms">'+(totC>0?(totNew/totC*100).toFixed(1):0)+'% share</div></div>';
  h+='<div class="metric mgreen"><div class="ml">Retained</div><div class="mv">'+numL(totRet)+'</div><div class="ms">'+retRate.toFixed(1)+'% retention</div></div>';
  h+='<div class="metric"><div class="ml">Reactivated</div><div class="mv">'+numL(totReact)+'</div><div class="ms">'+(totC>0?(totReact/totC*100).toFixed(1):0)+'% share</div></div>';
  h+='<div class="metric mblue"><div class="ml">Avg ₹ / Customer</div><div class="mv">'+fmtL(avgSPC)+'</div><div class="ms">Blended avg</div></div>';
  h+='</div>';

  h+='<div class="g2">';
  h+='<div class="card"><div class="ct">Customer Segment Split</div><div class="lgnd"><span class="le"><span class="ld" style="background:#2563EB"></span>New</span><span class="le"><span class="ld" style="background:#16a34a"></span>Retained</span><span class="le"><span class="ld" style="background:#d97706"></span>Reactivated</span></div><div style="position:relative;height:200px;"><canvas id="donut"></canvas></div></div>';
  h+='<div class="card"><div class="ct">Sales Trend by Period</div><div class="lgnd"><span class="le"><span class="ld" style="background:#2563EB"></span>Sales (₹)</span><span class="le"><span class="ld" style="background:#d97706"></span>Customers</span></div><div style="position:relative;height:200px;"><canvas id="trend"></canvas></div></div>';

  h+='<div class="card"><div class="ct">Sales by Cluster Manager</div>';
  h+=cms.slice(0,8).map(function(c){return '<div class="brow"><div class="blbl" title="'+c+'">'+c+'</div><div class="btrk"><div class="bfil" style="width:'+(maxCM>0?(cmMap[c].sales/maxCM*100).toFixed(1):0)+'%;background:#2563EB;"></div></div><div class="bval">'+fmtL(cmMap[c].sales)+'</div></div>';}).join('');
  h+='</div>';

  h+='<div class="card"><div class="ct">Sales by Product Category</div>';
  h+=sortedCats.map(function(e){return '<div class="brow"><div class="blbl">'+e[0]+'</div><div class="btrk"><div class="bfil" style="width:'+(maxCat>0?(e[1]/maxCat*100).toFixed(1):0)+'%;background:#059669;"></div></div><div class="bval">'+fmtL(e[1])+'</div></div>';}).join('');
  h+='</div>';

  h+='<div class="card full"><div class="ct">New / Retained / Reactivated — by Period</div>';
  h+='<div class="lgnd"><span class="le"><span class="ld" style="background:#2563EB"></span>New</span><span class="le"><span class="ld" style="background:#16a34a"></span>Retained</span><span class="le"><span class="ld" style="background:#d97706"></span>Reactivated</span></div>';
  h+='<div style="position:relative;height:220px;"><canvas id="stack"></canvas></div></div>';
  h+='</div>';

  h+='<div class="tcard"><div class="ct" style="margin-bottom:12px;">Cluster Manager Detail</div><table>';
  h+='<thead><tr><th>Manager</th><th style="text-align:right">New</th><th style="text-align:right">Retained</th><th style="text-align:right">Reactivated</th><th style="text-align:right">Total</th><th style="text-align:right">Total Sales</th><th style="text-align:right">NOB</th><th style="text-align:right">₹/Customer</th></tr></thead><tbody>';
  h+=cms.map(function(c){var d=cmMap[c];return '<tr><td><strong>'+c+'</strong></td><td style="text-align:right"><span class="badge bn">'+fmt(d.new)+'</span></td><td style="text-align:right"><span class="badge br">'+fmt(d.ret)+'</span></td><td style="text-align:right"><span class="badge brc">'+fmt(d.react)+'</span></td><td style="text-align:right">'+fmt(d.tot)+'</td><td style="text-align:right">'+fmtL(d.sales)+'</td><td style="text-align:right">'+fmt(d.nob)+'</td><td style="text-align:right">'+(d.tot>0?fmtL(d.sales/d.tot):'—')+'</td></tr>';}).join('');
  h+='</tbody></table></div>';
  document.getElementById('content').innerHTML=h;

  // Charts
  var dc=document.getElementById('donut');
  if(dc) myCharts.donut=new Chart(dc,{type:'doughnut',data:{labels:['New','Retained','Reactivated'],datasets:[{data:[Math.round(totNew),Math.round(totRet),Math.round(totReact)],backgroundColor:['#2563EB','#16a34a','#d97706'],borderWidth:0}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{label:function(c){return c.label+': '+fmt(c.raw)+' ('+(totC>0?((c.raw/totC)*100).toFixed(1):0)+'%)';}}}}}});

  var tc=document.getElementById('trend');
  if(tc&&periods.length) myCharts.trend=new Chart(tc,{type:'line',data:{labels:lbl,datasets:[
    {label:'Sales',data:periods.map(function(p){return Math.round(pMap[p].sales);}),borderColor:'#2563EB',backgroundColor:'rgba(37,99,235,0.07)',fill:true,tension:0.35,yAxisID:'y',borderWidth:2,pointRadius:3,pointBackgroundColor:'#2563EB'},
    {label:'Customers',data:periods.map(function(p){return Math.round(pMap[p].new+pMap[p].retained+pMap[p].reactivated);}),borderColor:'#d97706',fill:false,tension:0.35,yAxisID:'y1',borderDash:[5,4],borderWidth:2,pointRadius:3,pointBackgroundColor:'#d97706'}
  ]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{ticks:{autoSkip:true,maxRotation:45,font:{size:10}},grid:{display:false}},y:{ticks:{callback:function(v){return fmtL(v);},font:{size:10}},grid:{color:'#f0f0f0'}},y1:{position:'right',ticks:{callback:function(v){return numL(v);},font:{size:10}},grid:{display:false}}}}});

  var sc=document.getElementById('stack');
  if(sc&&periods.length) myCharts.stack=new Chart(sc,{type:'bar',data:{labels:lbl,datasets:[
    {label:'New',data:periods.map(function(p){return Math.round(pMap[p].new);}),backgroundColor:'#2563EB'},
    {label:'Retained',data:periods.map(function(p){return Math.round(pMap[p].retained);}),backgroundColor:'#16a34a'},
    {label:'Reactivated',data:periods.map(function(p){return Math.round(pMap[p].reactivated);}),backgroundColor:'#d97706'}
  ]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{stacked:true,ticks:{autoSkip:true,maxRotation:45,font:{size:10}},grid:{display:false}},y:{stacked:true,ticks:{callback:function(v){return numL(v);},font:{size:10}},grid:{color:'#f0f0f0'}}}}});
}

loadData();
</script>
</body>
</html>
