<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>Último Sobrevivente</title>

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
  background:#080b12;
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
  z-index:10;
  top:12px;
  left:50%;
  transform:translateX(-50%);
  display:flex;
  gap:14px;
  padding:9px 14px;
  border-radius:15px;
  background:#000c;
  border:1px solid #ffffff22;
  color:white;
  font-weight:bold;
  white-space:nowrap;
}

.hp{color:#ff5265}
.score{color:#ffd83d}
.coin{color:#4de8ff}
.time{color:#9cff70}

.screen{
  position:fixed;
  inset:0;
  z-index:50;
  display:flex;
  align-items:center;
  justify-content:center;
  background:#000d;
}

.hidden{
  display:none!important;
}

.panel{
  width:min(600px,92vw);
  padding:32px 24px;
  text-align:center;
  color:white;
  border-radius:25px;
  background:
    radial-gradient(circle at top,#273f67,#080c15 70%);
  border:2px solid #ffffff22;
  box-shadow:0 20px 80px #000;
}

h1{
  font-size:clamp(40px,10vw,70px);
  color:#fff;
  text-shadow:0 5px #394cff;
}

h2{
  color:#ffd83d;
  margin:8px;
}

p{
  color:#dce5ee;
  line-height:1.6;
  margin:15px 0;
}

button{
  border:0;
  border-radius:15px;
  padding:15px 28px;
  color:white;
  background:linear-gradient(#ff566b,#b51d38);
  font-size:18px;
  font-weight:bold;
  box-shadow:0 5px #671323;
}

button:active{
  transform:translateY(3px);
  box-shadow:0 2px #671323;
}

#mobile{
  position:fixed;
  z-index:20;
  left:0;
  right:0;
  bottom:15px;
  display:none;
  justify-content:space-between;
  padding:0 18px;
}

.group{
  display:flex;
  gap:8px;
}

.mob{
  width:62px;
  height:62px;
  padding:0;
  border-radius:50%;
  background:#07101ddd;
  border:2px solid #ffffff55;
  box-shadow:0 4px 15px #0008;
}

#attack{
  width:75px;
  height:75px;
  position:fixed;
  right:20px;
  bottom:105px;
  z-index:21;
  display:none;
  padding:0;
  border-radius:50%;
  background:linear-gradient(#ff4d61,#a70d27);
}

@media(max-width:800px){
  #mobile{
    display:flex;
  }

  #attack{
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
  ❤️ <span class="hp" id="hp">100</span>
  ⭐ <span class="score" id="score">0</span>
  🪙 <span class="coin" id="coins">0</span>
  ⏱️ <span class="time" id="time">0</span>s
</div>

<div id="start" class="screen">
  <div class="panel">
    <h1>ÚLTIMO</h1>
    <h2>SOBREVIVENTE</h2>

    <p>
      Sobreviva o máximo possível,
      derrote os inimigos e consiga a maior pontuação.
    </p>

    <p>
      ⌨️ WASD / Setas = mover<br>
      ⚔️ Espaço = atacar
    </p>

    <button id="startBtn">COMEÇAR</button>
  </div>
</div>

<div id="end" class="screen hidden">
  <div class="panel">
    <h1>💀 FIM</h1>
    <p id="endText"></p>
    <button id="restartBtn">JOGAR NOVAMENTE</button>
  </div>
</div>

<div id="mobile">
  <div class="group">
    <button class="mob" id="up">▲</button>
  </div>

  <div class="group">
    <button class="mob" id="left">◀</button>
    <button class="mob" id="down">▼</button>
    <button class="mob" id="right">▶</button>
  </div>
</div>

<button id="attack">⚔️</button>

<script>
"use strict";

/* =========================
   CANVAS
========================= */

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


/* =========================
   ELEMENTOS
========================= */

const start=document.getElementById("start");
const end=document.getElementById("end");

const hpEl=document.getElementById("hp");
const scoreEl=document.getElementById("score");
const coinsEl=document.getElementById("coins");
const timeEl=document.getElementById("time");

const endText=document.getElementById("endText");


/* =========================
   CONTROLES
========================= */

const keys={
  up:false,
  down:false,
  left:false,
  right:false,
  attack:false
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

  if(k===" ")
    keys.attack=true;

  if(
    ["arrowup","arrowdown","arrowleft","arrowright"," "]
    .includes(k)
  ){
    e.preventDefault();
  }
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

  if(k===" ")
    keys.attack=false;
});


/* =========================
   CONTROLES TOUCH
========================= */

function hold(id,key){

  const b=document.getElementById(id);

  b.addEventListener("pointerdown",e=>{
    e.preventDefault();
    keys[key]=true;

    try{
      b.setPointerCapture(e.pointerId);
    }catch(_){}
  });

  function release(e){
    if(e)e.preventDefault();
    keys[key]=false;
  }

  b.addEventListener("pointerup",release);
  b.addEventListener("pointercancel",release);
  b.addEventListener("lostpointercapture",release);
}

hold("up","up");
hold("down","down");
hold("left","left");
hold("right","right");
hold("attack","attack");


/* =========================
   ESTADO
========================= */

const game={
  running:false,
  hp:100,
  maxHp:100,
  score:0,
  coins:0,
  time:0,
  spawnTimer:0,
  attackTimer:0,
  difficulty:1,
  shake:0
};

const player={
  x:0,
  y:0,
  r:22,
  speed:4.5,
  facingX:1,
  facingY:0,
  invincible:0
};

let enemies=[];
let coins=[];
let particles=[];


/* =========================
   UTILIDADES
========================= */

function clamp(v,a,b){
  return Math.max(a,Math.min(b,v));
}

function rand(a,b){
  return Math.random()*(b-a)+a;
}

function dist(a,b){
  return Math.hypot(a.x-b.x,a.y-b.y);
}


/* =========================
   INICIAR
========================= */

function startGame(){

  game.running=true;
  game.hp=100;
  game.score=0;
  game.coins=0;
  game.time=0;
  game.spawnTimer=20;
  game.attackTimer=0;
  game.difficulty=1;
  game.shake=0;

  enemies=[];
  coins=[];
  particles=[];

  player.x=W/2;
  player.y=H/2;
  player.invincible=60;

  start.classList.add("hidden");
  end.classList.add("hidden");

  updateHUD();
}


/* =========================
   PLAYER
========================= */

function updatePlayer(){

  let dx=0;
  let dy=0;

  if(keys.left)dx--;
  if(keys.right)dx++;
  if(keys.up)dy--;
  if(keys.down)dy++;

  if(dx!==0||dy!==0){

    const len=Math.hypot(dx,dy);

    dx/=len;
    dy/=len;

    player.x+=dx*player.speed;
    player.y+=dy*player.speed;

    player.facingX=dx;
    player.facingY=dy;
  }

  const margin=30;

  player.x=clamp(
    player.x,
    margin,
    W-margin
  );

  player.y=clamp(
    player.y,
    80,
    H-margin
  );

  if(player.invincible>0)
    player.invincible--;

  if(game.attackTimer>0)
    game.attackTimer--;
}


/* =========================
   ATAQUE
========================= */

function attack(){

  if(!game.running)return;
  if(game.attackTimer>0)return;

  game.attackTimer=18;

  const range=75;

  let hit=false;

  for(const e of enemies){

    if(e.dead)continue;

    const d=dist(player,e);

    if(d<range+e.r){

      e.hp--;
      hit=true;

      burst(
        e.x,
        e.y,
        "#ff5265",
        10
      );

      if(e.hp<=0){

        e.dead=true;

        game.score+=e.type==="big"?250:100;

        if(Math.random()<0.7){

          coins.push({
            x:e.x,
            y:e.y,
            r:8
          });
        }
      }
    }
  }

  if(hit)
    game.score+=25;
}


/* =========================
   INIMIGOS
========================= */

function spawnEnemy(){

  const side=Math.floor(Math.random()*4);

  let x;
  let y;

  if(side===0){
    x=-40;
    y=rand(80,H);
  }

  if(side===1){
    x=W+40;
    y=rand(80,H);
  }

  if(side===2){
    x=rand(0,W);
    y=50;
  }

  if(side===3){
    x=rand(0,W);
    y=H+40;
  }

  const big=
    Math.random()<Math.min(
      0.1+game.difficulty*0.015,
      0.35
    );

  enemies.push({

    x,
    y,

    r:big?28:18,

    speed:
      big
        ?1.15+game.difficulty*.05
        :1.5+game.difficulty*.09,

    hp:big?3:1,

    maxHp:big?3:1,

    type:big?"big":"normal",

    dead:false
  });
}


function updateEnemies(){

  game.spawnTimer--;

  if(game.spawnTimer<=0){

    spawnEnemy();

    if(game.difficulty>=4&&Math.random()<0.25)
      spawnEnemy();

    game.spawnTimer=Math.max(
      12,
      48-game.difficulty*2
    );
  }

  for(const e of enemies){

    if(e.dead)continue;

    const dx=player.x-e.x;
    const dy=player.y-e.y;

    const d=Math.hypot(dx,dy)||1;

    e.x+=(dx/d)*e.speed;
    e.y+=(dy/d)*e.speed;

    if(
      player.invincible<=0 &&
      d<player.r+e.r
    ){

      damage(8);

      e.x-=dx/d*30;
      e.y-=dy/d*30;
    }
  }

  enemies=enemies.filter(e=>!e.dead);
}


/* =========================
   DANO
========================= */

function damage(amount){

  if(player.invincible>0)
    return;

  game.hp-=amount;

  player.invincible=70;
  game.shake=10;

  burst(
    player.x,
    player.y,
    "#ff334d",
    18
  );

  if(game.hp<=0){

    game.hp=0;
    gameOver();
  }
}


/* =========================
   MOEDAS
========================= */

function updateCoins(){

  for(let i=coins.length-1;i>=0;i--){

    const c=coins[i];

    if(
      Math.hypot(
        player.x-c.x,
        player.y-c.y
      )<
      player.r+c.r
    ){

      game.coins++;
      game.score+=150;

      burst(
        c.x,
        c.y,
        "#ffd52e",
        12
      );

      coins.splice(i,1);
    }
  }
}


/* =========================
   TEMPO / DIFICULDADE
========================= */

function updateDifficulty(){

  game.time+=1/60;

  game.difficulty=
    1+
    Math.floor(game.time/15);
}


/* =========================
   PARTÍCULAS
========================= */

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

  for(let i=particles.length-1;i>=0;i--){

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


/* =========================
   HUD
========================= */

function updateHUD(){

  hpEl.textContent=Math.max(
    0,
    Math.ceil(game.hp)
  );

  scoreEl.textContent=game.score;
  coinsEl.textContent=game.coins;
  timeEl.textContent=Math.floor(game.time);
}


/* =========================
   FUNDO
========================= */

function drawBackground(){

  const g=ctx.createLinearGradient(
    0,0,0,H
  );

  g.addColorStop(0,"#121b38");
  g.addColorStop(1,"#05070d");

  ctx.fillStyle=g;
  ctx.fillRect(0,0,W,H);

  /* estrelas */

  ctx.fillStyle="#ffffff55";

  for(let i=0;i<70;i++){

    const x=(i*137)%W;
    const y=(i*83)%H;

    ctx.fillRect(
      x,
      y,
      2,
      2
    );
  }

  /* arena */

  ctx.strokeStyle="#1e3150";
  ctx.lineWidth=2;

  for(
    let x=0;
    x<W;
    x+=80
  ){

    ctx.beginPath();
    ctx.moveTo(x,70);
    ctx.lineTo(x,H);
    ctx.stroke();
  }

  for(
    let y=80;
    y<H;
    y+=80
  ){

    ctx.beginPath();
    ctx.moveTo(0,y);
    ctx.lineTo(W,y);
    ctx.stroke();
  }
}


/* =========================
   PLAYER
========================= */

function drawPlayer(){

  if(
    player.invincible>0 &&
    Math.floor(player.invincible/5)%2===0
  ){
    return;
  }

  ctx.save();

  ctx.translate(
    player.x,
    player.y
  );

  /* sombra */

  ctx.fillStyle="#0008";

  ctx.beginPath();

  ctx.ellipse(
    0,
    25,
    25,
    8,
    0,
    0,
    Math.PI*2
  );

  ctx.fill();

  /* corpo */

  ctx.fillStyle="#3e6cff";

  ctx.beginPath();

  ctx.arc(
    0,
    0,
    player.r,
    0,
    Math.PI*2
  );

  ctx.fill();

  ctx.strokeStyle="#9db4ff";
  ctx.lineWidth=3;
  ctx.stroke();

  /* visor */

  ctx.fillStyle="#071225";

  ctx.beginPath();

  ctx.arc(
    player.facingX*7,
    player.facingY*7-3,
    9,
    0,
    Math.PI*2
  );

  ctx.fill();

  /* ataque */

  if(game.attackTimer>0){

    ctx.strokeStyle="#7cf6ff";
    ctx.lineWidth=7;

    ctx.beginPath();

    ctx.arc(
      0,
      0,
      58,
      -1.1,
      1.1
    );

    ctx.stroke();
  }

  ctx.restore();
}


/* =========================
   INIMIGOS
========================= */

function drawEnemies(){

  for(const e of enemies){

    ctx.save();

    ctx.translate(
      e.x,
      e.y
    );

    if(e.type==="big"){

      ctx.fillStyle="#8c2eff";

      ctx.beginPath();

      ctx.arc(
        0,
        0,
        e.r,
        0,
        Math.PI*2
      );

      ctx.fill();

      ctx.strokeStyle="#e2b4ff";
      ctx.lineWidth=3;
      ctx.stroke();

      ctx.fillStyle="#ff304f";

      ctx.beginPath();

      ctx.arc(
        0,
        0,
        8,
        0,
        Math.PI*2
      );

      ctx.fill();

    }else{

      ctx.fillStyle="#e63251";

      ctx.beginPath();

      ctx.arc(
        0,
        0,
        e.r,
        0,
        Math.PI*2
      );

      ctx.fill();

      ctx.fillStyle="#ffb3bd";

      ctx.beginPath();

      ctx.arc(
        -5,
        -4,
        3,
        0,
        Math.PI*2
      );

      ctx.arc(
        5,
        -4,
        3,
        0,
        Math.PI*2
      );

      ctx.fill();
    }

    /* barra de vida */

    if(e.maxHp>1){

      ctx.fillStyle="#26060c";

      ctx.fillRect(
        -25,
        -40,
        50,
        5
      );

      ctx.fillStyle="#ff4055";

      ctx.fillRect(
        -25,
        -40,
        50*(e.hp/e.maxHp),
        5
      );
    }

    ctx.restore();
  }
}


/* =========================
   MOEDAS
========================= */

function drawCoins(){

  for(const c of coins){

    ctx.fillStyle="#ffd52e";

    ctx.beginPath();

    ctx.arc(
      c.x,
      c.y,
      c.r,
      0,
      Math.PI*2
    );

    ctx.fill();

    ctx.strokeStyle="#fff4a0";
    ctx.lineWidth=2;
    ctx.stroke();
  }
}


/* =========================
   PARTÍCULAS
========================= */

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


/* =========================
   RENDER
========================= */

function render(){

  ctx.clearRect(
    0,
    0,
    W,
    H
  );

  drawBackground();
  drawCoins();
  drawEnemies();
  drawPlayer();
  drawParticles();
}


/* =========================
   GAME OVER
========================= */

function gameOver(){

  game.running=false;

  endText.textContent=
    "Você sobreviveu por "+
    Math.floor(game.time)+
    " segundos! "+
    "Pontuação: "+
    game.score+
    " | Moedas: "+
    game.coins;

  end.classList.remove("hidden");
}


/* =========================
   BOTÕES
========================= */

document.getElementById("startBtn")
.addEventListener("click",startGame);

document.getElementById("restartBtn")
.addEventListener("click",startGame);


/* =========================
   LOOP
========================= */

function loop(){

  if(game.running){

    updatePlayer();

    if(keys.attack)
      attack();

    updateEnemies();
    updateCoins();
    updateDifficulty();
    updateParticles();
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


/* =========================
   INÍCIO
========================= */

updateHUD();

requestAnimationFrame(loop);

</script>

</body>
</html>