<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport"
      content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

<title>Nico: A Grande Aventura</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

html,
body {
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #10182e;
    font-family: Arial, Helvetica, sans-serif;
}

body {
    touch-action: none;
}

#game {
    position: relative;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
}

canvas {
    display: block;
    width: 100%;
    height: 100%;
    image-rendering: auto;
}

/* HUD */

#hud {
    position: fixed;
    top: 16px;
    left: 50%;
    transform: translateX(-50%);

    z-index: 20;

    display: flex;
    align-items: center;
    gap: 24px;

    padding: 10px 20px;

    color: white;

    background: rgba(15, 25, 55, .78);

    border: 2px solid rgba(255,255,255,.22);
    border-radius: 18px;

    box-shadow:
        0 8px 25px rgba(0,0,0,.25),
        inset 0 1px rgba(255,255,255,.15);

    backdrop-filter: blur(8px);

    font-size: 16px;
    font-weight: 800;

    white-space: nowrap;
}

.hud-item {
    display: flex;
    align-items: center;
    gap: 7px;
}

/* TELA INICIAL / FINAL */

.screen {
    position: fixed;
    inset: 0;

    z-index: 50;

    display: flex;
    align-items: center;
    justify-content: center;

    background:
        radial-gradient(
            circle at center,
            rgba(44, 93, 180, .35),
            rgba(7, 12, 30, .92)
        );

    padding: 20px;
}

.panel {
    width: min(650px, 94vw);

    padding: 38px 28px;

    color: white;
    text-align: center;

    background:
        linear-gradient(
            145deg,
            rgba(39,64,120,.96),
            rgba(20,28,65,.97)
        );

    border: 2px solid rgba(255,255,255,.2);
    border-radius: 28px;

    box-shadow:
        0 30px 80px rgba(0,0,0,.5),
        inset 0 1px rgba(255,255,255,.15);
}

.logo {
    font-size: clamp(38px, 8vw, 70px);
    font-weight: 1000;
    letter-spacing: -3px;

    color: #fff;

    text-shadow:
        0 5px 0 #174ea6,
        0 10px 20px rgba(0,0,0,.35);
}

.subtitle {
    margin-top: 8px;
    margin-bottom: 25px;

    color: #cfe8ff;

    font-size: 19px;
}

.panel h2 {
    font-size: clamp(28px, 6vw, 46px);
    margin-bottom: 12px;
}

.panel p {
    line-height: 1.6;
    color: #dce8ff;
}

.game-button {
    margin-top: 25px;

    padding: 15px 34px;

    border: 0;
    border-radius: 14px;

    color: #17213d;
    background: linear-gradient(#ffe66b, #ffc928);

    font-size: 20px;
    font-weight: 900;

    cursor: pointer;

    box-shadow:
        0 5px 0 #c28d00,
        0 12px 25px rgba(0,0,0,.25);

    transition:
        transform .12s,
        filter .12s;
}

.game-button:hover {
    filter: brightness(1.08);
    transform: translateY(-2px);
}

.game-button:active {
    transform: translateY(4px);
    box-shadow: 0 1px 0 #c28d00;
}

/* CONTROLES MOBILE */

#mobileControls {
    position: fixed;

    z-index: 30;

    left: 0;
    right: 0;
    bottom: 18px;

    display: none;

    justify-content: space-between;

    padding: 0 18px;

    pointer-events: none;
}

.mobile-group {
    display: flex;
    gap: 12px;
}

.mobile-button {
    width: 66px;
    height: 66px;

    border-radius: 50%;

    color: white;

    background: rgba(12,22,48,.65);

    border: 3px solid rgba(255,255,255,.7);

    font-size: 27px;
    font-weight: bold;

    box-shadow: 0 6px 15px rgba(0,0,0,.25);

    pointer-events: auto;

    user-select: none;
    -webkit-user-select: none;
}

.mobile-button:active {
    transform: scale(.92);
    background: rgba(255,255,255,.25);
}

@media (max-width: 800px) {
    #mobileControls {
        display: flex;
    }

    #hud {
        top: 9px;
        gap: 10px;
        padding: 8px 12px;
        font-size: 13px;
    }
}

.hidden {
    display: none !important;
}
</style>
</head>

<body>

<div id="game">

    <canvas id="canvas"></canvas>

    <div id="hud">

        <div class="hud-item">
            🪙 <span id="coins">0</span>
        </div>

        <div class="hud-item">
            ❤️ <span id="lives">3</span>
        </div>

        <div class="hud-item">
            ⭐ <span id="score">0</span>
        </div>

        <div class="hud-item">
            🏁 <span id="level">1</span>/3
        </div>

    </div>

    <div id="startScreen" class="screen">

        <div class="panel">

            <div class="logo">
                NICO
            </div>

            <div class="subtitle">
                A Grande Aventura
            </div>

            <p>
                Ajude Nico a atravessar o mundo,
                pegar moedas, derrotar os Bubus
                e chegar à bandeira!
            </p>

            <p style="margin-top:10px">
                <b>← →</b> mover &nbsp; • &nbsp;
                <b>Espaço / ↑</b> pular
            </p>

            <button id="startButton" class="game-button">
                JOGAR AGORA
            </button>

        </div>

    </div>

    <div id="endScreen" class="screen hidden">

        <div class="panel">

            <h2 id="endTitle">
                Você venceu! 🏆
            </h2>

            <p id="endText">
                Nico terminou a aventura!
            </p>

            <button id="restartButton" class="game-button">
                JOGAR NOVAMENTE
            </button>

        </div>

    </div>

    <div id="mobileControls">

        <div class="mobile-group">

            <button
                class="mobile-button"
                id="leftButton">
                ◀
            </button>

            <button
                class="mobile-button"
                id="rightButton">
                ▶
            </button>

        </div>

        <button
            class="mobile-button"
            id="jumpButton">
            ▲
        </button>

    </div>

</div>

<script>
"use strict";

/* =========================================================
   NICO — A GRANDE AVENTURA
   JOGO DE PLATAFORMA EM CANVAS
========================================================= */

const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

const coinsEl = document.getElementById("coins");
const livesEl = document.getElementById("lives");
const scoreEl = document.getElementById("score");
const levelEl = document.getElementById("level");

const startScreen = document.getElementById("startScreen");
const endScreen = document.getElementById("endScreen");

const endTitle = document.getElementById("endTitle");
const endText = document.getElementById("endText");

const startButton = document.getElementById("startButton");
const restartButton = document.getElementById("restartButton");


/* =========================================================
   CANVAS
========================================================= */

let W = window.innerWidth;
let H = window.innerHeight;

function resize() {

    W = window.innerWidth;
    H = window.innerHeight;

    const dpr = Math.min(window.devicePixelRatio || 1, 2);

    canvas.width = W * dpr;
    canvas.height = H * dpr;

    canvas.style.width = W + "px";
    canvas.style.height = H + "px";

    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
}

window.addEventListener("resize", resize);

resize();


/* =========================================================
   CONFIGURAÇÕES
========================================================= */

const TILE = 48;

const GRAVITY = 0.72;

const MOVE_SPEED = 6.2;

const JUMP_FORCE = 15;

const MAX_LIVES = 3;

const LEVEL_WIDTH = 4200;


/* =========================================================
   ESTADO
========================================================= */

const state = {

    running: false,

    level: 1,

    cameraX: 0,

    score: 0,

    coins: 0,

    lives: MAX_LIVES,

    time: 0,

    shake: 0
};


/* =========================================================
   TECLAS
========================================================= */

const keys = {

    left: false,

    right: false,

    jump: false
};


let jumpPressed = false;


document.addEventListener("keydown", event => {

    const key = event.key.toLowerCase();

    if (
        key === "arrowleft" ||
        key === "a"
    ) {

        keys.left = true;
        event.preventDefault();
    }

    if (
        key === "arrowright" ||
        key === "d"
    ) {

        keys.right = true;
        event.preventDefault();
    }

    if (
        key === "arrowup" ||
        key === "w" ||
        key === " "
    ) {

        if (!keys.jump) {
            jumpPressed = true;
        }

        keys.jump = true;

        event.preventDefault();
    }
});


document.addEventListener("keyup", event => {

    const key = event.key.toLowerCase();

    if (
        key === "arrowleft" ||
        key === "a"
    ) {
        keys.left = false;
    }

    if (
        key === "arrowright" ||
        key === "d"
    ) {
        keys.right = false;
    }

    if (
        key === "arrowup" ||
        key === "w" ||
        key === " "
    ) {
        keys.jump = false;
    }
});


/* =========================================================
   CONTROLES MOBILE
========================================================= */

function holdButton(button, property) {

    button.addEventListener("pointerdown", event => {

        event.preventDefault();

        if (property === "jump") {
            jumpPressed = true;
        }

        keys[property] = true;

        try {
            button.setPointerCapture(event.pointerId);
        } catch (_) {}
    });

    const release = () => {
        keys[property] = false;
    };

    button.addEventListener("pointerup", release);
    button.addEventListener("pointercancel", release);
    button.addEventListener("lostpointercapture", release);
}


holdButton(
    document.getElementById("leftButton"),
    "left"
);

holdButton(
    document.getElementById("rightButton"),
    "right"
);

holdButton(
    document.getElementById("jumpButton"),
    "jump"
);


/* =========================================================
   NICO
========================================================= */

const player = {

    x: 120,

    y: 300,

    w: 42,

    h: 62,

    vx: 0,

    vy: 0,

    grounded: false,

    facing: 1,

    invincible: 0,

    animation: 0,

    squash: 1,

    dead: false
};


/* =========================================================
   MUNDO
========================================================= */

let platforms = [];
let coins = [];
let enemies = [];
let particles = [];
let clouds = [];
let decorations = [];

let finishX = 3900;


/* =========================================================
   CRIAR NÍVEL
========================================================= */

function createLevel(level) {

    platforms = [];
    coins = [];
    enemies = [];
    particles = [];
    decorations = [];

    player.x = 120;
    player.y = 300;

    player.vx = 0;
    player.vy = 0;

    player.grounded = false;

    player.dead = false;

    state.cameraX = 0;

    const groundY = H - 105;

    /* CHÃO */

    platforms.push({
        x: 0,
        y: groundY,
        w: LEVEL_WIDTH + 400,
        h: 200,
        ground: true
    });


    /* PLATAFORMAS */

    const layouts = [

        [480, groundY - 100, 220],
        [820, groundY - 180, 180],
        [1120, groundY - 100, 250],
        [1500, groundY - 210, 220],
        [1840, groundY - 120, 260],
        [2220, groundY - 230, 200],
        [2520, groundY - 130, 260],
        [2900, groundY - 210, 230],
        [3250, groundY - 110, 270],
        [3600, groundY - 220, 220]
    ];

    for (const p of layouts) {

        platforms.push({
            x: p[0],
            y: p[1],
            w: p[2],
            h: 32,
            ground: false
        });
    }


    /* BLOCOS */

    const blocks = [
        [560, groundY - 190],
        [620, groundY - 190],
        [875, groundY - 270],
        [1180, groundY - 190],
        [1240, groundY - 190],
        [1580, groundY - 300],
        [1930, groundY - 210],
        [2280, groundY - 320],
        [2600, groundY - 220],
        [2970, groundY - 300],
        [3310, groundY - 200],
        [3670, groundY - 310]
    ];

    for (const b of blocks) {

        platforms.push({
            x: b[0],
            y: b[1],
            w: TILE,
            h: TILE,
            block: true
        });
    }


    /* MOEDAS */

    const coinPositions = [

        [520, groundY - 155],
        [575, groundY - 155],
        [630, groundY - 155],

        [850, groundY - 240],
        [900, groundY - 240],
        [950, groundY - 240],

        [1160, groundY - 155],
        [1220, groundY - 155],
        [1280, groundY - 155],

        [1530, groundY - 270],
        [1590, groundY - 270],
        [1650, groundY - 270],

        [1880, groundY - 175],
        [1940, groundY - 175],
        [2000, groundY - 175],

        [2260, groundY - 290],
        [2320, groundY - 290],
        [2380, groundY - 290],

        [2570, groundY - 185],
        [2630, groundY - 185],
        [2690, groundY - 185],

        [2940, groundY - 270],
        [3000, groundY - 270],
        [3060, groundY - 270],

        [3290, groundY - 170],
        [3350, groundY - 170],
        [3410, groundY - 170],

        [3640, groundY - 290],
        [3700, groundY - 290],
        [3760, groundY - 290]
    ];

    for (const c of coinPositions) {

        coins.push({
            x: c[0],
            y: c[1],
            r: 13,
            collected: false,
            spin: Math.random() * Math.PI * 2
        });
    }


    /* BUBUS */

    const bubuPositions = [

        [700, groundY - 45],
        [1050, groundY - 45],
        [1370, groundY - 45],
        [1770, groundY - 45],
        [2140, groundY - 45],
        [2470, groundY - 45],
        [2820, groundY - 45],
        [3180, groundY - 45],
        [3540, groundY - 45]
    ];

    for (let i = 0; i < bubuPositions.length; i++) {

        enemies.push({

            x: bubuPositions[i][0],

            y: bubuPositions[i][1],

            w: 48,

            h: 44,

            vx: i % 2 === 0 ? 1.1 : -1.1,

            vy: 0,

            alive: true,

            minX: bubuPositions[i][0] - 70,

            maxX: bubuPositions[i][0] + 70,

            animation: Math.random() * 10
        });
    }


    finishX = 3950;


    /* NUVENS */

    clouds = [];

    for (let i = 0; i < 25; i++) {

        clouds.push({

            x: i * 220 + Math.random() * 80,

            y: 60 + Math.random() * 180,

            scale: .65 + Math.random() * .8
        });
    }


    /* DECORAÇÕES */

    for (let i = 0; i < 80; i++) {

        decorations.push({

            x: i * 65 + Math.random() * 40,

            type: Math.random() > .5
                ? "flower"
                : "grass"
        });
    }

    updateHUD();
}


/* =========================================================
   UTILIDADES
========================================================= */

function clamp(value, min, max) {

    return Math.max(
        min,
        Math.min(max, value)
    );
}


function overlap(a, b) {

    return (
        a.x < b.x + b.w &&
        a.x + a.w > b.x &&
        a.y < b.y + b.h &&
        a.y + a.h > b.y
    );
}


function playerRect() {

    return {

        x: player.x,

        y: player.y,

        w: player.w,

        h: player.h
    };
}


/* =========================================================
   PARTÍCULAS
========================================================= */

function spawnParticles(
    x,
    y,
    color,
    amount = 8
) {

    for (let i = 0; i < amount; i++) {

        particles.push({

            x,
            y,

            vx:
                (Math.random() - .5) * 5,

            vy:
                -Math.random() * 4 - 1,

            life: 1,

            size:
                3 + Math.random() * 5,

            color
        });
    }
}


function updateParticles() {

    for (let i = particles.length - 1; i >= 0; i--) {

        const p = particles[i];

        p.x += p.vx;
        p.y += p.vy;

        p.vy += .15;

        p.life -= .035;

        if (p.life <= 0) {
            particles.splice(i, 1);
        }
    }
}


/* =========================================================
   MOEDAS
========================================================= */

function collectCoins() {

    const pr = playerRect();

    for (const coin of coins) {

        if (coin.collected) {
            continue;
        }

        const box = {

            x: coin.x - coin.r,

            y: coin.y - coin.r,

            w: coin.r * 2,

            h: coin.r * 2
        };

        if (overlap(pr, box)) {

            coin.collected = true;

            state.coins++;

            state.score += 100;

            spawnParticles(
                coin.x,
                coin.y,
                "#ffe45c",
                10
            );
        }
    }
}


/* =========================================================
   FÍSICA DO NICO
========================================================= */

function updatePlayer() {

    if (player.dead) {
        return;
    }


    /* MOVIMENTO */

    if (keys.left) {

        player.vx -= .75;

        player.facing = -1;

    } else if (keys.right) {

        player.vx += .75;

        player.facing = 1;

    } else {

        player.vx *= .80;
    }


    player.vx = clamp(
        player.vx,
        -MOVE_SPEED,
        MOVE_SPEED
    );


    /* PULO */

    if (
        jumpPressed &&
        player.grounded
    ) {

        player.vy = -JUMP_FORCE;

        player.grounded = false;

        player.squash = .8;

        spawnParticles(
            player.x + player.w / 2,
            player.y + player.h,
            "#ffffff",
            7
        );
    }

    jumpPressed = false;


    /* GRAVIDADE */

    player.vy += GRAVITY;

    if (player.vy > 18) {
        player.vy = 18;
    }


    /* MOVIMENTO HORIZONTAL */

    player.x += player.vx;

    player.x = clamp(
        player.x,
        0,
        LEVEL_WIDTH - player.w
    );


    /* COLISÃO HORIZONTAL */

    for (const platform of platforms) {

        if (!overlap(playerRect(), platform)) {
            continue;
        }

        if (platform.ground) {
            continue;
        }

        if (
            player.vx > 0 &&
            player.x + player.w >
            platform.x &&
            player.x <
            platform.x
        ) {

            player.x =
                platform.x - player.w;

            player.vx = 0;
        }

        else if (
            player.vx < 0 &&
            player.x <
            platform.x + platform.w &&
            player.x + player.w >
            platform.x + platform.w
        ) {

            player.x =
                platform.x + platform.w;

            player.vx = 0;
        }
    }


    /* MOVIMENTO VERTICAL */

    const oldBottom =
        player.y + player.h;

    player.y += player.vy;

    player.grounded = false;


    for (const platform of platforms) {

        if (
            player.x + player.w <= platform.x ||
            player.x >= platform.x + platform.w
        ) {
            continue;
        }


        const newBottom =
            player.y + player.h;


        /* CAINDO */

        if (
            player.vy >= 0 &&
            oldBottom <= platform.y &&
            newBottom >= platform.y
        ) {

            player.y =
                platform.y - player.h;

            player.vy = 0;

            player.grounded = true;

            player.squash = 1.12;

            break;
        }


        /* BATENDO A CABEÇA */

        if (
            player.vy < 0 &&
            player.y <= platform.y + platform.h &&
            oldBottom >= platform.y + platform.h
        ) {

            player.y =
                platform.y + platform.h;

            player.vy = 0;

            if (platform.block) {

                spawnParticles(
                    platform.x + platform.w / 2,
                    platform.y + platform.h,
                    "#f6b83d",
                    6
                );
            }

            break;
        }
    }


    /* CAIU DO MAPA */

    if (player.y > H + 200) {

        loseLife();
    }


    /* ANIMAÇÃO */

    player.animation +=
        Math.abs(player.vx) * .18;

    player.squash +=
        (1 - player.squash) * .15;

    if (player.invincible > 0) {
        player.invincible--;
    }
}


/* =========================================================
   BUBUS
========================================================= */

function updateEnemies() {

    const pr = playerRect();

    for (const enemy of enemies) {

        if (!enemy.alive) {
            continue;
        }


        enemy.x += enemy.vx;

        enemy.animation += .08;


        if (
            enemy.x < enemy.minX ||
            enemy.x > enemy.maxX
        ) {

            enemy.vx *= -1;
        }


        /* GRAVIDADE */

        enemy.vy += GRAVITY;

        enemy.y += enemy.vy;


        /* CHÃO / PLATAFORMAS */

        for (const platform of platforms) {

            if (
                enemy.x + enemy.w <= platform.x ||
                enemy.x >= platform.x + platform.w
            ) {
                continue;
            }


            if (
                enemy.y + enemy.h >= platform.y &&
                enemy.y + enemy.h <=
                platform.y + 30
            ) {

                enemy.y =
                    platform.y - enemy.h;

                enemy.vy = 0;

                break;
            }
        }


        if (!overlap(pr, enemy)) {
            continue;
        }


        /* PISOU */

        const playerBottom =
            player.y + player.h;

        const enemyTop =
            enemy.y;


        if (
            player.vy > 0 &&
            playerBottom - enemyTop < 25
        ) {

            enemy.alive = false;

            player.vy =
                -JUMP_FORCE * .65;

            state.score += 250;

            spawnParticles(
                enemy.x + enemy.w / 2,
                enemy.y + enemy.h / 2,
                "#ff8b38",
                16
            );

            state.shake = 7;

            continue;
        }


        /* BATEU NO BUBU */

        if (player.invincible <= 0) {

            loseLife();
        }
    }
}


/* =========================================================
   PERDER VIDA
========================================================= */

function loseLife() {

    if (
        !state.running ||
        player.invincible > 0
    ) {
        return;
    }


    state.lives--;

    state.shake = 12;

    spawnParticles(
        player.x + player.w / 2,
        player.y + player.h / 2,
        "#ff5368",
        18
    );


    if (state.lives <= 0) {

        state.running = false;

        showEnd(
            "Fim de jogo! 💥",
            "O Nico ficou sem vidas."
        );

        return;
    }


    player.invincible = 110;

    player.x =
        Math.max(100, player.x - 180);

    player.y =
        H - 105 - player.h;

    player.vx = 0;
    player.vy = 0;
}


/* =========================================================
   CHEGAR AO FINAL
========================================================= */

function checkFinish() {

    if (
        player.x + player.w >
        finishX
    ) {

        state.running = false;

        showEnd(
            "Você venceu! 🏆",
            `Nico terminou a aventura com ${state.coins} moedas e ${state.score} pontos!`
        );
    }
}


/* =========================================================
   CÂMERA
========================================================= */

function updateCamera() {

    const target =
        player.x - W * .38;

    state.cameraX +=
        (target - state.cameraX) * .10;

    state.cameraX =
        clamp(
            state.cameraX,
            0,
            LEVEL_WIDTH - W
        );
}


/* =========================================================
   DESENHO DO CÉU
========================================================= */

function drawSky() {

    const gradient =
        ctx.createLinearGradient(
            0,
            0,
            0,
            H
        );

    gradient.addColorStop(
        0,
        "#43bdf4"
    );

    gradient.addColorStop(
        .55,
        "#8fe4ff"
    );

    gradient.addColorStop(
        1,
        "#d8f5ff"
    );

    ctx.fillStyle = gradient;

    ctx.fillRect(
        0,
        0,
        W,
        H
    );


    /* SOL */

    const sunX =
        W - 120;

    const sunY =
        100;

    const glow =
        ctx.createRadialGradient(
            sunX,
            sunY,
            15,
            sunX,
            sunY,
            90
        );

    glow.addColorStop(
        0,
        "rgba(255,240,110,.95)"
    );

    glow.addColorStop(
        1,
        "rgba(255,240,110,0)"
    );

    ctx.fillStyle = glow;

    ctx.beginPath();

    ctx.arc(
        sunX,
        sunY,
        90,
        0,
        Math.PI * 2
    );

    ctx.fill();

    ctx.fillStyle = "#ffe36b";

    ctx.beginPath();

    ctx.arc(
        sunX,
        sunY,
        42,
        0,
        Math.PI * 2
    );

    ctx.fill();


    /* NUVENS PARALLAX */

    for (const cloud of clouds) {

        const x =
            cloud.x -
            state.cameraX * .22;

        if (
            x < -200 ||
            x > W + 200
        ) {
            continue;
        }

        drawCloud(
            x,
            cloud.y,
            cloud.scale
        );
    }
}


function drawCloud(x, y, scale) {

    ctx.save();

    ctx.translate(x, y);

    ctx.scale(scale, scale);

    ctx.fillStyle =
        "rgba(255,255,255,.88)";

    ctx.beginPath();

    ctx.arc(25, 25, 25, 0, Math.PI * 2);

    ctx.arc(60, 15, 35, 0, Math.PI * 2);

    ctx.arc(100, 25, 27, 0, Math.PI * 2);

    ctx.roundRect(
        10,
        20,
        105,
        35,
        20
    );

    ctx.fill();

    ctx.restore();
}


/* =========================================================
   MONTANHAS
========================================================= */

function drawMountains() {

    const offset =
        state.cameraX * .15;

    ctx.save();

    ctx.translate(
        -offset % 700,
        0
    );

    for (
        let x = -700;
        x < W + 1400;
        x += 700
    ) {

        ctx.fillStyle =
            "rgba(69,151,172,.55)";

        ctx.beginPath();

        ctx.moveTo(
            x,
            H - 105
        );

        ctx.lineTo(
            x + 260,
            H - 400
        );

        ctx.lineTo(
            x + 470,
            H - 105
        );

        ctx.closePath();

        ctx.fill();


        ctx.fillStyle =
            "rgba(255,255,255,.45)";

        ctx.beginPath();

        ctx.moveTo(
            x + 260,
            H - 400
        );

        ctx.lineTo(
            x + 205,
            H - 335
        );

        ctx.lineTo(
            x + 260,
            H - 350
        );

        ctx.lineTo(
            x + 310,
            H - 335
        );

        ctx.closePath();

        ctx.fill();
    }

    ctx.restore();
}


/* =========================================================
   TERRENO
========================================================= */

function drawPlatforms() {

    for (const platform of platforms) {

        const x =
            platform.x -
            state.cameraX;

        if (
            x + platform.w < 0 ||
            x > W
        ) {
            continue;
        }


        if (platform.ground) {

            /* GRAMA */

            ctx.fillStyle =
                "#38a84b";

            ctx.fillRect(
                x,
                platform.y,
                platform.w,
                16
            );


            /* TERRA */

            ctx.fillStyle =
                "#8b522e";

            ctx.fillRect(
                x,
                platform.y + 16,
                platform.w,
                platform.h - 16
            );


            /* TEXTURA */

            ctx.fillStyle =
                "rgba(80,38,20,.25)";

            for (
                let tx = x;
                tx < x + platform.w;
                tx += 70
            ) {

                ctx.fillRect(
                    tx + 10,
                    platform.y + 35,
                    20,
                    5
                );
            }

            continue;
        }


        if (platform.block) {

            drawBlock(
                x,
                platform.y
            );

            continue;
        }


        /* PLATAFORMA */

        ctx.fillStyle =
            "#9c5b32";

        ctx.fillRect(
            x,
            platform.y + 8,
            platform.w,
            platform.h - 8
        );


        ctx.fillStyle =
            "#47bd50";

        ctx.fillRect(
            x,
            platform.y,
            platform.w,
            11
        );


        ctx.fillStyle =
            "#31913b";

        ctx.fillRect(
            x,
            platform.y + 9,
            platform.w,
            4
        );
    }
}


function drawBlock(x, y) {

    const gradient =
        ctx.createLinearGradient(
            x,
            y,
            x,
            y + TILE
        );

    gradient.addColorStop(
        0,
        "#ffd34d"
    );

    gradient.addColorStop(
        1,
        "#e89b1c"
    );

    ctx.fillStyle = gradient;

    ctx.fillRect(
        x,
        y,
        TILE,
        TILE
    );


    ctx.strokeStyle =
        "#a96112";

    ctx.lineWidth = 4;

    ctx.strokeRect(
        x + 2,
        y + 2,
        TILE - 4,
        TILE - 4
    );


    ctx.fillStyle =
        "#fff3a0";

    ctx.font =
        "bold 27px Arial";

    ctx.textAlign = "center";
    ctx.textBaseline = "middle";

    ctx.fillText(
        "?",
        x + TILE / 2,
        y + TILE / 2 + 1
    );
}


/* =========================================================
   DECORAÇÕES
========================================================= */

function drawDecorations() {

    for (const d of decorations) {

        const x =
            d.x -
            state.cameraX;

        if (
            x < -30 ||
            x > W + 30
        ) {
            continue;
        }

        const groundY =
            H - 105;


        if (d.type === "grass") {

            ctx.strokeStyle =
                "#258c3b";

            ctx.lineWidth = 3;

            ctx.beginPath();

            ctx.moveTo(x, groundY);

            ctx.lineTo(
                x - 4,
                groundY - 14
            );

            ctx.moveTo(x, groundY);

            ctx.lineTo(
                x + 5,
                groundY - 18
            );

            ctx.stroke();

        } else {

            ctx.fillStyle =
                "#ffdc45";

            ctx.beginPath();

            ctx.arc(
                x,
                groundY - 9,
                5,
                0,
                Math.PI * 2
            );

            ctx.fill();

            ctx.fillStyle =
                "#ff6b7a";

            ctx.beginPath();

            ctx.arc(
                x - 5,
                groundY - 11,
                4,
                0,
                Math.PI * 2
            );

            ctx.arc(
                x + 5,
                groundY - 11,
                4,
                0,
                Math.PI * 2
            );

            ctx.fill();
        }
    }
}


/* =========================================================
   MOEDAS — GRÁFICO
========================================================= */

function drawCoins() {

    for (const coin of coins) {

        if (coin.collected) {
            continue;
        }

        const x =
            coin.x -
            state.cameraX;

        if (
            x < -30 ||
            x > W + 30
        ) {
            continue;
        }


        coin.spin += .06;

        const scale =
            .65 +
            Math.abs(
                Math.sin(coin.spin)
            ) * .35;


        ctx.save();

        ctx.translate(
            x,
            coin.y
        );

        ctx.scale(
            scale,
            1
        );


        ctx.shadowColor =
            "rgba(255,220,50,.7)";

        ctx.shadowBlur = 15;


        const gradient =
            ctx.createLinearGradient(
                -12,
                0,
                12,
                0
            );

        gradient.addColorStop(
            0,
            "#e3a400"
        );

        gradient.addColorStop(
            .5,
            "#fff16b"
        );

        gradient.addColorStop(
            1,
            "#e5a400"
        );

        ctx.fillStyle = gradient;

        ctx.beginPath();

        ctx.ellipse(
            0,
            0,
            12,
            16,
            0,
            0,
            Math.PI * 2
        );

        ctx.fill();

        ctx.shadowBlur = 0;

        ctx.strokeStyle =
            "#b77b00";

        ctx.lineWidth = 2;

        ctx.stroke();


        ctx.fillStyle =
            "rgba(255,255,255,.55)";

        ctx.beginPath();

        ctx.ellipse(
            -4,
            -5,
            3,
            6,
            0,
            0,
            Math.PI * 2
        );

        ctx.fill();

        ctx.restore();
    }
}


/* =========================================================
   BUBU — GRÁFICO
========================================================= */

function drawEnemies() {

    for (const e of enemies) {

        if (!e.alive) {
            continue;
        }

        const x =
            e.x -
            state.cameraX;

        const y =
            e.y;


        if (
            x < -80 ||
            x > W + 80
        ) {
            continue;
        }


        const bounce =
            Math.sin(e.animation) * 2;


        ctx.save();

        ctx.translate(
            x + e.w / 2,
            y + e.h / 2 + bounce
        );


        /* SOMBRA */

        ctx.fillStyle =
            "rgba(0,0,0,.2)";

        ctx.beginPath();

        ctx.ellipse(
            0,
            25,
            24,
            6,
            0,
            0,
            Math.PI * 2
        );

        ctx.fill();


        /* CHIFRES */

        drawHorn(
            -15,
            -25,
            -25
        );

        drawHorn(
            15,
            -25,
            25
        );


        /* CORPO */

        const body =
            ctx.createLinearGradient(
                -24,
                -18,
                24,
                22
            );

        body.addColorStop(
            0,
            "#ff9a3d"
        );

        body.addColorStop(
            .55,
            "#f07828"
        );

        body.addColorStop(
            1,
            "#c94d1c"
        );

        ctx.fillStyle = body;

        ctx.beginPath();

        ctx.roundRect(
            -25,
            -17,
            50,
            39,
            16
        );

        ctx.fill();


        ctx.strokeStyle =
            "#8d3518";

        ctx.lineWidth = 4;

        ctx.stroke();


        /* OLHOS */

        drawBubuEye(-10);
        drawBubuEye(10);


        /* BOCA */

        ctx.strokeStyle =
            "#6b2415";

        ctx.lineWidth = 3;

        ctx.beginPath();

        ctx.arc(
            0,
            8,
            8,
            0,
            Math.PI
        );

        ctx.stroke();


        ctx.restore();
    }
}


function drawHorn(x, y, rotation) {

    ctx.save();

    ctx.translate(
        x,
        y
    );

    ctx.rotate(
        rotation * Math.PI / 180
    );

    ctx.fillStyle =
        "#ffe05b";

    ctx.strokeStyle =
        "#8d3518";

    ctx.lineWidth = 3;

    ctx.beginPath();

    ctx.moveTo(0, 16);

    ctx.quadraticCurveTo(
        3,
        2,
        12,
        -4
    );

    ctx.quadraticCurveTo(
        7,
        12,
        0,
        16
    );

    ctx.fill();

    ctx.stroke();

    ctx.restore();
}


function drawBubuEye(x) {

    ctx.fillStyle =
        "#fff";

    ctx.beginPath();

    ctx.ellipse(
        x,
        -4,
        7,
        9,
        0,
        0,
        Math.PI * 2
    );

    ctx.fill();

    ctx.fillStyle =
        "#171717";

    ctx.beginPath();

    ctx.arc(
        x,
        -3,
        3,
        0,
        Math.PI * 2
    );

    ctx.fill();
}


/* =========================================================
   NICO — GRÁFICO
========================================================= */

function drawPlayer() {

    if (
        player.invincible > 0 &&
        Math.floor(player.invincible / 7) % 2 === 0
    ) {
        return;
    }


    const x =
        player.x -
        state.cameraX;

    const y =
        player.y;


    ctx.save();

    ctx.translate(
        x + player.w / 2,
        y + player.h / 2
    );


    ctx.scale(
        player.facing,
        1
    );


    /* ANIMAÇÃO */

    const walking =
        Math.abs(player.vx) > .5 &&
        player.grounded;

    const leg =
        walking
            ? Math.sin(player.animation) * 4
            : 0;


    /* SOMBRA */

    ctx.fillStyle =
        "rgba(0,0,0,.2)";

    ctx.beginPath();

    ctx.ellipse(
        0,
        34,
        21,
        6,
        0,
        0,
        Math.PI * 2
    );

    ctx.fill();


    /* PERNAS */

    ctx.fillStyle =
        "#174d91";

    ctx.fillRect(
        -15,
        17 + leg,
        11,
        15
    );

    ctx.fillRect(
        4,
        17 - leg,
        11,
        15
    );


    /* SAPATOS */

    ctx.fillStyle =
        "#542e20";

    ctx.beginPath();

    ctx.roundRect(
        -19,
        27 + leg,
        19,
        9,
        5
    );

    ctx.roundRect(
        1,
        27 - leg,
        19,
        9,
        5
    );

    ctx.fill();


    /* CORPO */

    const shirt =
        ctx.createLinearGradient(
            -22,
            -2,
            22,
            25
        );

    shirt.addColorStop(
        0,
        "#35d875"
    );

    shirt.addColorStop(
        1,
        "#15934c"
    );

    ctx.fillStyle = shirt;

    ctx.beginPath();

    ctx.roundRect(
        -21,
        -3,
        42,
        29,
        10
    );

    ctx.fill();


    ctx.strokeStyle =
        "#116638";

    ctx.lineWidth = 3;

    ctx.stroke();


    /* BRAÇOS */

    ctx.fillStyle =
        "#ffc98f";

    ctx.beginPath();

    ctx.arc(
        -22,
        9,
        7,
        0,
        Math.PI * 2
    );

    ctx.arc(
        22,
        9,
        7,
        0,
        Math.PI * 2
    );

    ctx.fill();


    /* PESCOÇO */

    ctx.fillStyle =
        "#eeb77d";

    ctx.fillRect(
        -6,
        -12,
        12,
        8
    );


    /* CABEÇA */

    ctx.fillStyle =
        "#ffc98f";

    ctx.beginPath();

    ctx.ellipse(
        0,
        -19,
        20,
        22,
        0,
        0,
        Math.PI * 2
    );

    ctx.fill();

    ctx.strokeStyle =
        "#713f2b";

    ctx.lineWidth = 3;

    ctx.stroke();


    /* CABELO */

    ctx.fillStyle =
        "#174ea6";

    ctx.beginPath();

    ctx.arc(
        -11,
        -34,
        11,
        0,
        Math.PI * 2
    );

    ctx.arc(
        0,
        -38,
        12,
        0,
        Math.PI * 2
    );

    ctx.arc(
        12,
        -34,
        10,
        0,
        Math.PI * 2
    );

    ctx.fill();


    /* FRANJA */

    ctx.beginPath();

    ctx.moveTo(
        -19,
        -29
    );

    ctx.lineTo(
        -4,
        -34
    );

    ctx.lineTo(
        1,
        -25
    );

    ctx.lineTo(
        9,
        -34
    );

    ctx.lineTo(
        19,
        -27
    );

    ctx.lineTo(
        19,
        -39
    );

    ctx.lineTo(
        -19,
        -39
    );

    ctx.closePath();

    ctx.fill();


    /* OLHOS */

    ctx.fillStyle =
        "#151515";

    ctx.beginPath();

    ctx.ellipse(
        -7,
        -20,
        3,
        5,
        0,
        0,
        Math.PI * 2
    );

    ctx.ellipse(
        7,
        -20,
        3,
        5,
        0,
        0,
        Math.PI * 2
    );

    ctx.fill();


    /* BRILHO DOS OLHOS */

    ctx.fillStyle =
        "#fff";

    ctx.beginPath();

    ctx.arc(
        -6,
        -22,
        1,
        0,
        Math.PI * 2
    );

    ctx.arc(
        8,
        -22,
        1,
        0,
        Math.PI * 2
    );

    ctx.fill();


    /* BOCA */

    ctx.strokeStyle =
        "#7b3028";

    ctx.lineWidth = 2;

    ctx.beginPath();

    ctx.arc(
        0,
        -11,
        6,
        0,
        Math.PI
    );

    ctx.stroke();


    ctx.restore();
}


/* =========================================================
   BANDEIRA
========================================================= */

function drawFinish() {

    const x =
        finishX -
        state.cameraX;

    if (
        x < -100 ||
        x > W + 100
    ) {
        return;
    }


    const groundY =
        H - 105;


    /* POSTE */

    ctx.fillStyle =
        "#e9e9e9";

    ctx.fillRect(
        x,
        groundY - 170,
        8,
        170
    );


    /* BANDEIRA */

    ctx.fillStyle =
        "#ffdf38";

    ctx.beginPath();

    ctx.moveTo(
        x + 7,
        groundY - 165
    );

    ctx.lineTo(
        x + 80,
        groundY - 145
    );

    ctx.lineTo(
        x + 7,
        groundY - 125
    );

    ctx.closePath();

    ctx.fill();


    ctx.strokeStyle =
        "#a87800";

    ctx.lineWidth = 3;

    ctx.stroke();


    ctx.fillStyle =
        "#fff";

    ctx.font =
        "bold 24px Arial";

    ctx.fillText(
        "N",
        x + 25,
        groundY - 140
    );
}


/* =========================================================
   PARTÍCULAS
========================================================= */

function drawParticles() {

    for (const p of particles) {

        const x =
            p.x -
            state.cameraX;

        ctx.globalAlpha =
            Math.max(0, p.life);

        ctx.fillStyle =
            p.color;

        ctx.beginPath();

        ctx.arc(
            x,
            p.y,
            p.size,
            0,
            Math.PI * 2
        );

        ctx.fill();
    }

    ctx.globalAlpha = 1;
}


/* =========================================================
   DESENHAR TUDO
========================================================= */

function render() {

    ctx.clearRect(
        0,
        0,
        W,
        H
    );


    drawSky();

    drawMountains();

    drawPlatforms();

    drawDecorations();

    drawCoins();

    drawFinish();

    drawEnemies();

    drawPlayer();

    drawParticles();
}


/* =========================================================
   HUD
========================================================= */

function updateHUD() {

    coinsEl.textContent =
        state.coins;

    livesEl.textContent =
        state.lives;

    scoreEl.textContent =
        state.score;

    levelEl.textContent =
        state.level;
}


/* =========================================================
   TELA FINAL
========================================================= */

function showEnd(title, text) {

    endTitle.textContent =
        title;

    endText.textContent =
        text;

    endScreen.classList.remove(
        "hidden"
    );
}


/* =========================================================
   INICIAR
========================================================= */

function startGame() {

    state.running = true;

    state.level = 1;

    state.score = 0;

    state.coins = 0;

    state.lives = MAX_LIVES;

    state.time = 0;

    createLevel(1);

    startScreen.classList.add(
        "hidden"
    );

    endScreen.classList.add(
        "hidden"
    );
}


startButton.addEventListener(
    "click",
    startGame
);


restartButton.addEventListener(
    "click",
    startGame
);


/* =========================================================
   ATUALIZAÇÃO
========================================================= */

function update() {

    if (!state.running) {
        return;
    }


    state.time += .016;


    updatePlayer();

    updateEnemies();

    collectCoins();

    checkFinish();

    updateParticles();

    updateCamera();


    if (state.shake > 0) {
        state.shake *= .85;

        if (state.shake < .2) {
            state.shake = 0;
        }
    }


    updateHUD();
}


/* =========================================================
   LOOP
========================================================= */

let lastTime = 0;

function loop(time) {

    const delta =
        Math.min(
            (time - lastTime) / 16.67,
            2
        );

    lastTime = time;


    /* Atualização baseada em tempo */

    if (state.running) {

        const steps =
            Math.max(
                1,
                Math.round(delta)
            );

        for (
            let i = 0;
            i < steps;
            i++
        ) {
            update();
        }
    }


    /* SHAKE */

    ctx.save();

    if (state.shake > 0) {

        ctx.translate(
            (Math.random() - .5) *
            state.shake,

            (Math.random() - .5) *
            state.shake
        );
    }


    render();

    ctx.restore();


    requestAnimationFrame(loop);
}


/* =========================================================
   COMEÇAR LOOP
========================================================= */

createLevel(1);

requestAnimationFrame(loop);

</script>

</body>
</html>