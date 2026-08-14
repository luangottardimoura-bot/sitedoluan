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
  background:#111;
  font-family:Arial,sans-serif;
  touch-action:none;
  user-select:none;
}

canvas{
  position:fixed;
  inset:0;
  width:100%;
  height:100%;
}

#hud{
  position:fixed;
  z-index:20;
  top:10px;
  left:50%;
  transform:translateX(-50%);
  background:#000c;
  color:#fff;
  padding:9px 14px;
  border-radius:14px;
  border:1px solid #ffffff33;
  display:flex;
  gap:12px;
  font-weight:bold;
  white-space:nowrap;
}

.speed{color:#55ddff}
.score{color:#ffe044}
.coin{color:#ffd52e}
.stage{color:#6dff78}

#nitroBox{
  position:fixed;
  z-index:20;
  top:55px;
  left:50%;
  transform:translateX(-50%);
  width:220px;
  height:12px;
  background:#000b;
  border:1px solid #ffffff55;
  border-radius:20px;
  overflow:hidden;
}

#nitro{
  width:100%;
  height:100%;
  background:linear-gradient(90deg,#653cff,#39dfff);
}

.screen{
  position:fixed;
  inset:0;
  z-index:100;
  display:flex;
  align-items:center;
  justify-content:center;
  background:#000c;
}

.screen.hidden{
  display:none !important;
}

.panel{
  width:min(600px,92vw);
  padding:35px 25px;
  text-align:center;
  color:#fff;
  border-radius:25px;
  background:linear-gradient(145deg,#17476b,#080f17);
  border:2px solid #ffffff22;
  box-shadow:0 20px 80px #000;
}

h1{
  font-size:clamp(45px,12vw,80px);
  font-style:italic;
  color:#fff;
  text-shadow:0 5px #12628d;
}

h2{
  color:#ffd83d;
  margin:8px 0 20px;
}

p{
  color:#dce8ee;
  line-height:1.6;
  margin:15px 0;
}

button{
  border:0;
  cursor:pointer;
  color:white;
  font-size:19px;
  font-weight:bold;
  border-radius:15px;
  padding:16px 30px;
  background:linear-gradient(#ff5d69,#b91f35);
  box-shadow:0 5px #681523;
}

button:active{
  transform:translateY(3px);
  box-shadow:0 2px #681523;
}

#mobile{
  position:fixed;
  z-index:30;
  left:0;
  right:0;
  bottom:15px;
  display:none;
  justify-content:space-between;
  padding:0 15px;
}

.mobileGroup{
  display:flex;
  gap:10px;
}

.mob{
  width:65px;
  height:65px;
  padding:0;
  border-radius:50%;
  background:#07121ddd;
  border:2px solid #ffffff55;
  box-shadow:0 5px 15px #0008;
}

#nitroButton{
  position:fixed;
  z-index:31;
  right:20px;
  bottom:105px;
  width:75px;
  height:75px;
  padding:0;
  border-radius:50%;
  background:linear-gradient(#714cff,#3113a2);
  display:none;
}

@media(max-width:800px){

  #mobile{
    display:flex;
  }

  #nitroButton{
    display:block;
  }

  #hud{
    font-size:11px;
    gap:6px;
    padding:7px 8px;
  }

  #nitroBox{
    width:180px;
  }
}
</style>
</head>

<body>

<canvas id="game"></canvas>

<!-- HUD -->
<div id="hud">
  🏎️ <span class="speed" id="speed">0</span> km/h
  ⭐ <span class="score" id="score">0</span>
  🪙 <span class="coin" id="coins">0</span>
  🏁 <span class="stage">Fase <span id="level">1</span>/10</span>
</div>

<div id="nitroBox">
  <div id="nitro"></div>
</div>

<!-- TELA INICIAL -->
<div id="start" class="screen">

  <div class="panel">

    <h1>TURBO</h1>
    <h2>🏁 RACER</h2>

    <p>
      Corra pela estrada, desvie dos carros,
      pegue moedas e use o nitro!
    </p>

    <p>
      ⌨️ A/D ou ←/→ = virar<br>
      ⬆️ W / ↑ = acelerar<br>
      ⬇️ S / ↓ = frear<br>
      🚀 Espaço = nitro
    </p>

    <button type="button" id="startBtn">
      🏁 COMEÇAR CORRIDA
    </button>

  </div>

</div>

<!-- FIM -->
<div id="end" class="screen hidden">

  <div class="panel">

    <h1 id="endTitle">FIM</h1>

    <p id="endText"></p>

    <button type="button" id="restartBtn">
      🔄 JOGAR NOVAMENTE
    </button>

  </div>

</div>

<!-- CONTROLES MOBILE -->
<div id="mobile">

  <div class="mobileGroup">
    <button class="mob" id="left">◀</button>
    <button class="mob" id="right">▶</button>
  </div>

  <div class="mobileGroup">
    <button class="mob" id="brake">▼</button>
    <button class="mob" id="gas">▲</button>
  </div>

</div>

<button id="nitroButton">🚀</button>

<script>
"use strict";

/* =====================================================
   CANVAS
===================================================== */

const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let W = 0;
let H = 0;

function resize(){

  W = window.innerWidth;
  H = window.innerHeight;

  canvas.width = W;
  canvas.height = H;
}

window.addEventListener("resize", resize);

resize();


/* =====================================================
   ELEMENTOS
===================================================== */

const startScreen = document.getElementById("start");
const endScreen = document.getElementById("end");

const startBtn = document.getElementById("startBtn");
const restartBtn = document.getElementById("restartBtn");

const speedEl = document.getElementById("speed");
const scoreEl = document.getElementById("score");
const coinsEl = document.getElementById("coins");
const levelEl = document.getElementById("level");
const nitroEl = document.getElementById("nitro");


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

window.addEventListener("keydown",function(e){

  const k = e.key.toLowerCase();

  if(k === "a" || k === "arrowleft"){
    keys.left = true;
    e.preventDefault();
  }

  if(k === "d" || k === "arrowright"){
    keys.right = true;
    e.preventDefault();
  }

  if(k === "w" || k === "arrowup"){
    keys.gas = true;
    e.preventDefault();
  }

  if(k === "s" || k === "arrowdown"){
    keys.brake = true;
    e.preventDefault();
  }

  if(k === " "){
    keys.nitro = true;
    e.preventDefault();
  }
});

window.addEventListener("keyup",function(e){

  const k = e.key.toLowerCase();

  if(k === "a" || k === "arrowleft")
    keys.left = false;

  if(k === "d" || k === "arrowright")
    keys.right = false;

  if(k === "w" || k === "arrowup")
    keys.gas = false;

  if(k === "s" || k === "arrowdown")
    keys.brake = false;

  if(k === " ")
    keys.nitro = false;
});


/* =====================================================
   MOBILE
===================================================== */

function mobileButton(id,key){

  const button = document.getElementById(id);

  button.addEventListener("pointerdown",function(e){

    e.preventDefault();

    keys[key] = true;

    try{
      button.setPointerCapture(e.pointerId);
    }catch(error){}
  });

  function release(e){

    if(e)
      e.preventDefault();

    keys[key] = false;
  }

  button.addEventListener("pointerup",release);
  button.addEventListener("pointercancel",release);
  button.addEventListener("lostpointercapture",release);
}

mobileButton("left","left");
mobileButton("right","right");
mobileButton("gas","gas");
mobileButton("brake","brake");
mobileButton("nitroButton","nitro");


/* =====================================================
   JOGO
===================================================== */

const game = {

  running:false,

  level:1,

  score:0,

  coins:0,

  speed:0,

  maxSpeed:15,

  nitro:100,

  distance:0,

  finish:9000,

  roadMove:0,

  spawn:0,

  coinSpawn:0,

  shake:0
};


/* =====================================================
   CARRO
===================================================== */

const player = {

  x:0,

  y:0,

  w:58,

  h:100,

  vx:0,

  invincible:0
};


/* =====================================================
   OBJETOS
===================================================== */

let enemies = [];
let coins = [];
let particles = [];


/* =====================================================
   FUNÇÕES
===================================================== */

function clamp(value,min,max){

  return Math.max(
    min,
    Math.min(max,value)
  );
}

function random(min,max){

  return Math.random()*(max-min)+min;
}

function collision(a,b){

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
    W*0.72,
    720
  );
}

function roadLeft(){

  return W/2-roadWidth()/2;
}


/* =====================================================
   CRIAR FASE
===================================================== */

function createLevel(){

  enemies = [];
  coins = [];
  particles = [];

  game.distance = 0;

  game.finish =
    8000 + game.level*700;

  game.maxSpeed =
    14 + game.level*0.6;

  game.speed = 0;

  game.nitro = 100;

  game.spawn = 40;

  game.coinSpawn = 30;

  player.x =
    W/2-player.w/2;

  player.y =
    H*0.72;

  player.vx = 0;

  player.invincible = 90;
}


/* =====================================================
   COMEÇAR JOGO
===================================================== */

function startGame(){

  console.log("INICIANDO JOGO");

  game.running = true;

  game.level = 1;
  game.score = 0;
  game.coins = 0;

  keys.left = false;
  keys.right = false;
  keys.gas = false;
  keys.brake = false;
  keys.nitro = false;

  createLevel();

  startScreen.classList.add("hidden");
  endScreen.classList.add("hidden");

  startScreen.style.display = "none";
  endScreen.style.display = "none";

  updateHUD();
}


/* =====================================================
   PRÓXIMA FASE
===================================================== */

function nextLevel(){

  if(game.level >= 10){

    winGame();

    return;
  }

  game.level++;

  game.score += 1000;

  createLevel();

  game.running = true;
}


/* =====================================================
   CARRO
===================================================== */

function updatePlayer(){

  /* acelerar */

  if(keys.gas){

    game.speed += 0.20;

  }else{

    game.speed -= 0.06;
  }

  /* freio */

  if(keys.brake){

    game.speed -= 0.30;
  }

  /* nitro */

  if(
    keys.nitro &&
    game.nitro > 0 &&
    game.speed > 1
  ){

    game.speed += 0.25;

    game.nitro -= 0.55;

    createNitro();

  }else{

    game.nitro += 0.08;
  }

  game.nitro =
    clamp(game.nitro,0,100);

  game.speed =
    clamp(
      game.speed,
      0,
      game.maxSpeed
    );


  /* direção */

  if(keys.left)
    player.vx -= 0.65;

  if(keys.right)
    player.vx += 0.65;

  if(!keys.left && !keys.right)
    player.vx *= 0.82;

  player.vx =
    clamp(
      player.vx,
      -7,
      7
    );

  player.x += player.vx;


  /* limite */

  const left =
    roadLeft()+15;

  const right =
    roadLeft()+
    roadWidth()-
    player.w-
    15;

  if(player.x < left){

    player.x = left;
    player.vx *= 0.4;
  }

  if(player.x > right){

    player.x = right;
    player.vx *= 0.4;
  }


  /* distância */

  game.distance +=
    game.speed*0.75;

  game.score +=
    Math.floor(game.speed*0.04);

  if(player.invincible > 0)
    player.invincible--;
}


/* =====================================================
   INIMIGOS
===================================================== */

function spawnEnemy(){

  const lanes = 4;

  const laneWidth =
    roadWidth()/lanes;

  const lane =
    Math.floor(
      Math.random()*lanes
    );

  const x =
    roadLeft()+
    lane*laneWidth+
    laneWidth/2-
    25;

  const colors = [
    "#2878d5",
    "#f1c72e",
    "#34a45b",
    "#bd3dcc",
    "#e86d2c",
    "#eeeeee"
  ];

  enemies.push({

    x:x,

    y:-120,

    w:50,

    h:90,

    speed:random(3,7),

    color:
      colors[
        Math.floor(
          Math.random()*colors.length
        )
      ],

    passed:false
  });
}


function updateEnemies(){

  game.spawn--;

  if(game.spawn <= 0){

    spawnEnemy();

    game.spawn =
      Math.max(
        22,
        60-game.level*3
      );
  }

  for(
    let i=enemies.length-1;
    i>=0;
    i--
  ){

    const enemy = enemies[i];

    enemy.y +=
      game.speed-
      enemy.speed;


    /* passou */

    if(
      !enemy.passed &&
      enemy.y >
      player.y+player.h
    ){

      enemy.passed = true;

      game.score += 150;
    }


    /* colisão */

    if(
      player.invincible <= 0 &&
      collision(player,enemy)
    ){

      crash();

      enemies.splice(i,1);

      continue;
    }


    if(
      enemy.y > H+150 ||
      enemy.y < -300
    ){

      enemies.splice(i,1);
    }
  }
}


/* =====================================================
   MOEDAS
===================================================== */

function spawnCoin(){

  const lanes = 4;

  const laneWidth =
    roadWidth()/lanes;

  const lane =
    Math.floor(
      Math.random()*lanes
    );

  coins.push({

    x:
      roadLeft()+
      lane*laneWidth+
      laneWidth/2,

    y:-30,

    r:11,

    angle:0
  });
}


function updateCoins(){

  game.coinSpawn--;

  if(game.coinSpawn <= 0){

    spawnCoin();

    game.coinSpawn =
      Math.max(
        15,
        35-game.level
      );
  }

  for(
    let i=coins.length-1;
    i>=0;
    i--
  ){

    const coin = coins[i];

    coin.y += game.speed;

    coin.angle += 0.12;

    const box = {

      x:coin.x-coin.r,

      y:coin.y-coin.r,

      w:coin.r*2,

      h:coin.r*2
    };

    if(collision(player,box)){

      game.coins++;

      game.score += 100;

      game.nitro =
        Math.min(
          100,
          game.nitro+8
        );

      burst(
        coin.x,
        coin.y,
        "#ffd52e",
        10
      );

      coins.splice(i,1);

      continue;
    }

    if(coin.y > H+50)
      coins.splice(i,1);
  }
}


/* =====================================================
   BATIDA
===================================================== */

function crash(){

  game.speed *= 0.3;

  player.vx =
    player.x < W/2
      ? -5
      : 5;

  player.invincible = 100;

  game.score =
    Math.max(
      0,
      game.score-250
    );

  game.shake = 15;

  burst(
    player.x+player.w/2,
    player.y+player.h/2,
    "#ff4b32",
    25
  );
}


/* =====================================================
   NITRO
===================================================== */

function createNitro(){

  for(let i=0;i<3;i++){

    particles.push({

      x:
        player.x+
        random(10,player.w-10),

      y:
        player.y+
        player.h,

      vx:random(-1,1),

      vy:random(3,7),

      life:1,

      color:
        Math.random()>0.5
          ? "#39eaff"
          : "#7952ff",

      size:random(3,7)
    });
  }
}


/* =====================================================
   PARTÍCULAS
===================================================== */

function burst(x,y,color,count){

  for(let i=0;i<count;i++){

    particles.push({

      x:x,

      y:y,

      vx:random(-5,5),

      vy:random(-5,5),

      life:1,

      color:color,

      size:random(2,6)
    });
  }
}


function updateParticles(){

  for(
    let i=particles.length-1;
    i>=0;
    i--
  ){

    const p=particles[i];

    p.x+=p.vx;
    p.y+=p.vy;

    p.vy+=0.1;

    p.life-=0.035;

    if(p.life<=0)
      particles.splice(i,1);
  }
}


/* =====================================================
   FINAL DA FASE
===================================================== */

function checkFinish(){

  if(
    game.distance >=
    game.finish
  ){

    nextLevel();
  }
}


/* =====================================================
   HUD
===================================================== */

function updateHUD(){

  speedEl.textContent =
    Math.floor(
      game.speed*20
    );

  scoreEl.textContent =
    game.score;

  coinsEl.textContent =
    game.coins;

  levelEl.textContent =
    game.level;

  nitroEl.style.width =
    game.nitro+"%";
}


/* =====================================================
   FUNDO
===================================================== */

function drawBackground(){

  const sky =
    ctx.createLinearGradient(
      0,0,0,H
    );

  sky.addColorStop(
    0,
    "#45b7e8"
  );

  sky.addColorStop(
    0.55,
    "#b9efff"
  );

  sky.addColorStop(
    1,
    "#72bd5d"
  );

  ctx.fillStyle = sky;

  ctx.fillRect(
    0,0,W,H
  );


  /* sol */

  ctx.fillStyle="#fff0a0";

  ctx.beginPath();

  ctx.arc(
    W*0.8,
    100,
    45,
    0,
    Math.PI*2
  );

  ctx.fill();


  /* montanhas */

  ctx.fillStyle="#508d67";

  ctx.beginPath();

  ctx.moveTo(0,H*0.48);

  ctx.lineTo(
    W*0.15,
    H*0.25
  );

  ctx.lineTo(
    W*0.30,
    H*0.48
  );

  ctx.lineTo(
    W*0.48,
    H*0.27
  );

  ctx.lineTo(
    W*0.68,
    H*0.48
  );

  ctx.lineTo(
    W*0.85,
    H*0.28
  );

  ctx.lineTo(
    W,
    H*0.48
  );

  ctx.lineTo(
    W,H
  );

  ctx.lineTo(
    0,H
  );

  ctx.closePath();

  ctx.fill();


  /* grama */

  ctx.fillStyle="#348548";

  ctx.fillRect(
    0,
    H*0.48,
    W,
    H
  );
}


/* =====================================================
   ESTRADA
===================================================== */

function drawRoad(){

  const left =
    roadLeft();

  const width =
    roadWidth();


  /* acostamento */

  ctx.fillStyle="#ddd6c5";

  ctx.fillRect(
    left-14,
    0,
    width+28,
    H
  );


  /* asfalto */

  ctx.fillStyle="#34373b";

  ctx.fillRect(
    left,
    0,
    width,
    H
  );


  /* faixas */

  const lanes = 4;

  const laneWidth =
    width/lanes;

  ctx.fillStyle="#f6e979";

  const dash = 70;
  const gap = 65;

  const offset =
    game.roadMove %
    (dash+gap);

  for(
    let lane=1;
    lane<lanes;
    lane++
  ){

    const x =
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


  /* bordas */

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
   DESENHAR CARRO
===================================================== */

function drawCar(){

  if(
    player.invincible > 0 &&
    Math.floor(
      player.invincible/6
    )%2 === 0
  ){

    return;
  }

  ctx.save();

  ctx.translate(
    player.x+player.w/2,
    player.y+player.h/2
  );

  ctx.rotate(
    player.vx*0.025
  );


  /* sombra */

  ctx.fillStyle="#0008";

  ctx.beginPath();

  ctx.ellipse(
    0,
    45,
    35,
    8,
    0,
    0,
    Math.PI*2
  );

  ctx.fill();


  /* rodas */

  ctx.fillStyle="#111";

  ctx.fillRect(-34,-30,10,25);
  ctx.fillRect(24,-30,10,25);

  ctx.fillRect(-34,5,10,25);
  ctx.fillRect(24,5,10,25);


  /* carroceria */

  ctx.fillStyle="#e52e43";

  ctx.beginPath();

  ctx.roundRect(
    -29,
    -50,
    58,
    100,
    12
  );

  ctx.fill();


  /* vidro */

  ctx.fillStyle="#183143";

  ctx.beginPath();

  ctx.roundRect(
    -20,
    -35,
    40,
    28,
    7
  );

  ctx.fill();


  /* faixa */

  ctx.fillStyle="#ffffffaa";

  ctx.fillRect(
    -4,
    -50,
    8,
    100
  );


  /* faróis */

  ctx.fillStyle="#fff2a8";

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

  ctx.fillStyle="#ff263c";

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


  /* nitro */

  if(
    keys.nitro &&
    game.nitro>0
  ){

    ctx.fillStyle="#42eaff";

    ctx.beginPath();

    ctx.moveTo(-15,50);
    ctx.lineTo(-7,78);
    ctx.lineTo(0,50);

    ctx.fill();

    ctx.beginPath();

    ctx.moveTo(7,50);
    ctx.lineTo(15,78);
    ctx.lineTo(22,50);

    ctx.fill();
  }

  ctx.restore();
}


/* =====================================================
   DESENHAR INIMIGOS
===================================================== */

function drawEnemies(){

  for(const enemy of enemies){

    ctx.save();

    ctx.translate(
      enemy.x+enemy.w/2,
      enemy.y+enemy.h/2
    );


    /* sombra */

    ctx.fillStyle="#0008";

    ctx.beginPath();

    ctx.ellipse(
      0,
      42,
      30,
      7,
      0,
      0,
      Math.PI*2
    );

    ctx.fill();


    /* rodas */

    ctx.fillStyle="#111";

    ctx.fillRect(
      -29,-28,9,24
    );

    ctx.fillRect(
      20,-28,9,24
    );

    ctx.fillRect(
      -29,5,9,24
    );

    ctx.fillRect(
      20,5,9,24
    );


    /* carro */

    ctx.fillStyle =
      enemy.color;

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

    ctx.fillStyle="#172e3d";

    ctx.beginPath();

    ctx.roundRect(
      -17,
      -30,
      34,
      27,
      6
    );

    ctx.fill();


    /* luzes */

    ctx.fillStyle="#fff1a8";

    ctx.fillRect(
      -18,-43,10,5
    );

    ctx.fillRect(
      8,-43,10,5
    );

    ctx.restore();
  }
}


/* =====================================================
   MOEDAS
===================================================== */

function drawCoins(){

  for(const coin of coins){

    ctx.save();

    ctx.translate(
      coin.x,
      coin.y
    );

    ctx.rotate(
      coin.angle
    );

    ctx.fillStyle="#ffd52e";

    ctx.beginPath();

    ctx.ellipse(
      0,
      0,
      7,
      12,
      0,
      0,
      Math.PI*2
    );

    ctx.fill();

    ctx.strokeStyle="#fff3a0";

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

    ctx.globalAlpha =
      p.life;

    ctx.fillStyle =
      p.color;

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

  drawEnemies();

  drawCar();

  drawParticles();
}


/* =====================================================
   LOOP
===================================================== */

function loop(){

  if(game.running){

    updatePlayer();

    updateEnemies();

    updateCoins();

    updateParticles();

    game.roadMove +=
      game.speed*1.8;

    checkFinish();

    updateHUD();


    if(game.shake>0)
      game.shake*=0.88;
  }


  ctx.save();

  if(game.shake>0){

    ctx.translate(
      random(
        -game.shake,
        game.shake
      ),
      random(
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

function gameOver(){

  game.running=false;

  endScreen.style.display="flex";

  document.getElementById(
    "endTitle"
  ).textContent="💥 GAME OVER";

  document.getElementById(
    "endText"
  ).textContent=
    "Você bateu demais! "+
    "Pontuação: "+
    game.score+
    " | Moedas: "+
    game.coins;
}


function winGame(){

  game.running=false;

  endScreen.style.display="flex";

  document.getElementById(
    "endTitle"
  ).textContent="🏆 CAMPEÃO!";

  document.getElementById(
    "endText"
  ).textContent=
    "Você venceu as 10 fases! "+
    "Pontuação: "+
    game.score+
    " | Moedas: "+
    game.coins;
}


/* =====================================================
   BOTÕES — CORREÇÃO PRINCIPAL
===================================================== */

startBtn.addEventListener(
  "click",
  function(e){

    e.preventDefault();

    startGame();
  }
);


restartBtn.addEventListener(
  "click",
  function(e){

    e.preventDefault();

    startGame();
  }
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