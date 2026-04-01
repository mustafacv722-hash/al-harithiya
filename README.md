<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
<title>UltimateAI - Smart Calculator Hub</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/decimal.js/10.4.3/decimal.min.js"></script>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{margin:0;padding:20px;font-family:'Segoe UI',Arial,sans-serif;background:radial-gradient(ellipse at center,#0a0f2a 0%,#03050b 100%);color:white;display:flex;justify-content:center;align-items:center;min-height:100vh;text-align:center;flex-direction:column;position:relative;overflow-x:hidden}
.stars{position:fixed;top:0;left:0;width:100%;height:100%;pointer-events:none;z-index:0}
.star{position:absolute;background:white;border-radius:50%;animation:moveUp linear infinite}
@keyframes moveUp{0%{transform:translateY(100vh);opacity:0.8}100%{transform:translateY(-20vh);opacity:0}}
.container{max-width:500px;width:90%;margin:auto;position:relative;z-index:1;background:rgba(10,15,40,0.6);backdrop-filter:blur(10px);border-radius:24px;padding:30px 20px;border:1px solid rgba(255,255,255,0.2);box-shadow:0 10px 40px rgba(0,0,0,0.3)}
.hidden{display:none}
button,.btn,.menu-btn{padding:12px 20px;margin:8px;border:none;border-radius:12px;color:white;background:linear-gradient(135deg,#ff9a9e,#fad0c4);cursor:pointer;transition:all 0.3s ease;font-size:16px;font-weight:600;box-shadow:0 4px 15px rgba(0,0,0,0.2)}
button:hover,.btn:hover,.menu-btn:hover{transform:translateY(-2px);box-shadow:0 6px 20px rgba(0,0,0,0.3);background:linear-gradient(135deg,#ff7a7e,#f8b0a4)}
input,select{padding:12px;width:85%;border-radius:12px;border:1px solid rgba(255,255,255,0.3);text-align:center;margin:10px auto;background:rgba(255,255,255,0.1);color:white;font-size:16px;outline:none;transition:0.2s}
input:focus,select:focus{border-color:#ff9a9e;background:rgba(255,255,255,0.2)}
select option{background:#0a0f2a;color:white}
.result-box{margin:20px auto;padding:15px;background:rgba(0,0,0,0.3);border-radius:16px;border:1px solid rgba(255,255,255,0.15)}
.result,.explain{margin:10px;padding:10px;background:rgba(0,0,0,0.3);border-radius:10px;font-size:16px}
.math-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin:15px auto}
.math-grid button{padding:12px;font-size:18px;font-weight:bold}
.physics-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:8px;margin:15px 0}
.physics-grid button{font-size:12px;padding:10px 8px;margin:4px}
.shape-buttons{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin:15px 0}
.shape-buttons button{font-size:14px;padding:12px}
#puzzleOptions{display:flex;flex-direction:column;gap:10px;margin:20px auto;width:90%}
#puzzleOptions button{width:100%;padding:12px;font-size:16px}
#puzzleQuestion{font-size:20px;font-weight:500;margin:20px;padding:20px;background:rgba(0,0,0,0.4);border-radius:16px}
h1{font-size:28px;background:linear-gradient(135deg,#fff,#ff9a9e);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;margin-bottom:20px}
h2{font-size:24px;margin-bottom:20px;color:#ff9a9e}
.back-btn{margin-top:20px;background:linear-gradient(135deg,#667eea,#764ba2)}
.back-btn:hover{background:linear-gradient(135deg,#5a67d8,#6b46a1)}
#overlay{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;z-index:1000}
#overlay3d{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;z-index:1000}
#puzzlePopup{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;z-index:1001}
.popup{background:linear-gradient(135deg,#1a1f3a,#0f1328);border:1px solid rgba(255,255,255,0.3);border-radius:20px;padding:30px;width:350px;max-width:90%;text-align:center}
.popup input{width:90%;margin:10px auto;padding:10px}
.popup button{margin:10px 5px}
.puzzle-popup-correct{border:2px solid #4caf50;box-shadow:0 0 30px rgba(76,175,80,0.3)}
.puzzle-popup-wrong{border:2px solid #f44336;box-shadow:0 0 30px rgba(244,67,54,0.3)}
</style>
</head>
<body>

<div class="stars" id="starsContainer"></div>
<div class="container">

<div id="welcome">
    <h1>✨ UltimateAI ✨</h1>
    <p>Advanced calculator with Math, Physics, Puzzles & 3D shapes</p>
    <button id="startBtn">🚀 Start</button>
</div>

<div id="menu" class="hidden">
    <h2>📱 Select Mode</h2>
    <button class="menu-btn" onclick="showPage('math')">🧮 Math</button>
    <button class="menu-btn" onclick="showPage('physics')">⚡ Physics</button>
    <button class="menu-btn" onclick="showPage('puzzle')">🎮 Puzzle</button>
    <button class="menu-btn" onclick="showPage('ai3d')">🎨 3D & AI</button>
</div>

<div id="math" class="hidden">
    <h2>🧮 Math Calculator</h2>
    <input id="mathInput" placeholder="Enter operation...">
    <div class="math-grid">
        <button onclick="addMath('+')">+</button>
        <button onclick="addMath('-')">-</button>
        <button onclick="addMath('*')">×</button>
        <button onclick="addMath('/')">÷</button>
        <button onclick="addMath('**')">^</button>
        <button onclick="addMath('%')">%</button>
        <button onclick="addMath('Math.sqrt(')">√</button>
        <button onclick="addMath('Math.PI')">π</button>
        <button onclick="addMath('Math.E')">e</button>
    </div>
    <div>
        <button onclick="clearMath()">🗑 Clear</button>
        <button onclick="calculateMath()">= Calculate</button>
    </div>
    <div class="result-box">
        <div class="result" id="mathRes">📊 Result: ---</div>
        <div class="explain" id="mathExplain">📝 Explanation: ---</div>
    </div>
    <button class="back-btn" onclick="showPage('menu')">🔙 Back to Menu</button>
</div>

<div id="physics" class="hidden">
    <h2>⚡ Physics Laws (20 Laws)</h2>
    <div class="physics-grid">
        <button onclick="showPhysicsPopup('Force')">💪 Force (F=ma)</button>
        <button onclick="showPhysicsPopup('Pressure')">📌 Pressure (P=F/A)</button>
        <button onclick="showPhysicsPopup('Kinetic Energy')">⚡ Kinetic Energy (½mv²)</button>
        <button onclick="showPhysicsPopup('Potential Energy')">🌍 Potential Energy (mgh)</button>
        <button onclick="showPhysicsPopup('Work')">🔧 Work (W=Fd)</button>
        <button onclick="showPhysicsPopup('Power')">💡 Power (P=W/t)</button>
        <button onclick="showPhysicsPopup('Momentum')">🎯 Momentum (p=mv)</button>
        <button onclick="showPhysicsPopup('Density')">⚖️ Density (ρ=m/V)</button>
        <button onclick="showPhysicsPopup('Acceleration')">📈 Acceleration (a=Δv/t)</button>
        <button onclick="showPhysicsPopup('Velocity')">🏃 Velocity (v=d/t)</button>
        <button onclick="showPhysicsPopup('Ohm Law')">🔌 Ohm's Law (V=IR)</button>
        <button onclick="showPhysicsPopup('Newton Gravity')">🌌 Newton Gravity</button>
        <button onclick="showPhysicsPopup('Hooke Law')">🪢 Hooke's Law (F=-kx)</button>
        <button onclick="showPhysicsPopup('Einstein E')">⚛️ E=mc²</button>
        <button onclick="showPhysicsPopup('Impulse')">💥 Impulse (J=Ft)</button>
        <button onclick="showPhysicsPopup('Torque')">🔄 Torque (τ=F×r)</button>
        <button onclick="showPhysicsPopup('Frequency')">📡 Frequency (f=1/T)</button>
        <button onclick="showPhysicsPopup('Wavelength')">🌊 Wavelength (λ=v/f)</button>
        <button onclick="showPhysicsPopup('Capacitance')">🔋 Capacitance (C=Q/V)</button>
        <button onclick="showPhysicsPopup('Resistance')">⚡ Resistance (R=ρL/A)</button>
    </div>
    <button class="back-btn" onclick="showPage('menu')">🔙 Back to Menu</button>
</div>

<div id="puzzle" class="hidden">
    <h2>🎮 Puzzle Game</h2>
    <select id="puzzleLevelSelect">
        <option value="easy">😊 Easy</option>
        <option value="medium">🤔 Medium</option>
        <option value="hard">🔥 Hard</option>
    </select>
    <button onclick="startPuzzleGame()">🎯 Start Game</button>
    <div id="puzzleQuestion">❓ Select level and press Start</div>
    <div id="puzzleOptions"></div>
    <div id="puzzleScore">⭐ Score: 0</div>
    <button class="back-btn" onclick="showPage('menu')">🔙 Back to Menu</button>
</div>

<div id="ai3d" class="hidden">
    <h2>🎨 3D Shapes Calculator</h2>
    <div class="shape-buttons">
        <button onclick="show3DPopup('cube')">📦 Cube</button>
        <button onclick="show3DPopup('sphere')">⚽ Sphere</button>
        <button onclick="show3DPopup('cone')">📐 Cone</button>
        <button onclick="show3DPopup('pyramid')">🔺 Pyramid</button>
        <button onclick="show3DPopup('cylinder')">🧴 Cylinder</button>
        <button onclick="show3DPopup('rectangular prism')">📏 Rectangular Prism</button>
    </div>
    <button class="back-btn" onclick="showPage('menu')">🔙 Back to Menu</button>
</div>

</div>

<!-- Physics Popup -->
<div id="overlay" style="display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;z-index:1000">
    <div class="popup">
        <h3 id="popupTitle">Physics Law</h3>
        <div id="popupFields"></div>
        <div id="popupResult" style="margin:15px;padding:10px;background:rgba(0,0,0,0.3);border-radius:10px">Result: ---</div>
        <button onclick="closePopup()">Close</button>
    </div>
</div>

<!-- 3D Popup -->
<div id="overlay3d" style="display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;z-index:1000">
    <div class="popup">
        <h3 id="popup3dTitle">3D Shape</h3>
        <div id="popup3dFields"></div>
        <div id="popup3dResult" style="margin:15px;padding:10px;background:rgba(0,0,0,0.3);border-radius:10px">Result: ---</div>
        <button onclick="close3DPopup()">Close</button>
    </div>
</div>

<!-- Puzzle Popup -->
<div id="puzzlePopup" style="display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;z-index:1001">
    <div class="popup" id="puzzlePopupContent">
        <div id="puzzlePopupIcon" style="font-size:50px;margin-bottom:10px">✓</div>
        <h3 id="puzzlePopupTitle">Correct!</h3>
        <p id="puzzlePopupMessage">+10 points</p>
        <button onclick="closePuzzlePopup()">Continue</button>
    </div>
</div>

<script>
// Stars
for(let i=0;i<40;i++){let s=document.createElement('div');s.className='star';let size=Math.random()*3+1;s.style.width=size+'px';s.style.height=size+'px';s.style.left=Math.random()*100+'%';s.style.animationDuration=(Math.random()*8+5)+'s';s.style.animationDelay=(Math.random()*10)+'s';document.getElementById('starsContainer').appendChild(s);}

// Navigation
function showPage(pageId) {
    let pages = ['menu', 'math', 'physics', 'puzzle', 'ai3d'];
    for(let i=0; i<pages.length; i++) {
        let el = document.getElementById(pages[i]);
        if(el) el.classList.add('hidden');
    }
    let target = document.getElementById(pageId);
    if(target) target.classList.remove('hidden');
}

// Start Button
document.getElementById('startBtn').addEventListener('click', function() {
    document.getElementById('welcome').classList.add('hidden');
    document.getElementById('menu').classList.remove('hidden');
});

// ========== MATH FUNCTIONS ==========
function addMath(val){
    let input = document.getElementById('mathInput');
    if(val==='Math.sqrt('){input.value+='Math.sqrt(';}
    else if(val==='Math.PI'){input.value+='Math.PI';}
    else if(val==='Math.E'){input.value+='Math.E';}
    else{input.value+=val;}
}
function clearMath(){
    document.getElementById('mathInput').value='';
    document.getElementById('mathRes').innerHTML='📊 Result: ---';
    document.getElementById('mathExplain').innerHTML='📝 Explanation: ---';
}
function calculateMath(){
    let input = document.getElementById('mathInput').value;
    try{
        let expression = input.replace(/×/g,'*').replace(/÷/g,'/');
        let res = eval(expression);
        document.getElementById('mathRes').innerHTML='📊 Result: '+res;
        document.getElementById('mathExplain').innerHTML='📝 '+input+' = '+res;
    }catch(e){
        document.getElementById('mathRes').innerHTML='❌ Error';
        document.getElementById('mathExplain').innerHTML='⚠️ Invalid expression';
    }
}

// ========== PHYSICS POPUP ==========
let currentPhysicsLaw='';
function showPhysicsPopup(law){
    currentPhysicsLaw=law;
    document.getElementById('popupTitle').innerHTML=law;
    let html='';
    switch(law){
        case 'Force':html='<input id="m" type="number" placeholder="Mass (m) in kg"><br><input id="a" type="number" placeholder="Acceleration (a) in m/s²">';break;
        case 'Pressure':html='<input id="F" type="number" placeholder="Force (F) in N"><br><input id="A" type="number" placeholder="Area (A) in m²">';break;
        case 'Kinetic Energy':html='<input id="m" type="number" placeholder="Mass (m) in kg"><br><input id="v" type="number" placeholder="Velocity (v) in m/s">';break;
        case 'Potential Energy':html='<input id="m" type="number" placeholder="Mass (m) in kg"><br><input id="h" type="number" placeholder="Height (h) in m"><br><small>🌍 g=9.8 m/s²</small>';break;
        case 'Work':html='<input id="F" type="number" placeholder="Force (F) in N"><br><input id="d" type="number" placeholder="Distance (d) in m">';break;
        case 'Power':html='<input id="W" type="number" placeholder="Work (W) in J"><br><input id="t" type="number" placeholder="Time (t) in s">';break;
        case 'Momentum':html='<input id="m" type="number" placeholder="Mass (m) in kg"><br><input id="v" type="number" placeholder="Velocity (v) in m/s">';break;
        case 'Density':html='<input id="m" type="number" placeholder="Mass (m) in kg"><br><input id="V" type="number" placeholder="Volume (V) in m³">';break;
        case 'Acceleration':html='<input id="dv" type="number" placeholder="Change in Velocity (Δv) in m/s"><br><input id="t" type="number" placeholder="Time (t) in s">';break;
        case 'Velocity':html='<input id="d" type="number" placeholder="Distance (d) in m"><br><input id="t" type="number" placeholder="Time (t) in s">';break;
        case 'Ohm Law':html='<input id="I" type="number" placeholder="Current (I) in A"><br><input id="R" type="number" placeholder="Resistance (R) in Ω">';break;
        case 'Newton Gravity':html='<input id="m1" type="number" placeholder="Mass 1 (m₁) in kg"><br><input id="m2" type="number" placeholder="Mass 2 (m₂) in kg"><br><input id="r" type="number" placeholder="Distance (r) in m"><br><small>🌌 G=6.674×10⁻¹¹</small>';break;
        case 'Hooke Law':html='<input id="k" type="number" placeholder="Spring Constant (k) in N/m"><br><input id="x" type="number" placeholder="Displacement (x) in m">';break;
        case 'Einstein E':html='<input id="m" type="number" placeholder="Mass (m) in kg"><br><small>⚛️ c²=9×10¹⁶ m²/s²</small>';break;
        case 'Impulse':html='<input id="F" type="number" placeholder="Force (F) in N"><br><input id="t" type="number" placeholder="Time (t) in s">';break;
        case 'Torque':html='<input id="F" type="number" placeholder="Force (F) in N"><br><input id="r" type="number" placeholder="Radius (r) in m">';break;
        case 'Frequency':html='<input id="T" type="number" placeholder="Period (T) in s">';break;
        case 'Wavelength':html='<input id="v" type="number" placeholder="Wave Speed (v) in m/s"><br><input id="f" type="number" placeholder="Frequency (f) in Hz">';break;
        case 'Capacitance':html='<input id="Q" type="number" placeholder="Charge (Q) in C"><br><input id="V" type="number" placeholder="Voltage (V) in V">';break;
        case 'Resistance':html='<input id="rho" type="number" placeholder="Resistivity (ρ) in Ω·m"><br><input id="L" type="number" placeholder="Length (L) in m"><br><input id="A" type="number" placeholder="Area (A) in m²">';break;
    }
    document.getElementById('popupFields').innerHTML=html+'<br><button onclick="calculatePhysicsPopup()">Calculate</button>';
    document.getElementById('popupResult').innerHTML='Result: ---';
    document.getElementById('overlay').style.display='flex';
}
function closePopup(){document.getElementById('overlay').style.display='none';}
function calculatePhysicsPopup(){
    let res=0,unit='';
    try{
        switch(currentPhysicsLaw){
            case 'Force':res=parseFloat(document.getElementById('m').value)*parseFloat(document.getElementById('a').value);unit='N';break;
            case 'Pressure':res=parseFloat(document.getElementById('F').value)/parseFloat(document.getElementById('A').value);unit='Pa';break;
            case 'Kinetic Energy':res=0.5*parseFloat(document.getElementById('m').value)*Math.pow(parseFloat(document.getElementById('v').value),2);unit='J';break;
            case 'Potential Energy':res=parseFloat(document.getElementById('m').value)*9.8*parseFloat(document.getElementById('h').value);unit='J';break;
            case 'Work':res=parseFloat(document.getElementById('F').value)*parseFloat(document.getElementById('d').value);unit='J';break;
            case 'Power':res=parseFloat(document.getElementById('W').value)/parseFloat(document.getElementById('t').value);unit='W';break;
            case 'Momentum':res=parseFloat(document.getElementById('m').value)*parseFloat(document.getElementById('v').value);unit='kg·m/s';break;
            case 'Density':res=parseFloat(document.getElementById('m').value)/parseFloat(document.getElementById('V').value);unit='kg/m³';break;
            case 'Acceleration':res=parseFloat(document.getElementById('dv').value)/parseFloat(document.getElementById('t').value);unit='m/s²';break;
            case 'Velocity':res=parseFloat(document.getElementById('d').value)/parseFloat(document.getElementById('t').value);unit='m/s';break;
            case 'Ohm Law':res=parseFloat(document.getElementById('I').value)*parseFloat(document.getElementById('R').value);unit='V';break;
            case 'Newton Gravity':let m1=parseFloat(document.getElementById('m1').value),m2=parseFloat(document.getElementById('m2').value),r=parseFloat(document.getElementById('r').value);res=6.674e-11*m1*m2/(r*r);unit='N';break;
            case 'Hooke Law':res=parseFloat(document.getElementById('k').value)*parseFloat(document.getElementById('x').value);unit='N';break;
            case 'Einstein E':res=parseFloat(document.getElementById('m').value)*9e16;unit='J';break;
            case 'Impulse':res=parseFloat(document.getElementById('F').value)*parseFloat(document.getElementById('t').value);unit='N·s';break;
            case 'Torque':res=parseFloat(document.getElementById('F').value)*parseFloat(document.getElementById('r').value);unit='N·m';break;
            case 'Frequency':res=1/parseFloat(document.getElementById('T').value);unit='Hz';break;
            case 'Wavelength':res=parseFloat(document.getElementById('v').value)/parseFloat(document.getElementById('f').value);unit='m';break;
            case 'Capacitance':res=parseFloat(document.getElementById('Q').value)/parseFloat(document.getElementById('V').value);unit='F';break;
            case 'Resistance':res=parseFloat(document.getElementById('rho').value)*parseFloat(document.getElementById('L').value)/parseFloat(document.getElementById('A').value);unit='Ω';break;
        }
        if(isNaN(res))throw new Error();
        document.getElementById('popupResult').innerHTML='Result: '+res.toFixed(6)+' '+unit;
    }catch(e){document.getElementById('popupResult').innerHTML='❌ Error: Enter valid numbers';}
}

// ========== 3D POPUP ==========
let current3DShape='';
function show3DPopup(shape){
    current3DShape=shape;
    document.getElementById('popup3dTitle').innerHTML=shape.toUpperCase();
    let html='';
    if(shape==='cube'){
        html='<input id="side" type="number" placeholder="Side length (cm)"><br><select id="calcType3D"><option valu
