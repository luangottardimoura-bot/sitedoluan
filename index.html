<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport"
      content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no">

<title>Nico: Zombie Assault</title>

<style>
*{
    box-sizing:border-box;
    margin:0;
    padding:0;
}

html,
body{
    width:100%;
    height:100%;
    overflow:hidden;
    background:#080d12;
    font-family:Arial,Helvetica,sans-serif;
    touch-action:none;
}

body{
    user-select:none;
    -webkit-user-select:none;
}

button{
    -webkit-tap-highlight-color:transparent;
    touch-action:none;
}

#game{
    position:relative;
    width:100vw;
    height:100vh;
    overflow:hidden;
}

canvas{
    position:absolute;
    inset:0;
    width:100%;
    height:100%;
    display:block;
}

/* =========================
   HUD
========================= */

#hud{
    position:fixed;
    z-index:20;
    top:14px;
    left:50%;
    transform:translateX(-50%);
    display:flex;
    gap:14px;
    align-items:center;
    padding:10px 16px;
    color:white;
    background:rgba(5,10,15,.82);
    border:1px solid rgba(255,255,255,.18);
    border-radius:16px;
    box-shadow:
        0 8px 30px rgba(0,0,0,.4),
        inset 0 1px rgba(255,255,255,.08);
    backdrop-filter:blur(8px);
    -webkit-backdrop-filter:blur(8px);
    font-size:14px;
    font-weight:900;
    white-space:nowrap;
}

.hudItem{
    display:flex;
    align-items:center;
    gap:5px;
}

#ammo{color:#ffd84a}
#health{color:#ff5364}
#score{color:#63d8ff}
#wave{color:#a8ff70}
#kills{color:#e8edf0}

/* =========================
   TELAS
========================= */

.screen{
    position:fixed;
    inset:0;
    z-index:100;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:20px;
    background:
        radial-gradient(
            circle at center,
            rgba(30,65,80,.35),
            rgba(2,6,10,.94)
        );
}

.panel{
    width:min(700px,94vw);
    max-height:94vh;
    overflow:auto;
    padding:38px 28px;
    text-align:center;
    color:white;
    background:
        linear-gradient(
            145deg,
            rgba(24,38,48,.98),
            rgba(8,14,20,.98)
        );
    border:2px solid rgba(255,255,255,.12);
    border-radius:28px;
    box-shadow:
        0 30px 100px rgba(0,0,0,.65),
        inset 0 1px rgba(255,255,255,.08);
}

.logo{
    font-size:clamp(38px,8vw,72px);
    font-weight:1000;
    letter-spacing:-3px;
    color:#fff;
    text-shadow:
        0 4px 0 #b3202d,
        0 10px 30px rgba(0,0,0,.6);
}

.subtitle{
    margin-top:4px;
    margin-bottom:22px;
    color:#ff5b69;
    font-size:20px;
    font-weight:900;
    text-transform:uppercase;
}

.panel p{
    color:#d3dce2;
    line-height:1.65;
    margin-top:10px;
}

.instructions{
    margin:20px auto 0;
    padding:15px;
    max-width:550px;
    background:rgba(255,255,255,.05);
    border:1px solid rgba(255,255,255,.08);
    border-radius:15px;
    color:#e7edf1;
    line-height:1.5;
}

.gameButton{
    margin-top:26px;
    padding:15px 34px;
    border:0;
    border-radius:13px;
    background:
        linear-gradient(
            #ff5b69,
            #c92838
        );
    color:white;
    font-size:19px;
    font-weight:1000;
    cursor:pointer;
    box-shadow:
        0 5px 0 #761b26,
        0 12px 30px rgba(0,0,0,.35);
    transition:.12s;
}

.gameButton:hover{
    filter:brightness(1.1);
    transform:translateY(-2px);
}

.gameButton:active{
    transform:translateY(4px);
    box-shadow:0 1px 0 #761b26;
}

.hidden{
    display:none!important;
}

/* =========================
   CONTROLES MOBILE
========================= */

#mobileControls{
    position:fixed;
    z-index:30;
    inset:auto 0 15px 0;
    display:none;
    justify-content:space-between;
    align-items:flex-end;
    padding:0 16px;
    pointer-events:none;
}

.controlGroup{
    display:flex;
    gap:10px;
}

.mobileButton{
    width:66px;
    height:66px;
    border-radius:50%;
    border:2px solid rgba(255,255,255,.6);
    background:rgba(5,12,18,.68);
    color:white;
    font-size:25px;
    font-weight:1000;
    pointer-events:auto;
    box-shadow:0 7px 20px rgba(0,0,0,.35);
}

.mobileButton:active{
    transform:scale(.9);
    background:rgba(255,70,80,.35);
}

#fireButton{
    position:fixed;
    z-index:31;
    right:22px;
    bottom:94px;
    width:72px;
    height:72px;
    border-radius:50%;
    border:2px solid rgba(255,100,100,.8);
    background:rgba(110,15,25,.78);
    color:white;
    font-size:28px;
    font-weight:bold;
    display:none;
    pointer-events:auto;
}

#reloadButton{
    position:fixed;
    z-index:31;
    right:22px;
    bottom:178px;
    width:58px;
    height:58px;
    border-radius:50%;
    border:2px solid rgba(255,255,255,.55);
    background:rgba(5,12,18,.75);
    color:#ffd84a;
    font-size:23px;
    font-weight:bold;
    display:none;
    pointer-events:auto;
}

#mobileHint{
    position:fixed;
    z-index:25;
    left:50%;
    bottom:12px;
    transform:translateX(-50%);
    color:rgba(255,255,255,.6);
    font-size:10px;
    font-weight:bold;
    pointer-events:none;
    white-space:nowrap;
}

@media(max-width:800px){

    #mobileControls{
        display:flex;
    }

    #fireButton,
    #reloadButton{
        display:block;
    }

    #hud{
        top:8px;
        gap:7px;
        padding:8px 10px;
        font-size:11px;
        max-width:96vw;
        overflow:hidden;
    }

    .panel{
        padding:28px 18px;
        border-radius:22px;
    }

    .instructions{
        font-size:13px;
    }

    #mobileHint{
        display:block;
    }
}

@media(min-width:801px){
    #mobileHint{
        display:none;
    }
}
</style>
</head>

<body>

<div id="game">

<canvas id="canvas"></canvas>

<div id="hud">

    <div class="hudItem">
        ❤️ <span id="health">100</span>
    </div>

    <div class="hudItem">
        🔫 <span id="ammo">12 / 60</span>
    </div>

    <div class="hudItem">
        🧟 <span id="kills">0</span>
    </div>

    <div class="hudItem">
        ⭐ <span id="score">0</span>
    </div>

    <div class="hudItem">
        🌊 <span id="wave">1</span>
    </div>

</div>

<!-- =========================
     TELA INICIAL
========================= -->

<div id="startScreen" class="screen">

    <div class="panel">

        <div class="logo">NICO</div>

        <div class="subtitle">
            Zombie Assault
        </div>

        <p>
            Uma aventura perigosa começou.
            Sobreviva às ondas de zumbis,
            proteja Nico e elimine todos os inimigos.
        </p>

        <div class="instructions">

            <b>🖥️ PC</b><br>

            A / D ou ← / → = mover<br>
            W / ↑ / Espaço = pular<br>
            Mouse = mirar e atirar<br>
            R = recarregar

            <br><br>

            <b>📱 Celular</b><br>

            ◀ ▶ = andar<br>
            ▲ = pular<br>
            🔥 = atirar automaticamente no zumbi mais próximo<br>
            ↻ = recarregar

        </div>

        <button
            id="startButton"
            class="gameButton"
            type="button"
        >
            COMEÇAR MISSÃO
        </button>

    </div>

</div>

<!-- =========================
     TELA FINAL
========================= -->

<div id="endScreen" class="screen hidden">

    <div class="panel">

        <div
            class="logo"
            id="endTitle"
        >
            MISSÃO CONCLUÍDA
        </div>

        <p id="endText">
            Nico conseguiu sobreviver!
        </p>

        <button
            id="restartButton"
            class="gameButton"
            type="button"
        >
            JOGAR NOVAMENTE
        </button>

    </div>

</div>

<!-- =========================
     CONTROLES MOBILE
========================= -->

<div id="mobileControls">

    <div class="controlGroup">

        <button
            class="mobileButton"
            id="leftButton"
            type="button"
        >
            ◀
        </button>

        <button
            class="mobileButton"
            id="rightButton"
            type="button"
        >
            ▶
        </button>

    </div>

    <button
        class="mobileButton"
        id="jumpButton"
        type="button"
    >
        ▲
    </button>

</div>

<button
    id="fireButton"
    type="button"
>
    🔥
</button>

<button
    id="reloadButton"
    type="button"
>
    ↻
</button>

<div id="mobileHint">
    🔥 mira automaticamente no inimigo mais próximo
</div>

</div>

<script>
"use strict";

/* =========================================================
   CANVAS
========================================================= */

const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

let W = window.innerWidth;
let H = window.innerHeight;
let dpr = 1;

function resize(){

    W = window.innerWidth;
    H = window.innerHeight;

    dpr = Math.min(
        window.devicePixelRatio || 1,
        2
    );

    canvas.width = Math.floor(W*dpr);
    canvas.height = Math.floor(H*dpr);

    canvas.style.width = W+"px";
    canvas.style.height = H+"px";

    ctx.setTransform(
        dpr,
        0,
        0,
        dpr,
        0,
        0
    );
}

resize();

/* =========================================================
   ELEMENTOS
========================================================= */

const healthEl =
    document.getElementById("health");

const ammoEl =
    document.getElementById("ammo");

const killsEl =
    document.getElementById("kills");

const scoreEl =
    document.getElementById("score");

const waveEl =
    document.getElementById("wave");

const startScreen =
    document.getElementById("startScreen");

const endScreen =
    document.getElementById("endScreen");

const startButton =
    document.getElementById("startButton");

const restartButton =
    document.getElementById("restartButton");

const endTitle =
    document.getElementById("endTitle");

const endText =
    document.getElementById("endText");

const fireButton =
    document.getElementById("fireButton");

const reloadButton =
    document.getElementById("reloadButton");

/* =========================================================
   CONFIGURAÇÃO
========================================================= */

const WORLD_WIDTH = 6000;

const GROUND_HEIGHT = 200;
const GROUND_OFFSET = 95;

const GRAVITY = .72;
const MOVE_SPEED = 4.8;
const JUMP = 13;

const BULLET_SPEED = 15;

const MAX_ZOMBIES = 18;

/* =========================================================
   ESTADO
========================================================= */

const state = {

    running:false,

    score:0,
    kills:0,

    wave:1,
    waveKills:0,

    health:100,
    maxHealth:100,

    ammo:12,
    maxAmmo:12,
    reserve:60,

    cameraX:0,

    shake:0,

    time:0,

    spawnTimer:0,
    waveTimer:0,

    gameWon:false

};

/* =========================================================
   OBJETOS DO MUNDO
========================================================= */

let platforms = [];
let zombies = [];
let bullets = [];
let particles = [];
let pickups = [];
let trees = [];
let rocks = [];
let blood = [];

/* =========================================================
   CONTROLES
========================================================= */

const keys = {

    left:false,
    right:false,
    up:false

};

const mouse = {

    x:W/2,
    y:H/2,
    down:false

};

let mobileFire = false;
let jumpLock = false;

/* =========================================================
   UTILIDADES
========================================================= */

function clamp(
    value,
    min,
    max
){

    return Math.max(
        min,
        Math.min(max,value)
    );
}

function random(
    min,
    max
){

    return Math.random() *
        (max-min) +
        min;
}

function rectsOverlap(a,b){

    return (
        a.x < b.x+b.w &&
        a.x+a.w > b.x &&
        a.y < b.y+b.h &&
        a.y+a.h > b.y
    );
}

function isMobile(){

    return window.matchMedia(
        "(max-width:800px)"
    ).matches;
}

function getGroundY(){

    return H-GROUND_OFFSET;
}

/* =========================================================
   SOM
========================================================= */

let audioContext = null;

function sound(type){

    try{

        if(!audioContext){

            audioContext =
                new (
                    window.AudioContext ||
                    window.webkitAudioContext
                )();

        }

        if(
            audioContext.state ===
            "suspended"
        ){

            audioContext.resume();

        }

        const osc =
            audioContext.createOscillator();

        const gain =
            audioContext.createGain();

        osc.connect(gain);
        gain.connect(
            audioContext.destination
        );

        const now =
            audioContext.currentTime;

        if(type === "shot"){

            osc.type = "square";

            osc.frequency.setValueAtTime(
                180,
                now
            );

            osc.frequency.exponentialRampToValueAtTime(
                70,
                now+.07
            );

            gain.gain.setValueAtTime(
                .06,
                now
            );

            gain.gain.exponentialRampToValueAtTime(
                .001,
                now+.08
            );

            osc.start(now);
            osc.stop(now+.08);
        }

        else if(type === "hit"){

            osc.type = "sawtooth";

            osc.frequency.setValueAtTime(
                100,
                now
            );

            osc.frequency.exponentialRampToValueAtTime(
                40,
                now+.12
            );

            gain.gain.setValueAtTime(
                .08,
                now
            );

            gain.gain.exponentialRampToValueAtTime(
                .001,
                now+.12
            );

            osc.start(now);
            osc.stop(now+.12);
        }

        else if(type === "reload"){

            osc.type = "sine";

            osc.frequency.setValueAtTime(
                400,
                now
            );

            osc.frequency.exponentialRampToValueAtTime(
                800,
                now+.16
            );

            gain.gain.setValueAtTime(
                .04,
                now
            );

            gain.gain.exponentialRampToValueAtTime(
                .001,
                now+.18
            );

            osc.start(now);
            osc.stop(now+.18);
        }

        else if(type === "jump"){

            osc.type = "sine";

            osc.frequency.setValueAtTime(
                220,
                now
            );

            osc.frequency.exponentialRampToValueAtTime(
                400,
                now+.1
            );

            gain.gain.setValueAtTime(
                .025,
                now
            );

            gain.gain.exponentialRampToValueAtTime(
                .001,
                now+.12
            );

            osc.start(now);
            osc.stop(now+.12);
        }

    }catch(_){}
}

/* =========================================================
   MUNDO
========================================================= */

function createWorld(){

    platforms = [];
    zombies = [];
    bullets = [];
    particles = [];
    pickups = [];
    trees = [];
    rocks = [];
    blood = [];

    const groundY =
        getGroundY();

    /* CHÃO */

    platforms.push({

        x:0,
        y:groundY,
        w:WORLD_WIDTH,
        h:GROUND_HEIGHT

    });

    /* PLATAFORMAS */

    const p = [

        [550,120,220],
        [950,200,180],
        [1300,130,250],
        [1720,220,210],
        [2100,130,300],
        [2550,230,230],
        [3000,140,280],
        [3450,240,220],
        [3900,150,300],
        [4400,230,220],
        [4800,130,280],
        [5250,210,250]

    ];

    for(const item of p){

        platforms.push({

            x:item[0],

            y:groundY-item[1],

            w:item[2],

            h:30

        });

    }

    /* ÁRVORES */

    for(let i=0;i<90;i++){

        trees.push({

            x:random(
                50,
                WORLD_WIDTH-50
            ),

            y:groundY,

            scale:random(
                .7,
                1.25
            )

        });

    }

    /* PEDRAS */

    for(let i=0;i<80;i++){

        rocks.push({

            x:random(
                20,
                WORLD_WIDTH-20
            ),

            y:
                groundY-
                random(2,9),

            size:random(5,14)

        });

    }

    /* PICKUPS */

    for(let i=0;i<20;i++){

        pickups.push({

            x:
                450+
                i*270+
                random(-50,50),

            y:
                groundY-30,

            type:
                i%3===0
                ? "ammo"
                : "medkit",

            collected:false

        });

    }

}

/* =========================================================
   AJUSTE AO REDIMENSIONAR
========================================================= */

function repositionWorldForResize(
    oldH,
    newH
){

    if(!platforms.length){
        return;
    }

    const delta =
        newH-oldH;

    for(const p of platforms){
        p.y += delta;
    }

    for(const t of trees){
        t.y += delta;
    }

    for(const r of rocks){
        r.y += delta;
    }

    for(const item of pickups){
        item.y += delta;
    }

    for(const z of zombies){
        z.y += delta;
    }

    for(const b of bullets){
        b.y += delta;
    }

    for(const p of particles){
        p.y += delta;
    }

    for(const b of blood){
        b.y += delta;
    }

    player.y += delta;

    player.y =
        Math.min(
            player.y,
            getGroundY()-player.h
        );
}

/* =========================================================
   RESIZE
========================================================= */

window.addEventListener(
    "resize",
    function(){

        const oldH = H;

        resize();

        if(state.running){

            repositionWorldForResize(
                oldH,
                H
            );

        }

    }
);

/* =========================================================
   RESET
========================================================= */

function resetGame(){

    state.running = true;

    state.score = 0;
    state.kills = 0;

    state.wave = 1;
    state.waveKills = 0;

    state.health = 100;

    state.ammo = 12;
    state.reserve = 60;

    state.cameraX = 0;

    state.shake = 0;

    state.time = 0;

    state.spawnTimer = 50;
    state.waveTimer = 0;

    state.gameWon = false;

    player.x = 250;

    player.y =
        getGroundY()-300;

    player.vx = 0;
    player.vy = 0;

    player.grounded = false;

    player.facing = 1;

    player.shootCooldown = 0;
    player.invincible = 0;
    player.walk = 0;

    mouse.x = W/2;
    mouse.y = H/2;
    mouse.down = false;

    mobileFire = false;
    jumpLock = false;

    keys.left = false;
    keys.right = false;
    keys.up = false;

    createWorld();

    startScreen.classList.add(
        "hidden"
    );

    endScreen.classList.add(
        "hidden"
    );

    updateHUD();
}

/* =========================================================
   HUD
========================================================= */

function updateHUD(){

    healthEl.textContent =
        Math.max(
            0,
            Math.ceil(state.health)
        );

    ammoEl.textContent =
        state.ammo+
        " / "+
        state.reserve;

    killsEl.textContent =
        state.kills;

    scoreEl.textContent =
        state.score;

    waveEl.textContent =
        state.wave;

}

/* =========================================================
   TECLADO
========================================================= */

document.addEventListener(
    "keydown",
    function(e){

        const k =
            e.key.toLowerCase();

        if(
            k==="a" ||
            k==="arrowleft"
        ){

            keys.left = true;
            e.preventDefault();

        }

        if(
            k==="d" ||
            k==="arrowright"
        ){

            keys.right = true;
            e.preventDefault();

        }

        if(
            k==="w" ||
            k==="arrowup" ||
            k===" "
        ){

            keys.up = true;
            e.preventDefault();

        }

        if(k==="r"){

            reload();

        }

    }
);

document.addEventListener(
    "keyup",
    function(e){

        const k =
            e.key.toLowerCase();

        if(
            k==="a" ||
            k==="arrowleft"
        ){

            keys.left = false;

        }

        if(
            k==="d" ||
            k==="arrowright"
        ){

            keys.right = false;

        }

        if(
            k==="w" ||
            k==="arrowup" ||
            k===" "
        ){

            keys.up = false;

        }

    }
);

/* =========================================================
   MOUSE
========================================================= */

canvas.addEventListener(
    "mousemove",
    function(e){

        mouse.x = e.clientX;
        mouse.y = e.clientY;

    }
);

canvas.addEventListener(
    "mousedown",
    function(e){

        if(e.button===0){

            mouse.down = true;

        }

    }
);

window.addEventListener(
    "mouseup",
    function(e){

        if(e.button===0){

            mouse.down = false;

        }

    }
);

/* =========================================================
   CONTROLES TOUCH
========================================================= */

function holdButton(
    id,
    property
){

    const button =
        document.getElementById(id);

    button.addEventListener(
        "pointerdown",
        function(e){

            e.preventDefault();

            keys[property] = true;

            try{

                button.setPointerCapture(
                    e.pointerId
                );

            }catch(_){}

        }
    );

    function release(e){

        if(e){
            e.preventDefault();
        }

        keys[property] = false;

    }

    button.addEventListener(
        "pointerup",
        release
    );

    button.addEventListener(
        "pointercancel",
        release
    );

    button.addEventListener(
        "lostpointercapture",
        release
    );
}

holdButton(
    "leftButton",
    "left"
);

holdButton(
    "rightButton",
    "right"
);

/* =========================
   PULO
========================= */

const jumpButton =
    document.getElementById(
        "jumpButton"
    );

jumpButton.addEventListener(
    "pointerdown",
    function(e){

        e.preventDefault();

        if(!jumpLock){

            keys.up = true;
            jumpLock = true;

        }

        try{

            jumpButton.setPointerCapture(
                e.pointerId
            );

        }catch(_){}

    }
);

function releaseJump(e){

    if(e){
        e.preventDefault();
    }

    keys.up = false;
    jumpLock = false;

}

jumpButton.addEventListener(
    "pointerup",
    releaseJump
);

jumpButton.addEventListener(
    "pointercancel",
    releaseJump
);

jumpButton.addEventListener(
    "lostpointercapture",
    releaseJump
);

/* =========================
   TIRO
========================= */

fireButton.addEventListener(
    "pointerdown",
    function(e){

        e.preventDefault();

        mobileFire = true;

        try{

            fireButton.setPointerCapture(
                e.pointerId
            );

        }catch(_){}

    }
);

function stopMobileFire(){

    mobileFire = false;

}

fireButton.addEventListener(
    "pointerup",
    stopMobileFire
);

fireButton.addEventListener(
    "pointercancel",
    stopMobileFire
);

fireButton.addEventListener(
    "lostpointercapture",
    stopMobileFire
);

/* =========================
   RECARGA
========================= */

reloadButton.addEventListener(
    "pointerdown",
    function(e){

        e.preventDefault();

        reload();

    }
);

/* =========================================================
   LIBERAR CONTROLES AO PERDER FOCO
========================================================= */

window.addEventListener(
    "blur",
    function(){

        mouse.down = false;
        mobileFire = false;

        keys.left = false;
        keys.right = false;
        keys.up = false;

        jumpLock = false;

    }
);

/* =========================================================
   JOGADOR
========================================================= */

const player = {

    x:250,
    y:200,

    w:42,
    h:64,

    vx:0,
    vy:0,

    grounded:false,

    facing:1,

    shootCooldown:0,

    invincible:0,

    walk:0

};

/* =========================================================
   COLISÃO VERTICAL DO JOGADOR
========================================================= */

function resolvePlayerVertical(
    oldY
){

    player.grounded = false;

    for(const p of platforms){

        if(
            player.x+player.w <= p.x ||
            player.x >= p.x+p.w
        ){
            continue;
        }

        /* CAINDO */

        if(
            player.vy>=0 &&
            oldY+player.h<=p.y &&
            player.y+player.h>=p.y
        ){

            player.y =
                p.y-player.h;

            player.vy = 0;
            player.grounded = true;

            return;

        }

        /* SUBINDO */

        if(
            player.vy<0 &&
            oldY>=p.y+p.h &&
            player.y<=p.y+p.h
        ){

            player.y =
                p.y+p.h;

            player.vy = 0;

        }

    }

}

/* =========================================================
   ATUALIZA JOGADOR
========================================================= */

function updatePlayer(){

    /* MOVIMENTO */

    if(keys.left){

        player.vx -= .65;
        player.facing = -1;

    }

    if(keys.right){

        player.vx += .65;
        player.facing = 1;

    }

    if(
        !keys.left &&
        !keys.right
    ){

        player.vx *= .82;

    }

    player.vx =
        clamp(
            player.vx,
            -MOVE_SPEED,
            MOVE_SPEED
        );

    /* PULO */

    if(
        keys.up &&
        player.grounded
    ){

        player.vy = -JUMP;
        player.grounded = false;

        spawnParticles(
            player.x+player.w/2,
            player.y+player.h,
            "#c5d1d8",
            7
        );

        sound("jump");

        keys.up = false;

    }

    /* GRAVIDADE */

    player.vy += GRAVITY;

    player.vy =
        Math.min(
            player.vy,
            17
        );

    /* =========================
       HORIZONTAL
    ========================= */

    const oldX =
        player.x;

    player.x += player.vx;

    player.x =
        clamp(
            player.x,
            0,
            WORLD_WIDTH-player.w
        );

    for(const p of platforms){

        if(
            player.y+player.h<=p.y ||
            player.y>=p.y+p.h
        ){
            continue;
        }

        if(
            oldX+player.w<=p.x &&
            player.x+player.w>p.x
        ){

            player.x =
                p.x-player.w;

            player.vx = 0;

        }

        else if(
            oldX>=p.x+p.w &&
            player.x<p.x+p.w
        ){

            player.x =
                p.x+p.w;

            player.vx = 0;

        }

    }

    /* =========================
       VERTICAL
    ========================= */

    const oldY =
        player.y;

    player.y += player.vy;

    resolvePlayerVertical(
        oldY
    );

    /* CAIU DO MAPA */

    if(
        player.y >
        H+250
    ){

        damagePlayer(25);

        if(state.running){

            player.x =
                clamp(
                    player.x-180,
                    50,
                    WORLD_WIDTH-player.w-50
                );

            player.y =
                getGroundY()-300;

            player.vy = 0;

        }

    }

    player.walk +=
        Math.abs(player.vx)*.18;

    if(player.invincible>0){
        player.invincible--;
    }

    if(player.shootCooldown>0){
        player.shootCooldown--;
    }

}

/* =========================================================
   MIRA DO MOUSE
========================================================= */

function getWorldMouse(){

    return {

        x:
            mouse.x+
            state.cameraX,

        y:
            mouse.y

    };

}

/* =========================================================
   MIRA AUTOMÁTICA MOBILE
========================================================= */

function getMobileTarget(){

    let closest = null;

    let closestDistance =
        Infinity;

    const px =
        player.x+
        player.w/2;

    const py =
        player.y+
        player.h/2;

    for(const z of zombies){

        if(!z.alive){
            continue;
        }

        const zx =
            z.x+
            z.w/2;

        const zy =
            z.y+
            z.h/2;

        const dx =
            zx-px;

        const dy =
            zy-py;

        const distance =
            Math.hypot(
                dx,
                dy
            );

        /*
         * Evita mirar em inimigos absurdamente
         * distantes do jogador.
         */

        if(
            distance<
            closestDistance &&
            distance<1100
        ){

            closest = z;
            closestDistance = distance;

        }

    }

    if(closest){

        return {

            x:
                closest.x+
                closest.w/2,

            y:
                closest.y+
                closest.h/2

        };

    }

    /* Sem inimigo: mira para frente */

    return {

        x:
            player.x+
            player.w/2+
            player.facing*600,

        y:
            player.y+30

    };

}

/* =========================================================
   TIRO
========================================================= */

function shoot(){

    if(!state.running){
        return;
    }

    if(player.shootCooldown>0){
        return;
    }

    if(state.ammo<=0){

        reload();
        return;

    }

    const target =
        mobileFire && isMobile()
        ? getMobileTarget()
        : getWorldMouse();

    const gunX =
        player.x+
        player.w/2;

    const gunY =
        player.y+29;

    let dx =
        target.x-gunX;

    let dy =
        target.y-gunY;

    const len =
        Math.hypot(
            dx,
            dy
        ) || 1;

    dx /= len;
    dy /= len;

    bullets.push({

        x:
            gunX+
            dx*27,

        y:
            gunY+
            dy*27,

        vx:
            dx*BULLET_SPEED,

        vy:
            dy*BULLET_SPEED,

        life:80,

        damage:1

    });

    state.ammo--;

    player.shootCooldown = 8;

    state.shake =
        Math.max(
            state.shake,
            2.5
        );

    spawnParticles(
        gunX+dx*27,
        gunY+dy*27,
        "#ffd84a",
        5
    );

    sound("shot");

}

/* =========================================================
   RECARREGAR
========================================================= */

function reload(){

    if(!state.running){
        return;
    }

    if(
        state.ammo>=state.maxAmmo ||
        state.reserve<=0
    ){

        return;

    }

    const needed =
        state.maxAmmo-
        state.ammo;

    const amount =
        Math.min(
            needed,
            state.reserve
        );

    state.ammo += amount;
    state.reserve -= amount;

    sound("reload");

    updateHUD();

}

/* =========================================================
   BALAS
========================================================= */

function updateBullets(){

    for(
        let i=bullets.length-1;
        i>=0;
        i--
    ){

        const b =
            bullets[i];

        b.x += b.vx;
        b.y += b.vy;

        b.life--;

        /* FORA DO MAPA */

        if(
            b.life<=0 ||
            b.x<-100 ||
            b.x>WORLD_WIDTH+100 ||
            b.y<-100 ||
            b.y>H+300
        ){

            bullets.splice(i,1);
            continue;

        }

        /* COLISÃO COM PLATAFORMAS */

        let wallHit = false;

        for(const p of platforms){

            if(
                b.x>=p.x &&
                b.x<=p.x+p.w &&
                b.y>=p.y &&
                b.y<=p.y+p.h
            ){

                wallHit = true;

                spawnParticles(
                    b.x,
                    b.y,
                    "#d4d4d4",
                    4
                );

                break;

            }

        }

        if(wallHit){

            bullets.splice(i,1);
            continue;

        }

        /* COLISÃO COM ZUMBIS */

        let hit = false;

        for(const z of zombies){

            if(!z.alive){
                continue;
            }

            const box = {

                x:z.x,
                y:z.y,
                w:z.w,
                h:z.h

            };

            if(
                b.x>=box.x &&
                b.x<=box.x+box.w &&
                b.y>=box.y &&
                b.y<=box.y+box.h
            ){

                damageZombie(
                    z,
                    b.damage
                );

                hit = true;

                break;

            }

        }

        if(hit){

            bullets.splice(i,1);

        }

    }

}

/* =========================================================
   SPAWN ZUMBI
========================================================= */

function spawnZombie(){

    /*
     * Nunca ultrapassa o limite absoluto.
     */

    if(
        zombies.length>=
        MAX_ZOMBIES
    ){

        return false;

    }

    /*
     * Procura uma posição fora da tela,
     * mas não exageradamente distante.
     */

    let x = player.x;

    let attempts = 0;

    while(
        attempts<20
    ){

        const side =
            Math.random()<.5
            ? -1
            : 1;

        const distance =
            random(
                500,
                900
            );

        x =
            player.x+
            side*distance;

        x =
            clamp(
                x,
                80,
                WORLD_WIDTH-100
            );

        if(
            Math.abs(
                x-player.x
            )>=350
        ){

            break;

        }

        attempts++;

    }

    const chance =
        Math.random();

    let type;

    if(chance<.18){

        type="fast";

    }
    else if(chance<.32){

        type="tank";

    }
    else{

        type="normal";

    }

    const groundY =
        getGroundY();

    const z = {

        x:x,

        y:
            groundY-60,

        w:44,
        h:58,

        vx:0,
        vy:0,

        alive:true,

        type:type,

        hp:1,
        maxHp:1,

        speed:1.15,

        damage:8,

        attackCooldown:0,

        anim:
            Math.random()*10,

        grounded:false

    };

    if(type==="fast"){

        z.w=36;
        z.h=50;

        z.hp=1;
        z.maxHp=1;

        z.speed=2.1;
        z.damage=6;

    }

    if(type==="tank"){

        z.w=58;
        z.h=70;

        z.hp=3;
        z.maxHp=3;

        z.speed=.65;
        z.damage=15;

    }

    z.y =
        groundY-z.h;

    zombies.push(z);

    return true;

}

/* =========================================================
   DANO NO ZUMBI
========================================================= */

function damageZombie(
    z,
    damage
){

    if(!z.alive){
        return;
    }

    z.hp -= damage;

    spawnParticles(
        z.x+z.w/2,
        z.y+z.h/2,
        "#ff5364",
        8
    );

    sound("hit");

    if(z.hp<=0){

        z.alive = false;

        state.kills++;
        state.waveKills++;

        if(z.type==="tank"){

            state.score += 500;

        }
        else if(z.type==="fast"){

            state.score += 200;

        }
        else{

            state.score += 100;

        }

        state.shake = 5;

        blood.push({

            x:
                z.x+
                z.w/2,

            y:
                z.y+
                z.h,

            life:1

        });

        spawnParticles(
            z.x+z.w/2,
            z.y+z.h/2,
            "#d52d3e",
            18
        );

        /*
         * Chance de dropar item.
         */

        if(
            Math.random()<.15
        ){

            pickups.push({

                x:
                    z.x+
                    z.w/2,

                y:
                    z.y+
                    z.h,

                type:
                    Math.random()<.5
                    ? "ammo"
                    : "medkit",

                collected:false

            });

        }

    }

}

/* =========================================================
   FÍSICA DOS ZUMBIS
========================================================= */

function updateZombies(){

    const pr = {

        x:player.x,
        y:player.y,
        w:player.w,
        h:player.h

    };

    for(const z of zombies){

        if(!z.alive){
            continue;
        }

        const dx =
            player.x-z.x;

        /*
         * Perseguição.
         */

        if(
            Math.abs(dx)>4
        ){

            z.vx =
                Math.sign(dx)*
                z.speed;

        }
        else{

            z.vx=0;

        }

        const oldX =
            z.x;

        z.x += z.vx;

        z.x =
            clamp(
                z.x,
                0,
                WORLD_WIDTH-z.w
            );

        /* COLISÃO HORIZONTAL */

        for(const p of platforms){

            if(
                z.y+z.h<=p.y ||
                z.y>=p.y+p.h
            ){

                continue;

            }

            if(
                oldX+z.w<=p.x &&
                z.x+z.w>p.x
            ){

                z.x =
                    p.x-z.w;

                z.vx=0;

            }

            else if(
                oldX>=p.x+p.w &&
                z.x<p.x+p.w
            ){

                z.x =
                    p.x+p.w;

                z.vx=0;

            }

        }

        /* GRAVIDADE */

        const oldY =
            z.y;

        z.vy += GRAVITY;

        z.vy =
            Math.min(
                z.vy,
                17
            );

        z.y += z.vy;

        z.grounded = false;

        for(const p of platforms){

            if(
                z.x+z.w<=p.x ||
                z.x>=p.x+p.w
            ){

                continue;

            }

            if(
                z.vy>=0 &&
                oldY+z.h<=p.y &&
                z.y+z.h>=p.y
            ){

                z.y =
                    p.y-z.h;

                z.vy=0;
                z.grounded=true;

                break;

            }

        }

        /* EVITA QUEDA INFINITA */

        if(
            z.y>H+300
        ){

            z.y =
                getGroundY()-z.h;

            z.vy=0;

        }

        z.anim += .1;

        if(
            z.attackCooldown>0
        ){

            z.attackCooldown--;

        }

        /* ATAQUE */

        if(
            rectsOverlap(
                pr,
                z
            )
        ){

            if(
                z.attackCooldown<=0
            ){

                damagePlayer(
                    z.damage
                );

                z.attackCooldown=55;

                if(state.running){

                    player.vx =
                        player.x<z.x
                        ? -5
                        : 5;

                    player.vy=-4;

                }

            }

        }

    }

    zombies =
        zombies.filter(
            z=>z.alive
        );

}

/* =========================================================
   DANO JOGADOR
========================================================= */

function damagePlayer(
    amount
){

    if(
        player.invincible>0 ||
        !state.running
    ){

        return;

    }

    state.health -= amount;

    player.invincible = 50;

    state.shake = 10;

    spawnParticles(
        player.x+player.w/2,
        player.y+player.h/2,
        "#ff3147",
        16
    );

    if(
        state.health<=0
    ){

        state.health=0;

        endGame(
            false,
            "Nico foi derrotado pelos zumbis."
        );

    }

}

/* =========================================================
   ONDAS
========================================================= */

function updateWaves(){

    /*
     * Quantidade total necessária
     * na onda atual.
     */

    const needed =
        5+
        state.wave*3;

    /*
     * A onda só termina quando:
     *
     * 1. todas as mortes necessárias
     * foram feitas;
     * 2. não existem zumbis vivos.
     */

    if(
        state.waveKills>=needed &&
        zombies.length===0
    ){

        if(
            state.wave>=5
        ){

            endGame(
                true,
                "Nico sobreviveu às 5 ondas de zumbis e completou a missão!"
            );

            return;

        }

        state.wave++;

        state.waveKills=0;

        state.waveTimer=150;

        state.spawnTimer=70;

        state.score += 500;

        return;

    }

    /*
     * Intervalo entre ondas.
     */

    if(
        state.waveTimer>0
    ){

        state.waveTimer--;

        return;

    }

    /*
     * NÃO cria inimigos além
     * do necessário para completar
     * a onda.
     */

    if(
        state.waveKills+
        zombies.length>=needed
    ){

        return;

    }

    state.spawnTimer--;

    if(
        state.spawnTimer<=0
    ){

        spawnZombie();

        state.spawnTimer =
            Math.max(
                30,
                100-
                state.wave*10
            );

    }

}

/* =========================================================
   PICKUPS
========================================================= */

function updatePickups(){

    const pr = {

        x:player.x,
        y:player.y,
        w:player.w,
        h:player.h

    };

    for(const item of pickups){

        if(item.collected){
            continue;
        }

        const box = {

            x:item.x-14,
            y:item.y-14,

            w:28,
            h:28

        };

        if(
            rectsOverlap(
                pr,
                box
            )
        ){

            item.collected=true;

            if(
                item.type==="ammo"
            ){

                state.reserve += 18;

            }
            else{

                state.health =
                    Math.min(
                        state.maxHealth,
                        state.health+25
                    );

            }

            state.score += 50;

            spawnParticles(
                item.x,
                item.y,
                item.type==="ammo"
                    ? "#ffd84a"
                    : "#67ff82",
                10
            );

        }

    }

    pickups =
        pickups.filter(
            item=>!item.collected
        );

}

/* =========================================================
   PARTÍCULAS
========================================================= */

function spawnParticles(
    x,
    y,
    color,
    amount=8
){

    color =
        String(color).trim();

    for(
        let i=0;
        i<amount;
        i++
    ){

        particles.push({

            x:x,
            y:y,

            vx:random(-4,4),

            vy:random(-5,-1),

            life:1,

            size:random(2,5),

            color:color

        });

    }

}

function updateParticles(){

    for(
        let i=particles.length-1;
        i>=0;
        i--
    ){

        const p =
            particles[i];

        p.x += p.vx;
        p.y += p.vy;

        p.vy += .15;

        p.life -= .035;

        if(
            p.life<=0
        ){

            particles.splice(i,1);

        }

    }

}

/* =========================================================
   SANGUE
========================================================= */

function updateBlood(){

    for(
        let i=blood.length-1;
        i>=0;
        i--
    ){

        const b =
            blood[i];

        b.life -= .003;

        if(
            b.life<=0
        ){

            blood.splice(i,1);

        }

    }

}

/* =========================================================
   CÂMERA
========================================================= */

function updateCamera(){

    const target =
        player.x-
        W*.38;

    state.cameraX +=
        (
            target-
            state.cameraX
        )*.1;

    state.cameraX =
        clamp(
            state.cameraX,
            0,
            Math.max(
                0,
                WORLD_WIDTH-W
            )
        );

}

/* =========================================================
   CÉU
========================================================= */

function drawSky(){

    const g =
        ctx.createLinearGradient(
            0,
            0,
            0,
            H
        );

    g.addColorStop(
        0,
        "#182c3a"
    );

    g.addColorStop(
        .5,
        "#284653"
    );

    g.addColorStop(
        1,
        "#10181c"
    );

    ctx.fillStyle=g;

    ctx.fillRect(
        0,
        0,
        W,
        H
    );

    /* LUA */

    ctx.fillStyle =
        "rgba(220,235,220,.9)";

    ctx.beginPath();

    ctx.arc(
        W-120,
        100,
        38,
        0,
        Math.PI*2
    );

    ctx.fill();

    ctx.fillStyle =
        "rgba(20,35,40,.5)";

    ctx.beginPath();

    ctx.arc(
        W-105,
        91,
        35,
        0,
        Math.PI*2
    );

    ctx.fill();

    /* ESTRELAS */

    for(
        let i=0;
        i<50;
        i++
    ){

        const x =
            (i*137)%W;

        const y =
            30+
            (i*71)%250;

        const alpha =
            .35+
            Math.sin(
                state.time*2+i
            )*.2;

        ctx.fillStyle =
            `rgba(255,255,255,${alpha})`;

        ctx.fillRect(
            x,
            y,
            2,
            2
        );

    }

}

/* =========================================================
   MONTANHAS
========================================================= */

function drawMountains(){

    const offset =
        state.cameraX*.12;

    ctx.save();

    ctx.translate(
        -offset%700,
        0
    );

    for(
        let x=-700;
        x<W+1400;
        x+=700
    ){

        ctx.fillStyle =
            "#172c35";

        ctx.beginPath();

        ctx.moveTo(
            x,
            getGroundY()
        );

        ctx.lineTo(
            x+270,
            H-410
        );

        ctx.lineTo(
            x+500,
            getGroundY()
        );

        ctx.closePath();

        ctx.fill();

        ctx.fillStyle =
            "#213e48";

        ctx.beginPath();

        ctx.moveTo(
            x+270,
            H-410
        );

        ctx.lineTo(
            x+225,
            H-350
        );

        ctx.lineTo(
            x+270,
            H-370
        );

        ctx.lineTo(
            x+315,
            H-350
        );

        ctx.closePath();

        ctx.fill();

    }

    ctx.restore();

}

/* =========================================================
   ÁRVORES
========================================================= */

function drawTrees(){

    for(const t of trees){

        const x =
            t.x-
            state.cameraX;

        if(
            x<-80 ||
            x>W+80
        ){

            continue;

        }

        const s=t.scale;
        const groundY=t.y;

        ctx.fillStyle =
            "#4b3024";

        ctx.fillRect(
            x-7*s,
            groundY-70*s,
            14*s,
            70*s
        );

        ctx.fillStyle =
            "#173f2b";

        ctx.beginPath();

        ctx.arc(
            x,
            groundY-95*s,
            34*s,
            0,
            Math.PI*2
        );

        ctx.arc(
            x-24*s,
            groundY-70*s,
            27*s,
            0,
            Math.PI*2
        );

        ctx.arc(
            x+25*s,
            groundY-68*s,
            29*s,
            0,
            Math.PI*2
        );

        ctx.fill();

        ctx.fillStyle =
            "#245c38";

        ctx.beginPath();

        ctx.arc(
            x-8*s,
            groundY-105*s,
            21*s,
            0,
            Math.PI*2
        );

        ctx.fill();

    }

}

/* =========================================================
   PEDRAS
========================================================= */

function drawRocks(){

    for(const r of rocks){

        const x =
            r.x-
            state.cameraX;

        if(
            x<-30 ||
            x>W+30
        ){

            continue;

        }

        ctx.fillStyle =
            "#364149";

        ctx.beginPath();

        ctx.ellipse(
            x,
            r.y,
            r.size,
            r.size*.65,
            0,
            0,
            Math.PI*2
        );

        ctx.fill();

    }

}

/* =========================================================
   PLATAFORMAS
========================================================= */

function drawPlatforms(){

    for(const p of platforms){

        const x =
            p.x-
            state.cameraX;

        if(
            x+p.w<0 ||
            x>W
        ){

            continue;

        }

        ctx.fillStyle =
            "#24352b";

        ctx.fillRect(
            x,
            p.y,
            p.w,
            Math.min(
                15,
                p.h
            )
        );

        if(p.h>15){

            ctx.fillStyle =
                "#493629";

            ctx.fillRect(
                x,
                p.y+15,
                p.w,
                p.h-15
            );

        }

        ctx.fillStyle =
            "rgba(0,0,0,.15)";

        for(
            let i=x;
            i<x+p.w;
            i+=55
        ){

            ctx.fillRect(
                i+10,
                p.y+30,
                18,
                5
            );

        }

    }

}

/* =========================================================
   PICKUPS
========================================================= */

function drawPickups(){

    for(const item of pickups){

        if(item.collected){
            continue;
        }

        const x =
            item.x-
            state.cameraX;

        if(
            x<-40 ||
            x>W+40
        ){

            continue;

        }

        const bob =
            Math.sin(
                state.time*4+
                item.x
            )*4;

        ctx.save();

        ctx.translate(
            x,
            item.y+bob
        );

        if(
            item.type==="ammo"
        ){

            ctx.fillStyle =
                "#d69b27";

            ctx.fillRect(
                -13,
                -10,
                26,
                20
            );

            ctx.fillStyle =
                "#ffe65b";

            ctx.fillRect(
                -8,
                -6,
                5,
                12
            );

            ctx.fillRect(
                2,
                -6,
                5,
                12
            );

        }
        else{

            ctx.fillStyle =
                "#f2f2f2";

            ctx.fillRect(
                -13,
                -13,
                26,
                26
            );

            ctx.fillStyle =
                "#e8394c";

            ctx.fillRect(
                -4,
                -10,
                8,
                20
            );

            ctx.fillRect(
                -10,
                -4,
                20,
                8
            );

        }

        ctx.restore();

    }

}

/* =========================================================
   ZUMBIS
========================================================= */

function drawZombies(){

    for(const z of zombies){

        if(!z.alive){
            continue;
        }

        const x =
            z.x-
            state.cameraX;

        const y=z.y;

        if(
            x<-100 ||
            x>W+100
        ){

            continue;

        }

        const bob =
            Math.sin(
                z.anim
            )*2;

        ctx.save();

        ctx.translate(
            x+z.w/2,
            y+z.h/2+bob
        );

        const scale =
            z.type==="tank"
            ? 1.18
            : z.type==="fast"
            ? .9
            : 1;

        ctx.scale(
            scale,
            scale
        );

        /* SOMBRA */

        ctx.fillStyle =
            "rgba(0,0,0,.35)";

        ctx.beginPath();

        ctx.ellipse(
            0,
            z.h/2+4,
            z.w*.45,
            6,
            0,
            0,
            Math.PI*2
        );

        ctx.fill();

        /* BRAÇOS */

        ctx.strokeStyle =
            "#6f9b61";

        ctx.lineWidth=9;
        ctx.lineCap="round";

        ctx.beginPath();

        ctx.moveTo(
            -17,
            -2
        );

        ctx.lineTo(
            -30,
            12
        );

        ctx.moveTo(
            17,
            -2
        );

        ctx.lineTo(
            30,
            12
        );

        ctx.stroke();

        /* CORPO */

        const body =
            ctx.createLinearGradient(
                0,
                -25,
                0,
                30
            );

        body.addColorStop(
            0,
            z.type==="tank"
                ? "#7eab63"
                : "#5d984f"
        );

        body.addColorStop(
            1,
            "#294b32"
        );

        ctx.fillStyle=body;

        ctx.beginPath();

        drawRoundRect(
            ctx,
            -z.w/2,
            -z.h/2+5,
            z.w,
            z.h-8,
            13
        );

        ctx.fill();

        ctx.strokeStyle =
            "#17251b";

        ctx.lineWidth=3;

        ctx.stroke();

        /* CABEÇA */

        ctx.fillStyle =
            "#83ad69";

        ctx.beginPath();

        ctx.arc(
            0,
            -z.h/2,
            z.w*.38,
            0,
            Math.PI*2
        );

        ctx.fill();

        ctx.stroke();

        /* OLHOS */

        ctx.fillStyle =
            "#d9f2bd";

        ctx.beginPath();

        ctx.arc(
            -8,
            -z.h/2-3,
            5,
            0,
            Math.PI*2
        );

        ctx.arc(
            8,
            -z.h/2-3,
            5,
            0,
            Math.PI*2
        );

        ctx.fill();

        ctx.fillStyle =
            "#101514";

        ctx.beginPath();

        ctx.arc(
            -8,
            -z.h/2-3,
            2,
            0,
            Math.PI*2
        );

        ctx.arc(
            8,
            -z.h/2-3,
            2,
            0,
            Math.PI*2
        );

        ctx.fill();

        /* BOCA */

        ctx.strokeStyle =
            "#2c1918";

        ctx.lineWidth=3;

        ctx.beginPath();

        ctx.arc(
            0,
            -z.h/2+9,
            9,
            0,
            Math.PI
        );

        ctx.stroke();

        /* VIDA DO TANK */

        if(
            z.type==="tank"
        ){

            ctx.fillStyle =
                "#210f12";

            ctx.fillRect(
                -24,
                -45,
                48,
                5
            );

            ctx.fillStyle =
                "#ff4656";

            ctx.fillRect(
                -24,
                -45,
                48*
                (z.hp/z.maxHp),
                5
            );

        }

        ctx.restore();

    }

}

/* =========================================================
   RETÂNGULO ARREDONDADO
========================================================= */

function drawRoundRect(
    context,
    x,
    y,
    w,
    h,
    r
){

    r =
        Math.min(
            r,
            w/2,
            h/2
        );

    context.moveTo(
        x+r,
        y
    );

    context.lineTo(
        x+w-r,
        y
    );

    context.quadraticCurveTo(
        x+w,
        y,
        x+w,
        y+r
    );

    context.lineTo(
        x+w,
        y+h-r
    );

    context.quadraticCurveTo(
        x+w,
        y+h,
        x+w-r,
        y+h
    );

    context.lineTo(
        x+r,
        y+h
    );

    context.quadraticCurveTo(
        x,
        y+h,
        x,
        y+h-r
    );

    context.lineTo(
        x,
        y+r
    );

    context.quadraticCurveTo(
        x,
        y,
        x+r,
        y
    );

    context.closePath();

}

/* =========================================================
   NICO
========================================================= */

function drawPlayer(){

    if(
        player.invincible>0 &&
        Math.floor(
            player.invincible/5
        )%2===0
    ){

        return;

    }

    const x =
        player.x-
        state.cameraX;

    const y =
        player.y;

    const target =
        mobileFire &&
        isMobile()
        ? getMobileTarget()
        : getWorldMouse();

    let angle =
        Math.atan2(
            target.y-
            (player.y+30),
            target.x-
            (player.x+player.w/2)
        );

    if(
        Math.cos(angle)<0
    ){

        player.facing=-1;

    }
    else{

        player.facing=1;

    }

    ctx.save();

    ctx.translate(
        x+player.w/2,
        y+player.h/2
    );

    ctx.scale(
        player.facing,
        1
    );

    /*
     * Ajusta o ângulo da arma para o
     * sistema de coordenadas espelhado.
     */

    let weaponAngle =
        player.facing===1
        ? angle
        : Math.PI-angle;

    const leg =
        player.grounded
        ? Math.sin(
            player.walk
        )*4
        : 0;

    /* SOMBRA */

    ctx.fillStyle =
        "rgba(0,0,0,.35)";

    ctx.beginPath();

    ctx.ellipse(
        0,
        34,
        23,
        6,
        0,
        0,
        Math.PI*2
    );

    ctx.fill();

    /* PERNAS */

    ctx.fillStyle =
        "#263d50";

    ctx.fillRect(
        -15,
        15+leg,
        11,
        17
    );

    ctx.fillRect(
        4,
        15-leg,
        11,
        17
    );

    /* BOTAS */

    ctx.fillStyle =
        "#171c21";

    ctx.beginPath();

    drawRoundRect(
        ctx,
        -19,
        27+leg,
        20,
        10,
        5
    );

    drawRoundRect(
        ctx,
        0,
        27-leg,
        20,
        10,
        5
    );

    ctx.fill();

    /* CORPO */

    const shirt =
        ctx.createLinearGradient(
            -22,
            -5,
            22,
            25
        );

    shirt.addColorStop(
        0,
        "#2e78a8"
    );

    shirt.addColorStop(
        1,
        "#16415f"
    );

    ctx.fillStyle=shirt;

    ctx.beginPath();

    drawRoundRect(
        ctx,
        -21,
        -4,
        42,
        31,
        9
    );

    ctx.fill();

    ctx.strokeStyle =
        "#102d40";

    ctx.lineWidth=3;

    ctx.stroke();

    /* COLETE */

    ctx.fillStyle =
        "#29333a";

    ctx.fillRect(
        -17,
        1,
        7,
        20
    );

    ctx.fillRect(
        10,
        1,
        7,
        20
    );

    /* BRAÇO */

    ctx.strokeStyle =
        "#d99e6e";

    ctx.lineWidth=9;

    ctx.lineCap="round";

    ctx.beginPath();

    ctx.moveTo(
        15,
        3
    );

    ctx.lineTo(
        24,
        9
    );

    ctx.stroke();

    /* CABEÇA */

    ctx.fillStyle =
        "#dca878";

    ctx.beginPath();

    ctx.ellipse(
        0,
        -20,
        20,
        22,
        0,
        0,
        Math.PI*2
    );

    ctx.fill();

    ctx.strokeStyle =
        "#6e3d2d";

    ctx.lineWidth=3;

    ctx.stroke();

    /* CABELO */

    ctx.fillStyle =
        "#182b3b";

    ctx.beginPath();

    ctx.arc(
        -12,
        -35,
        11,
        0,
        Math.PI*2
    );

    ctx.arc(
        0,
        -39,
        13,
        0,
        Math.PI*2
    );

    ctx.arc(
        12,
        -35,
        11,
        0,
        Math.PI*2
    );

    ctx.fill();

    /* OLHOS */

    ctx.fillStyle =
        "#111";

    ctx.beginPath();

    ctx.ellipse(
        -7,
        -21,
        3,
        5,
        0,
        0,
        Math.PI*2
    );

    ctx.ellipse(
        7,
        -21,
        3,
        5,
        0,
        0,
        Math.PI*2
    );

    ctx.fill();

    /* ARMA */

    ctx.save();

    ctx.rotate(
        clamp(
            weaponAngle,
            -Math.PI*.85,
            Math.PI*.85
        )
    );

    ctx.fillStyle =
        "#20272c";

    ctx.fillRect(
        14,
        1,
        30,
        8
    );

    ctx.fillStyle =
        "#11171b";

    ctx.fillRect(
        37,
        -2,
        13,
        6
    );

    ctx.fillStyle =
        "#5b6870";

    ctx.fillRect(
        18,
        9,
        8,
        12
    );

    ctx.restore();

    ctx.restore();

}

/* =========================================================
   BALAS
========================================================= */

function drawBullets(){

    for(const b of bullets){

        const x =
            b.x-
            state.cameraX;

        ctx.fillStyle =
            "#ffe75c";

        ctx.shadowColor =
            "#ffb000";

        ctx.shadowBlur=10;

        ctx.beginPath();

        ctx.arc(
            x,
            b.y,
            3,
            0,
            Math.PI*2
        );

        ctx.fill();

        ctx.shadowBlur=0;

    }

}

/* =========================================================
   PARTÍCULAS
========================================================= */

function drawParticles(){

    for(const p of particles){

        const x =
            p.x-
            state.cameraX;

        ctx.globalAlpha =
            Math.max(
                0,
                p.life
            );

        ctx.fillStyle =
            p.color;

        ctx.beginPath();

        ctx.arc(
            x,
            p.y,
            p.size,
            0,
            Math.PI*2
        );

        ctx.fill();

    }

    ctx.globalAlpha=1;

}

/* =========================================================
   SANGUE
========================================================= */

function drawBlood(){

    for(const b of blood){

        const x =
            b.x-
            state.cameraX;

        ctx.globalAlpha =
            Math.max(
                0,
                b.life
            );

        ctx.fillStyle =
            "#b4141e";

        ctx.beginPath();

        ctx.ellipse(
            x,
            b.y,
            22,
            5,
            0,
            0,
            Math.PI*2
        );

        ctx.fill();

    }

    ctx.globalAlpha=1;

}

/* =========================================================
   MIRA
========================================================= */

function drawCrosshair(){

    if(isMobile()){
        return;
    }

    ctx.save();

    ctx.strokeStyle =
        "rgba(255,255,255,.9)";

    ctx.lineWidth=2;

    const x=mouse.x;
    const y=mouse.y;

    ctx.beginPath();

    ctx.moveTo(
        x-12,
        y
    );

    ctx.lineTo(
        x-4,
        y
    );

    ctx.moveTo(
        x+4,
        y
    );

    ctx.lineTo(
        x+12,
        y
    );

    ctx.moveTo(
        x,
        y-12
    );

    ctx.lineTo(
        x,
        y-4
    );

    ctx.moveTo(
        x,
        y+4
    );

    ctx.lineTo(
        x,
        y+12
    );

    ctx.stroke();

    ctx.fillStyle =
        "#ff5261";

    ctx.beginPath();

    ctx.arc(
        x,
        y,
        2,
        0,
        Math.PI*2
    );

    ctx.fill();

    ctx.restore();

}

/* =========================================================
   INDICADOR DE MIRA MOBILE
========================================================= */

function drawMobileTarget(){

    if(
        !isMobile() ||
        !mobileFire
    ){

        return;

    }

    const target =
        getMobileTarget();

    if(!target){
        return;
    }

    const x =
        target.x-
        state.cameraX;

    const y =
        target.y;

    if(
        x<-30 ||
        x>W+30 ||
        y<-30 ||
        y>H+30
    ){

        return;

    }

    ctx.save();

    ctx.strokeStyle =
        "rgba(255,70,80,.7)";

    ctx.lineWidth=2;

    ctx.beginPath();

    ctx.arc(
        x,
        y,
        16,
        0,
        Math.PI*2
    );

    ctx.stroke();

    ctx.beginPath();

    ctx.moveTo(
        x-22,
        y
    );

    ctx.lineTo(
        x-10,
        y
    );

    ctx.moveTo(
        x+10,
        y
    );

    ctx.lineTo(
        x+22,
        y
    );

    ctx.moveTo(
        x,
        y-22
    );

    ctx.lineTo(
        x,
        y-10
    );

    ctx.moveTo(
        x,
        y+10
    );

    ctx.lineTo(
        x,
        y+22
    );

    ctx.stroke();

    ctx.restore();

}

/* =========================================================
   VINHETA
========================================================= */

function drawVignette(){

    const g =
        ctx.createRadialGradient(
            W/2,
            H/2,
            H*.25,
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
        "rgba(0,0,0,.55)"
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
   TEXTO DA ONDA
========================================================= */

function drawWaveMessage(){

    if(
        !state.running ||
        state.waveTimer<=0
    ){

        return;

    }

    ctx.save();

    ctx.fillStyle =
        "rgba(255,255,255,.9)";

    ctx.font =
        "900 26px Arial";

    ctx.textAlign="center";

    ctx.fillText(
        "ONDA "+state.wave,
        W/2,
        H*.22
    );

    ctx.restore();

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

    drawSky();

    drawMountains();

    drawTrees();

    drawRocks();

    drawPlatforms();

    drawBlood();

    drawPickups();

    drawZombies();

    drawBullets();

    drawPlayer();

    drawParticles();

    drawCrosshair();

    drawMobileTarget();

    drawWaveMessage();

    drawVignette();

}

/* =========================================================
   FIM DE JOGO
========================================================= */

function endGame(
    win,
    text
){

    if(!state.running){
        return;
    }

    state.running=false;

    state.gameWon=win;

    endTitle.textContent =
        win
        ? "MISSÃO CONCLUÍDA 🏆"
        : "VOCÊ FOI DERROTADO 💀";

    endText.textContent =
        text+
        " Pontuação: "+
        state.score+
        " | Zumbis derrotados: "+
        state.kills+
        ".";

    endScreen.classList.remove(
        "hidden"
    );

    mouse.down=false;
    mobileFire=false;

    keys.left=false;
    keys.right=false;
    keys.up=false;

}

/* =========================================================
   ATUALIZAÇÃO
========================================================= */

function update(){

    if(!state.running){
        return;
    }

    state.time += .016;

    updatePlayer();

    if(
        mouse.down ||
        mobileFire
    ){

        shoot();

    }

    updateBullets();

    updateZombies();

    updatePickups();

    updateParticles();

    updateBlood();

    updateWaves();

    updateCamera();

    updateHUD();

    if(state.shake>0){

        state.shake *= .84;

        if(
            state.shake<.2
        ){

            state.shake=0;

        }

    }

}

/* =========================================================
   LOOP
========================================================= */

let lastTime=0;

function loop(time){

    if(!lastTime){

        lastTime=time;

    }

    const delta =
        Math.min(
            (time-lastTime)/16.67,
            2
        );

    lastTime=time;

    const steps =
        Math.max(
            1,
            Math.round(delta)
        );

    for(
        let i=0;
        i<steps;
        i++
    ){

        update();

    }

    ctx.save();

    if(state.shake>0){

        ctx.translate(
            random(
                -state.shake,
                state.shake
            ),
            random(
                -state.shake,
                state.shake
            )
        );

    }

    render();

    ctx.restore();

    requestAnimationFrame(
        loop
    );

}

/* =========================================================
   BOTÕES
========================================================= */

startButton.addEventListener(
    "click",
    function(){

        resetGame();

    }
);

restartButton.addEventListener(
    "click",
    function(){

        resetGame();

    }
);

/* =========================================================
   INICIALIZAÇÃO
========================================================= */

createWorld();

updateHUD();

requestAnimationFrame(
    loop
);

</script>

</body>
</html>