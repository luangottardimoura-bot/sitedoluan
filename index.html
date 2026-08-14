<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>A CASA CAIU!</title>

<style>
*{box-sizing:border-box;margin:0;padding:0}
html,body{
  width:100%;height:100%;overflow:hidden;
  background:#111;font-family:Arial,sans-serif;
  touch-action:none;user-select:none
}
canvas{display:block;width:100%;height:100%}

#hud{
  position:fixed;z-index:20;top:10px;left:50%;
  transform:translateX(-50%);
  display:flex;gap:12px;align-items:center;
  padding:9px 13px;color:#fff;
  background:#000c;border:1px solid #ffffff33;
  border-radius:14px;font-weight:bold;
  white-space:nowrap;font-size:14px
}
.life{color:#ff4b55}
.time{color:#5eeaff}
.score{color:#ffd83d}
.police{color:#ff5570}

#wanted{
  position:fixed;z-index:20;top:55px;left:50%;
  transform:translateX(-50%);
  color:#fff;background:#000b;
  padding:5px 12px;border-radius:12px;
  font-weight:bold;font-size:13px
}

.screen{
  position:fixed;inset:0;z-index:100;
  display:flex;align-items:center;justify-content:center;
  background:#000d
}
.hidden{display:none!important}

.panel{
  width:min(600px,92vw);
  padding:32px 24px;text-align:center;color:white;
  border-radius:25px;
  background:linear-gradient(145deg,#55202a,#09131c);
  border:2px solid #ffffff22;
  box-shadow:0 20px 80px #000
}
h1{
  font-size:clamp(42px,12vw,78px);
  font-style:italic;
  text-shadow:0 5px #170609
}
h2{color:#ffd83d;margin:8px 0 18px}
p{color:#dce8ee;line-height:1.6;margin:15px 0}

button{
  border:0;border-radius:15px;
  padding:16px 28px;color:#fff;
  background:linear-gradient(#ff5d69,#b91f35);
  box-shadow:0 5px #681523;
  font-size:18px;font-weight:bold;
  cursor:pointer
}
button:active{
  transform:translateY(3px);
  box-shadow:0 2px #681523
}

#mobile{
  position:fixed;z-index:30;
  left:0;right:0;bottom:15px;
  display:none;justify-content:space-between;
  padding:0 15px
}
.group{display:flex;gap:10px}
.mob{
  width:68px;height:68px;padding:0;
  border-radius:50%;
  background:#07121ddd;
  border:2px solid #ffffff55;
  color:#fff;font-size:25px;
  box-shadow:0 5px 15px #0008
}

@media(max-width:800px){
  #mobile{display:flex}
  #hud{font-size:10px;gap:5px;padding:7px 8px}
  #wanted{top:52px;font-size:11px}
}
</style>
</head>

<body>

<canvas id="game"></canvas>

<div id="hud">
  ❤️ <span class="life" id="life">12</span>
  ⏱️ <span class="time" id="time">0</span>s
  ⭐ <span class="score" id="score">0</span>
  🚓 <span class="police" id="police">1</span>
</div>

<div id="wanted">🚨 PROCURADO</div>

<div id="start" class="screen">
  <div class="panel">
    <h1>A CASA CAIU!</h1>
    <h2>🚓 FUGA URBANA</h2>

    <p>
      Entre na cidade, faça o máximo de pontos
      e fuja da polícia pelo maior tempo possível.
    </p>

    <p>
      ❤️ Você aguenta 12 batidas.<br>
      🚓 A polícia persegue você.<br>
      ⏱️ A cada 100 segundos chegam mais policiais.<br>
      🚁 Depois de ser cercado, o helicóptero entra na perseguição.
    </p>

    <p>
      ⌨️ A/D ou ←/→ = dirigir<br>
      ⬆️ W / ↑ = acelerar<br>
      ⬇️ S / ↓ = frear
    </p>

    <button id="startBtn">🚨 COMEÇAR FUGA</button>
  </div>
</div>

<div id="end" class="screen hidden">
  <div class="panel">
    <h1 id="endTitle">FIM</h1>
    <p id="endText"></p>
    <button id="restartBtn">🔄 JOGAR NOVAMENTE</button>
  </div>
</div>

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

/* =========================
   CANVAS
========================= */

const canvas=document.getElementById("game");
const ctx=canvas.getContext("2d");

let W=innerWidth,H=innerHeight;

function resize(){
  W=innerWidth;
  H=innerHeight;
  canvas.width=W;
  canvas.height=H;
}
addEventListener("resize",resize);
resize();

/* =========================
   HUD
========================= */

const lifeEl=document.getElementById("life");
const timeEl=document.getElementById("time");
const scoreEl=document.getElementById("score");
const policeEl=document.getElementById("police");

/* =========================
   CONTROLES
========================= */

const keys={
  left:false,
  right:false,
  gas:false,
  brake:false
};

addEventListener("keydown",e=>{
  const k=e.key.toLowerCase();

  if(k==="a"||k==="arrowleft")keys.left=true;
  if(k==="d"||k==="arrowright")keys.right=true;
  if(k==="w"||k==="arrowup")keys.gas=true;
  if(k==="s"||k==="arrowdown")keys.brake=true;

  if(["a","d","w","s","arrowleft","arrowright","arrowup","arrowdown"]
    .includes(k))e.preventDefault();
});

addEventListener("keyup",e=>{
  const k=e.key.toLowerCase();

  if(k==="a"||k==="arrowleft")keys.left=false;
  if(k==="d"||k==="arrowright")keys.right=false;
  if(k==="w"||k==="arrowup")keys.gas=false;
  if(k==="s"||k==="arrowdown")keys.brake=false;
});

function hold(id,key){
  const b=document.getElementById(id);

  b.addEventListener("pointerdown",e=>{
    e.preventDefault();
    keys[key]=true;
    try{b.setPointerCapture(e.pointerId)}catch(_){}
  });

  const stop=e=>{
    if(e)e.preventDefault();
    keys[key]=false;
  };

  b.addEventListener("pointerup",stop);
  b.addEventListener("pointercancel",stop);
  b.addEventListener("lostpointercapture",stop);
}

hold("left","left");
hold("right","right");
hold("gas","gas");
hold("brake","brake");

/* =========================
   JOGO
========================= */

const game={
  running:false,
  speed:0,
  distance:0,
  score:0,
  time:0,
  hits:0,
  maxHits:12,
  police:1,
  spawn:0,
  obstacleSpawn:0,
  coinSpawn:0,
  policeSpawn:0,
  helicopter:false,
  helicopterTimer:0,
  shake:0,
  roadMove:0
};

const player={
  x:0,
  y:0,
  w:56,
  h:94,
  vx:0,
  invincible:0
};

let traffic=[];
let police=[];
let obstacles=[];
let coins=[];
let particles=[];

/* =========================
   UTIL
========================= */

function clamp(v,a,b){
  return Math.max(a,Math.min(b,v));
}

function rand(a,b){
  return Math.random()*(b-a)+a;
}

function hit(a,b){
  return a.x<b.x+b.w &&
         a.x+a.w>b.x &&
         a.y<b.y+b.h &&
         a.y+a.h>b.y;
}

/* =========================
   CIDADE
========================= */

function roadWidth(){
  return Math.min(W*.78,760);
}

function roadLeft(){
  return W/2-roadWidth()/2;
}

/* =========================
   INICIAR
========================= */

function startGame(){

  game.running=true;
  game.speed=0;
  game.distance=0;
  game.score=0;
  game.time=0;
  game.hits=0;
  game.police=1;
  game.spawn=30;
  game.obstacleSpawn=70;
  game.coinSpawn=40;
  game.policeSpawn=100;
  game.helicopter=false;
  game.helicopterTimer=0;
  game.shake=0;

  traffic=[];
  police=[];
  obstacles=[];
  coins=[];
  particles=[];

  player.x=W/2-player.w/2;
  player.y=H*.72;
  player.vx=0;
  player.invincible=80;

  document.getElementById("start").classList.add("hidden");
  document.getElementById("end").classList.add("hidden");

  updateHUD();
}

/* =========================
   PLAYER
========================= */

function updatePlayer(){

  if(keys.gas)
    game.speed+=.16;
  else
    game.speed-=.045;

  if(keys.brake)
    game.speed-=.25;

  game.speed=clamp(game.speed,0,16);

  if(keys.left)player.vx-=.6;
  if(keys.right)player.vx+=.6;

  if(!keys.left&&!keys.right)
    player.vx*=.84;

  player.vx=clamp(player.vx,-7,7);
  player.x+=player.vx;

  const left=roadLeft()+12;
  const right=roadLeft()+roadWidth()-player.w-12;

  if(player.x<left){
    player.x=left;
    player.vx*=-.3;
  }

  if(player.x>right){
    player.x=right;
    player.vx*=-.3;
  }

  game.distance+=game.speed*.65;
  game.score+=Math.floor(game.speed*.07);

  if(player.invincible>0)
    player.invincible--;
}

/* =========================
   TRÂNSITO
========================= */

function spawnTraffic(){

  const lanes=4;
  const lw=roadWidth()/lanes;
  const lane=Math.floor(Math.random()*lanes);

  traffic.push({
    x:roadLeft()+lane*lw+lw/2-25,
    y:-120,
    w:50,
    h:88,
    speed:rand(3,8),
    color:["#2980d8","#f1c52e","#39a85b",
           "#c83fd1","#ed6d29","#ddd"][Math.floor(Math.random()*6)]
  });
}

function updateTraffic(){

  game.spawn--;

  if(game.spawn<=0){
    spawnTraffic();
    game.spawn=Math.max(18,48-Math.min(25,game.time/10));
  }

  for(let i=traffic.length-1;i>=0;i--){

    const c=traffic[i];

    c.y+=game.speed-c.speed*.35;

    if(hit(player,c)&&player.invincible<=0){
      crash("trânsito");
      traffic.splice(i,1);
      continue;
    }

    if(c.y>H+150||c.y<-300)
      traffic.splice(i,1);
  }
}

/* =========================
   OBSTÁCULOS
========================= */

function spawnObstacle(){

  const lw=roadWidth()/4;
  const lane=Math.floor(Math.random()*4);

  obstacles.push({
    x:roadLeft()+lane*lw+lw/2-24,
    y:-70,
    w:48,
    h:48,
    type:Math.random()<.5?"barrier":"cone"
  });
}

function updateObstacles(){

  game.obstacleSpawn--;

  if(game.obstacleSpawn<=0){
    spawnObstacle();
    game.obstacleSpawn=Math.max(30,75-game.time/8);
  }

  for(let i=obstacles.length-1;i>=0;i--){

    const o=obstacles[i];

    o.y+=game.speed+1;

    if(hit(player,o)&&player.invincible<=0){
      crash("obstáculo");
      obstacles.splice(i,1);
      continue;
    }

    if(o.y>H+100)
      obstacles.splice(i,1);
  }
}

/* =========================
   MOEDAS
========================= */

function spawnCoin(){

  const lw=roadWidth()/4;
  const lane=Math.floor(Math.random()*4);

  coins.push({
    x:roadLeft()+lane*lw+lw/2,
    y:-30,
    r:10,
    rot:0
  });
}

function updateCoins(){

  game.coinSpawn--;

  if(game.coinSpawn<=0){
    spawnCoin();
    game.coinSpawn=30;
  }

  for(let i=coins.length-1;i>=0;i--){

    const c=coins[i];

    c.y+=game.speed+1;
    c.rot+=.12;

    const box={
      x:c.x-c.r,
      y:c.y-c.r,
      w:c.r*2,
      h:c.r*2
    };

    if(hit(player,box)){
      game.score+=250;
      burst(c.x,c.y,"#ffd52e",12);
      coins.splice(i,1);
      continue;
    }

    if(c.y>H+50)
      coins.splice(i,1);
  }
}

/* =========================
   POLÍCIA
========================= */

function spawnPolice(){

  const side=Math.random()<.5?-1:1;

  police.push({
    x:side<0
      ?roadLeft()-90
      :roadLeft()+roadWidth()+40,
    y:rand(H*.15,H*.55),
    w:55,
    h:92,
    speed:rand(1.5,3.2),
    siren:0
  });
}

function updatePolice(){

  game.policeSpawn--;

  if(game.policeSpawn<=0){

    if(police.length<game.police){
      spawnPolice();
    }

    game.policeSpawn=100;
  }

  for(const p of police){

    p.siren+=.2;

    const targetX=player.x;

    p.x+=(targetX-p.x)*.012;

    p.y+=(player.y-p.y)*.006;

    p.y+=game.speed*.35;

    if(hit(player,p)&&player.invincible<=0){

      crash("polícia");

      p.x+=Math.random()<.5?-100:100;
    }
  }

  /* helicóptero */

  if(game.time>=200&&!game.helicopter){

    game.helicopter=true;
    game.helicopterTimer=0;
  }

  if(game.helicopter){

    game.helicopterTimer++;

    if(game.helicopterTimer%100===0){

      burst(
        player.x+player.w/2,
        player.y-20,
        "#ff3344",
        8
      );

      if(player.invincible<=0){
        game.hits++;
        game.score=Math.max(0,game.score-150);
        game.shake=8;
        player.invincible=35;

        if(game.hits>=game.maxHits)
          explode();
      }
    }
  }
}

/* =========================
   BATIDA
========================= */

function crash(reason){

  if(player.invincible>0)return;

  game.hits++;
  game.speed*=.45;
  game.score=Math.max(0,game.score-100);

  player.invincible=75;
  game.shake=14;

  burst(
    player.x+player.w/2,
    player.y+player.h/2,
    "#ff4938",
    20
  );

  if(game.hits>=game.maxHits)
    explode();
}

/* =========================
   EXPLOSÃO
========================= */

function explode(){

  game.running=false;

  for(let i=0;i<50;i++)
    burst(
      player.x+player.w/2,
      player.y+player.h/2,
      Math.random()<.5?"#ff3b20":"#ffd52e",
      1
    );

  document.getElementById("endTitle").textContent="💥 CARRO DESTRUIDO!";

  document.getElementById("endText").textContent=
    "Você sofreu 12 batidas. "+
    "Pontuação: "+game.score+
    " | Tempo: "+Math.floor(game.time)+" segundos.";

  document.getElementById("end").classList.remove("hidden");
}

/* =========================
   TEMPO
========================= */

let lastTime=performance.now();

function updateTime(now){

  const dt=Math.min(.05,(now-lastTime)/1000);
  lastTime=now;

  if(game.running){

    game.time+=dt;

    /*
      A cada 100 segundos:
      aumenta o número de policiais.
    */
    const wantedLevel=
      Math.floor(game.time/100)+1;

    game.police=
      Math.min(8,wantedLevel);
  }
}

/* =========================
   PARTÍCULAS
========================= */

function burst(x,y,color,n){

  for(let i=0;i<n;i++){

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
    p.vy+=.12;
    p.life-=.035;

    if(p.life<=0)
      particles.splice(i,1);
  }
}

/* =========================
   HUD
========================= */

function updateHUD(){

  lifeEl.textContent=
    Math.max(0,game.maxHits-game.hits);

  timeEl.textContent=
    Math.floor(game.time);

  scoreEl.textContent=
    game.score;

  policeEl.textContent=
    game.police;
}

/* =========================
   FUNDO
========================= */

function drawBackground(){

  const sky=ctx.createLinearGradient(0,0,0,H);

  sky.addColorStop(0,"#151d38");
  sky.addColorStop(.5,"#303c54");
  sky.addColorStop(1,"#111820");

  ctx.fillStyle=sky;
  ctx.fillRect(0,0,W,H);

  /* prédios */

  const offset=
    (game.distance*.12)%180;

  for(let x=-180;x<W+180;x+=120){

    const h=rand(100,280);
    const xx=x-offset;

    ctx.fillStyle=
      x%240===0?"#202838":"#283344";

    ctx.fillRect(
      xx,
      H*.48-h,
      100,
      h
    );

    ctx.fillStyle="#ffd95a";

    for(let wy=0;wy<5;wy++){
      for(let wx=0;wx<3;wx++){

        if(Math.random()<.7){

          ctx.fillRect(
            xx+15+wx*25,
            H*.48-h+20+wy*35,
            9,
            13
          );
        }
      }
    }
  }

  /* grama/calçada */

  ctx.fillStyle="#1c4d32";
  ctx.fillRect(0,H*.48,W,H);

  ctx.fillStyle="#777";
  ctx.fillRect(0,H*.55,roadLeft(),H);
  ctx.fillRect(
    roadLeft()+roadWidth(),
    H*.55,
    W-roadLeft()-roadWidth(),
    H
  );
}

/* =========================
   ESTRADA
========================= */

function drawRoad(){

  const left=roadLeft();
  const width=roadWidth();

  ctx.fillStyle="#555";
  ctx.fillRect(left-18,0,width+36,H);

  ctx.fillStyle="#292d32";
  ctx.fillRect(left,0,width,H);

  const laneW=width/4;
  const offset=(game.distance*2)%120;

  ctx.fillStyle="#e8e8d2";

  for(let lane=1;lane<4;lane++){

    const x=left+lane*laneW-3;

    for(let y=-100+offset;y<H;y+=120){

      ctx.fillRect(x,y,6,65);
    }
  }

  ctx.fillStyle="#fff";
  ctx.fillRect(left,0,6,H);
  ctx.fillRect(left+width-6,0,6,H);
}

/* =========================
   CARRO DO JOGADOR
========================= */

function drawPlayer(){

  if(
    player.invincible>0 &&
    Math.floor(player.invincible/6)%2===0
  )return;

  ctx.save();

  ctx.translate(
    player.x+player.w/2,
    player.y+player.h/2
  );

  ctx.rotate(player.vx*.025);

  /* sombra */

  ctx.fillStyle="#0009";
  ctx.beginPath();
  ctx.ellipse(0,48,35,8,0,0,Math.PI*2);
  ctx.fill();

  /* rodas */

  ctx.fillStyle="#080808";

  ctx.fillRect(-33,-32,10,25);
  ctx.fillRect(23,-32,10,25);
  ctx.fillRect(-33,8,10,25);
  ctx.fillRect(23,8,10,25);

  /* carroceria */

  ctx.fillStyle="#e52c3f";
  ctx.beginPath();
  ctx.roundRect(-28,-47,56,94,12);
  ctx.fill();

  /* vidro */

  ctx.fillStyle="#142b3c";
  ctx.beginPath();
  ctx.roundRect(-19,-33,38,27,7);
  ctx.fill();

  /* faixa */

  ctx.fillStyle="#ffffff99";
  ctx.fillRect(-4,-47,8,94);

  /* faróis */

  ctx.fillStyle="#fff3a3";
  ctx.fillRect(-20,-44,11,6);
  ctx.fillRect(9,-44,11,6);

  /* lanternas */

  ctx.fillStyle="#ff2038";
  ctx.fillRect(-20,38,11,6);
  ctx.fillRect(9,38,11,6);

  ctx.restore();
}

/* =========================
   TRÂNSITO
========================= */

function drawTraffic(){

  for(const c of traffic){

    ctx.save();

    ctx.translate(
      c.x+c.w/2,
      c.y+c.h/2
    );

    ctx.fillStyle="#111";
    ctx.fillRect(-29,-30,9,24);
    ctx.fillRect(20,-30,9,24);
    ctx.fillRect(-29,7,9,24);
    ctx.fillRect(20,7,9,24);

    ctx.fillStyle=c.color;
    ctx.beginPath();
    ctx.roundRect(-25,-44,50,88,10);
    ctx.fill();

    ctx.fillStyle="#172e3d";
    ctx.beginPath();
    ctx.roundRect(-17,-30,34,27,6);
    ctx.fill();

    ctx.fillStyle="#fff0a0";
    ctx.fillRect(-18,-42,10,5);
    ctx.fillRect(8,-42,10,5);

    ctx.restore();
  }
}

/* =========================
   POLÍCIA
========================= */

function drawPolice(){

  for(const p of police){

    ctx.save();

    ctx.translate(
      p.x+p.w/2,
      p.y+p.h/2
    );

    ctx.fillStyle="#111";
    ctx.fillRect(-30,-30,10,24);
    ctx.fillRect(20,-30,10,24);
    ctx.fillRect(-30,7,10,24);
    ctx.fillRect(20,7,10,24);

    ctx.fillStyle="#e8e8e8";
    ctx.beginPath();
    ctx.roundRect(-27,-46,54,92,10);
    ctx.fill();

    ctx.fillStyle="#17283c";
    ctx.fillRect(-27,-5,54,12);

    ctx.fillStyle="#111";
    ctx.beginPath();
    ctx.roundRect(-18,-31,36,25,6);
    ctx.fill();

    /* sirene */

    ctx.fillStyle=
      Math.sin(p.siren)>0
        ?"red":"#238cff";

    ctx.fillRect(-13,-52,12,7);

    ctx.fillStyle=
      Math.sin(p.siren)>0
        ?"#238cff":"red";

    ctx.fillRect(1,-52,12,7);

    ctx.restore();
  }
}

/* =========================
   OBSTÁCULOS
========================= */

function drawObstacles(){

  for(const o of obstacles){

    if(o.type==="cone"){

      ctx.fillStyle="#ff711c";

      ctx.beginPath();
      ctx.moveTo(o.x+o.w/2,o.y);
      ctx.lineTo(o.x+o.w,o.y+o.h);
      ctx.lineTo(o.x,o.y+o.h);
      ctx.closePath();
      ctx.fill();

      ctx.fillStyle="#fff";
      ctx.fillRect(o.x+10,o.y+25,o.w-20,8);

    }else{

      ctx.fillStyle="#ff9f25";
      ctx.fillRect(o.x,o.y,o.w,o.h);

      ctx.fillStyle="#fff";
      ctx.fillRect(o.x,o.y+10,o.w,8);
      ctx.fillRect(o.x,o.y+30,o.w,8);
    }
  }
}

/* =========================
   MOEDAS
========================= */

function drawCoins(){

  for(const c of coins){

    ctx.save();
    ctx.translate(c.x,c.y);
    ctx.rotate(c.rot);

    ctx.fillStyle="#ffd52e";

    ctx.beginPath();
    ctx.ellipse(0,0,7,12,0,0,Math.PI*2);
    ctx.fill();

    ctx.strokeStyle="#fff3a0";
    ctx.lineWidth=2;
    ctx.stroke();

    ctx.restore();
  }
}

/* =========================
   HELICÓPTERO
========================= */

function drawHelicopter(){

  if(!game.helicopter)return;

  const x=
    player.x+
    player.w/2+
    Math.sin(game.time*2)*100;

  const y=
    player.y-150+
    Math.sin(game.time*3)*15;

  ctx.save();
  ctx.translate(x,y);

  ctx.fillStyle="#20242b";
  ctx.fillRect(-35,-15,70,30);

  ctx.fillStyle="#111";
  ctx.beginPath();
  ctx.ellipse(0,-17,65,5,0,0,Math.PI*2);
  ctx.fill();

  ctx.fillStyle="#ff374c";
  ctx.fillRect(-18,-8,36,6);

  ctx.fillStyle="#8deaff";
  ctx.fillRect(-22,-4,44,12);

  /* tiros */

  if(Math.floor(game.helicopterTimer/15)%2===0){

    ctx.strokeStyle="#ff3845";
    ctx.lineWidth=3;

    ctx.beginPath();
    ctx.moveTo(0,15);
    ctx.lineTo(
      player.x+player.w/2-x,
      player.y-y
    );
    ctx.stroke();
  }

  ctx.restore();
}

/* =========================
   PARTÍCULAS
========================= */

function drawParticles(){

  for(const p of particles){

    ctx.globalAlpha=p.life;
    ctx.fillStyle=p.color;

    ctx.beginPath();
    ctx.arc(p.x,p.y,p.size,0,Math.PI*2);
    ctx.fill();
  }

  ctx.globalAlpha=1;
}

/* =========================
   RENDER
========================= */

function render(){

  ctx.clearRect(0,0,W,H);

  drawBackground();
  drawRoad();
  drawCoins();
  drawObstacles();
  drawTraffic();
  drawPolice();
  drawHelicopter();
  drawPlayer();
  drawParticles();

  /* texto de perigo */

  if(game.police>=3){

    ctx.textAlign="center";
    ctx.font="bold 22px Arial";
    ctx.fillStyle="#ff4050";
    ctx.fillText(
      "🚨 PERSEGUIÇÃO INTENSA!",
      W/2,
      H*.18
    );
  }

  if(game.helicopter){

    ctx.font="bold 18px Arial";
    ctx.fillStyle="#ff3045";
    ctx.fillText(
      "🚁 HELICÓPTERO!",
      W/2,
      H*.22
    );
  }
}

/* =========================
   LOOP
========================= */

function loop(now){

  updateTime(now);

  if(game.running){

    updatePlayer();
    updateTraffic();
    updateObstacles();
    updateCoins();
    updatePolice();
    updateParticles();

    game.roadMove+=game.speed;

    updateHUD();

    if(game.shake>0)
      game.shake*=.88;
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

/* =========================
   BOTÕES
========================= */

document.getElementById("startBtn").addEventListener("click",()=>{
  startGame();
});

document.getElementById("restartBtn").addEventListener("click",()=>{
  startGame();
});

/* =========================
   INÍCIO
========================= */

updateHUD();
requestAnimationFrame(loop);

</script>
</body>
</html>