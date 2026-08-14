<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no">
<title>Nico: Zona Zumbi</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
}

html,body{
    width:100%;
    height:100%;
    overflow:hidden;
    background:#07100d;
    font-family:Arial,Helvetica,sans-serif;
}

body{
    touch-action:none;
}

#game{
    position:relative;
    width:100vw;
    height:100vh;
    overflow:hidden;
}

canvas{
    display:block;
    width:100%;
    height:100%;
}

/* ================= HUD ================= */

#hud{
    position:fixed;
    z-index:20;
    top:14px;
    left:50%;
    transform:translateX(-50%);

    display:flex;
    gap:12px;
    align-items:center;

    padding:10px 16px;

    color:white;
    background:rgba(5,12,10,.82);

    border:2px solid rgba(255,255,255,.18);
    border-radius:16px;

    box-shadow:0 8px 30px rgba(0,0,0,.4);

    font-weight:900;
    font-size:15px;

    white-space:nowrap;
}

.hudItem{
    display:flex;
    gap:5px;
    align-items:center;
}

/* ================= TELA ================= */

.screen{
    position:fixed;
    inset:0;
    z-index:50;

    display:flex;
    align-items:center;
    justify-content:center;

    padding:20px;

    background:
        radial-gradient(
            circle at center,
            rgba(45,90,65,.5),
            rgba(2,7,6,.96)
        );
}

.panel{
    width:min(680px,94vw);

    padding:38px 28px;

    color:white;
    text-align:center;

    background:
        linear-gradient(
            145deg,
            rgba(25,55,42,.97),
            rgba(7,18,14,.98)
        );

    border:2px solid rgba(115,255,150,.25);
    border-radius:25px;

    box-shadow:
        0 30px 80px rgba(0,0,0,.65),
        inset 0 1px rgba(255,255,255,.1);
}

.logo{
    font-size:clamp(42px,9vw,78px);
    font-weight:1000;
    letter-spacing:-4px;

    color:#d8ff55;

    text-shadow:
        0 5px 0 #427316,
        0 10px 30px rgba(0,0,0,.7);
}

.subtitle{
    margin:5px 0 25px;
    color:#a7dcb5;
    font-size:21px;
    font-weight:bold;
}

.panel p{
    color:#dceee2;
    line-height:1.6;
}

.button{
    margin-top:25px;

    padding:15px 35px;

    border:0;
    border-radius:13px;

    background:linear-gradient(#dcff63,#87c92c);
    color:#17220d;

    font-size:20px;
    font-weight:1000;

    cursor:pointer;

    box-shadow:
        0 5px 0 #477b19,
        0 12px 25px rgba(0,0,0,.35);
}

.button:active{
    transform:translateY(4px);
    box-shadow:0 1px 0 #477b19;
}

/* ================= CONTROLES ================= */

#controls{
    position:fixed;
    z-index:30;
    left:0;
    right:0;
    bottom:18px;

    display:none;
    justify-content:space-between;

    padding:0 18px;

    pointer-events:none;
}

.group{
    display:flex;
    gap:10px;
}

.control{
    width:64px;
    height:64px;

    border-radius:50%;

    border:3px solid rgba(255,255,255,.65);

    background:rgba(5,15,10,.65);

    color:white;
    font-size:25px;
    font-weight:bold;

    pointer-events:auto;
}

.control:active{
    transform:scale(.9);
    background:rgba(130,255,130,.25);
}

.hidden{
    display:none!important;
}

@media(max-width:800px){
    #controls{
        display:flex;
    }

    #hud{
        top:8px;
        gap:7px;
        padding:7px 9px;
        font-size:12px;
    }
}
</style>
</head>

<body>

<div id="game">

<canvas id="canvas"></canvas>

<div id="hud">
    <div class="hudItem">❤️ <span id="lives">3</span></div>
    <div class="hudItem">🔫 <span id="ammo">30</span></div>
    <div class="hudItem">🧟 <span id="kills">0</span></div>
    <div class="hudItem">⭐ <span id="score">0</span></div>
    <div class="hudItem">🏆 <span id="mission">0%</span></div>
</div>

<div id="start" class="screen">
    <div class="panel">

        <div class="logo">NICO</div>

        <div class="subtitle">
            ZONA ZUMBI
        </div>

        <p>
            A cidade foi tomada por zumbis!
            Explore o mapa, sobreviva aos ataques,
            encontre munição e elimine todos os inimigos.
        </p>

        <p style="margin-top:12px">
            <b>← →</b> mover •
            <b>↑</b> pular •
            <b>ESPAÇO</b> atirar
        </p>

        <button id="startButton" class="button">
            COMEÇAR MISSÃO
        </button>

    </div>
</div>

<div id="end" class="screen hidden">
    <div class="panel">

        <h1 id="endTitle">
            MISSÃO CONCLUÍDA!
        </h1>

        <p id="endText"></p>

        <button id="restart" class="button">
            JOGAR NOVAMENTE
        </button>

    </div>
</div>

<div id="controls">

    <div class="group">
        <button class="control" id="left">◀</button>
        <button class="control" id="right">▶</button>
    </div>

    <div class="group">
        <button class="control" id="shoot">🔫</button>
        <button class="control" id="jump">▲</button>
    </div>

</div>

</div>

<script>
"use strict";

/* =====================================================
   NICO — ZONA ZUMBI
===================================================== */

const canvas=document.getElementById("canvas");
const ctx=canvas.getContext("2d");

let W=innerWidth;
let H=innerHeight;

function resize(){

    W=innerWidth;
    H=innerHeight;

    const dpr=Math.min(devicePixelRatio||1,2);

    canvas.width=W*dpr;
    canvas.height=H*dpr;

    canvas.style.width=W+"px";
    canvas.style.height=H+"px";

    ctx.setTransform(dpr,0,0,dpr,0,0);
}

addEventListener("resize",resize);
resize();


/* =====================================================
   ESTADO
===================================================== */

const state={
    running:false,
    camera:0,
    score:0,
    kills:0,
    lives:3,
    ammo:30,
    maxAmmo:30,
    shake:0,
    time:0
};


/* =====================================================
   CONTROLES
===================================================== */

const keys={
    left:false,
    right:false,
    jump:false,
    shoot:false
};

let jumpPressed=false;
let shootPressed=false;

addEventListener("keydown",e=>{

    const k=e.key.toLowerCase();

    if(k==="arrowleft"||k==="a"){
        keys.left=true;
        e.preventDefault();
    }

    if(k==="arrowright"||k==="d"){
        keys.right=true;
        e.preventDefault();
    }

    if(k==="arrowup"||k==="w"){
        if(!keys.jump) jumpPressed=true;
        keys.jump=true;
        e.preventDefault();
    }

    if(k===" "){
        shootPressed=true;
        keys.shoot=true;
        e.preventDefault();
    }
});

addEventListener("keyup",e=>{

    const k=e.key.toLowerCase();

    if(k==="arrowleft"||k==="a")
        keys.left=false;

    if(k==="arrowright"||k==="d")
        keys.right=false;

    if(k==="arrowup"||k==="w")
        keys.jump=false;

    if(k===" ")
        keys.shoot=false;
});


function button(id,key){

    const b=document.getElementById(id);

    b.addEventListener("pointerdown",e=>{

        e.preventDefault();

        if(key==="jump")
            jumpPressed=true;

        if(key==="shoot")
            shootPressed=true;

        keys[key]=true;

        try{
            b.setPointerCapture(e.pointerId);
        }catch(_){}
    });

    const release=()=>{
        keys[key]=false;
    };

    b.addEventListener("pointerup",release);
    b.addEventListener("pointercancel",release);
    b.addEventListener("lostpointercapture",release);
}

button("left","left");
button("right","right");
button("jump","jump");
button("shoot","shoot");


/* =====================================================
   JOGADOR
===================================================== */

const player={

    x:160,
    y:0,

    w:42,
    h:62,

    vx:0,
    vy:0,

    speed:6,
    jump:15,

    grounded:false,
    facing:1,

    shootCooldown:0,
    invincible:0,

    animation:0
};


/* =====================================================
   MUNDO
===================================================== */

const WORLD=5200;

const ground={
    y:0,
    h:300
};

let platforms=[];
let zombies=[];
let bullets=[];
let particles=[];
let ammoBoxes=[];
let medkits=[];
let trees=[];
let buildings=[];


/* =====================================================
   CRIAR MUNDO
===================================================== */

function createWorld(){

    platforms=[];
    zombies=[];
    bullets=[];
    particles=[];
    ammoBoxes=[];
    medkits=[];
    trees=[];
    buildings=[];

    ground.y=H-105;

    player.x=160;
    player.y=ground.y-player.h;

    player.vx=0;
    player.vy=0;

    /* PLATAFORMAS */

    const p=[
        [500,ground.y-100,220],
        [900,ground.y-180,190],
        [1220,ground.y-110,250],
        [1600,ground.y-210,220],
        [1950,ground.y-120,260],
        [2350,ground.y-230,220],
        [2700,ground.y-130,280],
        [3150,ground.y-220,230],
        [3500,ground.y-120,300],
        [4000,ground.y-210,250],
        [4400,ground.y-120,300]
    ];

    for(const a of p){

        platforms.push({
            x:a[0],
            y:a[1],
            w:a[2],
            h:28
        });
    }


    /* ZUMBIS */

    const positions=[
        650,1050,1350,1750,
        2100,2500,2900,3300,
        3700,4100,4500,4800
    ];

    positions.forEach((x,i)=>{

        zombies.push({

            x,
            y:ground.y-55,

            w:46,
            h:52,

            vx:0,
            vy:0,

            speed:1.0+Math.random()*.55,

            health:i%4===0?2:1,

            alive:true,

            animation:Math.random()*10
        });
    });


    /* CAIXAS DE MUNIÇÃO */

    [780,1450,2300,3200,3950,4700]
    .forEach(x=>{
        ammoBoxes.push({
            x,
            y:ground.y-35,
            collected:false
        });
    });


    /* KITS */

    [1100,2850,4300]
    .forEach(x=>{
        medkits.push({
            x,
            y:ground.y-35,
            collected:false
        });
    });


    /* ÁRVORES */

    for(let x=250;x<WORLD;x+=180+Math.random()*130){

        trees.push({
            x,
            y:ground.y,
            scale:.8+Math.random()*.6
        });
    }


    /* PRÉDIOS */

    for(let x=350;x<WORLD;x+=500){

        buildings.push({
            x,
            w:130+Math.random()*100,
            h:100+Math.random()*140
        });
    }
}


/* =====================================================
   COLISÃO
===================================================== */

function overlap(a,b){

    return(
        a.x<b.x+b.w &&
        a.x+a.w>b.x &&
        a.y<b.y+b.h &&
        a.y+a.h>b.y
    );
}


/* =====================================================
   PARTÍCULAS
===================================================== */

function particles(x,y,color,count=10){

    for(let i=0;i<count;i++){

        particles.list.push({
            x,
            y,

            vx:(Math.random()-.5)*6,
            vy:-Math.random()*5,

            life:1,

            size:2+Math.random()*5,

            color
        });
    }
}

particles.list=[];

function updateParticles(){

    for(let i=particles.list.length-1;i>=0;i--){

        const p=particles.list[i];

        p.x+=p.vx;
        p.y+=p.vy;

        p.vy+=.18;
        p.life-=.035;

        if(p.life<=0)
            particles.list.splice(i,1);
    }
}

function drawParticles(){

    for(const p of particles.list){

        ctx.globalAlpha=p.life;
        ctx.fillStyle=p.color;

        ctx.beginPath();

        ctx.arc(
            p.x-state.camera,
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
   JOGADOR
===================================================== */

function updatePlayer(){

    if(player.invincible>0)
        player.invincible--;

    if(keys.left){

        player.vx-=.75;
        player.facing=-1;

    }else if(keys.right){

        player.vx+=.75;
        player.facing=1;

    }else{

        player.vx*=.8;
    }

    player.vx=Math.max(
        -player.speed,
        Math.min(player.speed,player.vx)
    );


    if(jumpPressed && player.grounded){

        player.vy=-player.jump;
        player.grounded=false;

        particles(
            player.x+player.w/2,
            player.y+player.h,
            "#d7ead9",
            8
        );
    }

    jumpPressed=false;


    player.vy+=.72;

    if(player.vy>18)
        player.vy=18;


    player.x+=player.vx;

    player.x=Math.max(
        0,
        Math.min(WORLD-player.w,player.x)
    );


    const oldBottom=player.y+player.h;

    player.y+=player.vy;

    player.grounded=false;


    /* CHÃO */

    if(player.y+player.h>=ground.y){

        player.y=ground.y-player.h;

        player.vy=0;
        player.grounded=true;
    }


    /* PLATAFORMAS */

    for(const p of platforms){

        if(
            player.x+player.w<=p.x ||
            player.x>=p.x+p.w
        ) continue;

        if(
            player.vy>=0 &&
            oldBottom<=p.y &&
            player.y+player.h>=p.y
        ){

            player.y=p.y-player.h;
            player.vy=0;
            player.grounded=true;
        }
    }


    /* TIRO */

    if(
        (keys.shoot||shootPressed) &&
        player.shootCooldown<=0
    ){

        shoot();
        shootPressed=false;
    }

    if(player.shootCooldown>0)
        player.shootCooldown--;


    player.animation+=Math.abs(player.vx)*.18;
}


/* =====================================================
   TIRO
===================================================== */

function shoot(){

    if(state.ammo<=0)
        return;

    state.ammo--;

    player.shootCooldown=10;

    bullets.push({

        x:player.facing===1
            ? player.x+player.w
            : player.x,

        y:player.y+27,

        vx:player.facing*13,

        life:70
    });

    state.shake=2;

    particles(
        player.x+(player.facing===1?player.w:0),
        player.y+27,
        "#ffd85a",
        5
    );
}


/* =====================================================
   BALAS
===================================================== */

function updateBullets(){

    for(let i=bullets.length-1;i>=0;i--){

        const b=bullets[i];

        b.x+=b.vx;
        b.life--;

        let remove=false;


        for(const z of zombies){

            if(!z.alive)
                continue;

            const hit={

                x:b.x-4,
                y:b.y-3,
                w:8,
                h:6
            };

            if(overlap(hit,z)){

                z.health--;

                particles(
                    b.x,
                    b.y,
                    "#d84b38",
                    8
                );

                remove=true;

                if(z.health<=0){

                    z.alive=false;

                    state.kills++;
                    state.score+=250;

                    state.shake=7;

                    particles(
                        z.x+z.w/2,
                        z.y+z.h/2,
                        "#70d66d",
                        18
                    );
                }

                break;
            }
        }


        if(
            remove ||
            b.life<=0 ||
            b.x<0 ||
            b.x>WORLD
        ){

            bullets.splice(i,1);
        }
    }
}


/* =====================================================
   ZUMBIS
===================================================== */

function updateZombies(){

    for(const z of zombies){

        if(!z.alive)
            continue;

        z.animation+=.08;


        const distance=
            player.x-z.x;


        /* IA */

        if(Math.abs(distance)<650){

            z.vx=
                Math.sign(distance)*
                z.speed;

        }else{

            z.vx=
                Math.sin(z.animation)*.7;
        }


        z.x+=z.vx;


        z.vy+=.7;

        z.y+=z.vy;


        if(z.y+z.h>=ground.y){

            z.y=ground.y-z.h;
            z.vy=0;
        }


        /* ATAQUE */

        if(
            overlap(player,z) &&
            player.invincible<=0
        ){

            state.lives--;

            player.invincible=100;

            state.shake=12;

            particles(
                player.x+player.w/2,
                player.y+player.h/2,
                "#e94b4b",
                15
            );

            player.x-=
                Math.sign(distance)*70;

            if(state.lives<=0){

                state.running=false;

                finish(
                    "NICO FOI DERROTADO 💀",
                    "Os zumbis dominaram a cidade."
                );
            }
        }
    }
}


/* =====================================================
   ITENS
===================================================== */

function updateItems(){

    const pr={
        x:player.x,
        y:player.y,
        w:player.w,
        h:player.h
    };


    for(const box of ammoBoxes){

        if(
            !box.collected &&
            overlap(pr,{
                x:box.x,
                y:box.y,
                w:30,
                h:30
            })
        ){

            box.collected=true;

            state.ammo=
                Math.min(
                    state.maxAmmo,
                    state.ammo+15
                );

            state.score+=50;

            particles(
                box.x,
                box.y,
                "#ffd34d",
                12
            );
        }
    }


    for(const kit of medkits){

        if(
            !kit.collected &&
            overlap(pr,{
                x:kit.x,
                y:kit.y,
                w:30,
                h:30
            })
        ){

            kit.collected=true;

            state.lives=
                Math.min(3,state.lives+1);

            state.score+=100;

            particles(
                kit.x,
                kit.y,
                "#ff6475",
                12
            );
        }
    }
}


/* =====================================================
   CÂMERA
===================================================== */

function updateCamera(){

    const target=
        player.x-W*.38;

    state.camera+=
        (target-state.camera)*.1;

    state.camera=Math.max(
        0,
        Math.min(WORLD-W,state.camera)
    );
}


/* =====================================================
   FUNDO
===================================================== */

function drawBackground(){

    const sky=
        ctx.createLinearGradient(
            0,0,0,H
        );

    sky.addColorStop(0,"#071b28");
    sky.addColorStop(.55,"#12352f");
    sky.addColorStop(1,"#182419");

    ctx.fillStyle=sky;

    ctx.fillRect(0,0,W,H);


    /* LUA */

    ctx.fillStyle="#d9e9c8";

    ctx.beginPath();

    ctx.arc(
        W-130,
        100,
        43,
        0,
        Math.PI*2
    );

    ctx.fill();


    /* NÉVOA */

    ctx.fillStyle="rgba(190,220,190,.07)";

    for(let i=0;i<8;i++){

        ctx.beginPath();

        ctx.ellipse(
            i*180-(state.camera*.15%180),
            H-180,
            180,
            45,
            0,
            0,
            Math.PI*2
        );

        ctx.fill();
    }


    /* PRÉDIOS */

    for(const b of buildings){

        const x=b.x-state.camera*.35;

        if(x<-250||x>W+250)
            continue;

        ctx.fillStyle="#172522";

        ctx.fillRect(
            x,
            ground.y-b.h,
            b.w,
            b.h
        );


        for(let wy=b.y||0;wy<b.h-20;wy+=35){

            for(let wx=15;wx<b.w-15;wx+=35){

                ctx.fillStyle=
                    Math.random()>.25
                    ? "#b7b75c"
                    : "#273b34";

                ctx.fillRect(
                    x+wx,
                    ground.y-b.h+wy+10,
                    12,
                    16
                );
            }
        }
    }
}


/* =====================================================
   TERRENO
===================================================== */

function drawGround(){

    const x=-state.camera;

    ctx.fillStyle="#304a2c";

    ctx.fillRect(
        x,
        ground.y,
        WORLD,
        300
    );


    ctx.fillStyle="#3e7834";

    ctx.fillRect(
        x,
        ground.y,
        WORLD,
        14
    );


    /* ESTRADA */

    ctx.fillStyle="#222925";

    ctx.fillRect(
        x,
        ground.y+50,
        WORLD,
        90
    );


    ctx.strokeStyle="#b2ad63";
    ctx.lineWidth=5;

    for(
        let xx=0;
        xx<WORLD;
        xx+=100
    ){

        ctx.beginPath();

        ctx.moveTo(
            xx-state.camera,
            ground.y+93
        );

        ctx.lineTo(
            xx+55-state.camera,
            ground.y+93
        );

        ctx.stroke();
    }
}


/* =====================================================
   PLATAFORMAS
===================================================== */

function drawPlatforms(){

    for(const p of platforms){

        const x=p.x-state.camera;

        ctx.fillStyle="#465c38";

        ctx.fillRect(
            x,
            p.y,
            p.w,
            p.h
        );

        ctx.fillStyle="#6b9146";

        ctx.fillRect(
            x,
            p.y,
            p.w,
            7
        );
    }
}


/* =====================================================
   ÁRVORES
===================================================== */

function drawTrees(){

    for(const t of trees){

        const x=t.x-state.camera*.75;

        if(x<-100||x>W+100)
            continue;

        ctx.save();

        ctx.translate(
            x,
            t.y
        );

        ctx.scale(
            t.scale,
            t.scale
        );

        ctx.fillStyle="#4a2c1d";

        ctx.fillRect(
            -8,
            -75,
            16,
            75
        );

        ctx.fillStyle="#163b24";

        ctx.beginPath();

        ctx.arc(
            0,
            -90,
            35,
            0,
            Math.PI*2
        );

        ctx.arc(
            -25,
            -70,
            28,
            0,
            Math.PI*2
        );

        ctx.arc(
            25,
            -70,
            28,
            0,
            Math.PI*2
        );

        ctx.fill();

        ctx.restore();
    }
}


/* =====================================================
   ITENS
===================================================== */

function drawItems(){

    for(const box of ammoBoxes){

        if(box.collected)
            continue;

        const x=box.x-state.camera;

        ctx.fillStyle="#c99b31";

        ctx.fillRect(
            x,
            box.y,
            30,
            30
        );

        ctx.strokeStyle="#ffe47c";

        ctx.lineWidth=3;

        ctx.strokeRect(
            x+2,
            box.y+2,
            26,
            26
        );

        ctx.fillStyle="#191e18";

        ctx.font="bold 16px Arial";
        ctx.textAlign="center";

        ctx.fillText(
            "AM",
            x+15,
            box.y+20
        );
    }


    for(const kit of medkits){

        if(kit.collected)
            continue;

        const x=kit.x-state.camera;

        ctx.fillStyle="#e9e9df";

        ctx.fillRect(
            x,
            kit.y,
            30,
            25
        );

        ctx.fillStyle="#d92e40";

        ctx.fillRect(
            x+12,
            kit.y+4,
            6,
            17
        );

        ctx.fillRect(
            x+6,
            kit.y+10,
            18,
            6
        );
    }
}


/* =====================================================
   ZUMBI — GRÁFICO
===================================================== */

function drawZombies(){

    for(const z of zombies){

        if(!z.alive)
            continue;

        const x=z.x-state.camera;

        const bob=
            Math.sin(z.animation)*2;

        ctx.save();

        ctx.translate(
            x+z.w/2,
            z.y+z.h/2+bob
        );


        /* sombra */

        ctx.fillStyle="rgba(0,0,0,.3)";

        ctx.beginPath();

        ctx.ellipse(
            0,
            29,
            25,
            6,
            0,
            0,
            Math.PI*2
        );

        ctx.fill();


        /* pernas */

        ctx.fillStyle="#24382e";

        ctx.fillRect(
            -15,
            13,
            11,
            18
        );

        ctx.fillRect(
            4,
            13,
            11,
            18
        );


        /* corpo */

        const body=
            ctx.createLinearGradient(
                -22,-5,22,25
            );

        body.addColorStop(
            0,
            "#6fa85c"
        );

        body.addColorStop(
            1,
            "#315c38"
        );

        ctx.fillStyle=body;

        ctx.beginPath();

        ctx.roundRect(
            -22,
            -6,
            44,
            31,
            8
        );

        ctx.fill();


        /* cabeça */

        ctx.fillStyle="#82b96a";

        ctx.beginPath();

        ctx.arc(
            0,
            -20,
            20,
            0,
            Math.PI*2
        );

        ctx.fill();


        /* cabelo */

        ctx.fillStyle="#18231c";

        ctx.beginPath();

        ctx.arc(
            -11,
            -36,
            10,
            0,
            Math.PI*2
        );

        ctx.arc(
            0,
            -39,
            11,
            0,
            Math.PI*2
        );

        ctx.arc(
            11,
            -35,
            9,
            0,
            Math.PI*2
        );

        ctx.fill();


        /* olhos */

        ctx.fillStyle="#d9ff66";

        ctx.beginPath();

        ctx.arc(
            -7,
            -20,
            4,
            0,
            Math.PI*2
        );

        ctx.arc(
            7,
            -20,
            4,
            0,
            Math.PI*2
        );

        ctx.fill();


        /* boca */

        ctx.strokeStyle="#371d19";
        ctx.lineWidth=3;

        ctx.beginPath();

        ctx.moveTo(-8,-8);
        ctx.lineTo(8,-8);

        ctx.stroke();


        /* braços */

        ctx.strokeStyle="#82b96a";
        ctx.lineWidth=9;
        ctx.lineCap="round";

        ctx.beginPath();

        ctx.moveTo(-18,0);
        ctx.lineTo(-32,10);

        ctx.moveTo(18,0);
        ctx.lineTo(32,7);

        ctx.stroke();


        ctx.restore();
    }
}


/* =====================================================
   NICO — GRÁFICO
===================================================== */

function drawPlayer(){

    if(
        player.invincible>0 &&
        Math.floor(player.invincible/7)%2===0
    )
        return;

    const x=player.x-state.camera;
    const y=player.y;

    ctx.save();

    ctx.translate(
        x+player.w/2,
        y+player.h/2
    );

    ctx.scale(player.facing,1);


    /* sombra */

    ctx.fillStyle="rgba(0,0,0,.35)";

    ctx.beginPath();

    ctx.ellipse(
        0,
        35,
        23,
        6,
        0,
        0,
        Math.PI*2
    );

    ctx.fill();


    const walk=
        player.grounded &&
        Math.abs(player.vx)>.4;

    const leg=
        walk
        ? Math.sin(player.animation)*5
        : 0;


    /* pernas */

    ctx.fillStyle="#172f5c";

    ctx.fillRect(
        -15,
        16+leg,
        11,
        17
    );

    ctx.fillRect(
        4,
        16-leg,
        11,
        17
    );


    /* botas */

    ctx.fillStyle="#38231b";

    ctx.beginPath();

    ctx.roundRect(
        -20,
        27+leg,
        20,
        10,
        5
    );

    ctx.roundRect(
        0,
        27-leg,
        20,
        10,
        5
    );

    ctx.fill();


    /* corpo */

    ctx.fillStyle="#2ba85d";

    ctx.beginPath();

    ctx.roundRect(
        -21,
        -4,
        42,
        29,
        10
    );

    ctx.fill();


    ctx.strokeStyle="#164e2d";
    ctx.lineWidth=3;
    ctx.stroke();


    /* mochila */

    ctx.fillStyle="#70462b";

    ctx.fillRect(
        -25,
        -1,
        7,
        23
    );


    /* braços */

    ctx.strokeStyle="#ffc98f";
    ctx.lineWidth=8;
    ctx.lineCap="round";

    ctx.beginPath();

    ctx.moveTo(-19,4);
    ctx.lineTo(-27,14);

    ctx.stroke();


    ctx.beginPath();

    ctx.moveTo(19,4);
    ctx.lineTo(28,5);

    ctx.stroke();


    /* arma */

    ctx.strokeStyle="#202522";
    ctx.lineWidth=6;

    ctx.beginPath();

    ctx.moveTo(17,4);
    ctx.lineTo(43,4);

    ctx.stroke();

    ctx.fillStyle="#111";

    ctx.fillRect(
        38,
        1,
        11,
        6
    );


    /* pescoço */

    ctx.fillStyle="#e7af76";

    ctx.fillRect(
        -6,
        -13,
        12,
        8
    );


    /* cabeça */

    ctx.fillStyle="#ffc98f";

    ctx.beginPath();

    ctx.ellipse(
        0,
        -21,
        20,
        22,
        0,
        0,
        Math.PI*2
    );

    ctx.fill();


    ctx.strokeStyle="#713f2b";
    ctx.lineWidth=3;
    ctx.stroke();


    /* cabelo */

    ctx.fillStyle="#174ea6";

    ctx.beginPath();

    ctx.arc(
        -11,
        -36,
        11,
        0,
        Math.PI*2
    );

    ctx.arc(
        0,
        -39,
        12,
        0,
        Math.PI*2
    );

    ctx.arc(
        12,
        -35,
        10,
        0,
        Math.PI*2
    );

    ctx.fill();


    /* franja */

    ctx.beginPath();

    ctx.moveTo(-18,-31);
    ctx.lineTo(-5,-35);
    ctx.lineTo(0,-27);
    ctx.lineTo(9,-35);
    ctx.lineTo(18,-28);
    ctx.lineTo(18,-40);
    ctx.lineTo(-18,-40);

    ctx.closePath();

    ctx.fill();


    /* olhos */

    ctx.fillStyle="#111";

    ctx.beginPath();

    ctx.ellipse(
        -7,-21,3,5,
        0,0,Math.PI*2
    );

    ctx.ellipse(
        7,-21,3,5,
        0,0,Math.PI*2
    );

    ctx.fill();


    /* brilho */

    ctx.fillStyle="#fff";

    ctx.beginPath();

    ctx.arc(
        -6,-23,1,
        0,Math.PI*2
    );

    ctx.arc(
        8,-23,1,
        0,Math.PI*2
    );

    ctx.fill();


    /* boca */

    ctx.strokeStyle="#7b3028";
    ctx.lineWidth=2;

    ctx.beginPath();

    ctx.arc(
        0,-12,6,
        0,Math.PI
    );

    ctx.stroke();


    ctx.restore();
}


/* =====================================================
   BALAS
===================================================== */

function drawBullets(){

    for(const b of bullets){

        ctx.fillStyle="#ffe36a";

        ctx.shadowColor="#ffb300";
        ctx.shadowBlur=10;

        ctx.beginPath();

        ctx.arc(
            b.x-state.camera,
            b.y,
            4,
            0,
            Math.PI*2
        );

        ctx.fill();

        ctx.shadowBlur=0;
    }
}


/* =====================================================
   RENDER
===================================================== */

function render(){

    ctx.clearRect(
        0,0,W,H
    );

    drawBackground();
    drawTrees();
    drawGround();
    drawPlatforms();
    drawItems();
    drawZombies();
    drawBullets();
    drawPlayer();
    drawParticles();
}


/* =====================================================
   HUD
===================================================== */

function updateHUD(){

    document.getElementById("lives")
        .textContent=state.lives;

    document.getElementById("ammo")
        .textContent=state.ammo;

    document.getElementById("kills")
        .textContent=state.kills;

    document.getElementById("score")
        .textContent=state.score;


    const percent=Math.min(
        100,
        Math.floor(
            player.x/(WORLD-300)*100
        )
    );

    document.getElementById("mission")
        .textContent=percent+"%";
}


/* =====================================================
   FINAL
===================================================== */

function finish(title,text){

    document.getElementById("endTitle")
        .textContent=title;

    document.getElementById("endText")
        .textContent=text;

    document.getElementById("end")
        .classList.remove("hidden");
}


/* =====================================================
   UPDATE
===================================================== */

function update(){

    if(!state.running)
        return;

    state.time+=.016;

    updatePlayer();
    updateBullets();
    updateZombies();
    updateItems();
    updateParticles();
    updateCamera();


    /* VITÓRIA */

    const remaining=zombies.filter(
        z=>z.alive
    ).length;

    if(
        player.x>WORLD-350 &&
        remaining===0
    ){

        state.running=false;

        finish(
            "MISSÃO CONCLUÍDA! 🏆",
            `Nico sobreviveu à Zona Zumbi! ${state.kills} zumbis derrotados e ${state.score} pontos.`
        );
    }


    /* caiu */

    if(player.y>H+200){

        state.lives--;

        player.x=Math.max(
            100,
            player.x-250
        );

        player.y=ground.y-player.h;

        player.vy=0;

        if(state.lives<=0){

            state.running=false;

            finish(
                "FIM DE JOGO 💀",
                "Nico não conseguiu sobreviver."
            );
        }
    }


    if(state.shake>0){

        state.shake*=.85;

        if(state.shake<.2)
            state.shake=0;
    }

    updateHUD();
}


/* =====================================================
   LOOP
===================================================== */

let last=0;

function loop(time){

    const delta=
        Math.min(
            (time-last)/16.67,
            2
        );

    last=time;

    if(state.running){

        const steps=Math.max(
            1,
            Math.round(delta)
        );

        for(let i=0;i<steps;i++)
            update();
    }


    ctx.save();

    if(state.shake>0){

        ctx.translate(
            (Math.random()-.5)*state.shake,
            (Math.random()-.5)*state.shake
        );
    }

    render();

    ctx.restore();

    requestAnimationFrame(loop);
}


/* =====================================================
   START
===================================================== */

function startGame(){

    state.running=true;
    state.score=0;
    state.kills=0;
    state.lives=3;
    state.ammo=30;
    state.camera=0;
    state.shake=0;

    createWorld();

    document.getElementById("start")
        .classList.add("hidden");

    document.getElementById("end")
        .classList.add("hidden");

    updateHUD();
}


document.getElementById("startButton")
    .addEventListener("click",startGame);

document.getElementById("restart")
    .addEventListener("click",startGame);


createWorld();
updateHUD();

requestAnimationFrame(loop);

</script>

</body>
</html>