<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>Hero Quest - 10 Fases</title>

<style>
*{box-sizing:border-box;margin:0;padding:0}

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
  display:block;
  width:100%;
  height:100%;
}

#hud{
  position:fixed;
  z-index:10;
  top:10px;
  left:50%;
  transform:translateX(-50%);
  display:flex;
  gap:14px;
  padding:9px 15px;
  color:white;
  background:#000b;
  border:1px solid #ffffff33;
  border-radius:15px;
  font-weight:bold;
  white-space:nowrap;
}

#hud .hp{color:#ff6872}
#hud .coin{color:#ffd83d}
#hud .score{color:#65d9ff}
#hud .stage{color:#9cff70}

.screen{
  position:fixed;
  inset:0;
  z-index:50;
  display:flex;
  align-items:center;
  justify-content:center;
  background:#000c;
}

.panel{
  width:min(620px,92vw);
  padding:35px 25px;
  text-align:center;
  color:#fff;
  border-radius:25px;
  background:linear-gradient(145deg,#28495d,#09131c);
  border:2px solid #ffffff22;
  box-shadow:0 20px 80px #000;
}

h1{
  font-size:clamp(40px,10vw,72px);
  text-shadow:0 5px #174c6b;
}

h2{
  color:#ffd83d;
  margin:10px;
}

p{
  margin:15px 0;
  line-height:1.6;
  color:#dce8ee;
}

button{
  border:0;
  border-radius:14px;
  padding:15px 28px;
  color:white;
  background:linear-gradient(#ff626c,#bd2536);
  font-size:18px;
  font-weight:bold;
  box-shadow:0 5px #701723;
  cursor:pointer;
}

button:active{
  transform:translateY(3px);
  box-shadow:0 2px #701723;
}

.hidden{
  display:none!important;
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
  pointer-events:none;
}

.group{
  display:flex;
  gap:10px;
}

.mob{
  width:64px;
  height:64px;
  padding:0;
  border-radius:50%;
  background:#07121ddd;
  border:2px solid #ffffff66;
  color:#fff;
  font-size:24px;
  pointer-events:auto;
  box-shadow:0 5px 15px #0008;
}

#attack{
  position:fixed;
  right:20px;
  bottom:100px;
  width:72px;
  height:72px;
  z-index:21;
  display:none;
  padding:0;
  border-radius:50%;
  background:#a7192b;
  pointer-events:auto;
}

@media(max-width:800px){
  #mobile{display:flex}

  #attack{display:block}

  #hud{
    font-size:11px;
    gap:7px;
    padding:7px 9px;
    max-width:96vw;
    overflow:hidden;
  }
}
</style>
</head>

<body>

<canvas id="game"></canvas>

<div id="hud">
  ❤️ <span class="hp" id="hp">100</span>
  🪙 <span class="coin" id="coins">0</span>
  ⭐ <span class="score" id="score">0</span>
  🏆 <span class="stage">Fase <span id="level">1</span>/10</span>
</div>

<div id="start" class="screen">
  <div class="panel">
    <h1>HERO QUEST</h1>
    <h2>10 FASES</h2>

    <p>
      Explore, pule, pegue moedas, enfrente criaturas
      e chegue ao final de cada fase.
    </p>

    <p>
      ⌨️ A/D ou ←/→ = andar<br>
      ⬆️ W / ↑ / Espaço = pular<br>
      ⚔️ X / Z = atacar
    </p>

    <button id="startBtn">COMEÇAR</button>
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
  <div class="group">
    <button class="mob" id="left">◀</button>
    <button class="mob" id="right">▶</button>
  </div>

  <button class="mob" id="jump">▲</button>
</div>

<button id="attack">⚔️</button>

<script>
"use strict";

/* =====================================================
   CANVAS
===================================================== */

const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

let W = window.innerWidth;
let H = window.innerHeight;

function resize(){
  W = window.innerWidth;
  H = window.innerHeight;

  const oldGround = platforms.length
    ? platforms[0].y
    : H - 90;

  canvas.width = W;
  canvas.height = H;

  /*
    Corrige o problema de redimensionar a janela:
    o chão e os objetos que dependem da altura
    acompanham a nova resolução.
  */
  if(game.running && platforms.length){
    const newGround = H - 90;
    const delta = newGround - oldGround;

    for(const p of platforms){
      p.y += delta;
    }

    for(const e of enemies){
      e.y += delta;
    }

    for(const c of coins){
      c.y += delta;
    }

    for(const p of powerups){
      p.y += delta;
    }

    if(goal){
      goal.y += delta;
    }

    player.y += delta;
  }
}

window.addEventListener("resize", resize);

/* =====================================================
   HUD
===================================================== */

const hpEl = document.getElementById("hp");
const coinsEl = document.getElementById("coins");
const scoreEl = document.getElementById("score");
const levelEl = document.getElementById("level");

/* =====================================================
   CONTROLES
===================================================== */

const keys = {
  left:false,
  right:false,
  jump:false,
  attack:false
};

let attackPressed = false;

window.addEventListener("keydown", e => {

  const k = e.key.toLowerCase();

  if(k === "a" || k === "arrowleft"){
    keys.left = true;
    e.preventDefault();
  }

  if(k === "d" || k === "arrowright"){
    keys.right = true;
    e.preventDefault();
  }

  if(k === "w" || k === "arrowup" || k === " "){
    keys.jump = true;
    e.preventDefault();
  }

  if(k === "x" || k === "z"){
    /*
      Ataque passa a ser disparado uma vez
      por pressionamento, evitando spam acidental.
    */
    if(!keys.attack){
      attackPressed = true;
    }

    keys.attack = true;
    e.preventDefault();
  }
});

window.addEventListener("keyup", e => {

  const k = e.key.toLowerCase();

  if(k === "a" || k === "arrowleft")
    keys.left = false;

  if(k === "d" || k === "arrowright")
    keys.right = false;

  if(k === "w" || k === "arrowup" || k === " ")
    keys.jump = false;

  if(k === "x" || k === "z")
    keys.attack = false;
});

/* =====================================================
   CONTROLES MOBILE
===================================================== */

function hold(id,key){

  const button = document.getElementById(id);

  button.addEventListener("pointerdown", e => {

    e.preventDefault();

    keys[key] = true;

    if(key === "attack"){
      attackPressed = true;
    }

    try{
      button.setPointerCapture(e.pointerId);
    }catch(_){}
  });

  const stop = e => {

    if(e)
      e.preventDefault();

    keys[key] = false;
  };

  button.addEventListener("pointerup", stop);
  button.addEventListener("pointercancel", stop);
  button.addEventListener("lostpointercapture", stop);
}

hold("left","left");
hold("right","right");
hold("jump","jump");
hold("attack","attack");

/* =====================================================
   ESTADO
===================================================== */

const game = {
  running:false,
  level:1,
  score:0,
  coins:0,
  hp:100,
  maxHp:100,
  camera:0,
  world:6000,
  finishX:5600,
  message:"",
  messageTime:0,
  shake:0,
  levelChanging:false
};

/* =====================================================
   PLAYER
===================================================== */

const player = {
  x:150,
  y:100,
  w:42,
  h:62,
  vx:0,
  vy:0,
  speed:5,
  jump:13,
  grounded:false,
  facing:1,
  attackTimer:0,
  invincible:0
};

let platforms = [];
let enemies = [];
let coins = [];
let powerups = [];
let particles = [];
let goal = null;

/* =====================================================
   UTILIDADES
===================================================== */

function clamp(v,a,b){
  return Math.max(a,Math.min(b,v));
}

function rand(a,b){
  return Math.random() * (b-a) + a;
}

function overlap(a,b){
  return (
    a.x < b.x+b.w &&
    a.x+a.w > b.x &&
    a.y < b.y+b.h &&
    a.y+a.h > b.y
  );
}

/*
  Verifica se um retângulo está dentro de uma plataforma
  no eixo X.
*/
function horizontalOverlap(a,b){
  return (
    a.x + a.w > b.x &&
    a.x < b.x + b.w
  );
}

/* =====================================================
   CRIAÇÃO DA FASE
===================================================== */

function createLevel(){

  platforms = [];
  enemies = [];
  coins = [];
  powerups = [];
  particles = [];
  goal = null;

  game.world = 6000;
  game.finishX = 5600;
  game.levelChanging = false;

  const ground = H - 90;
  const difficulty = game.level;

  /* CHÃO */

  platforms.push({
    x:0,
    y:ground,
    w:game.world,
    h:200
  });

  /* PLATAFORMAS */

  for(let i=0;i<10+difficulty;i++){

    const x = 350 + i*450 + rand(-70,70);
    const y = ground - rand(100,250);
    const w = rand(130,250);

    /*
      Não deixa plataformas ultrapassarem
      o limite do mundo.
    */
    if(x+w > game.world-150)
      continue;

    platforms.push({
      x,
      y,
      w,
      h:28
    });
  }

  /* MOEDAS */

  for(let i=0;i<35+difficulty*4;i++){

    const x = 250 + i*145 + rand(-30,30);

    if(x > game.finishX-100)
      continue;

    const y = ground - rand(70,240);

    coins.push({
      x,
      y,
      r:9,
      collected:false
    });
  }

  /* INIMIGOS */

  const enemyCount = 5 + difficulty*2;

  for(let i=0;i<enemyCount;i++){

    const x = 650 + i*480 + rand(-100,100);

    if(x > game.finishX-250)
      continue;

    enemies.push({
      x,
      y:ground-48,
      w:42,
      h:48,
      vx:0,
      vy:0,
      speed:.7+difficulty*.12,
      hp:difficulty>=5?2:1,
      maxHp:difficulty>=5?2:1,
      alive:true,
      type:"normal",
      direction:1
    });
  }

  /* TANQUES */

  if(difficulty >= 3){

    for(let i=0;i<difficulty-1;i++){

      const x = 1200+i*650;

      if(x > game.finishX-500)
        continue;

      enemies.push({
        x,
        y:ground-60,
        w:55,
        h:60,
        vx:0,
        vy:0,
        speed:.45+difficulty*.08,
        hp:3,
        maxHp:3,
        alive:true,
        type:"tank",
        direction:1
      });
    }
  }

  /* CHEFE FINAL */

  if(game.level === 10){

    enemies.push({
      x:4600,
      y:ground-110,
      w:90,
      h:110,
      vx:0,
      vy:0,
      speed:.9,
      hp:30,
      maxHp:30,
      alive:true,
      type:"boss",
      direction:1
    });
  }

  /* VIDA */

  powerups.push({
    x:900,
    y:ground-150,
    w:28,
    h:28,
    type:"health",
    collected:false
  });

  /* PODER */

  if(game.level >= 4){

    powerups.push({
      x:2800,
      y:ground-170,
      w:28,
      h:28,
      type:"power",
      collected:false
    });
  }

  /* OBJETIVO */

  goal = {
    x:game.finishX,
    y:ground-110,
    w:35,
    h:110
  };

  /* PLAYER */

  player.x = 150;
  player.y = ground-player.h-20;
  player.vx = 0;
  player.vy = 0;
  player.speed = 5;
  player.jump = 13;
  player.grounded = false;
  player.attackTimer = 0;
  player.invincible = 80;

  game.camera = 0;

  game.message = "FASE " + game.level;
  game.messageTime = 120;
}

/* =====================================================
   INÍCIO
===================================================== */

function startGame(){

  game.running = true;
  game.level = 1;
  game.score = 0;
  game.coins = 0;
  game.hp = 100;
  game.camera = 0;

  keys.left = false;
  keys.right = false;
  keys.jump = false;
  keys.attack = false;
  attackPressed = false;

  createLevel();

  document.getElementById("start").classList.add("hidden");
  document.getElementById("end").classList.add("hidden");

  updateHUD();
}

/* =====================================================
   PRÓXIMA FASE
===================================================== */

function nextLevel(){

  /*
    Impede a função de criar várias fases
    no mesmo frame.
  */
  if(game.levelChanging)
    return;

  game.levelChanging = true;

  if(game.level >= 10){
    winGame();
    return;
  }

  game.level++;

  /*
    Pequena recompensa entre fases.
  */
  game.hp = Math.min(
    game.maxHp,
    game.hp + 15
  );

  createLevel();

  game.message = "FASE " + game.level;
  game.messageTime = 150;

  updateHUD();
}

/* =====================================================
   PLAYER
===================================================== */

function updatePlayer(){

  /* MOVIMENTO */

  if(keys.left){
    player.vx -= .7;
    player.facing = -1;
  }

  if(keys.right){
    player.vx += .7;
    player.facing = 1;
  }

  if(!keys.left && !keys.right){
    player.vx *= .82;
  }

  if(Math.abs(player.vx) < .05)
    player.vx = 0;

  player.vx = clamp(
    player.vx,
    -player.speed,
    player.speed
  );

  /* PULO */

  if(keys.jump && player.grounded){

    player.vy = -player.jump;
    player.grounded = false;

    keys.jump = false;

    particlesBurst(
      player.x+player.w/2,
      player.y+player.h,
      "#fff",
      8
    );
  }

  /* GRAVIDADE */

  player.vy += .7;

  if(player.vy > 17)
    player.vy = 17;

  /* MOVIMENTO X */

  const oldX = player.x;

  player.x += player.vx;

  player.x = clamp(
    player.x,
    0,
    game.world-player.w
  );

  /* COLISÃO HORIZONTAL */

  for(const p of platforms){

    if(
      player.y+player.h <= p.y ||
      player.y >= p.y+p.h
    )
      continue;

    if(
      oldX+player.w <= p.x &&
      player.x+player.w > p.x
    ){
      player.x = p.x-player.w;
      player.vx = 0;
    }

    if(
      oldX >= p.x+p.w &&
      player.x < p.x+p.w
    ){
      player.x = p.x+p.w;
      player.vx = 0;
    }
  }

  /* MOVIMENTO Y */

  const oldY = player.y;

  player.y += player.vy;

  player.grounded = false;

  for(const p of platforms){

    if(!horizontalOverlap(player,p))
      continue;

    /* CAINDO */

    if(
      player.vy >= 0 &&
      oldY+player.h <= p.y &&
      player.y+player.h >= p.y
    ){

      player.y = p.y-player.h;
      player.vy = 0;
      player.grounded = true;
    }

    /* BATENDO A CABEÇA */

    if(
      player.vy < 0 &&
      oldY >= p.y+p.h &&
      player.y <= p.y+p.h
    ){

      player.y = p.y+p.h;
      player.vy = 0;
    }
  }

  /* QUEDA */

  if(player.y > H+250){

    damagePlayer(25);

    if(game.running){

      player.x = Math.max(
        100,
        player.x-300
      );

      player.y = H-250;
      player.vy = 0;
    }
  }

  /* ATAQUE */

  if(player.attackTimer > 0)
    player.attackTimer--;

  if(player.invincible > 0)
    player.invincible--;
}

/* =====================================================
   ATAQUE
===================================================== */

function attack(){

  if(!game.running)
    return;

  if(player.attackTimer > 0)
    return;

  player.attackTimer = 22;

  const hitbox = {
    x:
      player.facing > 0
        ? player.x+player.w-5
        : player.x-55,

    y:player.y+10,
    w:60,
    h:45
  };

  let hit = false;

  for(const e of enemies){

    if(!e.alive)
      continue;

    if(overlap(hitbox,e)){

      hit = true;

      e.hp--;

      game.score += 50;

      particlesBurst(
        e.x+e.w/2,
        e.y+e.h/2,
        "#ff5364",
        12
      );

      /*
        Recuo ao receber golpe.
      */
      e.x += player.facing * 12;

      if(e.hp <= 0){

        e.alive = false;

        if(e.type === "boss")
          game.score += 5000;

        else if(e.type === "tank")
          game.score += 500;

        else
          game.score += 100;

        game.coins +=
          e.type === "boss" ? 20 : 2;

        particlesBurst(
          e.x+e.w/2,
          e.y+e.h/2,
          e.type === "boss"
            ? "#ffd83d"
            : "#79ff78",
          20
        );
      }
    }
  }

  if(hit)
    game.shake = 5;
}

/* =====================================================
   INIMIGOS
===================================================== */

function updateEnemies(){

  for(const e of enemies){

    if(!e.alive)
      continue;

    /*
      Chefes e inimigos perseguem o jogador,
      mas com limite para não atravessarem o mundo.
    */
    const dx = player.x-e.x;

    if(Math.abs(dx) > 20)
      e.vx = Math.sign(dx)*e.speed;
    else
      e.vx = 0;

    const oldX = e.x;

    e.x += e.vx;

    e.x = clamp(
      e.x,
      0,
      game.world-e.w
    );

    /* COLISÃO HORIZONTAL */

    for(const p of platforms){

      if(
        e.y+e.h <= p.y ||
        e.y >= p.y+p.h
      )
        continue;

      if(
        oldX+e.w <= p.x &&
        e.x+e.w > p.x
      ){
        e.x = p.x-e.w;
        e.vx = 0;
      }

      if(
        oldX >= p.x+p.w &&
        e.x < p.x+p.w
      ){
        e.x = p.x+p.w;
        e.vx = 0;
      }
    }

    /* GRAVIDADE */

    const oldY = e.y;

    e.vy += .7;

    if(e.vy > 17)
      e.vy = 17;

    e.y += e.vy;

    /* COLISÃO VERTICAL */

    for(const p of platforms){

      if(!horizontalOverlap(e,p))
        continue;

      if(
        e.vy >= 0 &&
        oldY+e.h <= p.y &&
        e.y+e.h >= p.y
      ){

        e.y = p.y-e.h;
        e.vy = 0;
      }

      if(
        e.vy < 0 &&
        oldY >= p.y+p.h &&
        e.y <= p.y+p.h
      ){

        e.y = p.y+p.h;
        e.vy = 0;
      }
    }

    /* ATAQUE DO INIMIGO */

    if(overlap(player,e)){

      if(player.attackTimer <= 0){

        damagePlayer(
          e.type === "boss"
            ? 20
            : e.type === "tank"
              ? 15
              : 8
        );
      }
    }
  }

  /*
    Remove somente depois do processamento.
  */
  enemies = enemies.filter(e => e.alive);
}

/* =====================================================
   DANO
===================================================== */

function damagePlayer(amount){

  if(!game.running)
    return;

  if(player.invincible > 0)
    return;

  game.hp -= amount;

  game.hp = Math.max(
    0,
    game.hp
  );

  player.invincible = 70;
  game.shake = 12;

  particlesBurst(
    player.x+player.w/2,
    player.y+player.h/2,
    "#ff3045",
    15
  );

  if(game.hp <= 0){

    game.hp = 0;

    endGame(
      "DERROTA",
      "Você perdeu todas as suas vidas."
    );
  }
}

/* =====================================================
   MOEDAS
===================================================== */

function updateCoins(){

  const box = {
    x:player.x,
    y:player.y,
    w:player.w,
    h:player.h
  };

  for(const c of coins){

    if(c.collected)
      continue;

    const b = {
      x:c.x-c.r,
      y:c.y-c.r,
      w:c.r*2,
      h:c.r*2
    };

    if(overlap(box,b)){

      c.collected = true;

      game.coins++;
      game.score += 25;

      particlesBurst(
        c.x,
        c.y,
        "#ffd83d",
        8
      );
    }
  }

  coins = coins.filter(
    c => !c.collected
  );
}

/* =====================================================
   POWERUPS
===================================================== */

function updatePowerups(){

  for(const p of powerups){

    if(p.collected)
      continue;

    if(overlap(player,p)){

      p.collected = true;

      if(p.type === "health"){

        game.hp = Math.min(
          game.maxHp,
          game.hp+30
        );

        game.message = "+30 VIDA";
      }

      if(p.type === "power"){

        player.speed = 7;
        player.jump = 15;

        game.message = "SUPER PODER!";
      }

      game.messageTime = 90;
      game.score += 200;

      particlesBurst(
        p.x+p.w/2,
        p.y+p.h/2,
        p.type === "health"
          ? "#39e878"
          : "#b65cff",
        15
      );
    }
  }

  powerups = powerups.filter(
    p => !p.collected
  );
}

/* =====================================================
   OBJETIVO
===================================================== */

function checkGoal(){

  if(!goal)
    return;

  /*
    Só considera a chegada quando o jogador
    realmente atravessa a bandeira.
  */
  if(
    player.x+player.w < goal.x
  )
    return;

  const bossAlive = enemies.some(
    e => e.type === "boss" && e.alive
  );

  if(bossAlive){

    game.message = "DERROTE O CHEFE!";
    game.messageTime = 60;

    /*
      Recuo para impedir que o jogador
      fique preso dentro da bandeira.
    */
    player.x = goal.x-player.w-5;

    return;
  }

  nextLevel();
}

/* =====================================================
   PARTÍCULAS
===================================================== */

function particlesBurst(x,y,color,n){

  for(let i=0;i<n;i++){

    particles.push({
      x,
      y,
      vx:rand(-4,4),
      vy:rand(-5,1),
      life:1,
      color,
      size:rand(2,6)
    });
  }
}

function updateParticles(){

  for(let i=particles.length-1;i>=0;i--){

    const p = particles[i];

    p.x += p.vx;
    p.y += p.vy;
    p.vy += .15;
    p.life -= .03;

    if(p.life <= 0)
      particles.splice(i,1);
  }
}

/* =====================================================
   CÂMERA
===================================================== */

function updateCamera(){

  const target =
    player.x-W*.38;

  game.camera +=
    (target-game.camera)*.1;

  game.camera = clamp(
    game.camera,
    0,
    Math.max(
      0,
      game.world-W
    )
  );
}

/* =====================================================
   HUD
===================================================== */

function updateHUD(){

  hpEl.textContent =
    Math.ceil(Math.max(0,game.hp));

  coinsEl.textContent =
    game.coins;

  scoreEl.textContent =
    game.score;

  levelEl.textContent =
    game.level;
}

/* =====================================================
   FUNDO
===================================================== */

function drawBackground(){

  const hue =
    215-game.level*8;

  const g =
    ctx.createLinearGradient(
      0,0,0,H
    );

  g.addColorStop(
    0,
    `hsl(${hue},55%,27%)`
  );

  g.addColorStop(
    1,
    `hsl(${hue},45%,8%)`
  );

  ctx.fillStyle = g;
  ctx.fillRect(0,0,W,H);

  /* LUA */

  ctx.fillStyle = "#fff4c4";

  ctx.beginPath();

  ctx.arc(
    W-110,
    90,
    42,
    0,
    Math.PI*2
  );

  ctx.fill();

  /* MONTANHAS */

  ctx.fillStyle = "#10232f";

  for(let i=-1;i<10;i++){

    const x =
      i*500-game.camera*.15;

    ctx.beginPath();

    ctx.moveTo(x,H-90);
    ctx.lineTo(x+250,H-400);
    ctx.lineTo(x+500,H-90);

    ctx.fill();
  }
}

/* =====================================================
   PLATAFORMAS
===================================================== */

function drawPlatforms(){

  for(const p of platforms){

    const x =
      p.x-game.camera;

    if(x+p.w<0 || x>W)
      continue;

    ctx.fillStyle = "#493727";

    ctx.fillRect(
      x,
      p.y,
      p.w,
      p.h
    );

    ctx.fillStyle = "#55a948";

    ctx.fillRect(
      x,
      p.y,
      p.w,
      9
    );

    ctx.fillStyle = "#33271e";

    for(let i=0;i<p.w;i+=35){

      ctx.fillRect(
        x+i,
        p.y+16,
        18,
        4
      );
    }
  }
}

/* =====================================================
   MOEDAS
===================================================== */

function drawCoins(){

  for(const c of coins){

    const x =
      c.x-game.camera;

    if(x<-20 || x>W+20)
      continue;

    ctx.fillStyle = "#ffd52e";

    ctx.beginPath();

    ctx.arc(
      x,
      c.y,
      c.r,
      0,
      Math.PI*2
    );

    ctx.fill();

    ctx.strokeStyle = "#fff19a";
    ctx.lineWidth = 2;
    ctx.stroke();

    ctx.fillStyle = "#fff3a3";

    ctx.fillRect(
      x-2,
      c.y-6,
      4,
      12
    );
  }
}

/* =====================================================
   POWERUPS
===================================================== */

function drawPowerups(){

  for(const p of powerups){

    const x =
      p.x-game.camera;

    ctx.fillStyle =
      p.type === "health"
        ? "#39e878"
        : "#b65cff";

    ctx.fillRect(
      x,
      p.y,
      p.w,
      p.h
    );

    ctx.fillStyle = "#fff";
    ctx.font = "bold 19px Arial";
    ctx.textAlign = "center";

    ctx.fillText(
      p.type === "health"
        ? "❤"
        : "★",
      x+p.w/2,
      p.y+21
    );
  }
}

/* =====================================================
   INIMIGOS
===================================================== */

function drawEnemies(){

  for(const e of enemies){

    const x =
      e.x-game.camera;

    if(x<-100 || x>W+100)
      continue;

    ctx.save();

    ctx.translate(
      x+e.w/2,
      e.y+e.h/2
    );

    if(e.type === "boss")
      drawBoss(e);
    else
      drawEnemy(e);

    ctx.restore();
  }
}

function drawEnemy(e){

  const s =
    e.type === "tank"
      ? 1.15
      : 1;

  ctx.scale(s,s);

  ctx.fillStyle = "#0008";

  ctx.beginPath();

  ctx.ellipse(
    0,
    e.h/2+5,
    e.w*.45,
    6,
    0,
    0,
    Math.PI*2
  );

  ctx.fill();

  ctx.fillStyle =
    e.type === "tank"
      ? "#704a37"
      : "#3f8050";

  ctx.fillRect(
    -e.w/2,
    -e.h/2+10,
    e.w,
    e.h-10
  );

  ctx.fillStyle = "#79ad68";

  ctx.beginPath();

  ctx.arc(
    0,
    -e.h/2,
    e.w*.4,
    0,
    Math.PI*2
  );

  ctx.fill();

  ctx.fillStyle = "#ff3e4d";

  ctx.beginPath();

  ctx.arc(
    -7,
    -e.h/2-2,
    4,
    0,
    Math.PI*2
  );

  ctx.arc(
    7,
    -e.h/2-2,
    4,
    0,
    Math.PI*2
  );

  ctx.fill();

  if(e.maxHp > 1){

    ctx.fillStyle = "#240b0e";

    ctx.fillRect(
      -25,
      -45,
      50,
      5
    );

    ctx.fillStyle = "#ff394b";

    ctx.fillRect(
      -25,
      -45,
      50*(e.hp/e.maxHp),
      5
    );
  }
}

/* =====================================================
   CHEFE
===================================================== */

function drawBoss(e){

  ctx.fillStyle = "#250d16";

  ctx.fillRect(
    -45,
    -50,
    90,
    100
  );

  ctx.fillStyle = "#a52e42";

  ctx.beginPath();

  ctx.arc(
    0,
    -52,
    42,
    0,
    Math.PI*2
  );

  ctx.fill();

  ctx.fillStyle = "#ffd52e";

  ctx.beginPath();

  ctx.arc(
    -14,
    -55,
    7,
    0,
    Math.PI*2
  );

  ctx.arc(
    14,
    -55,
    7,
    0,
    Math.PI*2
  );

  ctx.fill();

  ctx.fillStyle = "#111";

  ctx.fillRect(
    -23,
    -32,
    46,
    12
  );

  ctx.fillStyle = "#fff";

  for(let i=-18;i<=18;i+=9){

    ctx.fillRect(
      i,
      -31,
      5,
      8
    );
  }

  /* COROA */

  ctx.fillStyle = "#ffd329";

  ctx.beginPath();

  ctx.moveTo(-32,-88);
  ctx.lineTo(-22,-110);
  ctx.lineTo(-8,-92);
  ctx.lineTo(5,-112);
  ctx.lineTo(18,-91);
  ctx.lineTo(32,-106);
  ctx.lineTo(36,-80);
  ctx.lineTo(-36,-80);

  ctx.closePath();
  ctx.fill();

  /* VIDA */

  ctx.fillStyle = "#22060b";

  ctx.fillRect(
    -50,
    -125,
    100,
    9
  );

  ctx.fillStyle = "#ff354b";

  ctx.fillRect(
    -50,
    -125,
    100*Math.max(
      0,
      e.hp/e.maxHp
    ),
    9
  );
}

/* =====================================================
   PLAYER
===================================================== */

function drawPlayer(){

  /*
    Piscada de invulnerabilidade.
  */
  if(
    player.invincible>0 &&
    Math.floor(player.invincible/5)%2===0
  )
    return;

  const x =
    player.x-game.camera;

  const y =
    player.y;

  ctx.save();

  ctx.translate(
    x+player.w/2,
    y+player.h/2
  );

  ctx.scale(
    player.facing,
    1
  );

  /* SOMBRA */

  ctx.fillStyle = "#0008";

  ctx.beginPath();

  ctx.ellipse(
    0,
    35,
    24,
    6,
    0,
    0,
    Math.PI*2
  );

  ctx.fill();

  /* PERNAS */

  ctx.fillStyle = "#243a55";

  ctx.fillRect(
    -15,
    12,
    11,
    22
  );

  ctx.fillRect(
    4,
    12,
    11,
    22
  );

  /* BOTAS */

  ctx.fillStyle = "#111820";

  ctx.fillRect(
    -19,
    28,
    20,
    9
  );

  ctx.fillRect(
    0,
    28,
    20,
    9
  );

  /* CORPO */

  ctx.fillStyle = "#2879ad";

  ctx.fillRect(
    -21,
    -7,
    42,
    32
  );

  ctx.strokeStyle = "#0e3148";
  ctx.lineWidth = 3;

  ctx.strokeRect(
    -21,
    -7,
    42,
    32
  );

  /* BRAÇO */

  ctx.strokeStyle = "#d59b6b";
  ctx.lineWidth = 9;
  ctx.lineCap = "round";

  ctx.beginPath();

  ctx.moveTo(15,-1);
  ctx.lineTo(27,8);

  ctx.stroke();

  /* CABEÇA */

  ctx.fillStyle = "#dfa576";

  ctx.beginPath();

  ctx.arc(
    0,
    -25,
    20,
    0,
    Math.PI*2
  );

  ctx.fill();

  /* CABELO */

  ctx.fillStyle = "#16283a";

  ctx.beginPath();

  ctx.arc(
    -10,
    -39,
    12,
    0,
    Math.PI*2
  );

  ctx.arc(
    2,
    -42,
    13,
    0,
    Math.PI*2
  );

  ctx.arc(
    13,
    -37,
    10,
    0,
    Math.PI*2
  );

  ctx.fill();

  /* OLHOS */

  ctx.fillStyle = "#111";

  ctx.beginPath();

  ctx.arc(
    -7,
    -24,
    3,
    0,
    Math.PI*2
  );

  ctx.arc(
    7,
    -24,
    3,
    0,
    Math.PI*2
  );

  ctx.fill();

  /* ESPADA */

  ctx.strokeStyle = "#dcecff";
  ctx.lineWidth = 6;

  ctx.beginPath();

  if(player.attackTimer > 0){

    ctx.moveTo(18,3);
    ctx.lineTo(60,-30);

  }else{

    ctx.moveTo(18,5);
    ctx.lineTo(48,-13);
  }

  ctx.stroke();

  ctx.restore();
}

/* =====================================================
   BANDEIRA
===================================================== */

function drawGoal(){

  if(!goal)
    return;

  const x =
    goal.x-game.camera;

  ctx.fillStyle = "#ddd";

  ctx.fillRect(
    x,
    goal.y,
    6,
    goal.h
  );

  ctx.fillStyle = "#ff4355";

  ctx.beginPath();

  ctx.moveTo(
    x+6,
    goal.y
  );

  ctx.lineTo(
    x+55,
    goal.y+20
  );

  ctx.lineTo(
    x+6,
    goal.y+40
  );

  ctx.closePath();
  ctx.fill();
}

/* =====================================================
   PARTÍCULAS
===================================================== */

function drawParticles(){

  for(const p of particles){

    ctx.globalAlpha = p.life;
    ctx.fillStyle = p.color;

    ctx.beginPath();

    ctx.arc(
      p.x-game.camera,
      p.y,
      p.size,
      0,
      Math.PI*2
    );

    ctx.fill();
  }

  ctx.globalAlpha = 1;
}

/* =====================================================
   MENSAGEM
===================================================== */

function drawMessage(){

  if(game.messageTime <= 0)
    return;

  ctx.textAlign = "center";
  ctx.font = "900 38px Arial";
  ctx.fillStyle = "#fff";
  ctx.shadowColor = "#000";
  ctx.shadowBlur = 10;

  ctx.fillText(
    game.message,
    W/2,
    H*.23
  );

  ctx.shadowBlur = 0;
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
  drawPlatforms();
  drawCoins();
  drawPowerups();
  drawGoal();
  drawEnemies();
  drawPlayer();
  drawParticles();
  drawMessage();

  /* VINHETA */

  const v =
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

  ctx.fillStyle = v;

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

    updatePlayer();

    /*
      Ataque agora ocorre por pressionamento.
    */
    if(attackPressed){

      attackPressed = false;
      attack();

    }

    updateEnemies();
    updateCoins();
    updatePowerups();
    updateParticles();
    updateCamera();
    checkGoal();

    if(game.messageTime > 0)
      game.messageTime--;

    updateHUD();

    if(game.shake > 0){

      game.shake *= .85;

      if(game.shake < .1)
        game.shake = 0;
    }
  }

  ctx.save();

  if(game.shake > 0){

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
   FIM
===================================================== */

function endGame(title,text){

  game.running = false;

  keys.left = false;
  keys.right = false;
  keys.jump = false;
  keys.attack = false;
  attackPressed = false;

  document.getElementById(
    "endTitle"
  ).textContent = title;

  document.getElementById(
    "endText"
  ).textContent =
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

  game.running = false;

  keys.left = false;
  keys.right = false;
  keys.jump = false;
  keys.attack = false;
  attackPressed = false;

  document.getElementById(
    "endTitle"
  ).textContent =
    "🏆 VOCÊ VENCEU!";

  document.getElementById(
    "endText"
  ).textContent =
    "Você completou as 10 fases e derrotou o chefe final! "+
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
).addEventListener(
  "click",
  startGame
);

document.getElementById(
  "restartBtn"
).addEventListener(
  "click",
  startGame
);

/* =====================================================
   EVITA MENU DE CONTEXTO NO CELULAR
===================================================== */

document.addEventListener(
  "contextmenu",
  e => e.preventDefault()
);

/* =====================================================
   INICIALIZAÇÃO
===================================================== */

resize();
createLevel();
updateHUD();

requestAnimationFrame(loop);

</script>

</body>
</html>