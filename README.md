<!doctype html>
<html lang="es">
<head><meta charset="utf-8"><title>Malla Interactiva - Opción 2 - Ingeniería Ambiental (Unillanos)</title>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<style>
:root{
  --bg:#f7fbff;
  --card:#ffffff;
  --accent1:#2b8cbe;
  --accent2:#7fb8e6;
  --accent3:#f29e4c;
  --accent4:#6abf69;
  --muted:#667085;
}
body{font-family:Inter, Arial, sans-serif; background:var(--bg); color:#0b1726; margin:18px}
.container{max-width:1200px;margin:0 auto}
.header{display:flex;justify-content:space-between;align-items:center;gap:12px}
h1{font-size:20px;margin:0}
.controls{margin-top:12px;display:flex;gap:8px;align-items:center;flex-wrap:wrap}
.btn{padding:8px 10px;border-radius:8px;border:0;cursor:pointer}
.primary{background:var(--accent1);color:white}
.ghost{background:transparent;border:1px solid rgba(0,0,0,0.08)}
.panel{margin-top:14px;background:var(--card);padding:12px;border-radius:10px;box-shadow:0 6px 18px rgba(11,23,38,0.06)}
.grid{display:grid;grid-template-columns:repeat(5,1fr);gap:10px}
.sem-box{background:linear-gradient(180deg, rgba(255,255,255,0.9), rgba(250,250,255,0.9));border-radius:8px;padding:8px;border:1px solid rgba(11,23,38,0.06)}
.sem-title{font-weight:700;margin-bottom:6px;color:var(--accent1)}
.course-box{padding:8px;border-radius:6px;margin-bottom:6px;display:flex;justify-content:space-between;align-items:center;gap:8px;cursor:pointer}
.course-name{font-size:13px;font-weight:600}
.course-meta{font-size:11px;color:var(--muted)}
.locked{opacity:0.35;filter:grayscale(20%)}
.completed{outline:2px solid rgba(0,0,0,0.06);opacity:0.6;text-decoration:line-through}
.color-1{background:linear-gradient(90deg,var(--accent2),var(--accent1));color:white}
.color-2{background:linear-gradient(90deg,#ffe6c2,var(--accent3));color:#432f13}
.color-3{background:linear-gradient(90deg,#d7f7e0,var(--accent4));color:#0b3b1e}
.color-4{background:linear-gradient(90deg,#f0e8ff,#c9b8ff);color:#2b125b}
.legend{display:flex;gap:8px;align-items:center;margin-top:8px;font-size:13px}
.legend .item{display:flex;gap:6px;align-items:center;padding:6px;border-radius:6px;background:white}
.note{font-size:13px;color:var(--muted)}
.electives{margin-top:12px}
.search{padding:8px;border-radius:8px;border:1px solid #e6eef7;flex:1}
.topbar-right{display:flex;gap:8px}
@media (max-width:1000px){
  .grid{grid-template-columns:repeat(2,1fr)}
}
@media (max-width:600px){
  .grid{grid-template-columns:1fr}
  .header{flex-direction:column;align-items:flex-start;gap:8px}
}
</style>
</head>
<body>
<div class="container">
<div class="header">
  <div>
    <h1>Malla Interactiva — Opción 2 (Formato visual)</h1>
    <div class="note">Universidad de los Llanos — Ingeniería Ambiental. Créditos totales (propuestos): 159 (verificar).</div>
  </div>
  <div class="topbar-right">
    <input id="search" class="search" placeholder="Buscar asignatura... (ej. Hidrología)">
    <button class="btn ghost" id="reset">Reiniciar</button>
    <button class="btn primary" id="download">Descargar PDF (Print)</button>
  </div>
</div>

<div class="controls">
  <button class="btn ghost" id="viewList">Ver: Lista</button>
  <button class="btn ghost" id="viewGrid">Ver: Malla (grid)</button>
  <div class="legend">
    <div class="item"><div style="width:18px;height:18px;background:linear-gradient(90deg,var(--accent2),var(--accent1));border-radius:4px"></div> Tronco básico</div>
    <div class="item"><div style="width:18px;height:18px;background:linear-gradient(90deg,#ffe6c2,var(--accent3));border-radius:4px"></div> Ciencias aplicadas</div>
    <div class="item"><div style="width:18px;height:18px;background:linear-gradient(90deg,#d7f7e0,var(--accent4));border-radius:4px"></div> Gestión y políticas</div>
    <div class="item"><div style="width:18px;height:18px;background:linear-gradient(90deg,#f0e8ff,#c9b8ff);border-radius:4px"></div> Electivas / Proyecto</div>
  </div>
</div>

<div class="panel" id="gridView">
  <div class="grid">
<!-- GRID: las casillas se generan en script -->
  </div>
</div>

<div class="panel" id="listView" style="display:none">
<!-- LIST: se genera en script -->
</div>

<div class="panel electives">
  <div class="sem-title">Electivas sugeridas por semestre (opcional)</div>
  <div id="electivesContent" style="margin-top:10px"></div>
</div>

<script>
/* Datos: código, nombre, créditos y prerrequisitos (propuesta para interacción).
   Revisa correlativas oficiales en la universidad. */
const malla = {
 "1-1": {"name":"Biología general y laboratorio", "credits":4, "prereqs":[]},
 "1-2": {"name":"Química general y laboratorio", "credits":5, "prereqs":[]},
 "1-3": {"name":"Cálculo diferencial", "credits":4, "prereqs":[]},
 "1-4": {"name":"Física mecánica", "credits":4, "prereqs":[]},
 "1-5": {"name":"Introducción a la Ingeniería Ambiental", "credits":2, "prereqs":[]},

 "2-1": {"name":"Ecología", "credits":4, "prereqs":["1-1"]},
 "2-2": {"name":"Química orgánica y laboratorio", "credits":4, "prereqs":["1-2"]},
 "2-3": {"name":"Cálculo integral", "credits":4, "prereqs":["1-3"]},
 "2-4": {"name":"Física (Calor y ondas)", "credits":4, "prereqs":["1-4"]},
 "2-5": {"name":"Álgebra lineal", "credits":3, "prereqs":[]},

 "3-1": {"name":"Cálculo multivariado", "credits":4, "prereqs":["2-3"]},
 "3-2": {"name":"Estadística", "credits":3, "prereqs":["2-3"]},
 "3-3": {"name":"Bioquímica", "credits":3, "prereqs":["1-2","1-1"]},
 "3-4": {"name":"Topografía y cartografía", "credits":3, "prereqs":["1-4"]},
 "3-5": {"name":"Termodinámica", "credits":3, "prereqs":["1-4","1-3"]},
 "3-6": {"name":"Pensamiento lógico-matemático", "credits":2, "prereqs":[]},

 "4-1": {"name":"Ecuaciones diferenciales", "credits":3, "prereqs":["3-1"]},
 "4-2": {"name":"Diseño experimental", "credits":3, "prereqs":["3-2"]},
 "4-3": {"name":"Microbiología y laboratorio", "credits":4, "prereqs":["3-3"]},
 "4-4": {"name":"Mecánica de fluidos y laboratorio", "credits":4, "prereqs":["3-5"]},
 "4-5": {"name":"Geología y geomorfología (laboratorio)", "credits":3, "prereqs":[]},
 "4-6": {"name":"Química analítica", "credits":3, "prereqs":["1-2"]},

 "5-1": {"name":"Balance de materia y energía", "credits":4, "prereqs":["4-1","4-4"]},
 "5-2": {"name":"Sistemas de Información Geográfica (SIG)", "credits":3, "prereqs":["3-4"]},
 "5-3": {"name":"Hidráulica y laboratorio", "credits":4, "prereqs":["4-4"]},
 "5-4": {"name":"Climatología", "credits":3, "prereqs":["2-4"]},
 "5-5": {"name":"Química ambiental y laboratorio", "credits":3, "prereqs":["4-6"]},
 "5-6": {"name":"Procesos comunicativos", "credits":2, "prereqs":[]},

 "6-1": {"name":"Hidrología", "credits":4, "prereqs":["5-3"]},
 "6-2": {"name":"Suelos y laboratorio", "credits":3, "prereqs":["4-5"]},
 "6-3": {"name":"Legislación ambiental", "credits":2, "prereqs":[]},
 "6-4": {"name":"Gestión del agua y laboratorio", "credits":3, "prereqs":["5-3"]},
 "6-5": {"name":"Emisiones atmosféricas y calidad del aire", "credits":3, "prereqs":["5-4"]},
 "6-6": {"name":"Ciencias, tecnología y desarrollo", "credits":2, "prereqs":[]},

 "7-1": {"name":"Manejo de recursos naturales", "credits":3, "prereqs":["2-1","6-2"]},
 "7-2": {"name":"Impacto ambiental", "credits":3, "prereqs":["4-3","5-5"]},
 "7-3": {"name":"Gestión de aguas residuales", "credits":3, "prereqs":["6-1","5-3"]},
 "7-4": {"name":"Gestión de residuos sólidos", "credits":3, "prereqs":["5-5"]},
 "7-5": {"name":"Sistema de gestión ambiental", "credits":3, "prereqs":["7-2"]},
 "7-6": {"name":"Economía ambiental", "credits":2, "prereqs":[]},

 "8-1": {"name":"Ordenamiento territorial", "credits":3, "prereqs":["7-1"]},
 "8-2": {"name":"Saneamiento básico", "credits":3, "prereqs":["7-3"]},
 "8-3": {"name":"Modelación ambiental", "credits":3, "prereqs":["5-3","3-1"]},
 "8-4": {"name":"Tecnologías ambientales", "credits":3, "prereqs":["5-1","5-5"]},
 "8-5": {"name":"Electiva de profundización I", "credits":2, "prereqs":[]},
 "8-6": {"name":"Cátedra Democracia y Paz", "credits":1, "prereqs":[]},

 "9-1": {"name":"Ética y ambiente", "credits":2, "prereqs":[]},
 "9-2": {"name":"Electiva I", "credits":2, "prereqs":[]},
 "9-3": {"name":"Electiva de profundización II", "credits":2, "prereqs":["8-5"]},
 "9-4": {"name":"Gestión del riesgo y prevención de desastres", "credits":3, "prereqs":["7-2"]},
 "9-5": {"name":"Toxicología ambiental", "credits":2, "prereqs":["4-3"]},
 "9-6": {"name":"Anteproyecto de grado", "credits":2, "prereqs":["7-2","8-3"]},

 "10-1": {"name":"Electiva II", "credits":2, "prereqs":[]},
 "10-2": {"name":"Electiva de profundización III", "credits":2, "prereqs":["9-3"]},
 "10-3": {"name":"Proyecto de grado", "credits":6, "prereqs":["9-6"]}
};

const semMap = {
 "1": ["1-1","1-2","1-3","1-4","1-5"],
 "2": ["2-1","2-2","2-3","2-4","2-5"],
 "3": ["3-1","3-2","3-3","3-4","3-5","3-6"],
 "4": ["4-1","4-2","4-3","4-4","4-5","4-6"],
 "5": ["5-1","5-2","5-3","5-4","5-5","5-6"],
 "6": ["6-1","6-2","6-3","6-4","6-5","6-6"],
 "7": ["7-1","7-2","7-3","7-4","7-5","7-6"],
 "8": ["8-1","8-2","8-3","8-4","8-5","8-6"],
 "9": ["9-1","9-2","9-3","9-4","9-5","9-6"],
 "10": ["10-1","10-2","10-3"]
};

const electives_by_semester = {
 "8": ["Gestión de fauna","Restauración ecológica","Gestión de cuencas"],
 "9": ["Monitoreo ambiental","Tecnologías limpias","Educación ambiental"],
 "10": ["Optativa profesional","Prácticas empresariales","Seminario de investigación"]
};

const color_classes = ["color-1","color-2","color-3","color-4"];

function hasAllPrereqs(key){
  const prere = malla[key].prereqs;
  if(!prere || prere.length===0) return true;
  return prere.every(p => localStorage.getItem('completed_'+p) === '1');
}

function renderGrid(){
  const grid = document.querySelector('#gridView .grid');
  grid.innerHTML = '';
  semOrder = Object.keys(semMap);
  semOrder.forEach(sem=>{
    const box = document.createElement('div');
    box.className = 'sem-box';
    box.innerHTML = `<div class="sem-title">Semestre ${sem}°</div>`;
    let idx = 0;
    semMap[sem].forEach(key=>{
      const c = malla[key];
      const color = color_classes[idx % color_classes.length];
      const prereq_names = c.prereqs.map(p => malla[p].name).join(', ') || 'Ninguno';
      const elective_flag = c.name.includes('Electiva') ? ' (Electiva)' : '';
      const card = document.createElement('div');
      card.className = 'course-box ' + color;
      card.id = key;
      card.innerHTML = `<div><div class="course-name">${c.name}${elective_flag}</div><div class="course-meta">${c.credits} cr — Prerrequisitos: ${prereq_names}</div></div><div><input type="checkbox" data-key="${key}" ${localStorage.getItem('completed_'+key)==='1'?'checked':''}></div>`;
      box.appendChild(card);
      idx++;
    });
    grid.appendChild(box);
  });
}

function renderList(){
  const list = document.getElementById('listView');
  list.innerHTML = '';
  Object.keys(semMap).forEach(sem=>{
    const block = document.createElement('div');
    block.style.marginBottom = '10px';
    block.innerHTML = `<div class="sem-title">Semestre ${sem}°</div>`;
    semMap[sem].forEach(key=>{
      const c = malla[key];
      const prereq_names = c.prereqs.map(p => malla[p].name).join(', ') || 'Ninguno';
      const elective_flag = c.name.includes('Electiva') ? ' (Electiva)' : '';
      const row = document.createElement('div');
      row.className = 'course-box color-4';
      row.id = 'list-'+key;
      row.innerHTML = `<div style="flex:1"><div class="course-name">${c.name}${elective_flag}</div><div class="course-meta">${c.credits} cr — Prerrequisitos: ${prereq_names}</div></div><div><input type="checkbox" data-key="${key}" ${localStorage.getItem('completed_'+key)==='1'?'checked':''}></div>`;
      block.appendChild(row);
    });
    list.appendChild(block);
  });
}

function renderElectives(){
  const el = document.getElementById('electivesContent');
  el.innerHTML = '';
  Object.keys(electives_by_semester).forEach(sem=>{
    const div = document.createElement('div');
    div.innerHTML = `<strong>Semestre ${sem}°:</strong> ${electives_by_semester[sem].join(' — ')}`;
    el.appendChild(div);
  });
}

function updateUI(){
  // lock/unlock and reflect completed
  Object.keys(malla).forEach(k=>{
    const card = document.getElementById(k);
    const checkbox = card && card.querySelector('input[type=checkbox]');
    if(!card) return;
    if(hasAllPrereqs(k)) card.classList.remove('locked'); else card.classList.add('locked');
    if(localStorage.getItem('completed_'+k) === '1'){ card.classList.add('completed'); if(checkbox) checkbox.checked = true; } else { card.classList.remove('completed'); if(checkbox) checkbox.checked = false; }
    if(checkbox) checkbox.disabled = !hasAllPrereqs(k);
  });
  // list view checkboxes
  Object.keys(malla).forEach(k=>{
    const listCard = document.getElementById('list-'+k);
    if(!listCard) return;
    const cb = listCard.querySelector('input[type=checkbox]');
    if(cb){
      cb.disabled = !hasAllPrereqs(k);
      cb.checked = localStorage.getItem('completed_'+k) === '1';
      if(localStorage.getItem('completed_'+k) === '1') listCard.classList.add('completed'); else listCard.classList.remove('completed');
    }
  });
  updateCredits();
}

function updateCredits(){
  let completed = 0;
  Object.keys(malla).forEach(k=>{ if(localStorage.getItem('completed_'+k) === '1') completed += malla[k].credits; });
  let badge = document.getElementById('creditsBadge');
  if(!badge){
    badge = document.createElement('div'); badge.id='creditsBadge'; badge.style.marginTop='8px';
    badge.innerHTML = '<strong>Créditos completados: </strong><span id="ccount">0</span> / 159';
    document.querySelector('.panel').appendChild(badge);
  }
  document.getElementById('ccount').innerText = completed;
}

document.addEventListener('click', function(e){
  // handle clicks on checkboxes inside course-box or list
  if(e.target && e.target.tagName === 'INPUT' && e.target.type === 'checkbox' && e.target.dataset.key){
    const key = e.target.dataset.key;
    if(e.target.checked) localStorage.setItem('completed_'+key,'1'); else localStorage.removeItem('completed_'+key);
    renderGrid(); renderList(); attachCheckboxHandlers(); updateUI();
  }
});

function attachCheckboxHandlers(){
  document.querySelectorAll('.course-box input[type=checkbox]').forEach(cb=>{
    cb.onchange = function(){
      const k = this.dataset.key;
      if(this.checked) localStorage.setItem('completed_'+k,'1'); else localStorage.removeItem('completed_'+k);
      renderGrid(); renderList(); attachCheckboxHandlers(); updateUI();
    };
  });
  document.querySelectorAll('#listView input[type=checkbox]').forEach(cb=>{
    cb.onchange = function(){
      const k = this.dataset.key;
      if(this.checked) localStorage.setItem('completed_'+k,'1'); else localStorage.removeItem('completed_'+k);
      renderGrid(); renderList(); attachCheckboxHandlers(); updateUI();
    };
  });
}

document.getElementById('reset').addEventListener('click', ()=>{ Object.keys(malla).forEach(k=> localStorage.removeItem('completed_'+k)); renderGrid(); renderList(); attachCheckboxHandlers(); updateUI(); });

document.getElementById('viewList').addEventListener('click', ()=>{ document.getElementById('gridView').style.display='none'; document.getElementById('listView').style.display='block'; });
document.getElementById('viewGrid').addEventListener('click', ()=>{ document.getElementById('gridView').style.display='block'; document.getElementById('listView').style.display='none'; });

document.getElementById('search').addEventListener('input', function(){
  const q = this.value.trim().toLowerCase();
  document.querySelectorAll('.course-box').forEach(cb=>{
    const name = cb.querySelector('.course-name').innerText.toLowerCase();
    cb.style.display = (!q || name.includes(q)) ? 'flex' : 'none';
  });
  document.querySelectorAll('#listView .course-box').forEach(cb=>{
    const name = cb.querySelector('.course-name').innerText.toLowerCase();
    cb.style.display = (!q || name.includes(q)) ? 'flex' : 'none';
  });
});

document.getElementById('download').addEventListener('click', function(){ window.print(); });

/* Inicialización */
renderGrid();
renderList();
renderElectives();
attachCheckboxHandlers();
updateUI();
</script>

</div>
</body>
</html>
