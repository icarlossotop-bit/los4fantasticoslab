<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Los 4 Fantásticos · Laboratorio virtual de enfriamiento</title>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:wght@600;800&family=Nunito:wght@400;600;700&display=swap">
<style>
:root{
  box-sizing:border-box;
  padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px);
  --bg:#fcf5e8;--panel:#fff;--ink:#251b48;--mute:#5b5478;--line:#e6dcc6;
  --raw:#b3a6d6;--sm:#251b48;--mod:#d9472f;--err:#7a3fc0;--btn:#f6b73c;--btnink:#251b48;--hl:#bf3a22;
}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
@media (prefers-color-scheme:dark){
  :root:not([data-theme="light"]){--bg:#251b48;--panel:#32275c;--ink:#fcf5e8;--mute:#b9b0d6;--line:#463a77;--raw:#8f84b8;--sm:#f6b73c;--mod:#f46f5a;--err:#c7a6ff;--hl:#f46f5a;color-scheme:dark}
}
:root[data-theme="dark"]{--bg:#251b48;--panel:#32275c;--ink:#fcf5e8;--mute:#b9b0d6;--line:#463a77;--raw:#8f84b8;--sm:#f6b73c;--mod:#f46f5a;--err:#c7a6ff;--hl:#f46f5a;color-scheme:dark}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--ink);font:15px/1.55 Nunito,"Segoe UI",system-ui,sans-serif}
main{max-width:1080px;margin:0 auto;padding:22px 16px 44px}
h1{font:800 2.1rem/1.1 Fraunces,Georgia,serif;margin:0 0 6px}
h2{font:600 1.15rem/1.2 Fraunces,Georgia,serif;margin:0 0 8px;color:var(--hl)}
.sub{color:var(--mute);margin:0 0 20px;max-width:70ch}
.panel{background:var(--panel);border:1px solid var(--line);border-radius:14px;padding:16px;margin-bottom:14px}
.grid{display:grid;grid-template-columns:1.2fr 1fr;gap:14px}
@media(max-width:780px){.grid{grid-template-columns:1fr}}
textarea{width:100%;min-height:150px;resize:vertical;background:var(--bg);color:var(--ink);border:1px solid var(--line);border-radius:10px;padding:8px;font:13px/1.4 ui-monospace,Consolas,monospace}
label{display:block;font-size:.82rem;color:var(--mute);margin:8px 0 3px}
input{width:100%;background:var(--bg);color:var(--ink);border:1px solid var(--line);border-radius:10px;padding:7px 9px;font:inherit}
.row{display:flex;gap:8px;flex-wrap:wrap;margin-top:10px}
button{background:var(--btn);color:var(--btnink);border:0;border-radius:10px;padding:8px 14px;font:700 .92rem Nunito,sans-serif;cursor:pointer}
button.sec{background:transparent;color:var(--ink);border:1px solid var(--line)}
button:focus-visible,input:focus-visible,textarea:focus-visible,summary:focus-visible{outline:2px solid var(--mod);outline-offset:2px}
.p2{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(135px,1fr));gap:10px}
.stat{border-left:4px solid var(--btn);padding:2px 10px}
.stat b{display:block;font:800 1.4rem Fraunces,Georgia,serif}
.stat span{color:var(--mute);font-size:.8rem}
canvas{width:100%;display:block}
.legend{display:flex;gap:16px;flex-wrap:wrap;font-size:.85rem;color:var(--mute);margin-bottom:6px}
.legend i{display:inline-block;width:20px;height:3px;vertical-align:middle;margin-right:6px;border-radius:2px}
.msg{font-size:.85rem;color:var(--mute);min-height:1.2em;margin-top:8px}
.tw{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:.9rem}
th,td{text-align:right;padding:6px 8px;border-bottom:1px solid var(--line)}
th:first-child,td:first-child{text-align:left}
tfoot td{font-weight:700}
pre{background:var(--bg);border:1px solid var(--line);border-radius:10px;padding:10px;overflow-x:auto;font-size:12.5px}
summary{cursor:pointer;font-weight:700}
</style>
</head>
<body>
<main>
<h1>Laboratorio virtual de enfriamiento</h1>
<p class="sub">Registra cómo baja la temperatura del agua con el DS18B20, suaviza la curva con media móvil simple y compárala con la Ley de Enfriamiento de Newton.</p>

<div class="grid">
  <section class="panel">
    <h2>1. Captura</h2>
    <label for="txt">Salida serie del ESP32 (un valor por línea) o CSV <code>t,T</code></label>
    <textarea id="txt" spellcheck="false" placeholder="Temperatura del agua: 84.7 °C&#10;Temperatura del agua: 84.5 °C"></textarea>
    <div class="row">
      <button id="bParse">Dibujar</button>
      <button id="bSerial" class="sec">Conectar ESP32 (USB)</button>
      <button id="bFile" class="sec">Cargar archivo</button>
      <button id="bDemo" class="sec">Ensayo de ejemplo</button>
      <button id="bClear" class="sec">Limpiar</button>
    </div>
    <input id="file" type="file" accept=".txt,.csv,.log" hidden>
    <div class="msg" id="msg"></div>
  </section>

  <section class="panel">
    <h2>2. Parámetros</h2>
    <div class="p2">
      <div><label for="tamb">Temperatura ambiente (°C)</label><input id="tamb" type="number" value="22" step="any"></div>
      <div><label for="win">Ventana media móvil (muestras)</label><input id="win" type="number" value="5" min="1" step="1"></div>
      <div><label for="dt">Intervalo entre lecturas (s)</label><input id="dt" type="number" value="1" min="0.01" step="any"></div>
      <div><label for="kman">k manual (1/s), vacío = automático</label><input id="kman" type="number" step="any" min="0" placeholder="automático"></div>
    </div>
    <label for="tref">Termómetro de referencia, lectura final (°C, opcional)</label>
    <input id="tref" type="number" step="any" placeholder="p. ej. 41.3">
    <p class="msg">Modelo: T(t) = T<sub>amb</sub> + (T₀ − T<sub>amb</sub>)·e<sup>−kt</sup>. La constante k se estima con una regresión de ln(T − T<sub>amb</sub>) contra t.</p>
  </section>
</div>

<section class="panel"><div class="stats" id="stats"></div></section>

<section class="panel">
  <div class="legend">
    <span><i style="background:var(--raw)"></i>Lectura cruda del sensor</span>
    <span><i style="background:var(--sm)"></i>Curva real (media móvil)</span>
    <span><i style="background:var(--mod)"></i>Modelo de Newton</span>
  </div>
  <canvas id="c1" height="350" role="img" aria-label="Curva de enfriamiento real y modelo de Newton"></canvas>
</section>

<section class="panel">
  <div class="legend"><span><i style="background:var(--err)"></i>Error = curva real − modelo (°C)</span></div>
  <canvas id="c2" height="160" role="img" aria-label="Error entre la curva real y el modelo"></canvas>
</section>

<section class="panel">
  <h2>3. Validación por repeticiones</h2>
  <p class="msg" style="margin-top:0">Guarda cada ensayo; con al menos 5 se calculan promedio y desviación estándar.</p>
  <div class="row" style="margin:0 0 10px"><button id="bSave">Guardar este ensayo</button><button id="bRuns" class="sec">Borrar ensayos</button></div>
  <div class="tw"><table>
    <thead><tr><th>Ensayo</th><th>k (1/s)</th><th>RMSE (°C)</th><th>Error % vs modelo</th><th>Error % vs referencia</th></tr></thead>
    <tbody id="runs"></tbody><tfoot id="runsF"></tfoot>
  </table></div>
</section>

<section class="panel">
  <details>
    <summary>Sketch del ESP32 para este laboratorio</summary>
    <p class="msg">Lee solo el DS18B20 (pin 4) y envía una lectura por segundo:</p>
<pre>#include &lt;OneWire.h&gt;
#include &lt;DallasTemperature.h&gt;

OneWire ow(4);
DallasTemperature ds(&amp;ow);

void setup() {
  Serial.begin(115200);
  ds.begin();
}

void loop() {
  ds.requestTemperatures();
  float t = ds.getTempCByIndex(0);
  Serial.print("Temperatura del agua: ");
  Serial.print(t, 1);
  Serial.println(" °C");
  delay(1000);
}</pre>
    <p class="msg">En la simulación, baja a mano el valor de temperatura del DS18B20 para imitar el enfriamiento.</p>
  </details>
</section>
</main>

<script>
(function(){
  var $=function(id){return document.getElementById(id)};
  var T=[],hover=-1,runs=[],cur=null;
  var NUM=/(-?\d+(?:[.,]\d+)?)\s*(?:°|º)?\s*[cC]?\s*$/;
  var num=function(s){return parseFloat(String(s).replace(',','.'))};
  var f=function(v,d){return (v==null||!isFinite(v))?'–':v.toFixed(d)};

  function addLine(raw){
    var l=raw.trim();if(!l)return false;
    if(/^[-+\d.\s,;\t]+$/.test(l)&&/[;\t]|,\s*-?\d/.test(l)){
      var p=l.split(/[;\t,]/).map(num).filter(function(v){return !isNaN(v)});
      if(p.length>=2){T.push({t:p[0],raw:p[1]});return true}
    }
    var m=l.match(NUM);if(!m)return false;
    var v=num(m[1]);if(isNaN(v))return false;
    T.push({t:null,raw:v});return true;
  }
  function times(){var dt=parseFloat($('dt').value);if(!(dt>0))dt=1;T.forEach(function(r,i){if(r.t==null||r.auto){r.t=i*dt;r.auto=true}})}

  function compute(){
    times();
    var N=Math.max(1,Math.round(parseFloat($('win').value)||1)),amb=parseFloat($('tamb').value);
    var rows=T.map(function(r,i){
      var a=Math.max(0,i-N+1),s=0;for(var j=a;j<=i;j++)s+=T[j].raw;
      return {t:r.t,raw:r.raw,sm:s/(i-a+1),mod:null};
    });
    cur=null;
    if(rows.length<3||isNaN(amb))return rows;
    var T0=rows[0].sm,t0=rows[0].t;
    // regresión ln(T-Tamb) vs t
    var n=0,sx=0,sy=0,sxx=0,sxy=0;
    rows.forEach(function(r){var d=r.sm-amb;if(d>0.3){var y=Math.log(d);n++;sx+=r.t;sy+=y;sxx+=r.t*r.t;sxy+=r.t*y}});
    var kAuto=null;
    if(n>=3){var den=n*sxx-sx*sx;if(Math.abs(den)>1e-12)kAuto=-(n*sxy-sx*sy)/den}
    var km=parseFloat($('kman').value),k=(km>=0)?km:kAuto;
    if(k==null||!isFinite(k))return rows;
    rows.forEach(function(r){r.mod=amb+(T0-amb)*Math.exp(-k*(r.t-t0))});
    var e=rows.map(function(r){return r.sm-r.mod}),m=rows.length;
    var rmse=Math.sqrt(e.reduce(function(a,b){return a+b*b},0)/m);
    var mae=e.reduce(function(a,b){return a+Math.abs(b)},0)/m;
    var pct=rows.reduce(function(a,r){return a+Math.abs(r.sm-r.mod)/Math.abs(r.mod)*100},0)/m;
    var mean=rows.reduce(function(a,r){return a+r.sm},0)/m;
    var sst=rows.reduce(function(a,r){return a+Math.pow(r.sm-mean,2)},0),ssr=e.reduce(function(a,b){return a+b*b},0);
    var tref=parseFloat($('tref').value),last=rows[rows.length-1].sm;
    cur={k:k,tau:k>0?1/k:null,T0:T0,rmse:rmse,mae:mae,pct:pct,r2:sst>0?1-ssr/sst:null,
         pref:(isFinite(tref)&&tref!==0)?Math.abs(last-tref)/Math.abs(tref)*100:null,auto:!(km>=0),n:m};
    return rows;
  }

  function stats(){
    var b=$('stats');
    if(!cur){b.innerHTML='<div class="stat"><b>'+T.length+'</b><span>lecturas; se necesitan al menos 3 por encima de la temperatura ambiente</span></div>';return}
    b.innerHTML=
      '<div class="stat"><b>'+f(cur.k,5)+'</b><span>k (1/s) '+(cur.auto?'estimada':'manual')+'</span></div>'+
      '<div class="stat"><b>'+f(cur.tau,1)+' s</b><span>constante de tiempo 1/k</span></div>'+
      '<div class="stat"><b>'+f(cur.T0,1)+' °C</b><span>temperatura inicial T₀</span></div>'+
      '<div class="stat"><b>'+f(cur.rmse,2)+' °C</b><span>RMSE vs modelo</span></div>'+
      '<div class="stat"><b>'+f(cur.pct,2)+' %</b><span>error porcentual medio</span></div>'+
      '<div class="stat"><b>'+f(cur.r2,3)+'</b><span>R² del ajuste</span></div>';
  }

  function css(n){return getComputedStyle(document.documentElement).getPropertyValue(n).trim()}
  function setup(c){
    var d=window.devicePixelRatio||1,w=c.clientWidth,h=parseInt(c.getAttribute('height'),10);
    c.width=Math.round(w*d);c.height=Math.round(h*d);
    var g=c.getContext('2d');g.setTransform(d,0,0,d,0,0);g.clearRect(0,0,w,h);return{g:g,w:w,h:h};
  }
  function step(r){var raw=r/5,p=Math.pow(10,Math.floor(Math.log10(raw))),b=raw/p;return(b<1.5?1:b<3?2:b<7?5:10)*p}

  function plot(c,S,defs,zero){
    var o=setup(c),g=o.g,W=o.w,H=o.h,L=46,R=12,Tp=10,B=28,pw=W-L-R,ph=H-Tp-B,ink=css('--mute'),ln=css('--line');
    var vals=[];S.forEach(function(s){defs.forEach(function(d){var v=d.get(s);if(v!=null&&isFinite(v))vals.push(v)})});
    if(zero)vals.push(0);
    if(!vals.length){g.fillStyle=ink;g.font='14px sans-serif';g.textAlign='center';g.fillText('Sin datos todavía',W/2,H/2);return null}
    var xmin=S[0].t,xmax=S[S.length-1].t;if(xmax<=xmin)xmax=xmin+1;
    var ymin=Math.min.apply(null,vals),ymax=Math.max.apply(null,vals);if(ymax-ymin<1e-6){ymax+=1;ymin-=1}
    var pad=(ymax-ymin)*.08;ymin-=pad;ymax+=pad;
    var X=function(x){return L+(x-xmin)/(xmax-xmin)*pw},Y=function(y){return Tp+(1-(y-ymin)/(ymax-ymin))*ph};
    g.font='11px Nunito,sans-serif';g.lineWidth=1;
    var st=step(ymax-ymin);
    for(var y=Math.ceil(ymin/st)*st;y<=ymax;y+=st){g.strokeStyle=ln;g.beginPath();g.moveTo(L,Y(y));g.lineTo(W-R,Y(y));g.stroke();g.fillStyle=ink;g.textAlign='right';g.fillText(Math.round(y*100)/100+'',L-6,Y(y)+4)}
    var sx=step(xmax-xmin);
    for(var x=Math.ceil(xmin/sx)*sx;x<=xmax+1e-9;x+=sx){g.fillStyle=ink;g.textAlign='center';g.fillText(Math.round(x*100)/100+' s',X(x),H-9)}
    g.strokeStyle=ink;g.beginPath();g.moveTo(L,Tp);g.lineTo(L,Tp+ph);g.lineTo(W-R,Tp+ph);g.stroke();
    if(zero){g.setLineDash([4,4]);g.beginPath();g.moveTo(L,Y(0));g.lineTo(W-R,Y(0));g.stroke();g.setLineDash([])}
    defs.forEach(function(d){
      g.strokeStyle=d.color;g.fillStyle=d.color;g.lineWidth=d.w;g.setLineDash(d.dash||[]);
      if(d.dots){S.forEach(function(s){var v=d.get(s);if(v!=null){g.beginPath();g.arc(X(s.t),Y(v),1.8,0,6.283);g.fill()}})}
      else{g.beginPath();var pen=false;S.forEach(function(s){var v=d.get(s);if(v==null){pen=false;return}if(!pen){g.moveTo(X(s.t),Y(v));pen=true}else g.lineTo(X(s.t),Y(v))});g.stroke()}
      g.setLineDash([]);
    });
    if(hover>=0&&hover<S.length){
      var s=S[hover],px=X(s.t);g.strokeStyle=ink;g.lineWidth=1;g.setLineDash([3,3]);g.beginPath();g.moveTo(px,Tp);g.lineTo(px,Tp+ph);g.stroke();g.setLineDash([]);
      var tx='t='+f(s.t,0)+' s';defs.forEach(function(d){var v=d.get(s);if(v!=null)tx+='   '+d.name+': '+f(v,2)});
      g.font='12px Nunito,sans-serif';var tw=g.measureText(tx).width+12,x0=Math.min(Math.max(px-tw/2,L),W-R-tw);
      g.fillStyle=css('--panel');g.strokeStyle=ln;g.fillRect(x0,Tp+2,tw,20);g.strokeRect(x0,Tp+2,tw,20);
      g.fillStyle=css('--ink');g.textAlign='left';g.fillText(tx,x0+6,Tp+16);
    }
    return{L:L,pw:pw,xmin:xmin,xmax:xmax};
  }

  var geo=null,S=[];
  function render(){
    S=compute();stats();
    geo=plot($('c1'),S,[
      {name:'Cruda',get:function(s){return s.raw},color:css('--raw'),w:1,dots:true},
      {name:'Real',get:function(s){return s.sm},color:css('--sm'),w:2.4},
      {name:'Modelo',get:function(s){return s.mod},color:css('--mod'),w:2.4,dash:[7,5]}
    ]);
    plot($('c2'),S,[{name:'Error',get:function(s){return s.mod==null?null:s.sm-s.mod},color:css('--err'),w:2}],true);
  }
  function renderRuns(){
    $('runs').innerHTML=runs.map(function(r,i){
      return '<tr><td>'+(i+1)+'</td><td>'+f(r.k,5)+'</td><td>'+f(r.rmse,2)+'</td><td>'+f(r.pct,2)+'</td><td>'+f(r.pref,2)+'</td></tr>';
    }).join('');
    var F=$('runsF');
    if(!runs.length){F.innerHTML='';return}
    var col=function(key){return runs.map(function(r){return r[key]}).filter(function(v){return v!=null&&isFinite(v)})};
    var mean=function(a){return a.length?a.reduce(function(x,y){return x+y},0)/a.length:null};
    var sd=function(a){if(a.length<2)return null;var m=mean(a);return Math.sqrt(a.reduce(function(x,y){return x+Math.pow(y-m,2)},0)/(a.length-1))};
    var ks=['k','rmse','pct','pref'],d=[5,2,2,2];
    F.innerHTML='<tr><td>Promedio</td>'+ks.map(function(k,i){return '<td>'+f(mean(col(k)),d[i])+'</td>'}).join('')+'</tr>'+
      '<tr><td>Desv. estándar</td>'+ks.map(function(k,i){return '<td>'+f(sd(col(k)),d[i])+'</td>'}).join('')+'</tr>'+
      (runs.length<5?'<tr><td colspan="5" style="text-align:left;font-weight:400;color:var(--mute)">Faltan '+(5-runs.length)+' ensayo(s) para llegar a las 5 repeticiones recomendadas.</td></tr>':'');
  }

  var msg=function(t){$('msg').textContent=t};
  function loadText(txt){T=[];var n=0;txt.split(/\r?\n/).forEach(function(l){if(addLine(l))n++});return n}

  $('bParse').onclick=function(){
    var n=loadText($('txt').value);
    msg(n?('Se leyeron '+n+' lecturas.'):'No encontré valores. Cada línea debe terminar en un número, por ejemplo “Temperatura del agua: 84.7 °C”.');
    render();
  };
  $('bClear').onclick=function(){T=[];$('txt').value='';msg('');render()};
  $('bFile').onclick=function(){$('file').click()};
  $('file').onchange=function(e){var fl=e.target.files[0];if(!fl)return;var r=new FileReader();r.onload=function(){$('txt').value=r.result;$('bParse').click()};r.readAsText(fl)};
  $('bDemo').onclick=function(){
    var out=[],k=0.012,amb=22,t0=85,seed=1;
    var rnd=function(){seed=(seed*16807)%2147483647;return seed/2147483647-.5};
    for(var i=0;i<240;i++){out.push('Temperatura del agua: '+(amb+(t0-amb)*Math.exp(-k*i*1.02)+rnd()*0.8).toFixed(1)+' °C')}
    $('txt').value=out.join('\n');$('tamb').value=22;$('kman').value='';$('bParse').click();
  };
  $('bSave').onclick=function(){
    if(!cur){msg('Primero dibuja un ensayo con datos suficientes.');return}
    runs.push({k:cur.k,rmse:cur.rmse,pct:cur.pct,pref:cur.pref});renderRuns();msg('Ensayo '+runs.length+' guardado.');
  };
  $('bRuns').onclick=function(){runs=[];renderRuns()};

  $('bSerial').onclick=async function(){
    if(!('serial' in navigator)){msg('Este navegador no soporta Web Serial. Usa Chrome o Edge, o pega el texto del monitor serie.');return}
    try{
      var port=await navigator.serial.requestPort();await port.open({baudRate:115200});
      msg('Conectado a 115200 baudios. Recibiendo lecturas…');T=[];$('txt').value='';
      var dec=new TextDecoderStream();port.readable.pipeTo(dec.writable).catch(function(){});
      var rd=dec.readable.getReader(),buf='';
      for(;;){var r=await rd.read();if(r.done)break;buf+=r.value;var ps=buf.split(/\r?\n/);buf=ps.pop();
        ps.forEach(function(l){if(addLine(l))$('txt').value+=l+'\n'});render()}
    }catch(err){msg('No se pudo usar el puerto serie: '+(err&&err.message?err.message:err))}
  };

  ['tamb','win','dt','kman','tref'].forEach(function(id){$(id).addEventListener('input',render)});
  $('c1').addEventListener('mousemove',function(e){
    if(!geo||!S.length)return;var rc=this.getBoundingClientRect(),fr=(e.clientX-rc.left-geo.L)/geo.pw;
    hover=Math.max(0,Math.min(S.length-1,Math.round(fr*(S.length-1))));render();
  });
  $('c1').addEventListener('mouseleave',function(){hover=-1;render()});
  window.addEventListener('resize',render);
  if(window.matchMedia)window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change',render);
  renderRuns();$('bDemo').click();
})();
</script>
</body>
</html>
