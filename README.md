# 7rofDrayah
لعبة خلية الحروف نسخة دراية
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>خلية الحروف | مجموعة دراية</title>

<style>
@font-face{
  font-family:'Thmanyah';
  src:url('fonts/thmanyahserifdisplay-Light.otf') format('opentype');
  font-weight:300;
  font-style:normal;
  font-display:swap;
}

@font-face{
  font-family:'Thmanyah';
  src:url('fonts/thmanyahserifdisplay-Regular.otf') format('opentype');
  font-weight:400;
  font-style:normal;
  font-display:swap;
}

@font-face{
  font-family:'Thmanyah';
  src:url('fonts/thmanyahserifdisplay-Medium.otf') format('opentype');
  font-weight:500;
  font-style:normal;
  font-display:swap;
}

@font-face{
  font-family:'Thmanyah';
  src:url('fonts/thmanyahserifdisplay-Bold.otf') format('opentype');
  font-weight:700;
  font-style:normal;
  font-display:swap;
}

@font-face{
  font-family:'Thmanyah';
  src:url('fonts/thmanyahserifdisplay-Black.otf') format('opentype');
  font-weight:900;
  font-style:normal;
  font-display:swap;
}

*{box-sizing:border-box}

:root{
  --bg1:#050712;
  --bg2:#101827;
  --text:#fff;
  --cell:#f8fafc;
  --cellText:#111827;
  --teamLR:#f97316;
  --teamTB:#2563eb;
  --gold:#d4af37;
}

body.light{
  --bg1:#f8fafc;
  --bg2:#e5e7eb;
  --text:#111827;
  --cell:#ffffff;
  --cellText:#111827;
}

body{
  margin:0;
  min-height:100vh;
  font-family:'Thmanyah',sans-serif;
  background:
    radial-gradient(circle at top,rgba(212,175,55,.25),transparent 28%),
    linear-gradient(135deg,var(--bg1),var(--bg2) 55%,var(--bg1));
  display:flex;
  justify-content:center;
  align-items:center;
  text-align:center;
  color:var(--text);
}

.page{
  width:100%;
  max-width:1180px;
  padding:20px;
}

.main-logo{
  width:185px;
  display:block;
  margin:0 auto 12px;
  filter:drop-shadow(0 0 30px rgba(212,175,55,.35));
}

.toolbar{
  display:flex;
  justify-content:center;
  align-items:center;
  gap:10px;
  flex-wrap:wrap;
  margin-bottom:10px;
}

.panel{
  display:flex;
  justify-content:center;
  align-items:center;
  gap:10px;
  flex-wrap:wrap;
  background:rgba(255,255,255,.08);
  border:1px solid rgba(255,255,255,.16);
  padding:10px 14px;
  border-radius:18px;
}

input,select{
  font-family:'Thmanyah',sans-serif;
  border:1px solid rgba(255,255,255,.2);
  background:rgba(255,255,255,.1);
  color:var(--text);
  border-radius:12px;
  padding:8px 10px;
  outline:none;
}

.btn{
  font-family:'Thmanyah',sans-serif;
  border:1px solid rgba(255,255,255,.18);
  background:rgba(255,255,255,.1);
  color:var(--text);
  padding:10px 22px;
  border-radius:15px;
  font-size:16px;
  font-weight:800;
  cursor:pointer;
  transition:.2s;
}

.btn:hover{
  transform:translateY(-2px);
  background:rgba(255,255,255,.16);
}

.timer-box{
  font-size:34px;
  font-weight:800;
  color:var(--gold);
  margin:4px 0 -18px;
}

.board-wrap{
  position:relative;
  width:820px;
  height:590px;
  margin:0 auto;
  display:flex;
  justify-content:center;
  align-items:center;
}

.board-glow{
  position:absolute;
  width:700px;
  height:520px;
  border-radius:50%;
  background:
    radial-gradient(circle at center,rgba(212,175,55,.16),transparent 55%),
    radial-gradient(circle at left,var(--teamLR),transparent 38%),
    radial-gradient(circle at right,var(--teamLR),transparent 38%),
    radial-gradient(circle at top,var(--teamTB),transparent 34%),
    radial-gradient(circle at bottom,var(--teamTB),transparent 34%);
  filter:blur(34px);
  opacity:.38;
}

svg{
  width:780px;
  height:580px;
  overflow:visible;
  z-index:2;
}

.cell{cursor:pointer}

.hex{
  fill:var(--cell);
  stroke:#111827;
  stroke-width:3;
  transition:.18s;
}

.letter{
  font-size:39px;
  font-weight:800;
  fill:var(--cellText);
  text-anchor:middle;
  dominant-baseline:middle;
  pointer-events:none;
}

.cell:hover .hex{fill:#fde68a}

.cell.selected .hex{
  stroke:#facc15;
  stroke-width:8;
  filter:drop-shadow(0 0 16px rgba(250,204,21,.9));
}

.cell.teamLR .hex{
  fill:var(--teamLR);
  filter:drop-shadow(0 0 16px var(--teamLR));
}

.cell.teamTB .hex{
  fill:var(--teamTB);
  filter:drop-shadow(0 0 16px var(--teamTB));
}

.cell.teamLR .letter,
.cell.teamTB .letter{
  fill:white;
}

.cell.win .hex{
  stroke:#fde68a;
  stroke-width:9;
  filter:drop-shadow(0 0 24px gold);
}

.modal{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.78);
  display:none;
  justify-content:center;
  align-items:center;
  z-index:100;
}

.modal.show{display:flex}

.modal-box{
  width:90%;
  max-width:620px;
  padding:36px;
  border-radius:32px;
  background:
    radial-gradient(circle at top,rgba(212,175,55,.25),transparent 35%),
    linear-gradient(145deg,#111827,#020617);
  border:1px solid rgba(212,175,55,.45);
  box-shadow:0 0 85px rgba(212,175,55,.35);
  color:white;
}

.modal-box h2{
  font-size:36px;
  margin:0 0 18px;
  color:#fde68a;
}

.start-grid{
  display:grid;
  gap:14px;
  text-align:right;
}

.start-row{
  background:rgba(255,255,255,.08);
  border:1px solid rgba(255,255,255,.14);
  border-radius:18px;
  padding:14px;
}

.start-row label{
  display:block;
  margin-bottom:8px;
  color:#fde68a;
  font-weight:800;
}

.start-row input{
  width:100%;
  margin-bottom:12px;
}

.palette{
  display:flex;
  gap:10px;
  flex-wrap:wrap;
}

.color-choice{
  width:44px;
  height:44px;
  border-radius:50%;
  border:3px solid rgba(255,255,255,.35);
  cursor:pointer;
  transition:.2s;
}

.color-choice:hover{
  transform:scale(1.1);
}

.color-choice.active{
  border-color:#fde68a;
  box-shadow:0 0 18px #fde68a;
}

.color-choice.disabled{
  opacity:.25;
  pointer-events:none;
}

.modal-box p{
  font-size:23px;
  line-height:1.8;
}

@media(max-width:850px){
  .main-logo{width:160px}
  .board-wrap{
    transform:scale(.72);
    margin:-70px auto;
  }
}
</style>
</head>

<body>

<div class="page">

  <img src="images/daraya-logo.png" class="main-logo" alt="مجموعة دراية">

  <div class="toolbar">
    <div class="panel">
      <button class="btn" onclick="toggleTheme()">ليلي / نهاري</button>
      <button class="btn" onclick="toggleFullscreen()">شاشة كاملة</button>
    </div>

    <div class="panel">
      <select id="minutes"></select>
      <span>دقيقة</span>
      <select id="seconds"></select>
      <span>ثانية</span>
      <button class="btn" onclick="startTimer()">تشغيل</button>
      <button class="btn" onclick="pauseTimer()">إيقاف</button>
    </div>
  </div>

  <div class="timer-box" id="timerDisplay">00:30</div>

  <div class="board-wrap">
    <div class="board-glow"></div>
    <svg id="svgBoard" viewBox="0 0 780 580"></svg>
  </div>

  <button class="btn" id="resetBtn" onclick="confirmReset()">إعادة اللعبة</button>

</div>

<div class="modal show" id="setupModal">
  <div class="modal-box">
    <h2>إعداد الفرق</h2>

    <div class="start-grid">
      <div class="start-row">
        <label>فريق فوق ↕ تحت</label>
        <input id="teamTBName" value="الفريق الأزرق">
        <div class="palette" id="paletteTB"></div>
      </div>

      <div class="start-row">
        <label>فريق يمين ↔ يسار</label>
        <input id="teamLRName" value="الفريق البرتقالي">
        <div class="palette" id="paletteLR"></div>
      </div>
    </div>

    <br>
    <button class="btn" onclick="startGame()">بدء اللعبة</button>
  </div>
</div>

<div class="modal" id="rulesModal">
  <div class="modal-box">
    <h2>طريقة الفوز</h2>
    <p id="rulesText"></p>
    <button class="btn" onclick="closeRules()">فهمت</button>
  </div>
</div>

<div class="modal" id="winnerModal">
  <div class="modal-box">
    <h2>🎉 انتهت المسابقة 🎉</h2>
    <p id="winnerText"></p>
    <button class="btn" onclick="resetGame()">لعبة جديدة</button>
  </div>
</div>

<script>
const colors = [
  {name:"أزرق", value:"#2563eb"},
  {name:"برتقالي", value:"#f97316"},
  {name:"بنفسجي", value:"#8b5cf6"},
  {name:"أخضر", value:"#10b981"},
  {name:"وردي", value:"#ec4899"},
  {name:"أحمر", value:"#ef4444"}
];

let selectedTBColor = "#2563eb";
let selectedLRColor = "#f97316";

const lettersPool = [
  "أ","ب","ت","ث","ج",
  "ح","خ","د","ذ","ر",
  "ز","س","ش","ص","ض",
  "ط","ظ","ع","غ","ف",
  "ق","ك","ل","م","ن"
];

const rows = 5;
const cols = 5;

const R = 60;
const W = Math.sqrt(3) * R;
const H = 2 * R;
const xStep = W;
const yStep = H * 0.75;
const startX = 140;
const startY = 85;

const svg = document.getElementById("svgBoard");
const cellMap = new Map();

let gameOver = false;
let gameStarted = false;
let timerInterval = null;
let remainingSeconds = 30;
let resetArmed = false;

setupDropdowns();
renderPalettes();
createBoard();
updateTimerDisplay();
applyTeamColors();

function setupDropdowns(){
  const m = document.getElementById("minutes");
  const s = document.getElementById("seconds");

  for(let i=0;i<=10;i++){
    m.innerHTML += `<option value="${i}">${i}</option>`;
  }

  for(let i=0;i<60;i+=5){
    s.innerHTML += `<option value="${i}">${i}</option>`;
  }

  m.value = 0;
  s.value = 30;
}

function renderPalettes(){
  const tb = document.getElementById("paletteTB");
  const lr = document.getElementById("paletteLR");

  tb.innerHTML = "";
  lr.innerHTML = "";

  colors.forEach(color => {
    const btnTB = document.createElement("button");
    btnTB.className = "color-choice";
    btnTB.style.background = color.value;
    btnTB.title = color.name;

    if(color.value === selectedTBColor) btnTB.classList.add("active");
    if(color.value === selectedLRColor) btnTB.classList.add("disabled");

    btnTB.onclick = () => {
      selectedTBColor = color.value;
      renderPalettes();
      applyTeamColors();
    };

    tb.appendChild(btnTB);

    const btnLR = document.createElement("button");
    btnLR.className = "color-choice";
    btnLR.style.background = color.value;
    btnLR.title = color.name;

    if(color.value === selectedLRColor) btnLR.classList.add("active");
    if(color.value === selectedTBColor) btnLR.classList.add("disabled");

    btnLR.onclick = () => {
      selectedLRColor = color.value;
      renderPalettes();
      applyTeamColors();
    };

    lr.appendChild(btnLR);
  });
}

function applyTeamColors(){
  document.documentElement.style.setProperty("--teamTB", selectedTBColor);
  document.documentElement.style.setProperty("--teamLR", selectedLRColor);
}

function startGame(){
  applyTeamColors();

  document.getElementById("setupModal").classList.remove("show");

  const tb = document.getElementById("teamTBName").value || "الفريق الأول";
  const lr = document.getElementById("teamLRName").value || "الفريق الثاني";

  document.getElementById("rulesText").innerHTML =
    `${tb}: من فوق لتحت<br>${lr}: من يمين ليسار`;

  document.getElementById("rulesModal").classList.add("show");
}

function closeRules(){
  gameStarted = true;
  document.getElementById("rulesModal").classList.remove("show");
}

function shuffle(array){
  const copy = [...array];
  for(let i = copy.length - 1; i > 0; i--){
    const j = Math.floor(Math.random() * (i + 1));
    [copy[i], copy[j]] = [copy[j], copy[i]];
  }
  return copy;
}

function createBoard(){
  const letters = shuffle(lettersPool);
  let index = 0;

  for(let row = 0; row < rows; row++){
    const offsetX = row % 2 === 0 ? W / 2 : 0;

    for(let col = 0; col < cols; col++){
      const cx = startX + offsetX + col * xStep;
      const cy = startY + row * yStep;
      const letter = letters[index++];

      const g = document.createElementNS("http://www.w3.org/2000/svg","g");
      g.setAttribute("class","cell");
      g.dataset.state = "0";
      g.dataset.row = row;
      g.dataset.col = col;
      g.dataset.key = `${row},${col}`;
      g.dataset.owner = "";

      const polygon = document.createElementNS("http://www.w3.org/2000/svg","polygon");
      polygon.setAttribute("points",getHexPoints(cx,cy,R));
      polygon.setAttribute("class","hex");

      const text = document.createElementNS("http://www.w3.org/2000/svg","text");
      text.setAttribute("x",cx);
      text.setAttribute("y",cy + 5);
      text.setAttribute("class","letter");
      text.textContent = letter;

      g.appendChild(polygon);
      g.appendChild(text);
      g.onclick = () => changeCellState(g);

      svg.appendChild(g);
      cellMap.set(`${row},${col}`,g);
    }
  }
}

function getHexPoints(cx,cy,r){
  const points = [];
  for(let i = 0; i < 6; i++){
    const angle = Math.PI / 180 * (60 * i - 30);
    const x = cx + r * Math.cos(angle);
    const y = cy + r * Math.sin(angle);
    points.push(`${x},${y}`);
  }
  return points.join(" ");
}

function changeCellState(cell){
  if(gameOver || !gameStarted) return;

  let state = Number(cell.dataset.state);

  cell.classList.remove("selected","teamLR","teamTB","win");

  state++;
  if(state > 3) state = 0;

  if(state === 1){
    cell.classList.add("selected");
    cell.dataset.owner = "";
  }

  if(state === 2){
    cell.classList.add("teamLR");
    cell.dataset.owner = "teamLR";
  }

  if(state === 3){
    cell.classList.add("teamTB");
    cell.dataset.owner = "teamTB";
  }

  if(state === 0){
    cell.dataset.owner = "";
  }

  cell.dataset.state = state;

  checkWinner("teamLR");
  checkWinner("teamTB");
}

function checkWinner(team){
  const cells = [...document.querySelectorAll(".cell")];

  if(team === "teamLR"){
    const leftSide = cells.filter(c =>
      c.dataset.owner === team && Number(c.dataset.col) === 0
    );

    const rightSide = cells.filter(c =>
      c.dataset.owner === team && Number(c.dataset.col) === cols - 1
    );

    for(const start of leftSide){
      const path = findPath(start,rightSide,team);
      if(path){
        showWinner(document.getElementById("teamLRName").value, path);
        return true;
      }
    }
  }

  if(team === "teamTB"){
    const topSide = cells.filter(c =>
      c.dataset.owner === team && Number(c.dataset.row) === 0
    );

    const bottomSide = cells.filter(c =>
      c.dataset.owner === team && Number(c.dataset.row) === rows - 1
    );

    for(const start of topSide){
      const path = findPath(start,bottomSide,team);
      if(path){
        showWinner(document.getElementById("teamTBName").value, path);
        return true;
      }
    }
  }

  return false;
}

function findPath(start,targets,team){
  const visited = new Set();
  const parent = new Map();
  const stack = [start];

  while(stack.length){
    const current = stack.pop();

    if(visited.has(current.dataset.key)) continue;
    visited.add(current.dataset.key);

    if(targets.includes(current)){
      const path = [];
      let node = current;

      while(node){
        path.push(node);
        node = parent.get(node.dataset.key);
      }

      return path;
    }

    getNeighbors(current).forEach(neighbor => {
      if(neighbor.dataset.owner === team && !visited.has(neighbor.dataset.key)){
        parent.set(neighbor.dataset.key,current);
        stack.push(neighbor);
      }
    });
  }

  return null;
}

function getNeighbors(cell){
  const r = Number(cell.dataset.row);
  const c = Number(cell.dataset.col);
  const pushedRight = r % 2 === 0;

  let coords = [
    [r,c - 1],
    [r,c + 1]
  ];

  if(pushedRight){
    coords.push([r-1,c],[r-1,c+1],[r+1,c],[r+1,c+1]);
  }else{
    coords.push([r-1,c],[r-1,c-1],[r+1,c],[r+1,c-1]);
  }

  return coords
    .map(([row,col]) => cellMap.get(`${row},${col}`))
    .filter(Boolean);
}

function showWinner(teamName,path){
  gameOver = true;
  pauseTimer();

  path.forEach(cell => cell.classList.add("win"));

  document.getElementById("winnerText").textContent = `فاز ${teamName}!`;

  setTimeout(() => {
    document.getElementById("winnerModal").classList.add("show");
  },600);
}

function startTimer(){
  pauseTimer();

  const min = Number(document.getElementById("minutes").value);
  const sec = Number(document.getElementById("seconds").value);

  remainingSeconds = min * 60 + sec;
  if(remainingSeconds <= 0) remainingSeconds = 30;

  updateTimerDisplay();

  timerInterval = setInterval(() => {
    remainingSeconds--;
    updateTimerDisplay();

    if(remainingSeconds <= 0){
      pauseTimer();
      alert("انتهى الوقت");
    }
  },1000);
}

function pauseTimer(){
  clearInterval(timerInterval);
}

function updateTimerDisplay(){
  const m = String(Math.floor(remainingSeconds / 60)).padStart(2,"0");
  const s = String(remainingSeconds % 60).padStart(2,"0");
  document.getElementById("timerDisplay").textContent = `${m}:${s}`;
}

function confirmReset(){
  const btn = document.getElementById("resetBtn");

  if(!resetArmed){
    resetArmed = true;
    btn.textContent = "اضغط مرة ثانية للتأكيد";

    setTimeout(() => {
      resetArmed = false;
      btn.textContent = "إعادة اللعبة";
    },2500);

    return;
  }

  resetGame();
}

function resetGame(){
  location.reload();
}

function toggleTheme(){
  document.body.classList.toggle("light");
}

function toggleFullscreen(){
  if(!document.fullscreenElement){
    document.documentElement.requestFullscreen();
  }else{
    document.exitFullscreen();
  }
}
</script>

</body>
</html>
