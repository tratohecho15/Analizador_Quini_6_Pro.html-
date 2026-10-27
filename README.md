
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#10182b">
<title>Analizador Quini 6 Pro</title>
<style>
:root{color-scheme:dark;--bg:#0b1120;--p:#121e33;--line:#2c405e;--txt:#eef4ff;--mut:#a8b8d2;--a:#62d5b4;--b:#78a9ff}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--txt);font:15px system-ui,sans-serif}
header{padding:24px 16px;background:#10182b;border-bottom:1px solid var(--line)}
main{max-width:1000px;margin:auto;padding:14px}
h1{margin:0;font-size:27px}h2{font-size:19px;margin:0 0 13px}
p{line-height:1.5}.muted,small{color:var(--mut)}
nav{display:flex;gap:7px;overflow:auto;margin-bottom:14px}
button{border:1px solid var(--line);background:#1b2b44;color:var(--txt);padding:11px 13px;border-radius:9px;font-weight:700;cursor:pointer}
button.primary{background:var(--a);color:#08251e;border-color:var(--a)}
button.danger{color:#ffabb6}
.tab{white-space:nowrap}.tab.active{border-color:var(--a);color:var(--a)}
.panel{padding:15px;background:var(--p);border:1px solid var(--line);border-radius:14px;margin-bottom:14px}
.field{min-width:0;flex:1}
label{display:block;font-size:12px;color:var(--mut);margin:0 0 5px}
input,select{width:100%;min-width:0;padding:11px 8px;background:#091426;color:var(--txt);border:1px solid #354967;border-radius:8px;font:inherit}
.row{display:flex;gap:9px;flex-wrap:wrap;align-items:end;margin:10px 0}
.grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px}
.six{display:grid;grid-template-columns:repeat(6,minmax(0,1fr));gap:6px}
.six input{text-align:center;font-size:17px;font-weight:700;padding:12px 2px}
.status{padding:10px;background:#0b1729;border-radius:8px;margin-top:10px;color:var(--mut)}
.good{color:var(--a)}.err{color:#ff9eaa}
.hidden{display:none!important}
.balls{display:flex;gap:7px;flex-wrap:wrap;margin:12px 0}
.ball{width:38px;height:38px;border-radius:50%;display:grid;place-items:center;background:#d7e8ff;color:#10213c;font-weight:900}
.stats{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:8px}
.stat{background:#091426;border:1px solid var(--line);padding:12px;border-radius:9px}
.stat b{display:block;font-size:22px}
.barline{display:grid;grid-template-columns:28px 1fr 30px;gap:7px;align-items:center;margin:8px 0;font-size:13px}
.bar{height:8px;background:#283953;border-radius:8px;overflow:hidden}
.bar i{display:block;height:100%;background:var(--a)}
.candidate{padding:12px;border:1px solid var(--line);border-radius:11px;margin:10px 0;background:#0c172a}
.tablewrap{overflow:auto}
table{border-collapse:collapse;width:100%;font-size:13px}
td,th{padding:10px 7px;text-align:left;border-bottom:1px solid var(--line);vertical-align:top}
th{color:var(--mut)}
.notice{padding:12px;border-left:3px solid #ffd37a;background:#2a251b;border-radius:7px;color:#f5dfb4;font-size:13px}
footer{padding:15px 0;color:var(--mut);font-size:12px}
@media(max-width:600px){.grid{grid-template-columns:1fr}.panel{padding:12px}main{padding:10px}.six{gap:4px}.six input{font-size:16px}}
</style>
</head>
<body>
<header>
<main>
<div class="muted">HERRAMIENTA ESTADÍSTICA PERSONAL</div>
<h1>Analizador Quini 6 Pro</h1>
<p class="muted">Historial · Frecuencias · Atrasos · Pronósticos · Evaluación</p>
</main>
</header>
<main>
<nav>
<button class="tab active" data-page="carga">Cargar sorteo</button>
<button class="tab" data-page="analisis">Análisis y pronóstico</button>
<button class="tab" data-page="historial">Historial</button>
<button class="tab" data-page="respaldo">Respaldo</button>
</nav>

<section id="carga" class="page">
<div class="panel">
<h2>Nuevo sorteo</h2>
<div class="row">
<div class="field"><label>Número de sorteo (opcional)</label><input id="drawNo" placeholder="Ej. 3312"></div>
<div class="field"><label>Fecha</label><input id="drawDate" type="date"></div>
</div>
<p class="muted">Completá los seis números de cada modalidad que quieras guardar. Los números permitidos son del 00 al 45, sin repetir dentro de una misma modalidad.</p>
<div id="entry"></div>
<div class="row">
<button class="primary" id="save">Guardar sorteo</button>
<button id="clear">Limpiar campos</button>
</div>
<div id="saveStatus" class="status">Los datos se guardan en este navegador. Exportá copias de seguridad regularmente.</div>
</div>
<div class="notice">El Quini 6 es un juego de azar. Los datos anteriores no permiten predecir ni garantizar resultados futuros.</div>
</section>

<section id="analisis" class="page hidden">
<div class="panel">
<h2>Análisis estadístico</h2>
<div class="row">
<div class="field"><label>Modalidad</label>
<select id="mode"><option>Tradicional</option><option>La Segunda</option><option>Revancha</option><option>Siempre Sale</option></select></div>
<div class="field"><label>Período</label>
<select id="window"><option value="all">Todo el historial</option><option value="75">Últimos 75 sorteos</option><option value="30">Últimos 30 sorteos</option><option value="15">Últimos 15 sorteos</option></select></div>
</div>
<div id="analysis"></div>
</div>
<div class="panel">
<h2>Evaluar un resultado</h2>
<p class="muted">Compará los candidatos generados con los seis números del sorteo real.</p>
<div class="row">
<div class="field"><label>Modalidad del resultado</label><select id="evalMode"><option>Tradicional</option><option>La Segunda</option><option>Revancha</option><option>Siempre Sale</option></select></div>
<div class="field"><label>Fecha del resultado</label><input id="evalDate" type="date"></div>
</div>
<div id="evalInputs" class="six"></div>
<div class="row"><button class="primary" id="evaluate">Evaluar coincidencias</button></div>
<div id="evalResult" class="status">Generá candidatos y cargá los números reales.</div>
</div>
</section>

<section id="historial" class="page hidden">
<div class="panel">
<h2>Sorteos guardados</h2>
<div class="row">
<div class="field"><label>Filtrar modalidad</label>
<select id="filter"><option value="Todas">Todas</option><option>Tradicional</option><option>La Segunda</option><option>Revancha</option><option>Siempre Sale</option></select></div>
<button id="refresh">Actualizar</button>
</div>
<div id="history"></div>
</div>
</section>

<section id="respaldo" class="page hidden">
<div class="panel">
<h2>Copias de seguridad</h2>
<p>Exportá los sorteos a un archivo JSON para conservarlos. Podés importarlos en otro dispositivo o después de reinstalar el navegador.</p>
<div class="row">
<button class="primary" id="export">Exportar respaldo</button>
<button id="import">Importar respaldo</button>
<input id="file" class="hidden" type="file" accept=".json,application/json">
</div>
<div id="backupStatus" class="status">Recordá guardar el archivo exportado en una carpeta segura.</div>
<hr style="border-color:var(--line);margin:20px 0">
<h2>Administración</h2>
<button class="danger" id="deleteAll">Borrar todo el historial</button>
</div>
</section>

<footer>Analizador Quini 6 Pro · Estadísticas descriptivas, sin garantías de acierto.</footer>
</main>

<script>
(()=>{
'use strict';
const MODES=['Tradicional','La Segunda','Revancha','Siempre Sale'];
const KEY='analizador_quini6_pro_v1';
const $=id=>document.getElementById(id);
let draws=readStorage();
let candidates={};

function readStorage(){
 try{
  const x=JSON.parse(localStorage.getItem(KEY)||'[]');
  return Array.isArray(x)?x:[];
 }catch(e){return []}
}
function persist(){
 try{localStorage.setItem(KEY,JSON.stringify(draws));return true}
 catch(e){return false}
}
function pad(n){return String(n).padStart(2,'0')}
function esc(s){return String(s??'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
function today(){const d=new Date();return `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())}`}
$('drawDate').value=today();
$('evalDate').value=today();

function sixInputs(prefix){
 return `<div class="six">${Array.from({length:6},(_,i)=>
 `<div><label>${i+1}</label><input id="${prefix}${i}" inputmode="numeric" maxlength="2" placeholder="00"></div>`).join('')}</div>`;
}
$('entry').innerHTML=MODES.map((m,i)=>`
 <div class="panel" style="background:#0b1729">
 <h3>${m}</h3>${sixInputs('n'+i+'_')}
 </div>`).join('');
$('evalInputs').innerHTML=sixInputs('e');

function readSix(prefix){
 const ns=[];
 for(let i=0;i<6;i++){
  const v=$(prefix+i).value.trim();
  if(!v)return {empty:true};
  if(!/^\d{1,2}$/.test(v))return {error:'Ingresá números enteros del 00 al 45.'};
  const n=Number(v);
  if(n<0||n>45)return {error:'Los números deben estar entre 00 y 45.'};
  ns.push(n);
 }
 if(new Set(ns).size!==6)return {error:'No se pueden repetir números dentro de la modalidad.'};
 return {nums:ns.sort((a,b)=>a-b)};
}
function message(id,text,kind=''){
 $(id).textContent=text;$(id).className='status '+kind;
}
function saveDraw(){
 const date=$('drawDate').value;
 if(!date){message('saveStatus','Ingresá una fecha.','err');return}
 const results={};
 for(let i=0;i<MODES.length;i++){
  const r=readSix('n'+i+'_');
  if(r.error){message('saveStatus',MODES[i]+': '+r.error,'err');return}
  if(!r.empty)results[MODES[i]]=r.nums;
 }
 if(!Object.keys(results).length){message('saveStatus','Completá al menos una modalidad.','err');return}
 const no=$('drawNo').value.trim();
 if(draws.some(d=>d.date===date&&d.no===no)&&!confirm('Ya existe un sorteo con esa fecha y número. ¿Guardar igualmente?'))return;
 draws.push({id:Date.now().toString(36)+Math.random().toString(36).slice(2),date,no,results});
 draws.sort((a,b)=>a.date.localeCompare(b.date));
 if(!persist()){message('saveStatus','No se pudo guardar. Exportá los datos y revisá el almacenamiento.','err');return}
 message('saveStatus','Sorteo guardado correctamente.','good');
 renderAnalysis();renderHistory();
}
function getList(mode){
 return draws.filter(d=>d.results&&d.results[mode]).sort((a,b)=>a.date.localeCompare(b.date));
}
function getStats(mode,win){
 let list=getList(mode);
 if(win!=='all')list=list.slice(-Number(win));
 const freq=Array(46).fill(0),last=Array(46).fill(-1);
 const pairs=Array.from({length:46},()=>Array(46).fill(0));
 list.forEach((d,i)=>{
  const ns=d.results[mode];
  ns.forEach(n=>{freq[n]++;last[n]=i});
  for(let a=0;a<ns.length;a++)for(let b=a+1;b<ns.length;b++){
   pairs[ns[a]][ns[b]]++;pairs[ns[b]][ns[a]]++;
  }
 });
 const nums=Array.from({length:46},(_,n)=>({
  n,f:freq[n],gap:last[n]<0?list.length:list.length-1-last[n]
 }));
 return {list,freq,last,pairs,nums,total:list.length};
}
function balls(ns){
 return `<div class="balls">${ns.map(n=>`<span class="ball">${pad(n)}</span>`).join('')}</div>`;
}
function bar(o,total){
 return `<div class="barline"><b>${pad(o.n)}</b><div class="bar"><i style="width:${total?o.f/total*100:0}%"></i></div><span>${o.f}</span></div>`;
}
function generate(st,mode){
 if(!st.total)return [];
 const recent=st.list.slice(-Math.min(15,st.total));
 const max=Math.max(1,...st.freq);
 const scores=st.nums.map(o=>{
  const rf=recent.reduce((s,d)=>s+(d.results[mode].includes(o.n)?1:0),0);
  const recentRate=rf/Math.max(1,recent.length);
  const freqRate=o.f/max;
  const delay=o.gap/Math.max(1,st.total);
  return {...o,score:.50*freqRate+.30*recentRate+.20*Math.min(1,delay)};
 }).sort((a,b)=>b.score-a.score||a.n-b.n);
 const out=[];
 for(let k=0;k<5;k++){
  const chosen=[];
  const pool=[...scores];
  while(chosen.length<6){
   pool.sort((a,b)=>{
    const bonus=x=>chosen.reduce((s,c)=>s+st.pairs[x.n][c.n]/Math.max(1,st.total)*.025,0);
    return (b.score+bonus(b))-(a.score+bonus(a));
   });
   let idx=0;
   if(chosen.length>=2){
    const odd=chosen.filter(x=>x.n%2).length;
    idx=pool.findIndex(x=>Math.abs((odd+(x.n%2))-3)<=Math.abs(odd-3));
    if(idx<0)idx=0;
   }
   const pick=pool.splice(idx,1)[0];
   if(!pick)break;
   chosen.push(pick);
  }
  // Diversificar las cinco propuestas sin reemplazar datos históricos.
  if(k>0){
   const replacement=scores.slice(0,20).filter(x=>!chosen.some(c=>c.n===x.n));
   if(replacement.length){
    const pos=(k+1)%6;
    chosen[pos]=replacement[(k-1)%replacement.length];
   }
  }
  const ns=[...new Set(chosen.map(x=>x.n))].sort((a,b)=>a-b);
  while(ns.length<6){
   const n=scores.find(x=>!ns.includes(x.n));
   if(!n)break;ns.push(n.n);ns.sort((a,b)=>a-b);
  }
  if(ns.length===6&&!out.some(x=>x.nums.join(',')===ns.join(','))){
   out.push({nums:ns,score:chosen.reduce((s,x)=>s+x.score,0)/6});
  }
 }
 // Si el historial produce combinaciones repetidas, crear variantes válidas.
 let shift=0;
 while(out.length<5&&shift<46){
  const ns=scores.slice(shift,shift+6).map(x=>x.n);
  shift++;
  if(ns.length===6&&new Set(ns).size===6&&!out.some(x=>x.nums.join(',')===ns.sort((a,b)=>a-b).join(','))){
   out.push({nums:ns.sort((a,b)=>a-b),score:0});
  }
 }
 return out;
}
function renderAnalysis(){
 const mode=$('mode').value,win=$('window').value;
 const st=getStats(mode,win),root=$('analysis');
 if(!st.total){
  candidates[mode]=[];
  root.innerHTML='<p class="muted">No hay sorteos guardados para esta modalidad. Cargá resultados o importá un respaldo.</p>';
  return;
 }
 const hot=[...st.nums].sort((a,b)=>b.f-a.f||a.n-b.n).slice(0,10);
 const cold=[...st.nums].sort((a,b)=>b.gap-a.gap||a.f-b.f).slice(0,10);
 const avg=st.list.reduce((s,d)=>s+d.results[mode].reduce((a,n)=>a+n,0),0)/st.total;
 root.innerHTML=`
 <div class="stats">
  <div class="stat"><b>${st.total}</b><small>Sorteos analizados</small></div>
  <div class="stat"><b>${avg.toFixed(1)}</b><small>Suma media de seis números</small></div>
  <div class="stat"><b>${st.nums.filter(x=>x.f===0).length}</b><small>Números sin aparecer en el período</small></div>
  <div class="stat"><b>${esc(st.list[st.list.length-1].date)}</b><small>Fecha del último registro</small></div>
 </div>
 <div class="grid" style="margin-top:18px">
  <div><h3>Más frecuentes</h3>${hot.map(x=>bar(x,st.total)).join('')}</div>
  <div><h3>Más atrasados</h3>${cold.map(x=>`<div class="barline"><b>${pad(x.n)}</b><div class="bar"><i style="width:${Math.min(100,x.gap/Math.max(1,st.total)*100)}%"></i></div><span>${x.gap}</span></div>`).join('')}</div>
 </div>
 <h3 style="margin-top:22px">Cinco combinaciones candidatas</h3>
 <p class="muted">Puntaje interno basado en frecuencia, recencia y atraso. No representa la probabilidad matemática de ganar.</p>
 <div id="candidateList"></div>
 <button class="primary" id="regen">Generar otras combinaciones</button>
 <h3 style="margin-top:22px">Frecuencia completa</h3>
 <div class="tablewrap"><table><thead><tr><th>Número</th><th>Apariciones</th><th>% de sorteos</th><th>Atraso</th></tr></thead>
 <tbody>${st.nums.map(x=>`<tr><td>${pad(x.n)}</td><td>${x.f}</td><td>${(x.f/st.total*100).toFixed(1)}%</td><td>${x.gap}</td></tr>`).join('')}</tbody></table></div>`;
 candidates[mode]=generate(st,mode);
 showCandidates(mode);
 $('regen').onclick=()=>{candidates[mode]=generate(st,mode);showCandidates(mode)};
}
function showCandidates(mode){
 const el=$('candidateList');if(!el)return;
 el.innerHTML=(candidates[mode]||[]).map((c,i)=>`
 <div class="candidate"><b>Candidato ${i+1}</b>${balls(c.nums)}
 <small>Puntaje relativo: ${(c.score*100).toFixed(1)}. No es un porcentaje de acierto.</small></div>`).join('')||'<p class="muted">No hay candidatos disponibles.</p>';
}
function renderHistory(){
 const mode=$('filter').value;
 let list=[...draws].sort((a,b)=>b.date.localeCompare(a.date));
 if(mode!=='Todas')list=list.filter(d=>d.results&&d.results[mode]);
 if(!list.length){$('history').innerHTML='<p class="muted">No hay registros para mostrar.</p>';return}
 $('history').innerHTML=`<p class="muted">${list.length} registro(s) guardado(s).</p>
 <div class="tablewrap"><table><thead><tr><th>Fecha</th><th>Sorteo</th><th>Resultados</th><th>Acción</th></tr></thead><tbody>
 ${list.map(d=>`<tr><td>${esc(d.date)}</td><td>${esc(d.no||'—')}</td><td>${Object.entries(d.results).filter(([m])=>mode==='Todas'||m===mode).map(([m,ns])=>`<b>${esc(m)}</b>: ${ns.map(pad).join(' - ')}`).join('<br>')}</td>
 <td><button class="danger" data-del="${esc(d.id)}">Eliminar</button></td></tr>`).join('')}
 </tbody></table></div>`;
 $('history').querySelectorAll('[data-del]').forEach(b=>b.onclick=()=>{
  if(confirm('¿Eliminar este registro?')){
   draws=draws.filter(d=>d.id!==b.dataset.del);persist();renderHistory();renderAnalysis();
  }
 });
}
function evaluate(){
 const mode=$('evalMode').value,r=readSix('e');
 if(r.error||r.empty){message('evalResult',r.error||'Completá los seis números reales.','err');return}
 const arr=candidates[mode]||[];
 if(!arr.length){message('evalResult','Primero generá candidatos para '+mode+'.','err');return}
 const actual=new Set(r.nums);
 const result=arr.map((c,i)=>{
  const hits=c.nums.filter(n=>actual.has(n));
  return `Candidato ${i+1}: ${hits.length}/6 coincidencias${hits.length?' ('+hits.map(pad).join(', ')+')':''}`;
 });
 const best=Math.max(...arr.map(c=>c.nums.filter(n=>actual.has(n)).length));
 message('evalResult',`Resultado ${mode}, ${$('evalDate').value||today()}: ${r.nums.map(pad).join(' - ')}. ${result.join(' | ')}. Mejor coincidencia: ${best}/6.`,'good');
}
function exportBackup(){
 const data={app:'Analizador Quini 6 Pro',version:1,exportedAt:new Date().toISOString(),draws};
 const blob=new Blob([JSON.stringify(data,null,2)],{type:'application/json'});
 const url=URL.createObjectURL(blob),a=document.createElement('a');
 a.href=url;a.download='respaldo-quini6-'+today()+'.json';a.click();
 setTimeout(()=>URL.revokeObjectURL(url),1000);
 message('backupStatus','Respaldo exportado. Guardá el archivo en una carpeta segura.','good');
}
function validDraw(d){
 if(!d||typeof d.date!=='string'||!d.results||typeof d.results!=='object')return false;
 return Object.entries(d.results).every(([m,ns])=>
 MODES.includes(m)&&Array.isArray(ns)&&ns.length===6&&
 ns.every(n=>Number.isInteger(n)&&n>=0&&n<=45)&&new Set(ns).size===6);
}
function importBackup(file){
 if(!file)return;
 const reader=new FileReader();
 reader.onload=()=>{
  try{
   const obj=JSON.parse(reader.result),incoming=Array.isArray(obj)?obj:obj.draws;
   if(!Array.isArray(incoming)||!incoming.every(validDraw))throw Error('Formato incorrecto o datos inválidos.');
   const map=new Map(draws.map(d=>[d.id,d]));
   incoming.forEach((d,i)=>{if(!d.id)d.id='import-'+Date.now()+'-'+i;map.set(d.id,d)});
   draws=[...map.values()].sort((a,b)=>a.date.localeCompare(b.date));
   if(!persist())throw Error('No se pudo guardar el respaldo en este navegador.');
   message('backupStatus','Importación completada: '+incoming.length+' registro(s).','good');
   renderHistory();renderAnalysis();
  }catch(e){message('backupStatus','Error al importar: '+e.message,'err')}
 };
 reader.readAsText(file);
}
document.querySelectorAll('[data-page]').forEach(b=>b.onclick=()=>{
 document.querySelectorAll('[data-page]').forEach(x=>x.classList.toggle('active',x===b));
 document.querySelectorAll('.page').forEach(x=>x.classList.toggle('hidden',x.id!==b.dataset.page));
 if(b.dataset.page==='analisis')renderAnalysis();
 if(b.dataset.page==='historial')renderHistory();
});
$('save').onclick=saveDraw;
$('clear').onclick=()=>{
 document.querySelectorAll('#entry input').forEach(x=>x.value='');
 $('drawNo').value='';
 message('saveStatus','Campos limpiados.');
};
$('mode').onchange=renderAnalysis;
$('window').onchange=renderAnalysis;
$('filter').onchange=renderHistory;
$('refresh').onclick=renderHistory;
$('evaluate').onclick=evaluate;
$('export').onclick=exportBackup;
$('import').onclick=()=>$('file').click();
$('file').onchange=e=>importBackup(e.target.files[0]);
$('deleteAll').onclick=()=>{
 if(confirm('¿Borrar todo el historial? Esta acción no se puede deshacer.')){
  draws=[];persist();candidates={};renderHistory();renderAnalysis();
  message('backupStatus','Historial borrado.','good');
 }
};
renderAnalysis();
renderHistory();
})();
</script>
</body>
</html>
