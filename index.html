```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>Turbo Racer</title>

<style>
*{
  box-sizing:border-box;
  margin:0;
  padding:0;
}

html,body{
  width:100%;
  height:100%;
  overflow:hidden;
  background:#10141b;
  font-family:Arial,sans-serif;
  touch-action:none;
  user-select:none;
}

canvas{
  width:100%;
  height:100%;
  display:block;
}

#hud{
  position:fixed;
  z-index:10;
  top:12px;
  left:50%;
  transform:translateX(-50%);
  display:flex;
  gap:14px;
  padding:9px 14px;
  border-radius:15px;
  background:#000b;
  border:1px solid #ffffff33;
  color:#fff;
  font-weight:bold;
  white-space:nowrap;
}

.speed{color:#55d9ff}
.score{color:#ffd83d}
.coins{color:#ffcf32}
.stage{color:#72ff75}
.nitro{color:#9c7cff}

.screen{
  position:fixed;
  inset:0;
  z-index:50;
  display:flex;
  justify-content:center;
  align-items:center;
  background:#000c;
}

.panel{
  width:min(620px,92vw);
  padding:35px 25px;
  text-align:center;
  color:#fff;
  border-radius:25px;
  background:linear-gradient(145deg,#173b5a,#090e16);
  border:2px solid #ffffff22;
  box-shadow:0 20px 80px #000;
}

h1{
  font-size:clamp(45px,11vw,80px);
  font-style:italic;
  text-shadow:0 5px #074a73;
}

h2{
  color:#ffd83d;
  margin:10px;
}

p{
  color:#dce8ee;
  line-height:1.6;
  margin:15px 0;
}

button{
  border:0;
  border-radius:14px;
  padding:15px 28px;
  color:#fff;
  background:linear-gradient(#ff626c,#b91f35);
  font-size:18px;
  font-weight:bold;
  box-shadow:0 5px #681523;
  cursor:pointer;
}

button:active{
  transform:translateY(3px);
  box-shadow:0 2px #681523;
}

.hidden{
  display:none!important;
}

#mobile{
  position:fixed;
  z-index:20;
  left:0;
  right:0;
  bottom:18px;
  display:none;
  justify-content:space-between;
  padding:0 18px;
  pointer-events:none;
}

.mobGroup{
  display:flex;
  gap:10px;
}

.mob{
  width:65px;
  height:65px;
  padding:0;
  border-radius:50%;
  background:#07111ddd;
  border:2px solid #ffffff66;
  font-size:25px;
  pointer-events:auto;
  box-shadow:0 5px 18px #0008;
}

#nitroBtn{
  position:fixed;
  z-index:21;
  right:20px;
  bottom:100px;
  width:78px;
  height:78px;
  padding:0;
  border-radius:50%;
  background:linear-gradient(#6e52ff,#3215a4);
  font-size:28px;
  display:none;
}

#boostBar{
  position:fixed;
  z-index:11;
  top:62px;
  left:50%;
  transform:translateX(-50%);
  width:min(250px,55vw);
  height:10px;
  border-radius:10px;
  background:#000b;
  overflow:hidden;
  border:1px solid #ffffff33;
}

#boostFill{
  width:100%;
  height:100%;
  background:linear-gradient(90deg,#563cff,#36d8ff);
}

@media(max-width:800px){
  #mobile{
    display:flex;
  }

  #nitroBtn{
    display:block;
  }

  #hud{
    font-size:11px;
    gap:7px;
    padding:7px 9px;
  }
}
</style>
</head>

<body>

<canvas id="game"></canvas>

<div id="hud">
  🏎️ <span class="speed" id="speed">0</span> km/h
  ⭐ <span class="score" id="score">0</span>
  🪙 <span class="coins" id="coins">0</span>
  🏁 <span class="stage">Fase <span id="level">1</span>/10</span>
</div>

<div id="boostBar">
  <div id="boostFill"></div>
</div>

<div id="start" class="screen">
  <div class="panel">
    <h1>TURBO<br>RACER</h1>
    <h2>10 FASES</h2>

    <p>
      Corra, ultrapasse carros, pegue moedas,
      use nitro e sobreviva até a linha de chegada!
    </p>

    <p>
      ⌨️ A/D ou ←/→ = virar<br>
      ⬆️ W / ↑ = acelerar<br>
      ⬇️ S / ↓ = frear<br>
      🚀 Espaço = Nitro
    </p>

    <button id="startBtn">CORRER!</button>
  </div>
</div>

<div id="end" class="screen hidden">
  <div class="panel">
    <h1 id="endTitle">FIM</h1>
    <p id="endText"></p>
    <button id="restartBtn">JOGAR NOVAMENTE</button>
  </div>
</div>

<div id="mobile">

  <div class="mobGroup">
    <button class="mob" id="left">◀</button>
    <button class="mob" id="right">▶</button>
  </div>

  <div class="mobGroup">
    <button class="mob" id="brake">▼</button>
    <button class="mob" id="gas">▲</button>
  </div>

</div>

<button id="nitroBtn">🚀</button>

<script>
"use strict";

/* =====================================================
   CANVAS
===================================================== */

const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let W = innerWidth;
let H = innerHeight;

function resize(){
  W = innerWidth;
  H = innerHeight;

  canvas.width = W;
  canvas.height = H;
}

addEventListener("resize",resize);
resize();

/* =====================================================
   HUD
===================================================== */

const speedEl = document.getElementById("speed");
const scoreEl = document.getElementById("score");
const coinsEl = document.getElementById("coins");
const levelEl = document.getElementById("level");
const boostFill = document.getElementById("boostFill");

/* =====================================================
   CONTROLES
===================================================== */

const keys = {
  left:false,
  right:false,
  gas:false,
  brake:false,
  nitro:false
};

let nitroPressed = false;

addEventListener("keydown",e=>{

  const k = e.key.toLowerCase();

  if(k==="a" || k==="arrowleft"){
    keys.left=true;
    e.preventDefault();
  }

  if(k==="d" || k==="arrowright"){
    keys.right=true;
    e.preventDefault();
  }

  if(k==="w" || k==="arrowup"){
    keys.gas=true;
    e.preventDefault();
  }

  if(k==="s" || k==="arrowdown"){
    keys.brake=true;
    e.preventDefault();
  }

  if(k===" "){
    if(!keys.nitro)
      nitroPressed=true;

    keys.nitro=true;
    e.preventDefault();
  }
});

addEventListener("keyup",e=>{

  const k=e.key.toLowerCase();

  if(k==="a" || k==="arrowleft")
    keys.left=false;

  if(k==="d" || k==="arrowright")
    keys.right=false;

  if(k==="w" || k==="arrowup")
    keys.gas=false;

  if(k==="s" || k==="arrowdown")
    keys.brake=false;

  if(k===" ")
    keys.nitro=false;
});

/* =====================================================
   CONTROLE MOBILE
===================================================== */

function hold(id,key){

  const btn=document.getElementById(id);

  btn.addEventListener("pointerdown",e=>{

    e.preventDefault();

    keys[key]=true;

    if(key==="nitro")
      nitroPressed=true;

    try{
      btn.setPointerCapture(e.pointerId);
    }catch(_){}
  });

  const stop=e=>{

    if(e)
      e.preventDefault();

    keys[key]=false;
  };

  btn.addEventListener("pointerup",stop);
  btn.addEventListener("pointercancel",stop);
  btn.addEventListener("lostpointercapture",stop);
}

hold("left","left");
hold("right","right");
hold("gas","gas");
hold("brake","brake");
hold("nitro","nitro");

/* =====================================================
   ESTADO
===================================================== */

const game = {

  running:false,

  level:1,

  score:0,

  coins:0,

  speed:0,

  maxSpeed:18,

  nitro:100,

  distance:0,

  targetDistance:10000,

  roadOffset:0,

  shake:0,

  spawnTimer:0,

  coinTimer:0,

  message:"",
  messageTime:0
};

/* =====================================================
   CARRO
===================================================== */

const car = {

  x:0,

  y:0,

  w:58,

  h:100,

  vx:0,

  color:"#e52d43",

  invincible:0
};

/* =====================================================
   OBJETOS
===================================================== */

let traffic=[];
let coins=[];
let particles=[];

/* =====================================================
   UTILIDADES
===================================================== */

function clamp(v,a,b){
  return Math.max(a,Math.min(b,v));
}

function rand(a,b){
  return Math.random()*(b-a)+a;
}

function overlap(a,b){

  return(
    a.x < b.x+b.w &&
    a.x+a.w > b.x &&
    a.y < b.y+b.h &&
    a.y+a.h > b.y
  );
}

/* =====================================================
   ESTRADA
===================================================== */

function roadWidth(){
  return Math.min(
    W*.72,
    760
  );
}

function roadLeft(){
  return W/2-roadWidth()/2;
}

function roadRight(){
  return W/2+roadWidth()/2;
}

/* =====================================================
   CRIAR FASE
===================================================== */

function createLevel(){

  traffic=[];
  coins=[];
  particles=[];

  game.distance=0;

  game.targetDistance =
    8500 + game.level*700;

  game.speed=0;

  game.maxSpeed =
    16 + game.level*.65;

  game.nitro=100;

  game.spawnTimer=50;

  game.coinTimer=30;

  car.x=W/2-car.w/2;
  car.y=H*.72;

  car.vx=0;
  car.invincible=100;

  game.message=
    "FASE "+game.level;

  game.messageTime=120;
}

/* =====================================================
   INÍCIO
===================================================== */

function startGame(){

  game.running=true;

  game.level=1;
  game.score=0;
  game.coins=0;
  game.speed=0;

  keys.left=false;
  keys.right=false;
  keys.gas=false;
  keys.brake=false;
  keys.nitro=false;

  nitroPressed=false;

  createLevel();

  document
    .getElementById("start")
    .classList.add("hidden");

  document
    .getElementById("end")
    .classList.add("hidden");

  updateHUD();
}

/* =====================================================
   PRÓXIMA FASE
===================================================== */

function nextLevel(){

  if(game.level>=10){

    winGame();

    return;
  }

  game.level++;

  game.score+=1000;

  game.message=
    "FASE "+game.level;

  game.messageTime=120;

  createLevel();
}

/* =====================================================
   ATUALIZAR CARRO
===================================================== */

function updateCar(){

  /* ACELERAÇÃO */

  if(keys.gas){

    game.speed += .22;

  }else{

    game.speed -= .07;
  }

  /* FREIO */

  if(keys.brake)
    game.speed-=.35;

  /* LIMITE */

  game.speed=clamp(
    game.speed,
    0,
    game.maxSpeed
  );

  /* NITRO */

  if(
    keys.nitro &&
    game.nitro>0 &&
    game.speed>2
  ){

    game.speed+=.28;

    game.nitro-=.55;

    if(game.nitro<0)
      game.nitro=0;

    createNitroParticles();

  }else{

    game.nitro+=.08;

    if(game.nitro>100)
      game.nitro=100;
  }

  /* DIREÇÃO */

  const steering =
    .8 + game.speed*.045;

  if(keys.left)
    car.vx-=.65*steering;

  if(keys.right)
    car.vx+=.65*steering;

  if(!keys.left && !keys.right)
    car.vx*=.82;

  car.vx=clamp(
    car.vx,
    -8,
    8
  );

  car.x+=car.vx;

  /* LIMITES DA ESTRADA */

  const left=roadLeft()+18;
  const right=roadRight()-car.w-18;

  if(car.x<left){

    car.x=left;
    car.vx*=.45;
  }

  if(car.x>right){

    car.x=right;
    car.vx*=.45;
  }

  /* DISTÂNCIA */

  game.distance+=
    game.speed*.75;

  /* PONTUAÇÃO */

  game.score+=
    Math.floor(game.speed*.08);

  if(car.invincible>0)
    car.invincible--;
}

/* =====================================================
   TRÂNSITO
===================================================== */

function spawnTraffic(){

  const lanes=4;

  const road=roadWidth();

  const laneWidth=road/lanes;

  const lane=
    Math.floor(
      Math.random()*lanes
    );

  const x=
    roadLeft()+
    lane*laneWidth+
    laneWidth/2-25;

  const colors=[
    "#2774d8",
    "#f4c62f",
    "#35a85b",
    "#b337c8",
    "#f17c31",
    "#e8e8e8"
  ];

  traffic.push({

    x:x,

    y:-130,

    w:50,

    h:90,

    speed:
      rand(2.5,7)+game.level*.2,

    color:
      colors[
        Math.floor(
          Math.random()*colors.length
        )
      ],

    passed:false
  });
}

/* =====================================================
   MOEDAS
===================================================== */

function spawnCoin(){

  const lanes=4;

  const laneWidth=
    roadWidth()/lanes;

  const lane=
    Math.floor(
      Math.random()*lanes
    );

  const x=
    roadLeft()+
    lane*laneWidth+
    laneWidth/2;

  coins.push({

    x:x,

    y:-30,

    r:11,

    rotation:0,

    collected:false
  });
}

/* =====================================================
   ATUALIZAR TRÂNSITO
===================================================== */

function updateTraffic(){

  game.spawnTimer--;

  if(game.spawnTimer<=0){

    spawnTraffic();

    game.spawnTimer=
      Math.max(
        24,
        70-game.level*4
      );
  }

  for(let i=traffic.length-1;i>=0;i--){

    const t=traffic[i];

    /*
      Os carros descem enquanto o jogador
      avança pela pista.
    */
    t.y +=
      game.speed-t.speed;

    /* ULTRAPASSAGEM */

    if(
      !t.passed &&
      t.y>car.y+car.h
    ){

      t.passed=true;

      game.score+=150;

      particlesBurst(
        t.x+t.w/2,
        t.y,
        "#55eaff",
        4
      );
    }

    /* COLISÃO */

    if(
      car.invincible<=0 &&
      overlap(car,t)
    ){

      crash(t);

      traffic.splice(i,1);

      continue;
    }

    /* REMOVE */

    if(
      t.y>H+150 ||
      t.y<-300
    ){

      traffic.splice(i,1);
    }
  }
}

/* =====================================================
   ATUALIZAR MOEDAS
===================================================== */

function updateCoins(){

  game.coinTimer--;

  if(game.coinTimer<=0){

    spawnCoin();

    game.coinTimer=
      Math.max(
        15,
        35-game.level*1.5
      );
  }

  for(let i=coins.length-1;i>=0;i--){

    const c=coins[i];

    c.y+=game.speed;

    c.rotation+=.12;

    const box={

      x:c.x-c.r,

      y:c.y-c.r,

      w:c.r*2,

      h:c.r*2
    };

    if(overlap(car,box)){

      game.coins++;
      game.score+=100;

      game.nitro=
        Math.min(
          100,
          game.nitro+8
        );

      particlesBurst(
        c.x,
        c.y,
        "#ffd83d",
        10
      );

      coins.splice(i,1);

      continue;
    }

    if(c.y>H+50)
      coins.splice(i,1);
  }
}

/* =====================================================
   COLISÃO
===================================================== */

function crash(t){

  game.speed*=.35;

  car.vx=
    (car.x<t.x ? -5 : 5);

  car.invincible=100;

  game.shake=18;

  game.score=
    Math.max(
      0,
      game.score-300
    );

  particlesBurst(
    car.x+car.w/2,
    car.y+car.h/2,
    "#ff5733",
    25
  );

  game.message="BATIDA!";

  game.messageTime=60;
}

/* =====================================================
   NITRO
===================================================== */

function createNitroParticles(){

  for(let i=0;i<3;i++){

    particles.push({

      x:
        car.x+
        rand(10,car.w-10),

      y:
        car.y+car.h,

      vx:
        rand(-1,1),

      vy:
        rand(3,7),

      life:1,

      color:
        Math.random()>.5
          ? "#48eaff"
          : "#7b52ff",

      size:
        rand(3,7)
    });
  }
}

/* =====================================================
   PARTÍCULAS
===================================================== */

function particlesBurst(
  x,
  y,
  color,
  count
){

  for(let i=0;i<count;i++){

    particles.push({

      x:x,

      y:y,

      vx:rand(-5,5),

      vy:rand(-5,5),

      life:1,

      color:color,

      size:rand(2,6)
    });
  }
}

function updateParticles(){

  for(let i=particles.length-1;i>=0;i--){

    const p=particles[i];

    p.x+=p.vx;
    p.y+=p.vy;

    p.vy+=.08;

    p.life-=.035;

    if(p.life<=0)
      particles.splice(i,1);
  }
}

/* =====================================================
   FIM DA FASE
===================================================== */

function checkFinish(){

  if(
    game.distance>=game.targetDistance
  ){

    nextLevel();
  }
}

/* =====================================================
   HUD
===================================================== */

function updateHUD(){

  speedEl.textContent=
    Math.floor(
      game.speed*18
    );

  scoreEl.textContent=
    game.score;

  coinsEl.textContent=
    game.coins;

  levelEl.textContent=
    game.level;

  boostFill.style.width=
    game.nitro+"%";
}

/* =====================================================
   FUNDO
===================================================== */

function drawBackground(){

  /* CÉU */

  const sky=
    ctx.createLinearGradient(
      0,
      0,
      0,
      H
    );

  sky.addColorStop(
    0,
    "#49b9ed"
  );

  sky.addColorStop(
    .55,
    "#b8edff"
  );

  sky.addColorStop(
    1,
    "#83bf62"
  );

  ctx.fillStyle=sky;

  ctx.fillRect(
    0,
    0,
    W,
    H
  );

  /* SOL */

  ctx.fillStyle="#fff1a1";

  ctx.beginPath();

  ctx.arc(
    W*.78,
    90,
    48,
    0,
    Math.PI*2
  );

  ctx.fill();

  /* MONTANHAS */

  ctx.fillStyle="#57906a";

  ctx.beginPath();

  ctx.moveTo(0,H*.48);
  ctx.lineTo(W*.15,H*.28);
  ctx.lineTo(W*.3,H*.48);
  ctx.lineTo(W*.48,H*.25);
  ctx.lineTo(W*.7,H*.48);
  ctx.lineTo(W*.85,H*.3);
  ctx.lineTo(W,H*.48);
  ctx.lineTo(W,H);
  ctx.lineTo(0,H);
  ctx.closePath();

  ctx.fill();

  /* GRAMA */

  ctx.fillStyle="#358548";

  ctx.fillRect(
    0,
    H*.48,
    W,
    H*.52
  );
}

/* =====================================================
   ESTRADA
===================================================== */

function drawRoad(){

  const left=roadLeft();
  const width=roadWidth();

  /* ACOSTAMENTO */

  ctx.fillStyle="#d7d2c2";

  ctx.fillRect(
    left-14,
    0,
    width+28,
    H
  );

  /* ASFALTO */

  ctx.fillStyle="#35383d";

  ctx.fillRect(
    left,
    0,
    width,
    H
  );

  /* FAIXAS */

  const lanes=4;

  const laneWidth=
    width/lanes;

  ctx.fillStyle="#f6e77a";

  const dash=75;
  const gap=70;

  const offset=
    game.roadOffset %
    (dash+gap);

  for(let lane=1;lane<lanes;lane++){

    const x=
      left+
      lane*laneWidth-3;

    for(
      let y=-dash+offset;
      y<H;
      y+=dash+gap
    ){

      ctx.fillRect(
        x,
        y,
        6,
        dash
      );
    }
  }

  /* BORDAS */

  ctx.fillStyle="#fff";

  ctx.fillRect(
    left,
    0,
    7,
    H
  );

  ctx.fillRect(
    left+width-7,
    0,
    7,
    H
  );
}

/* =====================================================
   CARRO DO JOGADOR
===================================================== */

function drawPlayer(){

  if(
    car.invincible>0 &&
    Math.floor(
      car.invincible/6
    )%2===0
  )
    return;

  ctx.save();

  ctx.translate(
    car.x+car.w/2,
    car.y+car.h/2
  );

  /* inclinação */

  ctx.rotate(
    car.vx*.025
  );

  /* sombra */

  ctx.fillStyle="#0008";

  ctx.beginPath();

  ctx.ellipse(
    0,
    43,
    34,
    9,
    0,
    0,
    Math.PI*2
  );

  ctx.fill();

  /* rodas */

  ctx.fillStyle="#111";

  ctx.fillRect(
    -34,
    -30,
    10,
    27
  );

  ctx.fillRect(
    24,
    -30,
    10,
    27
  );

  ctx.fillRect(
    -34,
    5,
    10,
    27
  );

  ctx.fillRect(
    24,
    5,
    10,
    27
  );

  /* carroceria */

  ctx.fillStyle=car.color;

  ctx.beginPath();

  ctx.roundRect(
    -29,
    -50,
    58,
    100,
    13
  );

  ctx.fill();

  /* vidro */

  ctx.fillStyle="#182d3d";

  ctx.beginPath();

  ctx.roundRect(
    -20,
    -35,
    40,
    28,
    8
  );

  ctx.fill();

  /* vidro traseiro */

  ctx.fillStyle="#284b60";

  ctx.beginPath();

  ctx.roundRect(
    -20,
    12,
    40,
    20,
    6
  );

  ctx.fill();

  /* faróis */

  ctx.fillStyle="#fff4b0";

  ctx.fillRect(
    -21,
    -47,
    12,
    6
  );

  ctx.fillRect(
    9,
    -47,
    12,
    6
  );

  /* lanternas */

  ctx.fillStyle="#ff263f";

  ctx.fillRect(
    -21,
    40,
    12,
    6
  );

  ctx.fillRect(
    9,
    40,
    12,
    6
  );

  /* faixa */

  ctx.fillStyle="#ffffff88";

  ctx.fillRect(
    -4,
    -50,
    8,
    100
  );

  /* nitro */

  if(keys.nitro && game.nitro>0){

    ctx.fillStyle="#45eaff";

    ctx.beginPath();

    ctx.moveTo(-14,50);
    ctx.lineTo(-7,78);
    ctx.lineTo(0,50);

    ctx.fill();

    ctx.beginPath();

    ctx.moveTo(7,50);
    ctx.lineTo(14,78);
    ctx.lineTo(21,50);

    ctx.fill();
  }

  ctx.restore();
}

/* =====================================================
   CARROS DE TRÂNSITO
===================================================== */

function drawTraffic(){

  for(const t of traffic){

    ctx.save();

    ctx.translate(
      t.x+t.w/2,
      t.y+t.h/2
    );

    /* sombra */

    ctx.fillStyle="#0007";

    ctx.beginPath();

    ctx.ellipse(
      0,
      43,
      29,
      7,
      0,
      0,
      Math.PI*2
    );

    ctx.fill();

    /* rodas */

    ctx.fillStyle="#111";

    ctx.fillRect(
      -29,
      -28,
      9,
      25
    );

    ctx.fillRect(
      20,
      -28,
      9,
      25
    );

    ctx.fillRect(
      -29,
      5,
      9,
      25
    );

    ctx.fillRect(
      20,
      5,
      9,
      25
    );

    /* carro */

    ctx.fillStyle=t.color;

    ctx.beginPath();

    ctx.roundRect(
      -25,
      -45,
      50,
      90,
      10
    );

    ctx.fill();

    /* vidro */

    ctx.fillStyle="#172d3d";

    ctx.beginPath();

    ctx.roundRect(
      -17,
      -30,
      34,
      27,
      7
    );

    ctx.fill();

    /* faróis */

    ctx.fillStyle="#fff1ad";

    ctx.fillRect(
      -18,
      -43,
      10,
      5
    );

    ctx.fillRect(
      8,
      -43,
      10,
      5
    );

    /* lanternas */

    ctx.fillStyle="#f3263d";

    ctx.fillRect(
      -18,
      38,
      10,
      5
    );

    ctx.fillRect(
      8,
      38,
      10,
      5
    );

    ctx.restore();
  }
}

/* =====================================================
   MOEDAS
===================================================== */

function drawCoins(){

  for(const c of coins){

    ctx.save();

    ctx.translate(
      c.x,
      c.y
    );

    ctx.rotate(
      c.rotation
    );

    ctx.fillStyle="#ffd52e";

    ctx.beginPath();

    ctx.ellipse(
      0,
      0,
      c.r*.65,
      c.r,
      0,
      0,
      Math.PI*2
    );

    ctx.fill();

    ctx.strokeStyle="#fff19a";
    ctx.lineWidth=2;

    ctx.stroke();

    ctx.restore();
  }
}

/* =====================================================
   PARTÍCULAS
===================================================== */

function drawParticles(){

  for(const p of particles){

    ctx.globalAlpha=p.life;

    ctx.fillStyle=p.color;

    ctx.beginPath();

    ctx.arc(
      p.x,
      p.y,
      p.size,
      0,
      Math.PI*2
    );

    ctx.fill();
  }

  ctx.globalAlpha=1;
}

/* =====================================================
   MENSAGEM
===================================================== */

function drawMessage(){

  if(game.messageTime<=0)
    return;

  ctx.textAlign="center";

  ctx.font=
    "900 42px Arial";

  ctx.fillStyle="#fff";

  ctx.shadowColor="#000";

  ctx.shadowBlur=12;

  ctx.fillText(
    game.message,
    W/2,
    H*.25
  );

  ctx.shadowBlur=0;
}

/* =====================================================
   RENDER
===================================================== */

function render(){

  ctx.clearRect(
    0,
    0,
    W,
    H
  );

  drawBackground();

  drawRoad();

  drawCoins();

  drawTraffic();

  drawPlayer();

  drawParticles();

  drawMessage();

  /* VINHETA */

  const v=
    ctx.createRadialGradient(
      W/2,
      H/2,
      100,
      W/2,
      H/2,
      H*.8
    );

  v.addColorStop(
    0,
    "transparent"
  );

  v.addColorStop(
    1,
    "rgba(0,0,0,.45)"
  );

  ctx.fillStyle=v;

  ctx.fillRect(
    0,
    0,
    W,
    H
  );
}

/* =====================================================
   LOOP
===================================================== */

function loop(){

  if(game.running){

    updateCar();

    updateTraffic();

    updateCoins();

    updateParticles();

    /*
      A estrada se move mais rápido
      conforme o carro acelera.
    */
    game.roadOffset+=
      game.speed*1.7;

    checkFinish();

    if(game.messageTime>0)
      game.messageTime--;

    updateHUD();

    if(game.shake>0){

      game.shake*=.88;

      if(game.shake<.1)
        game.shake=0;
    }
  }

  ctx.save();

  if(game.shake>0){

    ctx.translate(
      rand(
        -game.shake,
        game.shake
      ),
      rand(
        -game.shake,
        game.shake
      )
    );
  }

  render();

  ctx.restore();

  requestAnimationFrame(loop);
}

/* =====================================================
   FIM
===================================================== */

function endGame(title,text){

  game.running=false;

  keys.left=false;
  keys.right=false;
  keys.gas=false;
  keys.brake=false;
  keys.nitro=false;

  document.getElementById(
    "endTitle"
  ).textContent=title;

  document.getElementById(
    "endText"
  ).textContent=
    text+
    " Pontuação: "+
    game.score+
    " | Moedas: "+
    game.coins+
    " | Fase: "+
    game.level+"/10";

  document.getElementById(
    "end"
  ).classList.remove("hidden");
}

function winGame(){

  game.running=false;

  document.getElementById(
    "endTitle"
  ).textContent=
    "🏆 CAMPEÃO!";

  document.getElementById(
    "endText"
  ).textContent=
    "Você completou as 10 fases! "+
    "Pontuação: "+
    game.score+
    " | Moedas: "+
    game.coins;

  document.getElementById(
    "end"
  ).classList.remove("hidden");
}

/* =====================================================
   BOTÕES
===================================================== */

document.getElementById(
  "startBtn"
).onclick=startGame;

document.getElementById(
  "restartBtn"
).onclick=startGame;

document.addEventListener(
  "contextmenu",
  e=>e.preventDefault()
);

/* =====================================================
   INICIALIZAÇÃO
===================================================== */

createLevel();

updateHUD();

requestAnimationFrame(loop);

</script>

</body>
</html>
```
