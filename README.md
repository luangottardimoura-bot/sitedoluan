<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport"
      content="width=device-width,initial-scale=1.0,user-scalable=no">

<title>Nico Zombie Assault - FPS</title>

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
    background:#05080a;
    font-family:Arial,Helvetica,sans-serif;
    touch-action:none;
    user-select:none;
}

canvas{
    position:fixed;
    inset:0;
    width:100%;
    height:100%;
    display:block;
}

#hud{
    position:fixed;
    z-index:20;
    top:15px;
    left:50%;
    transform:translateX(-50%);
    display:flex;
    gap:18px;
    padding:10px 18px;
    border-radius:15px;
    color:white;
    background:rgba(0,0,0,.68);
    border:1px solid rgba(255,255,255,.15);
    font-weight:bold;
    font-size:14px;
    backdrop-filter:blur(8px);
}

#health{
    color:#ff4f5f;
}

#ammo{
    color:#ffd84a;
}

#wave{
    color:#72d7ff;
}

#kills{
    color:#a8ff70;
}

#crosshair{
    position:fixed;
    z-index:15;
    left:50%;
    top:50%;
    width:24px;
    height:24px;
    transform:translate(-50%,-50%);
    pointer-events:none;
}

#crosshair:before,
#crosshair:after{
    content:"";
    position:absolute;
    background:white;
}

#crosshair:before{
    left:11px;
    top:0;
    width:2px;
    height:24px;
}

#crosshair:after{
    left:0;
    top:11px;
    width:24px;
    height:2px;
}

#weapon{
    position:fixed;
    z-index:10;
    left:50%;
    bottom:-25px;
    width:210px;
    height:330px;
    transform:translateX(-50%);
    pointer-events:none;
}

.weaponBody{
    position:absolute;
    left:55px;
    bottom:0;
    width:105px;
    height:250px;
    background:linear-gradient(
        90deg,
        #101418,
        #343b40,
        #111518
    );
    border-radius:20px 20px 8px 8px;
    box-shadow:
        0 0 30px rgba(0,0,0,.8),
        inset 0 0 15px rgba(255,255,255,.08);
    transform:rotate(-4deg);
}

.weaponBarrel{
    position:absolute;
    left:82px;
    bottom:205px;
    width:45px;
    height:120px;
    background:#111;
    border-radius:8px;
    transform:rotate(-4deg);
}

.weaponGrip{
    position:absolute;
    left:73px;
    bottom:10px;
    width:55px;
    height:120px;
    background:#171b1e;
    border-radius:10px;
    transform:rotate(10deg);
}

#muzzle{
    position:absolute;
    left:101px;
    bottom:312px;
    width:28px;
    height:28px;
    border-radius:50%;
    background:#ffe95a;
    box-shadow:
        0 0 15px #ffb300,
        0 0 40px #ff5500;
    display:none;
}

#message{
    position:fixed;
    z-index:30;
    left:50%;
    top:25%;
    transform:translateX(-50%);
    color:white;
    font-size:28px;
    font-weight:900;
    text-shadow:0 3px 10px #000;
    text-align:center;
    pointer-events:none;
}

#menu,
#gameOver{
    position:fixed;
    z-index:100;
    inset:0;
    display:flex;
    align-items:center;
    justify-content:center;
    background:
        radial-gradient(
            circle,
            rgba(30,55,65,.5),
            rgba(0,0,0,.95)
        );
}

.panel{
    width:min(700px,92vw);
    padding:40px 30px;
    text-align:center;
    color:white;
    border-radius:25px;
    background:
        linear-gradient(
            145deg,
            rgba(25,35,40,.98),
            rgba(5,10,14,.98)
        );
    border:1px solid rgba(255,255,255,.15);
    box-shadow:0 30px 100px #000;
}

h1{
    font-size:clamp(42px,8vw,75px);
    margin-bottom:5px;
    color:white;
    text-shadow:
        0 5px 0 #a51e2b,
        0 15px 40px #000;
}

h2{
    color:#ff5364;
    margin-bottom:20px;
}

p{
    color:#d8e0e5;
    line-height:1.6;
}

button{
    margin-top:25px;
    padding:15px 35px;
    border:0;
    border-radius:12px;
    background:linear-gradient(#ff5968,#c92538);
    color:white;
    font-weight:900;
    font-size:18px;
    cursor:pointer;
}

.hidden{
    display:none!important;
}

/* CONTROLES MOBILE */

#mobile{
    display:none;
    position:fixed;
    inset:0;
    z-index:40;
    pointer-events:none;
}

.touch{
    position:absolute;
    width:70px;
    height:70px;
    border-radius:50%;
    border:2px solid rgba(255,255,255,.4);
    background:rgba(0,0,0,.45);
    color:white;
    font-size:25px;
    pointer-events:auto;
}

#left{
    left:20px;
    bottom:25px;
}

#right{
    left:105px;
    bottom:25px;
}

#shoot{
    right:25px;
    bottom:35px;
    background:rgba(130,20,30,.65);
}

#jump{
    right:115px;
    bottom:35px;
}

#reload{
    right:120px;
    bottom:120px;
    width:55px;
    height:55px;
    font-size:20px;
}

@media(max-width:800px){
    #mobile{
        display:block;
    }

    #hud{
        top:8px;
        gap:8px;
        padding:8px 10px;
        font-size:11px;
    }

    #weapon{
        transform:translateX(-50%) scale(.75);
        bottom:-70px;
    }
}
</style>
</head>

<body>

<canvas id="game"></canvas>

<div id="hud">
    ❤️ <span id="health">100</span>
    🔫 <span id="ammo">12 / 60</span>
    🧟 <span id="kills">0</span>
    🌊 FASE <span id="wave">1</span>/10
</div>

<div id="crosshair"></div>

<div id="weapon">
    <div class="weaponBarrel"></div>
    <div class="weaponBody"></div>
    <div class="weaponGrip"></div>
    <div id="muzzle"></div>
</div>

<div id="message"></div>

<div id="menu">
    <div class="panel">

        <h1>NICO</h1>

        <h2>ZOMBIE ASSAULT FPS</h2>

        <p>
            Explore o mundo aberto, sobreviva às hordas
            e complete as 10 fases.
        </p>

        <p style="margin-top:15px">
            <b>PC:</b><br>
            WASD = andar<br>
            Mouse = olhar<br>
            Clique = atirar<br>
            R = recarregar
        </p>

        <button id="start">
            INICIAR MISSÃO
        </button>

    </div>
</div>

<div id="gameOver" class="hidden">
    <div class="panel">

        <h1 id="overTitle">
            FIM
        </h1>

        <p id="overText"></p>

        <button id="restart">
            JOGAR NOVAMENTE
        </button>

    </div>
</div>

<div id="mobile">

    <button class="touch" id="left">◀</button>
    <button class="touch" id="right">▶</button>

    <button class="touch" id="shoot">🔥</button>
    <button class="touch" id="jump">▲</button>
    <button class="touch" id="reload">↻</button>

</div>

<script>

"use strict";

/* =========================================================
   CANVAS
========================================================= */

const canvas =
    document.getElementById("game");

const ctx =
    canvas.getContext("2d");

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

/* =========================================================
   HUD
========================================================= */

const healthEl =
    document.getElementById("health");

const ammoEl =
    document.getElementById("ammo");

const killsEl =
    document.getElementById("kills");

const waveEl =
    document.getElementById("wave");

const message =
    document.getElementById("message");

const muzzle =
    document.getElementById("muzzle");

const menu =
    document.getElementById("menu");

const gameOver =
    document.getElementById("gameOver");

const overTitle =
    document.getElementById("overTitle");

const overText =
    document.getElementById("overText");

/* =========================================================
   CONFIG
========================================================= */

const MAP_SIZE = 4200;

const FOV = Math.PI / 3;

const MAX_DEPTH = 1000;

const PLAYER_SPEED = 3.2;

const TURN_SPEED = 0.0025;

const GRAVITY = 0.5;

const JUMP_FORCE = -9;

/* =========================================================
   ESTADO
========================================================= */

const state = {

    running:false,

    health:100,

    maxHealth:100,

    ammo:12,

    maxAmmo:12,

    reserve:60,

    kills:0,

    phase:1,

    score:0,

    time:0,

    spawnTimer:0,

    phaseDelay:0,

    recoil:0,

    shake:0

};

/* =========================================================
   PLAYER
========================================================= */

const player = {

    x:MAP_SIZE/2,

    y:MAP_SIZE/2,

    z:0,

    vx:0,

    vy:0,

    angle:0,

    grounded:true,

    cooldown:0,

    invincible:0

};

/* =========================================================
   CONTROLES
========================================================= */

const keys = {

    w:false,

    s:false,

    a:false,

    d:false,

    shoot:false

};

let mouseLocked = false;

document.addEventListener(
    "keydown",
    e => {

        const k =
            e.key.toLowerCase();

        if(k==="w") keys.w=true;
        if(k==="s") keys.s=true;
        if(k==="a") keys.a=true;
        if(k==="d") keys.d=true;

        if(k==="r") reload();

        if(k===" "){
            jump();
            e.preventDefault();
        }
    }
);

document.addEventListener(
    "keyup",
    e => {

        const k =
            e.key.toLowerCase();

        if(k==="w") keys.w=false;
        if(k==="s") keys.s=false;
        if(k==="a") keys.a=false;
        if(k==="d") keys.d=false;
    }
);

/* =========================================================
   MOUSE
========================================================= */

canvas.addEventListener(
    "click",
    () => {

        if(!state.running)
            return;

        if(document.pointerLockElement !== canvas){

            canvas.requestPointerLock();

        }else{

            shoot();
        }
    }
);

document.addEventListener(
    "pointerlockchange",
    () => {

        mouseLocked =
            document.pointerLockElement === canvas;

    }
);

document.addEventListener(
    "mousemove",
    e => {

        if(!state.running)
            return;

        if(mouseLocked){

            player.angle +=
                e.movementX *
                TURN_SPEED;

        }

    }
);

/* =========================================================
   TOUCH
========================================================= */

function buttonHold(id,property){

    const b =
        document.getElementById(id);

    b.addEventListener(
        "pointerdown",
        e => {

            e.preventDefault();

            keys[property]=true;

        }
    );

    b.addEventListener(
        "pointerup",
        e => {

            e.preventDefault();

            keys[property]=false;

        }
    );

    b.addEventListener(
        "pointercancel",
        () => {

            keys[property]=false;

        }
    );
}

buttonHold("left","a");
buttonHold("right","d");

document.getElementById("shoot")
.addEventListener(
    "pointerdown",
    e => {

        e.preventDefault();

        keys.shoot=true;

    }
);

document.getElementById("shoot")
.addEventListener(
    "pointerup",
    () => {

        keys.shoot=false;

    }
);

document.getElementById("shoot")
.addEventListener(
    "pointercancel",
    () => {

        keys.shoot=false;

    }
);

document.getElementById("jump")
.addEventListener(
    "pointerdown",
    e => {

        e.preventDefault();

        jump();

    }
);

document.getElementById("reload")
.addEventListener(
    "pointerdown",
    e => {

        e.preventDefault();

        reload();

    }
);

/* =========================================================
   MUNDO
========================================================= */

let obstacles = [];

let zombies = [];

let particles = [];

function createWorld(){

    obstacles=[];

    /*
       Cria construções
       e obstáculos espalhados.
    */

    for(let i=0;i<70;i++){

        const size =
            40 + Math.random()*100;

        const x =
            Math.random()*
            MAP_SIZE;

        const y =
            Math.random()*
            MAP_SIZE;

        if(
            Math.hypot(
                x-player.x,
                y-player.y
            ) < 300
        ){
            continue;
        }

        obstacles.push({

            x,

            y,

            w:size,

            h:size,

            type:
                Math.random()<.5
                ? "building"
                : "rock"

        });

    }

}

/* =========================================================
   COLISÃO
========================================================= */

function blocked(x,y){

    const r=18;

    if(
        x<r ||
        y<r ||
        x>MAP_SIZE-r ||
        y>MAP_SIZE-r
    ){
        return true;
    }

    for(const o of obstacles){

        if(
            x+r>o.x &&
            x-r<o.x+o.w &&
            y+r>o.y &&
            y-r<o.y+o.h
        ){
            return true;
        }

    }

    return false;
}

/* =========================================================
   ZUMBIS
========================================================= */

function createZombie(){

    const distance =
        500 +
        Math.random()*600;

    const angle =
        player.angle +
        (Math.random()-.5)*2.8;

    let x =
        player.x+
        Math.cos(angle)*distance;

    let y =
        player.y+
        Math.sin(angle)*distance;

    x=Math.max(50,Math.min(MAP_SIZE-50,x));
    y=Math.max(50,Math.min(MAP_SIZE-50,y));

    const phase=state.phase;

    const tankChance =
        phase>=5 ? .18 : .08;

    const fastChance =
        phase>=3 ? .25 : .12;

    let type="normal";

    if(Math.random()<tankChance)
        type="tank";
    else if(Math.random()<fastChance)
        type="fast";

    let hp=1;
    let speed=0.65;

    if(type==="fast"){

        hp=1+
            Math.floor(phase/5);

        speed=
            1.05+
            phase*.06;

    }

    else if(type==="tank"){

        hp=
            4+
            phase*2;

        speed=
            .35+
            phase*.025;

    }

    else{

        hp=
            1+
            Math.floor(phase/3);

        speed=
            .55+
            phase*.035;

    }

    zombies.push({

        x,

        y,

        hp,

        maxHp:hp,

        speed,

        type,

        attack:0,

        hitFlash:0,

        alive:true

    });

}

/* =========================================================
   DIFICULDADE
========================================================= */

function zombiesForPhase(){

    return 6+
        state.phase*4;
}

/* =========================================================
   FASE
========================================================= */

let phaseKills=0;

function startPhase(){

    phaseKills=0;

    zombies=[];

    state.spawnTimer=0;

    message.textContent =
        "FASE "+state.phase;

    setTimeout(
        () => {

            if(state.running)
                message.textContent="";

        },
        2000
    );

}

/* =========================================================
   TIRO
========================================================= */

function shoot(){

    if(!state.running)
        return;

    if(player.cooldown>0)
        return;

    if(state.ammo<=0){

        reload();

        return;
    }

    state.ammo--;

    player.cooldown=9;

    state.recoil=1;

    state.shake=4;

    muzzle.style.display="block";

    setTimeout(
        () => {
            muzzle.style.display="none";
        },
        45
    );

    /*
       Raycast simplificado:
       procura o zumbi mais próximo
       dentro do cone da mira.
    */

    let best=null;

    let bestDist=Infinity;

    for(const z of zombies){

        const dx=z.x-player.x;

        const dy=z.y-player.y;

        const dist=Math.hypot(dx,dy);

        const targetAngle=
            Math.atan2(dy,dx);

        let diff=
            targetAngle-player.angle;

        while(diff>Math.PI)
            diff-=Math.PI*2;

        while(diff<-Math.PI)
            diff+=Math.PI*2;

        if(
            Math.abs(diff)<.10 &&
            dist<800 &&
            dist<bestDist
        ){

            best=z;

            bestDist=dist;

        }

    }

    if(best){

        best.hp--;

        best.hitFlash=5;

        spawnParticles(
            best.x,
            best.y,
            "#d9313e",
            8
        );

        if(best.hp<=0){

            best.alive=false;

            state.kills++;

            phaseKills++;

            state.score +=
                best.type==="tank"
                ? 300
                : best.type==="fast"
                ? 180
                : 100;

        }

    }

}

/* =========================================================
   RELOAD
========================================================= */

function reload(){

    if(!state.running)
        return;

    if(
        state.ammo>=state.maxAmmo ||
        state.reserve<=0
    ){
        return;
    }

    const need =
        state.maxAmmo-state.ammo;

    const amount =
        Math.min(
            need,
            state.reserve
        );

    state.ammo+=amount;

    state.reserve-=amount;

}

/* =========================================================
   PULO
========================================================= */

function jump(){

    if(
        state.running &&
        player.grounded
    ){

        player.vy=JUMP_FORCE;

        player.grounded=false;

    }

}

/* =========================================================
   PLAYER UPDATE
========================================================= */

function updatePlayer(){

    let dx=0;
    let dy=0;

    const forwardX=
        Math.cos(player.angle);

    const forwardY=
        Math.sin(player.angle);

    const sideX=
        Math.cos(player.angle+Math.PI/2);

    const sideY=
        Math.sin(player.angle+Math.PI/2);

    if(keys.w){

        dx+=forwardX;
        dy+=forwardY;

    }

    if(keys.s){

        dx-=forwardX;
        dy-=forwardY;

    }

    if(keys.a){

        dx-=sideX;
        dy-=sideY;

    }

    if(keys.d){

        dx+=sideX;
        dy+=sideY;

    }

    const len=
        Math.hypot(dx,dy);

    if(len){

        dx/=len;
        dy/=len;

        const speed=
            PLAYER_SPEED;

        const nx=
            player.x+
            dx*speed;

        const ny=
            player.y+
            dy*speed;

        if(!blocked(nx,player.y))
            player.x=nx;

        if(!blocked(player.x,ny))
            player.y=ny;

    }

    /*
       Gravidade simples
    */

    player.vy+=GRAVITY;

    player.z+=player.vy;

    if(player.z>=0){

        player.z=0;

        player.vy=0;

        player.grounded=true;

    }

    if(player.cooldown>0)
        player.cooldown--;

    if(player.invincible>0)
        player.invincible--;

    if(keys.shoot)
        shoot();

    if(state.recoil>0)
        state.recoil*=.75;

}

/* =========================================================
   ZOMBIES UPDATE
========================================================= */

function updateZombies(){

    for(const z of zombies){

        if(!z.alive)
            continue;

        const dx=
            player.x-z.x;

        const dy=
            player.y-z.y;

        const dist=
            Math.hypot(dx,dy);

        if(dist>30){

            const nx=
                dx/dist;

            const ny=
                dy/dist;

            const tx=
                z.x+
                nx*z.speed;

            const ty=
                z.y+
                ny*z.speed;

            if(!blocked(tx,z.y))
                z.x=tx;

            if(!blocked(z.x,ty))
                z.y=ty;

        }

        if(z.attack>0)
            z.attack--;

        if(dist<42 && z.attack<=0){

            damagePlayer(
                z.type==="tank"
                ? 18
                : z.type==="fast"
                ? 7
                : 10
            );

            z.attack=45;

        }

        if(z.hitFlash>0)
            z.hitFlash--;

    }

    zombies=
        zombies.filter(z=>z.alive);

}

/* =========================================================
   DANO
========================================================= */

function damagePlayer(amount){

    if(
        player.invincible>0 ||
        !state.running
    )
        return;

    state.health-=amount;

    player.invincible=40;

    state.shake=10;

    if(state.health<=0){

        state.health=0;

        endGame(false);

    }

}

/* =========================================================
   FASE UPDATE
========================================================= */

function updatePhase(){

    const target=
        zombiesForPhase();

    if(
        phaseKills>=target &&
        zombies.length===0
    ){

        if(state.phase>=10){

            endGame(true);

            return;

        }

        state.phase++;

        state.score+=500;

        startPhase();

        return;

    }

    /*
       Quantidade de inimigos
       aumenta conforme a fase.
    */

    if(
        zombies.length<
        Math.min(
            4+state.phase*2,
            12
        ) &&
        phaseKills<target
    ){

        state.spawnTimer--;

        if(state.spawnTimer<=0){

            createZombie();

            state.spawnTimer=
                Math.max(
                    15,
                    75-state.phase*5
                );

        }

    }

}

/* =========================================================
   PARTICULAS
========================================================= */

function spawnParticles(
    x,
    y,
    color,
    amount
){

    for(let i=0;i<amount;i++){

        particles.push({

            x,

            y,

            vx:(Math.random()-.5)*5,

            vy:(Math.random()-.5)*5,

            life:1,

            color

        });

    }

}

function updateParticles(){

    for(let i=particles.length-1;i>=0;i--){

        const p=particles[i];

        p.x+=p.vx;
        p.y+=p.vy;

        p.vx*=.94;
        p.vy*=.94;

        p.life-=.03;

        if(p.life<=0)
            particles.splice(i,1);

    }

}

/* =========================================================
   PROJEÇÃO 3D
========================================================= */

function project(
    x,
    y
){

    const dx=
        x-player.x;

    const dy=
        y-player.y;

    const dist=
        Math.hypot(dx,dy);

    const angle=
        Math.atan2(dy,dx);

    let relative=
        angle-player.angle;

    while(relative>Math.PI)
        relative-=Math.PI*2;

    while(relative<-Math.PI)
        relative+=Math.PI*2;

    return {

        dist,

        relative

    };

}

/* =========================================================
   CÉU
========================================================= */

function drawSky(){

    const horizon=
        H*.52+
        player.z*1.5+
        state.recoil*5;

    const sky=
        ctx.createLinearGradient(
            0,0,
            0,horizon
        );

    sky.addColorStop(
        0,
        "#07111b"
    );

    sky.addColorStop(
        .55,
        "#183746"
    );

    sky.addColorStop(
        1,
        "#45605d"
    );

    ctx.fillStyle=sky;

    ctx.fillRect(
        0,
        0,
        W,
        H
    );

    /*
       Lua
    */

    ctx.fillStyle=
        "rgba(235,240,220,.9)";

    ctx.beginPath();

    ctx.arc(
        W*.78,
        H*.20,
        45,
        0,
        Math.PI*2
    );

    ctx.fill();

}

/* =========================================================
   CHÃO
========================================================= */

function drawGround(){

    const horizon=
        H*.52+
        player.z;

    const ground=
        ctx.createLinearGradient(
            0,
            horizon,
            0,
            H
        );

    ground.addColorStop(
        0,
        "#33483c"
    );

    ground.addColorStop(
        1,
        "#101814"
    );

    ctx.fillStyle=ground;

    ctx.fillRect(
        0,
        horizon,
        W,
        H-horizon
    );

    /*
       Linhas de perspectiva
    */

    ctx.strokeStyle=
        "rgba(130,150,130,.12)";

    ctx.lineWidth=1;

    for(let i=1;i<15;i++){

        const y=
            horizon+
            Math.pow(i/15,2)*
            (H-horizon);

        ctx.beginPath();

        ctx.moveTo(0,y);

        ctx.lineTo(W,y);

        ctx.stroke();

    }

}

/* =========================================================
   OBJETOS DO MUNDO
========================================================= */

function drawWorld(){

    const visible=[];

    for(const o of obstacles){

        const p=
            project(
                o.x+o.w/2,
                o.y+o.h/2
            );

        if(
            p.dist<MAX_DEPTH &&
            Math.abs(p.relative)<FOV
        ){

            visible.push({
                object:o,
                projection:p
            });

        }

    }

    visible.sort(
        (a,b)=>
            b.projection.dist-
            a.projection.dist
    );

    for(const item of visible){

        drawObject(
            item.object,
            item.projection
        );

    }

}

/* =========================================================
   DESENHAR OBJETO
========================================================= */

function drawObject(o,p){

    const depth=
        p.dist;

    const scale=
        700/depth;

    const sx=
        W/2+
        Math.tan(p.relative)*
        W/2;

    const width=
        o.w*scale*.75;

    const height=
        o.h*scale*.75;

    const horizon=
        H*.52+
        player.z;

    const bottom=
        horizon+
        2500/depth;

    const top=
        bottom-height;

    if(o.type==="building"){

        const grad=
            ctx.createLinearGradient(
                0,
                top,
                0,
                bottom
            );

        grad.addColorStop(
            0,
            "#566064"
        );

        grad.addColorStop(
            .5,
            "#30383b"
        );

        grad.addColorStop(
            1,
            "#131719"
        );

        ctx.fillStyle=grad;

        ctx.fillRect(
            sx-width/2,
            top,
            width,
            height
        );

        /*
           Janelas
        */

        ctx.fillStyle=
            "rgba(255,210,100,.25)";

        const cols=3;

        const rows=3;

        for(let x=0;x<cols;x++){

            for(let y=0;y<rows;y++){

                ctx.fillRect(
                    sx-width/2+
                    width*.15+
                    x*width*.28,

                    top+
                    height*.15+
                    y*height*.25,

                    width*.12,

                    height*.12
                );

            }

        }

    }else{

        ctx.fillStyle="#424a46";

        ctx.beginPath();

        ctx.ellipse(
            sx,
            bottom-height*.25,
            width*.5,
            height*.3,
            0,
            0,
            Math.PI*2
        );

        ctx.fill();

    }

}

/* =========================================================
   ZUMBIS 3D
========================================================= */

function drawZombies(){

    const visible=[];

    for(const z of zombies){

        const p=
            project(
                z.x,
                z.y
            );

        if(
            p.dist<MAX_DEPTH &&
            Math.abs(p.relative)<FOV*.95
        ){

            visible.push({
                z,
                p
            });

        }

    }

    visible.sort(
        (a,b)=>
            b.p.dist-a.p.dist
    );

    for(const item of visible){

        drawZombie(
            item.z,
            item.p
        );

    }

}

/* =========================================================
   ZUMBI
========================================================= */

function drawZombie(z,p){

    const scale=
        900/p.dist;

    const sx=
        W/2+
        Math.tan(p.relative)*
        W/2;

    const base=
        H*.52+
        2500/p.dist;

    const height=
        90*scale;

    const width=
        45*scale;

    if(height<5)
        return;

    const top=
        base-height;

    /*
       Sombra
    */

    ctx.fillStyle=
        "rgba(0,0,0,.35)";

    ctx.beginPath();

    ctx.ellipse(
        sx,
        base,
        width*.65,
        height*.12,
        0,
        0,
        Math.PI*2
    );

    ctx.fill();

    /*
       Corpo
    */

    ctx.fillStyle=
        z.hitFlash>0
        ? "#ffdddd"
        : z.type==="tank"
        ? "#445d42"
        : z.type==="fast"
        ? "#66834d"
        : "#57744d";

    ctx.fillRect(
        sx-width*.35,
        top+height*.35,
        width*.7,
        height*.55
    );

    /*
       Cabeça
    */

    ctx.fillStyle=
        z.hitFlash>0
        ? "#fff"
        : "#76985e";

    ctx.beginPath();

    ctx.arc(
        sx,
        top+height*.25,
        width*.42,
        0,
        Math.PI*2
    );

    ctx.fill();

    /*
       Olhos
    */

    ctx.fillStyle="#ff3344";

    ctx.beginPath();

    ctx.arc(
        sx-width*.14,
        top+height*.23,
        Math.max(2,width*.07),
        0,
        Math.PI*2
    );

    ctx.arc(
        sx+width*.14,
        top+height*.23,
        Math.max(2,width*.07),
        0,
        Math.PI*2
    );

    ctx.fill();

    /*
       Barra de vida
    */

    if(z.type==="tank"){

        ctx.fillStyle="#190b0b";

        ctx.fillRect(
            sx-width/2,
            top-8,
            width,
            5
        );

        ctx.fillStyle="#e83c4e";

        ctx.fillRect(
            sx-width/2,
            top-8,
            width*
            (z.hp/z.maxHp),
            5
        );

    }

}

/* =========================================================
   VIGNETTE
========================================================= */

function drawVignette(){

    const g=
        ctx.createRadialGradient(
            W/2,
            H/2,
            H*.20,
            W/2,
            H/2,
            H*.75
        );

    g.addColorStop(
        0,
        "rgba(0,0,0,0)"
    );

    g.addColorStop(
        1,
        "rgba(0,0,0,.75)"
    );

    ctx.fillStyle=g;

    ctx.fillRect(
        0,
        0,
        W,
        H
    );

}

/* =========================================================
   RENDER
========================================================= */

function render(){

    ctx.clearRect(
        0,
        0,
        W,
        H
    );

    /*
       Recoil
    */

    ctx.save();

    if(state.shake>0){

        ctx.translate(
            (Math.random()-.5)*
            state.shake,

            (Math.random()-.5)*
            state.shake
        );

    }

    drawSky();

    drawGround();

    drawWorld();

    drawZombies();

    drawVignette();

    ctx.restore();

}

/* =========================================================
   HUD
========================================================= */

function updateHUD(){

    healthEl.textContent=
        Math.max(
            0,
            Math.ceil(state.health)
        );

    ammoEl.textContent=
        state.ammo+
        " / "+
        state.reserve;

    killsEl.textContent=
        state.kills;

    waveEl.textContent=
        state.phase;

}

/* =========================================================
   GAME OVER
========================================================= */

function endGame(win){

    state.running=false;

    if(document.pointerLockElement)
        document.exitPointerLock();

    gameOver.classList.remove(
        "hidden"
    );

    if(win){

        overTitle.textContent=
            "🏆 MISSÃO COMPLETA";

        overText.textContent=
            "Nico conseguiu sobreviver às 10 fases! "+
            "Pontuação: "+
            state.score+
            " | Zumbis derrotados: "+
            state.kills;

    }else{

        overTitle.textContent=
            "💀 NICO FOI DERROTADO";

        overText.textContent=
            "Você chegou à fase "+
            state.phase+
            ". Pontuação: "+
            state.score+
            " | Zumbis derrotados: "+
            state.kills;

    }

}

/* =========================================================
   RESET
========================================================= */

function resetGame(){

    state.running=true;

    state.health=100;

    state.ammo=12;

    state.reserve=60;

    state.kills=0;

    state.score=0;

    state.phase=1;

    state.spawnTimer=0;

    state.recoil=0;

    state.shake=0;

    player.x=MAP_SIZE/2;

    player.y=MAP_SIZE/2;

    player.z=0;

    player.vy=0;

    player.angle=0;

    player.grounded=true;

    player.cooldown=0;

    player.invincible=0;

    zombies=[];

    particles=[];

    createWorld();

    startPhase();

    menu.classList.add(
        "hidden"
    );

    gameOver.classList.add(
        "hidden"
    );

    updateHUD();

}

/* =========================================================
   UPDATE
========================================================= */

function update(){

    if(!state.running)
        return;

    state.time+=.016;

    updatePlayer();

    updateZombies();

    updatePhase();

    updateParticles();

    if(state.shake>0){

        state.shake*=.85;

        if(state.shake<.1)
            state.shake=0;

    }

    updateHUD();

}

/* =========================================================
   LOOP
========================================================= */

function loop(){

    update();

    render();

    requestAnimationFrame(loop);

}

/* =========================================================
   BOTÕES
========================================================= */

document
.getElementById("start")
.addEventListener(
    "click",
    () => {

        resetGame();

    }
);

document
.getElementById("restart")
.addEventListener(
    "click",
    () => {

        resetGame();

    }
);

/* =========================================================
   START
========================================================= */

createWorld();

updateHUD();

requestAnimationFrame(loop);

</script>

</body>
</html>