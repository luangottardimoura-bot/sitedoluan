<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>A CASA CAIU</title>

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
  display:flex;
  align-items:center;
  gap:12px;
  padding:9px 14px;
  color:white;
  background:#000d;
  border:1px solid #ffffff33;
  border-radius:14px;
  font-weight:bold;
  white-space:nowrap;
  font-size:14px;
}

.life{
  color:#ff5665;
}

.time{
  color:#5eeaff;
}

.score{
  color:#ffd83d;
}

.wanted{
  color:#ff4858;
}

#lifeBar{
  position:fixed;
  z-index:20;
  top:52px;
  left:50%;
  transform:translateX(-50%);
  width:220px;
  height:13px;
  border-radius:20px;
  background:#160609;
  border:1px solid #ffffff55;
  overflow:hidden;
}

#lifeFill{
  width:100%;
  height:100%;
  background:linear-gradient(90deg,#ff283d,#ff9b35);
  transition:width .15s;
}

.screen{
  position:fixed;
  inset:0;
  z-index:100;
  display:flex;
  align-items:center;
  justify-content:center;
  background:#000d;
}

.hidden{
  display:none!important;
}

.panel{
  width:min(620px,92vw);
  padding:32px 25px;
  text-align:center;
  color:white;
  border-radius:25px;
  background:linear-gradient(145deg,#253e52,#080e14);
  border:2px solid #ffffff22;
  box-shadow:0 20px 80px #000;
}

h1{
  font-size:clamp(42px,11vw,78px);
  font-style:italic;
  color:#fff;
  text-shadow:0 5px #8b1724;
}

h2{
  color:#ff4858;
  margin:5px 0 18px;
}

p{
  color:#dce8ee;
  line-height:1.6;
  margin:13px 0;
}

button{
  border:0;
  cursor:pointer;
  color:white;
  font-size:19px;
  font-weight:bold;
  border-radius:15px;
  padding:15px 28px;
  background:linear-gradient(#ff5968,#b51f35);
  box-shadow:0 5px #651321;
}

button:active{
  transform:translateY(3px);
}

#mobile{
  position:fixed;
  z-index:30;
  left:0;
  right:0;
  bottom:14px;
  display:none;
  justify-content:space-between;
  padding:0 15px;
}

.group{
  display:flex;
  gap:9px;
}

.mob{
  width:65px;
  height:65px;
  padding:0;
  border-radius:50%;
  background:#07121ddd;
  border:2px solid #ffffff55;
  box-shadow:0 5px 15px #0008;
  font-size:25px;
}

#message{
  position:fixed;
  z-index:25;
  top:85px;
  left:50%;
  transform:translateX(-50%);
  color:#fff;
  font-size:22px;
  font-weight:bold;
  text-shadow:0 3px 8px #000;
  pointer-events:none;
}

@media(max-width:800px){

  #mobile{
    display:flex;
  }

  #hud{
    font-size:10px;
    gap:6px;
    padding:7px 8px;
  }

  #lifeBar{
    width:180px;
    top:47px;
  }
}
</style>
</head>

<body>

<canvas id="game"></canvas>

<div id="hud">
  ❤️ <span class="life" id="lifeText">12</span>
  ⏱️ <span class="time" id="timeText">0</span>s
  ⭐ <span class="score" id="scoreText">0</span>
  🚨 <span class="wanted" id="wantedText">1</span>
</div>

<div id="lifeBar">
  <div id="lifeFill"></div>
</div>

<div id="message"></div>

<!-- INÍCIO -->

<div id="start" class="screen">

  <div class="panel">

    <h1>A CASA<br>CAIU</h1>

    <h2>🚨 FUGA URBANA</h2>

    <p>
      Entre na cidade, dirija para onde quiser,
      fuja da polícia e tente bater o recorde!
    </p>

    <p>
      ⌨️ <b>WASD / Setas</b> = dirigir<br>
      📱 Use os controles na tela no celular
    </p>

    <p>
      ❤️ Você tem 12 pontos de vida.<br>
      🚨 A polícia fica cada vez mais forte.<br>
      🚁 Em perseguições extremas aparece um helicóptero.
    </p>

    <button id="startBtn">
      🚨 COMEÇAR
    </button>

  </div>

</div>

<!-- FIM -->

<div id="end" class="screen hidden">

  <div class="panel">

    <h1 id="endTitle">FIM</h1>

    <p id="endText"></p>

    <button id="restartBtn">
      🔄 JOGAR NOVAMENTE
    </button>

  </div>

</div>

<!-- CONTROLES -->

<div id="mobile">

  <div class="group">
    <button class="mob" id="left">◀</button>
    <button class="mob" id="right">▶</button>
  </div>

  <div class="group">
    <button class="mob" id="brake">▼</button>
    <button class="mob" id="gas">▲</button>
  </div>

</div>

<script>
"use strict";

/* =====================================================
   CANVAS
===================================================== */

const canvas=document.getElementById("game");
const ctx=canvas.getContext("2d");

let W=innerWidth;
let H=innerHeight;

function resize(){

  W=innerWidth;
  H=innerHeight;

  canvas.width=W;
  canvas.height=H;
}

addEventListener("resize",resize);
resize();


/* =====================================================
   HUD
===================================================== */

const lifeText=document.getElementById("lifeText");
const timeText=document.getElementById("timeText");
const scoreText=document.getElementById("scoreText");
const wantedText=document.getElementById("wantedText");
const lifeFill=document.getElementById("lifeFill");
const message=document.getElementById("message");


/* =====================================================
   CONTROLES
===================================================== */

const keys={
  up:false,
  down:false,
  left:false,
  right:false
};

addEventListener("keydown",e=>{

  const k=e.key.toLowerCase();

  if(k==="w"||k==="arrowup")
    keys.up=true;

  if(k==="s"||k==="arrowdown")
    keys.down=true;

  if(k==="a"||k==="arrowleft")
    keys.left=true;

  if(k==="d"||k==="arrowright")
    keys.right=true;

  if([
    "w","s","a","d",
    "arrowup","arrowdown",
    "arrowleft","arrowright"
  ].includes(k))
    e.preventDefault();
});

addEventListener("keyup",e=>{

  const k=e.key.toLowerCase();

  if(k==="w"||k==="arrowup")
    keys.up=false;

  if(k==="s"||k==="arrowdown")
    keys.down=false;

  if(k==="a"||k==="arrowleft")
    keys.left=false;

  if(k==="d"||k==="arrowright")
    keys.right=false;
});


/* =====================================================
   CONTROLES MOBILE
===================================================== */

function hold(id,key){

  const b=document.getElementById(id);

  b.addEventListener("pointerdown",e=>{

    e.preventDefault();

    keys[key]=true;

    try{
      b.setPointerCapture(e.pointerId);
    }catch(_){}
  });

  const release=e=>{

    if(e)
      e.preventDefault();

    keys[key]=false;
  };

  b.addEventListener("pointerup",release);
  b.addEventListener("pointercancel",release);
  b.addEventListener("lostpointercapture",release);
}

hold("left","left");
hold("right","right");
hold("gas","up");
hold("brake","down");


/* =====================================================
   JOGO
===================================================== */

const game={

  running:false,

  worldW:6000,
  worldH:6000,

  cameraX:0,
  cameraY:0,

  time:0,
  score:0,

  wanted:1,

  policeTimer:100,

  shake:0,

  helicopter:false,

  helicopterTimer:0,

  messageTimer:0
};


/* =====================================================
   PLAYER
===================================================== */

const player={

  x:3000,
  y:3000,

  w:42,
  h:72,

  angle:0,

  speed:0,

  maxSpeed:7,

  health:12,

  invincible:0,

  color:"#e63145"
};


/* =====================================================
   OBJETOS
===================================================== */

let buildings=[];
let trees=[];
let cars=[];
let police=[];
let bullets=[];
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

function dist(a,b){

  return Math.hypot(
    a.x-b.x,
    a.y-b.y
  );
}

function collision(a,b){

  return(
    a.x<b.x+b.w &&
    a.x+a.w>b.x &&
    a.y<b.y+b.h &&
    a.y+a.h>b.y
  );
}


/* =====================================================
   CRIAR CIDADE
===================================================== */

function createCity(){

  buildings=[];
  trees=[];
  cars=[];
  police=[];
  bullets=[];
  coins=[];
  particles=[];

  /* prédios */

  for(let x=150;x<game.worldW-150;x+=380){

    for(let y=150;y<game.worldH-150;y+=380){

      const roadX=
        Math.abs(
          (x%900)-450
        );

      const roadY=
        Math.abs(
          (y%900)-450
        );

      if(
        roadX<170 ||
        roadY<170
      )
        continue;

      buildings.push({

        x:x+rand(-50,50),
        y:y+rand(-50,50),
        w:rand(180,280),
        h:rand(180,280),

        color:[
          "#555c67",
          "#725d51",
          "#3f5967",
          "#685b72",
          "#4f684f"
        ][Math.floor(rand(0,5))]
      });
    }
  }

  /* árvores */

  for(let i=0;i<280;i++){

    trees.push({

      x:rand(80,game.worldW-80),
      y:rand(80,game.worldH-80),
      r:rand(10,19)
    });
  }

  /* carros */

  for(let i=0;i<70;i++){

    const horizontal=Math.random()>0.5;

    const road=900*
      Math.floor(
        rand(0,game.worldW/900)
      );

    cars.push({

      x:horizontal
        ?rand(100,game.worldW-100)
        :road+rand(-100,100),

      y:horizontal
        ?road+rand(-100,100)
        :rand(100,game.worldH-100),

      w:38,
      h:65,

      angle:horizontal
        ?0
        :Math.PI/2,

      speed:rand(1.2,2.5),

      color:[
        "#2776c8",
        "#f0bf32",
        "#35a45d",
        "#c33cc4",
        "#e8752d",
        "#e9e9e9"
      ][Math.floor(rand(0,6))],

      police:false
    });
  }

  /* moedas */

  for(let i=0;i<100;i++){

    coins.push({

      x:rand(150,game.worldW-150),
      y:rand(150,game.worldH-150),
      taken:false
    });
  }
}


/* =====================================================
   INÍCIO
===================================================== */

function startGame(){

  game.running=true;

  game.time=0;
  game.score=0;
  game.wanted=1;
  game.policeTimer=100;
  game.helicopter=false;
  game.helicopterTimer=0;
  game.shake=0;

  player.x=game.worldW/2;
  player.y=game.worldH/2;
  player.angle=0;
  player.speed=0;
  player.health=12;
  player.invincible=100;

  createCity();

  document
    .getElementById("start")
    .classList.add("hidden");

  document
    .getElementById("end")
    .classList.add("hidden");

  updateHUD();
}


/* =====================================================
   PLAYER
===================================================== */

function updatePlayer(){

  if(keys.up)
    player.speed+=0.12;
  else
    player.speed-=0.035;

  if(keys.down)
    player.speed-=0.16;

  player.speed=
    clamp(
      player.speed,
      -2.5,
      player.maxSpeed
    );

  if(keys.left){

    player.angle-=
      0.045*
      (Math.abs(player.speed)+0.5);
  }

  if(keys.right){

    player.angle+=
      0.045*
      (Math.abs(player.speed)+0.5);
  }

  player.x+=
    Math.sin(player.angle)*
    player.speed;

  player.y+=
    -Math.cos(player.angle)*
    player.speed;

  player.x=
    clamp(
      player.x,
      40,
      game.worldW-40
    );

  player.y=
    clamp(
      player.y,
      40,
      game.worldH-40
    );

  if(player.invincible>0)
    player.invincible--;

  /* colisão com prédios */

  const box={
    x:player.x-player.w/2,
    y:player.y-player.h/2,
    w:player.w,
    h:player.h
  };

  for(const b of buildings){

    if(collision(box,b)){

      player.x-=
        Math.sin(player.angle)*
        player.speed;

      player.y+=
        Math.cos(player.angle)*
        player.speed;

      player.speed*=0.45;

      damage(1);

      break;
    }
  }

  game.score+=
    Math.max(
      0,
      Math.floor(Math.abs(player.speed)*0.03)
    );
}


/* =====================================================
   TRÂNSITO
===================================================== */

function updateCars(){

  for(const c of cars){

    if(c.angle===0)
      c.x+=c.speed;
    else
      c.y+=c.speed;

    if(c.x>game.worldW+100)
      c.x=-100;

    if(c.y>game.worldH+100)
      c.y=-100;

    const box={

      x:c.x-c.w/2,
      y:c.y-c.h/2,
      w:c.w,
      h:c.h
    };

    const pbox={

      x:player.x-player.w/2,
      y:player.y-player.h/2,
      w:player.w,
      h:player.h
    };

    if(
      collision(box,pbox) &&
      player.invincible<=0
    ){

      damage(1);

      player.speed*=0.4;

      player.invincible=50;

      burst(
        player.x,
        player.y,
        "#ff9c3b",
        15
      );
    }
  }
}


/* =====================================================
   POLÍCIA
===================================================== */

function spawnPolice(){

  const side=
    Math.floor(rand(0,4));

  let x,y;

  if(side===0){
    x=player.x+rand(-600,600);
    y=player.y-650;
  }

  if(side===1){
    x=player.x+650;
    y=player.y+rand(-600,600);
  }

  if(side===2){
    x=player.x+rand(-600,600);
    y=player.y+650;
  }

  if(side===3){
    x=player.x-650;
    y=player.y+rand(-600,600);
  }

  x=clamp(x,50,game.worldW-50);
  y=clamp(y,50,game.worldH-50);

  police.push({

    x,
    y,

    w:45,
    h:75,

    angle:0,

    speed:
      2.1+
      game.wanted*0.25,

    hitTimer:0
  });
}


function updatePolice(){

  game.policeTimer-=1/60;

  if(game.policeTimer<=0){

    game.wanted++;

    game.policeTimer=100;

    showMessage(
      "🚨 MAIS POLÍCIA!",
      150
    );

    for(
      let i=0;
      i<Math.min(2,game.wanted);
      i++
    )
      spawnPolice();

    if(game.wanted>=4)
      game.helicopter=true;
  }

  const desired=
    Math.min(
      10,
      2+game.wanted
    );

  while(police.length<desired)
    spawnPolice();

  for(const p of police){

    const dx=player.x-p.x;
    const dy=player.y-p.y;

    const targetAngle=
      Math.atan2(dx,-dy);

    let diff=
      targetAngle-p.angle;

    while(diff>Math.PI)
      diff-=Math.PI*2;

    while(diff<-Math.PI)
      diff+=Math.PI*2;

    p.angle+=
      clamp(diff,-0.045,0.045);

    p.x+=
      Math.sin(p.angle)*
      p.speed;

    p.y+=
      -Math.cos(p.angle)*
      p.speed;

    p.x=clamp(p.x,30,game.worldW-30);
    p.y=clamp(p.y,30,game.worldH-30);

    p.hitTimer--;

    const d=
      Math.hypot(
        player.x-p.x,
        player.y-p.y
      );

    if(
      d<55 &&
      p.hitTimer<=0
    ){

      p.hitTimer=70;

      damage(1);

      player.speed*=0.5;

      showMessage(
        "🚓 A POLÍCIA BATEU!",
        45
      );
    }
  }
}


/* =====================================================
   HELICÓPTERO
===================================================== */

const helicopter={

  x:0,
  y:0,
  angle:0,
  shoot:60
};


function updateHelicopter(){

  if(!game.helicopter)
    return;

  helicopter.angle+=0.01;

  helicopter.x=
    player.x+
    Math.cos(helicopter.angle)*
    430;

  helicopter.y=
    player.y+
    Math.sin(helicopter.angle)*
    430;

  helicopter.shoot--;

  if(helicopter.shoot<=0){

    helicopter.shoot=
      Math.max(
        30,
        75-game.wanted*5
      );

    bullets.push({

      x:helicopter.x,
      y:helicopter.y,

      vx:
        (player.x-helicopter.x)/
        70,

      vy:
        (player.y-helicopter.y)/
        70,

      life:100
    });
  }

  for(let i=bullets.length-1;i>=0;i--){

    const b=bullets[i];

    b.x+=b.vx;
    b.y+=b.vy;
    b.life--;

    if(
      Math.hypot(
        player.x-b.x,
        player.y-b.y
      )<28 &&
      player.invincible<=0
    ){

      damage(1);

      bullets.splice(i,1);

      continue;
    }

    if(b.life<=0)
      bullets.splice(i,1);
  }
}


/* =====================================================
   DANO
===================================================== */

function damage(amount){

  if(player.invincible>0)
    return;

  player.health-=amount;

  player.invincible=65;

  game.shake=12;

  burst(
    player.x,
    player.y,
    "#ff3448",
    18
  );

  if(player.health<=0){

    player.health=0;

    gameOver();
  }
}


/* =====================================================
   MOEDAS
===================================================== */

function updateCoins(){

  for(const c of coins){

    if(c.taken)
      continue;

    if(
      Math.hypot(
        player.x-c.x,
        player.y-c.y
      )<35
    ){

      c.taken=true;

      game.score+=250;

      burst(
        c.x,
        c.y,
        "#ffd52e",
        12
      );
    }
  }

  if(
    coins.filter(c=>!c.taken).length<20
  ){

    for(let i=0;i<30;i++){

      coins.push({

        x:rand(100,game.worldW-100),
        y:rand(100,game.worldH-100),
        taken:false
      });
    }
  }
}


/* =====================================================
   PARTÍCULAS
===================================================== */

function burst(x,y,color,n){

  for(let i=0;i<n;i++){

    particles.push({

      x,
      y,

      vx:rand(-4,4),
      vy:rand(-4,4),

      life:1,

      color,

      size:rand(2,6)
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

    p.vx*=0.98;
    p.vy*=0.98;

    p.life-=0.035;

    if(p.life<=0)
      particles.splice(i,1);
  }
}


/* =====================================================
   CÂMERA
===================================================== */

function updateCamera(){

  const targetX=
    player.x-W/2;

  const targetY=
    player.y-H/2;

  game.cameraX+=
    (targetX-game.cameraX)*0.08;

  game.cameraY+=
    (targetY-game.cameraY)*0.08;

  game.cameraX=
    clamp(
      game.cameraX,
      0,
      game.worldW-W
    );

  game.cameraY=
    clamp(
      game.cameraY,
      0,
      game.worldH-H
    );
}


/* =====================================================
   DESENHAR CIDADE
===================================================== */

function drawCity(){

  ctx.fillStyle="#3c9348";

  ctx.fillRect(
    0,
    0,
    W,
    H
  );

  /* ruas principais */

  ctx.fillStyle="#303338";

  for(
    let x=0;
    x<game.worldW;
    x+=900
  ){

    ctx.fillRect(
      x-game.cameraX,
      0,
      180,
      H
    );
  }

  for(
    let y=0;
    y<game.worldH;
    y+=900
  ){

    ctx.fillRect(
      0,
      y-game.cameraY,
      W,
      180
    );
  }

  /* ruas secundárias */

  ctx.fillStyle="#45474b";

  for(
    let x=450;
    x<game.worldW;
    x+=900
  ){

    ctx.fillRect(
      x-game.cameraX,
      0,
      80,
      H
    );
  }

  for(
    let y=450;
    y<game.worldH;
    y+=900
  ){

    ctx.fillRect(
      0,
      y-game.cameraY,
      W,
      80
    );
  }

  /* linhas das ruas */

  ctx.fillStyle="#d7cf69";

  for(
    let x=90;
    x<game.worldW;
    x+=900
  ){

    for(
      let y=-50;
      y<game.worldH;
      y+=90
    ){

      ctx.fillRect(
        x-game.cameraX,
        y-game.cameraY,
        6,
        45
      );
    }
  }

  for(
    let y=90;
    y<game.worldH;
    y+=900
  ){

    for(
      let x=-50;
      x<game.worldW;
      x+=90
    ){

      ctx.fillRect(
        x-game.cameraX,
        y-game.cameraY,
        45,
        6
      );
    }
  }
}


/* =====================================================
   PRÉDIOS
===================================================== */

function drawBuildings(){

  for(const b of buildings){

    const x=b.x-game.cameraX;
    const y=b.y-game.cameraY;

    if(
      x+b.w<0 ||
      x>W ||
      y+b.h<0 ||
      y>H
    )
      continue;

    ctx.fillStyle="#0005";

    ctx.fillRect(
      x+8,
      y+10,
      b.w,
      b.h
    );

    ctx.fillStyle=b.color;

    ctx.fillRect(
      x,
      y,
      b.w,
      b.h
    );

    /* telhado */

    ctx.fillStyle="#25282c";

    ctx.fillRect(
      x,
      y,
      b.w,
      12
    );

    /* janelas */

    ctx.fillStyle="#b9e8f0";

    for(
      let wx=x+20;
      wx<x+b.w-15;
      wx+=42
    ){

      for(
        let wy=y+30;
        wy<y+b.h-15;
        wy+=45
      ){

        ctx.fillRect(
          wx,
          wy,
          18,
          24
        );
      }
    }
  }
}


/* =====================================================
   ÁRVORES
===================================================== */

function drawTrees(){

  for(const t of trees){

    const x=t.x-game.cameraX;
    const y=t.y-game.cameraY;

    if(x<-40||x>W+40||y<-40||y>H+40)
      continue;

    ctx.fillStyle="#65422d";

    ctx.fillRect(
      x-4,
      y,
      8,
      24
    );

    ctx.fillStyle="#176d38";

    ctx.beginPath();

    ctx.arc(
      x,
      y-8,
      t.r,
      0,
      Math.PI*2
    );

    ctx.fill();

    ctx.fillStyle="#238b45";

    ctx.beginPath();

    ctx.arc(
      x-7,
      y-14,
      t.r*.65,
      0,
      Math.PI*2
    );

    ctx.fill();
  }
}


/* =====================================================
   CARROS
===================================================== */

function drawVehicle(c,policeCar=false){

  const x=c.x-game.cameraX;
  const y=c.y-game.cameraY;

  ctx.save();

  ctx.translate(x,y);
  ctx.rotate(c.angle);

  ctx.fillStyle="#0007";

  ctx.fillRect(
    -c.w/2+5,
    -c.h/2+5,
    c.w,
    c.h
  );

  ctx.fillStyle=
    policeCar
      ?"#eeeeee"
      :c.color;

  ctx.fillRect(
    -c.w/2,
    -c.h/2,
    c.w,
    c.h
  );

  ctx.fillStyle="#182a38";

  ctx.fillRect(
    -15,
    -24,
    30,
    24
  );

  ctx.fillStyle="#111";

  ctx.fillRect(
    -25,
    -28,
    7,
    20
  );

  ctx.fillRect(
    18,
    -28,
    7,
    20
  );

  ctx.fillRect(
    -25,
    8,
    7,
    20
  );

  ctx.fillRect(
    18,
    8,
    7,
    20
  );

  if(policeCar){

    ctx.fillStyle="#1477ff";

    ctx.fillRect(
      -9,
      -37,
      18,
      7
    );

    ctx.fillStyle="#ff2438";

    ctx.fillRect(
      -8,
      -37,
      8,
      7
    );
  }

  ctx.restore();
}


/* =====================================================
   JOGADOR
===================================================== */

function drawPlayer(){

  const x=player.x-game.cameraX;
  const y=player.y-game.cameraY;

  if(
    player.invincible>0 &&
    Math.floor(player.invincible/6)%2===0
  )
    return;

  drawVehicle({

    x,
    y,

    w:player.w,
    h:player.h,

    angle:player.angle,

    color:player.color

  });

  /* esconder diferença de câmera */

  ctx.save();

  ctx.translate(
    x,
    y
  );

  ctx.rotate(player.angle);

  ctx.fillStyle="#fff";

  ctx.fillRect(
    -4,
    -35,
    8,
    70
  );

  ctx.restore();
}


/* =====================================================
   POLÍCIA
===================================================== */

function drawPolice(){

  for(const p of police){

    drawVehicle({

      x:p.x,
      y:p.y,

      w:p.w,
      h:p.h,

      angle:p.angle,

      color:"#eee"

    },true);
  }
}


/* =====================================================
   MOEDAS
===================================================== */

function drawCoins(){

  for(const c of coins){

    if(c.taken)
      continue;

    const x=c.x-game.cameraX;
    const y=c.y-game.cameraY;

    ctx.fillStyle="#ffd52e";

    ctx.beginPath();

    ctx.arc(
      x,
      y,
      10,
      0,
      Math.PI*2
    );

    ctx.fill();

    ctx.strokeStyle="#fff3a0";
    ctx.lineWidth=2;
    ctx.stroke();

    ctx.fillStyle="#fff";

    ctx.font="bold 12px Arial";
    ctx.textAlign="center";

    ctx.fillText(
      "$",
      x,
      y+4
    );
  }
}


/* =====================================================
   HELICÓPTERO
===================================================== */

function drawHelicopter(){

  if(!game.helicopter)
    return;

  const x=
    helicopter.x-game.cameraX;

  const y=
    helicopter.y-game.cameraY;

  if(
    x<-100||
    x>W+100||
    y<-100||
    y>H+100
  )
    return;

  ctx.save();

  ctx.translate(x,y);

  ctx.fillStyle="#0008";

  ctx.beginPath();

  ctx.ellipse(
    0,
    0,
    45,
    18,
    0,
    0,
    Math.PI*2
  );

  ctx.fill();

  ctx.fillStyle="#252a30";

  ctx.fillRect(
    -28,
    -13,
    56,
    26
  );

  ctx.fillStyle="#273c50";

  ctx.fillRect(
    -12,
    -9,
    24,
    12
  );

  ctx.strokeStyle="#222";
  ctx.lineWidth=5;

  ctx.beginPath();

  ctx.moveTo(-55,0);
  ctx.lineTo(55,0);

  ctx.stroke();

  ctx.fillStyle="#ff354b";

  ctx.fillRect(
    -5,
    8,
    10,
    5
  );

  ctx.restore();
}


/* =====================================================
   TIROS
===================================================== */

function drawBullets(){

  for(const b of bullets){

    ctx.fillStyle="#ff4d38";

    ctx.beginPath();

    ctx.arc(
      b.x-game.cameraX,
      b.y-game.cameraY,
      5,
      0,
      Math.PI*2
    );

    ctx.fill();
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
      p.x-game.cameraX,
      p.y-game.cameraY,
      p.size,
      0,
      Math.PI*2
    );

    ctx.fill();
  }

  ctx.globalAlpha=1;
}


/* =====================================================
   HUD
===================================================== */

function updateHUD(){

  lifeText.textContent=player.health;

  timeText.textContent=
    Math.floor(game.time);

  scoreText.textContent=
    game.score;

  wantedText.textContent=
    Math.min(game.wanted,6);

  lifeFill.style.width=
    (player.health/12*100)+"%";
}


/* =====================================================
   MENSAGEM
===================================================== */

function showMessage(text,time){

  message.textContent=text;
  game.messageTimer=time;
}


/* =====================================================
   LOOP
===================================================== */

let last=performance.now();

function loop(now){

  const dt=
    Math.min(
      0.033,
      (now-last)/1000
    );

  last=now;

  if(game.running){

    game.time+=dt;

    updatePlayer();
    updateCars();
    updatePolice();
    updateHelicopter();
    updateCoins();
    updateParticles();
    updateCamera();

    if(game.messageTimer>0){

      game.messageTimer--;

      if(game.messageTimer<=0)
        message.textContent="";
    }

    updateHUD();

    if(game.shake>0)
      game.shake*=0.88;
  }

  ctx.save();

  if(game.shake>0){

    ctx.translate(
      rand(-game.shake,game.shake),
      rand(-game.shake,game.shake)
    );
  }

  render();

  ctx.restore();

  requestAnimationFrame(loop);
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

  drawCity();
  drawTrees();
  drawBuildings();
  drawCoins();

  for(const c of cars)
    drawVehicle(c,false);

  drawPolice();
  drawBullets();
  drawHelicopter();
  drawPlayer();
  drawParticles();

  /* borda da cidade */

  ctx.strokeStyle="#111";
  ctx.lineWidth=20;

  ctx.strokeRect(
    -game.cameraX,
    -game.cameraY,
    game.worldW,
    game.worldH
  );
}


/* =====================================================
   GAME OVER
===================================================== */

function gameOver(){

  game.running=false;

  document
    .getElementById("endTitle")
    .textContent="💥 CARRO DESTRUÍDO!";

  document
    .getElementById("endText")
    .textContent=
      "A polícia acabou com a sua fuga! "+
      "Você sobreviveu por "+
      Math.floor(game.time)+
      " segundos e fez "+
      game.score+
      " pontos.";

  document
    .getElementById("end")
    .classList.remove("hidden");
}


/* =====================================================
   BOTÕES
===================================================== */

document
  .getElementById("startBtn")
  .addEventListener("click",startGame);

document
  .getElementById("restartBtn")
  .addEventListener("click",startGame);


/* =====================================================
   INICIALIZAÇÃO
===================================================== */

createCity();

updateHUD();

requestAnimationFrame(loop);

</script>

</body>
</html>