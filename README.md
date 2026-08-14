<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<title>NICO: APOCALIPSE</title>

<style>
*{box-sizing:border-box;margin:0;padding:0}
html,body{
    width:100%;height:100%;overflow:hidden;
    background:#070b0d;color:white;
    font-family:Arial,Helvetica,sans-serif;
    touch-action:none;
}
body{user-select:none}
canvas{display:block;width:100%;height:100%}

#hud{
    position:fixed;z-index:20;top:14px;left:50%;
    transform:translateX(-50%);
    display:flex;gap:10px;align-items:center;
    padding:9px 13px;border-radius:16px;
    background:rgba(7,12,15,.78);
    border:1px solid rgba(255,255,255,.18);
    box-shadow:0 8px 30px rgba(0,0,0,.4);
    backdrop-filter:blur(8px);
    font-size:14px;font-weight:900;
}
.hudBox{padding:4px 9px;border-radius:9px;background:rgba(255,255,255,.07)}
#healthText{color:#6cff7b}
#ammoText{color:#ffd84d}
#scoreText{color:#62d8ff}
#waveText{color:#ff8a68}

#crosshair{
    position:fixed;z-index:25;width:24px;height:24px;
    pointer-events:none;display:none;
    transform:translate(-50%,-50%);
}
#crosshair:before,#crosshair:after{
    content:"";position:absolute;background:#fff;
    box-shadow:0 0 5px #000;
}
#crosshair:before{width:24px;height:2px;top:11px;left:0}
#crosshair:after{width:2px;height:24px;left:11px;top:0}
#crosshair i{
    position:absolute;left:9px;top:9px;width:6px;height:6px;
    border:2px solid #ff4545;border-radius:50%;
}

.screen{
    position:fixed;inset:0;z-index:50;
    display:flex;align-items:center;justify-content:center;
    padding:20px;
    background:
      radial-gradient(circle at 50% 35%,rgba(48,84,75,.25),transparent 40%),
      rgba(3,7,9,.86);
}
.panel{
    width:min(700px,94vw);
    padding:34px 26px;
    text-align:center;
    border:1px solid rgba(255,255,255,.16);
    border-radius:26px;
    background:linear-gradient(145deg,rgba(26,39,42,.97),rgba(9,16,18,.98));
    box-shadow:0 30px 90px rgba(0,0,0,.65);
}
.logo{
    font-size:clamp(40px,9vw,76px);
    font-weight:1000;
    letter-spacing:-4px;
    color:#e9ffdb;
    text-shadow:0 4px 0 #276b45,0 12px 30px #000;
}
.subtitle{
    margin:4px 0 22px;
    color:#91d8a8;font-weight:900;
    letter-spacing:3px;
}
.panel p{color:#c7d4d0;line-height:1.6;margin:8px 0}
.btn{
    margin-top:22px;
    padding:15px 32px;
    border:0;border-radius:13px;
    background:linear-gradient(#ffdf55,#e9a927);
    color:#15180d;font-size:19px;font-weight:1000;
    box-shadow:0 5px 0 #916718,0 12px 30px #0008;
    cursor:pointer;
}
.btn:active{transform:translateY(4px);box-shadow:0 1px 0 #916718}
.hidden{display:none!important}

#mobile{
    position:fixed;z-index:30;inset:auto 0 16px 0;
    display:none;justify-content:space-between;
    padding:0 16px;pointer-events:none;
}
.pad{display:flex;gap:9px}
.mbtn{
    width:62px;height:62px;border-radius:50%;
    border:2px solid #ffffffaa;
    background:#10191ccc;color:white;
    font-size:25px;font-weight:900;
    pointer-events:auto;
}
#fireBtn{background:#7d221ecc}
#reloadBtn{background:#755d13cc}

#reloadBar{
    position:fixed;z-index:22;left:50%;bottom:18px;
    transform:translateX(-50%);
    width:180px;height:8px;border-radius:10px;
    background:#0009;display:none;
}
#reloadFill{height:100%;width:0;background:#ffd84d;border-radius:10px}

@media(max-width:800px){
    #hud{top:7px;gap:4px;font-size:11px;padding:7px}
    .hudBox{padding:4px 6px}
    #mobile{display:flex}
    #crosshair{display:block}
}
</style>
</head>

<body>

<canvas id="game"></canvas>
<div id="crosshair"><i></i></div>

<div id="hud">
    <div class="hudBox">❤️ <span id="healthText">100</span></div>
    <div class="hudBox">🔫 <span id="ammoText">12/60</span></div>
    <div class="hudBox">🧟 <span id="waveText">1</span></div>
    <div class="hudBox">⭐ <span id="scoreText">0</span></div>
</div>

<div id="reloadBar"><div id="reloadFill"></div></div>

<div id="mobile">
    <div class="pad">
        <button class="mbtn" id="left">◀</button>
        <button class="mbtn" id="right">▶</button>
    </div>
    <div class="pad">
        <button class="mbtn" id="reloadBtn">R</button>
        <button class="mbtn" id="fireBtn">🔫</button>
    </div>
</div>

<div id="startScreen" class="screen">
    <div class="panel">
        <div class="logo">NICO</div>
        <div class="subtitle">APOCALIPSE</div>

        <p>
            A cidade caiu. Os zumbis tomaram as ruas.
            Nico precisa atravessar a zona infectada,
            sobreviver às hordas e chegar ao abrigo.
        </p>

        <p>
            <b>WASD / SETAS</b> — mover<br>
            <b>MOUSE</b> — mirar e atirar<br>
            <b>R</b> — recarregar<br>
            <b>ESPAÇO</b> — esquivar
        </p>

        <button class="btn" id="startBtn">COMEÇAR A AVENTURA</button>
    </div>
</div>

<div id="endScreen" class="screen hidden">
    <div class="panel">
        <h1 id="endTitle">Fim</h1>
        <p id="endText"></p>
        <button class="btn" id="againBtn">JOGAR NOVAMENTE</button>
    </div>
</div>

<script>
"use strict";

const canvas=document.getElementById("game");
const ctx=canvas.getContext("2d");

let W=innerWidth,H=innerHeight,DPR=1;

function resize(){
    W=innerWidth;H=innerHeight;
    DPR=Math.min(devicePixelRatio||1,2);
    canvas.width=W*DPR;
    canvas.height=H*DPR;
    canvas.style.width=W+"px";
    canvas.style.height=H+"px";
    ctx.setTransform(DPR,0,0,DPR,0,0);
}
addEventListener("resize",resize);
resize();

/* =========================
   ESTADO
========================= */

const game={
    running:false,
    score:0,
    wave:1,
    kills:0,
    cameraX:0,
    cameraY:0,
    shake:0,
    time:0,
    spawnTimer:0,
    boss:false
};

const input={
    left:false,right:false,up:false,down:false,
    mouseX:W/2,mouseY:H/2,
    firing:false
};

const player={
    x:500,y:0,w:34,h:48,
    vx:0,vy:0,
    health:100,
    maxHealth:100,
    speed:3.8,
    angle:0,
    ammo:12,
    maxAmmo:12,
    reserve:60,
    reload:0,
    inv:0,
    dash:0,
    shootCooldown:0
};

const WORLD_W=5200;
const WORLD_H=1500;
const ground={x:0,y:0,w:WORLD_W,h:WORLD_H};

let bullets=[];
let enemies=[];
let particles=[];
let pickups=[];
let decorations=[];

/* =========================
   TECLADO
========================= */

addEventListener("keydown",e=>{
    const k=e.key.toLowerCase();

    if(["w","arrowup"].includes(k))input.up=true;
    if(["s","arrowdown"].includes(k))input.down=true;
    if(["a","arrowleft"].includes(k))input.left=true;
    if(["d","arrowright"].includes(k))input.right=true;

    if(k==="r") reload();

    if(e.code==="Space"){
        dash();
        e.preventDefault();
    }
});

addEventListener("keyup",e=>{
    const k=e.key.toLowerCase();

    if(["w","arrowup"].includes(k))input.up=false;
    if(["s","arrowdown"].includes(k))input.down=false;
    if(["a","arrowleft"].includes(k))input.left=false;
    if(["d","arrowright"].includes(k))input.right=false;
});

canvas.addEventListener("mousemove",e=>{
    input.mouseX=e.clientX;
    input.mouseY=e.clientY;
});

canvas.addEventListener("mousedown",e=>{
    if(e.button===0)input.firing=true;
});

addEventListener("mouseup",e=>{
    if(e.button===0)input.firing=false;
});

canvas.addEventListener("contextmenu",e=>e.preventDefault());

/* =========================
   MOBILE
========================= */

function hold(id,prop){
    const b=document.getElementById(id);

    b.addEventListener("pointerdown",e=>{
        e.preventDefault();
        input[prop]=true;
    });

    ["pointerup","pointercancel","pointerleave"].forEach(ev=>{
        b.addEventListener(ev,e=>{
            input[prop]=false;
        });
    });
}

hold("left","left");
hold("right","right");

document.getElementById("fireBtn").addEventListener("pointerdown",()=>{
    input.firing=true;
});
document.getElementById("fireBtn").addEventListener("pointerup",()=>{
    input.firing=false;
});
document.getElementById("reloadBtn").addEventListener("pointerdown",reload);

/* =========================
   ÁUDIO SIMPLES
========================= */

let audio=null;

function sound(freq,duration,type="square",volume=.035){
    try{
        if(!audio)audio=new AudioContext();
        const o=audio.createOscillator();
        const g=audio.createGain();
        o.type=type;
        o.frequency.value=freq;
        g.gain.value=volume;
        o.connect(g);g.connect(audio.destination);
        o.start();
        g.gain.exponentialRampToValueAtTime(.001,audio.currentTime+duration);
        o.stop(audio.currentTime+duration);
    }catch(_){}
}

/* =========================
   NÍVEL
========================= */

function resetGame(){
    game.score=0;
    game.wave=1;
    game.kills=0;
    game.cameraX=0;
    game.cameraY=0;
    game.time=0;
    game.spawnTimer=0;
    game.boss=false;

    player.x=500;
    player.y=800;
    player.health=100;
    player.ammo=12;
    player.reserve=60;
    player.reload=0;
    player.inv=0;
    player.dash=0;

    bullets=[];
    enemies=[];
    particles=[];
    pickups=[];
    decorations=[];

    for(let i=0;i<260;i++){
        decorations.push({
            x:Math.random()*WORLD_W,
            y:500+Math.random()*600,
            type:Math.random()<.55?"grass":"rock",
            s:.6+Math.random()*1.4
        });
    }

    for(let i=0;i<18;i++){
        pickups.push({
            x:450+i*270+Math.random()*130,
            y:780+Math.random()*120,
            type:Math.random()<.75?"ammo":"med",
            taken:false
        });
    }

    for(let i=0;i<5;i++)spawnZombie(
        900+i*600,
        850+Math.random()*180,
        false
    );

    updateHUD();
}

/* =========================
   ZUMBIS
========================= */

function spawnZombie(x,y,boss=false){
    enemies.push({
        x,y,
        w:boss?70:42,
        h:boss?72:48,
        hp:boss?450:70+game.wave*8,
        maxHp:boss?450:70+game.wave*8,
        speed:boss?0.75:0.75+Math.random()*.65+game.wave*.025,
        damage:boss?18:8,
        boss,
        alive:true,
        hit:0,
        attack:0,
        phase:Math.random()*10
    });
}

function spawnWave(){
    const amount=Math.min(3+game.wave*2,16);

    for(let i=0;i<amount;i++){
        const side=Math.random()<.5?-1:1;
        const x=player.x+side*(550+Math.random()*700);
        const y=780+Math.random()*250;
        spawnZombie(
            Math.max(150,Math.min(WORLD_W-150,x)),
            y,
            false
        );
    }

    if(game.wave%4===0&&!game.boss){
        spawnZombie(
            Math.min(WORLD_W-300,player.x+1000),
            820,
            true
        );
        game.boss=true;
    }

    game.spawnTimer=0;
}

/* =========================
   MOVIMENTO
========================= */

function updatePlayer(){
    let dx=(input.right?1:0)-(input.left?1:0);
    let dy=(input.down?1:0)-(input.up?1:0);

    if(dx||dy){
        const len=Math.hypot(dx,dy)||1;
        dx/=len;dy/=len;
    }

    player.vx+=(dx*player.speed-player.vx)*.18;
    player.vy+=(dy*player.speed-player.vy)*.18;

    player.x+=player.vx;
    player.y+=player.vy;

    player.x=Math.max(80,Math.min(WORLD_W-80,player.x));
    player.y=Math.max(560,Math.min(1120,player.y));

    if(player.inv>0)player.inv--;
    if(player.dash>0)player.dash--;

    if(player.shootCooldown>0)player.shootCooldown--;

    if(player.reload>0){
        player.reload--;
        const max=90;
        document.getElementById("reloadBar").style.display="block";
        document.getElementById("reloadFill").style.width=
            ((max-player.reload)/max*100)+"%";

        if(player.reload===0){
            const need=player.maxAmmo-player.ammo;
            const give=Math.min(need,player.reserve);
            player.ammo+=give;
            player.reserve-=give;
            document.getElementById("reloadBar").style.display="none";
            sound(650,.12,"sine",.04);
        }
    }

    if(player.ammo===0&&player.reserve>0&&player.reload===0)reload();
}

function dash(){
    if(!game.running||player.dash>0)return;

    let dx=(input.right?1:0)-(input.left?1:0);
    let dy=(input.down?1:0)-(input.up?1:0);

    if(!dx&&!dy)dx=1;

    const len=Math.hypot(dx,dy)||1;

    player.x+=dx/len*120;
    player.y+=dy/len*120;

    player.x=Math.max(80,Math.min(WORLD_W-80,player.x));
    player.y=Math.max(560,Math.min(1120,player.y));

    player.inv=30;
    player.dash=70;

    burst(player.x,player.y,"#7fffd4",16);
    game.shake=5;
}

/* =========================
   TIRO
========================= */

function worldMouse(){
    return {
        x:input.mouseX+game.cameraX,
        y:input.mouseY+game.cameraY
    };
}

function shoot(){
    if(!game.running||player.reload>0||player.shootCooldown>0)return;

    if(player.ammo<=0){
        reload();
        return;
    }

    const m=worldMouse();

    const angle=Math.atan2(m.y-player.y,m.x-player.x);

    player.angle=angle;
    player.ammo--;
    player.shootCooldown=9;

    const speed=13;

    bullets.push({
        x:player.x+Math.cos(angle)*28,
        y:player.y+Math.sin(angle)*28,
        vx:Math.cos(angle)*speed,
        vy:Math.sin(angle)*speed,
        life:70,
        damage:34
    });

    burst(
        player.x+Math.cos(angle)*28,
        player.y+Math.sin(angle)*28,
        "#ffd45c",4
    );

    sound(120,.06,"sawtooth",.025);
    game.shake=2;
}

function reload(){
    if(
        !game.running||
        player.reload>0||
        player.ammo>=player.maxAmmo||
        player.reserve<=0
    )return;

    player.reload=90;
    sound(220,.08,"square",.025);
}

/* =========================
   BALAS
========================= */

function updateBullets(){
    for(let i=bullets.length-1;i>=0;i--){
        const b=bullets[i];

        b.x+=b.vx;
        b.y+=b.vy;
        b.life--;

        let remove=
            b.life<=0||
            b.x<0||b.x>WORLD_W||
            b.y<450||b.y>1250;

        for(const e of enemies){
            if(!e.alive)continue;

            if(
                b.x>e.x-e.w/2&&
                b.x<e.x+e.w/2&&
                b.y>e.y-e.h/2&&
                b.y<e.y+e.h/2
            ){
                e.hp-=b.damage;
                e.hit=5;
                burst(b.x,b.y,e.boss?"#ff3d4d":"#b8ff65",6);
                remove=true;

                if(e.hp<=0){
                    e.alive=false;
                    game.kills++;
                    game.score+=e.boss?2500:100;
                    burst(e.x,e.y,e.boss?"#ff493d":"#74e36c",e.boss?45:18);

                    if(e.boss){
                        game.boss=false;
                        sound(90,.45,"sawtooth",.08);
                    }else{
                        sound(180,.09,"square",.025);
                    }
                }

                break;
            }
        }

        if(remove)bullets.splice(i,1);
    }
}

/* =========================
   ZUMBIS
========================= */

function updateEnemies(){
    for(const e of enemies){
        if(!e.alive)continue;

        e.phase+=.06;
        if(e.hit>0)e.hit--;
        if(e.attack>0)e.attack--;

        const dx=player.x-e.x;
        const dy=player.y-e.y;
        const d=Math.hypot(dx,dy)||1;

        if(d>48){
            e.x+=dx/d*e.speed;
            e.y+=dy/d*e.speed;
        }else if(e.attack===0&&player.inv<=0){
            player.health-=e.damage;
            e.attack=45;
            player.inv=35;
            game.shake=8;
            burst(player.x,player.y,"#ff5364",14);
            sound(80,.12,"sawtooth",.04);

            if(player.health<=0)endGame(false);
        }
    }

    enemies=enemies.filter(e=>e.alive||Math.random()<.99);
}

/* =========================
   ITENS
========================= */

function updatePickups(){
    for(const p of pickups){
        if(p.taken)continue;

        if(Math.hypot(player.x-p.x,player.y-p.y)<35){
            p.taken=true;

            if(p.type==="ammo"){
                player.reserve=Math.min(120,player.reserve+24);
                game.score+=25;
                sound(700,.08,"sine",.03);
            }else{
                player.health=Math.min(player.maxHealth,player.health+30);
                game.score+=50;
                sound(850,.12,"sine",.03);
            }

            burst(p.x,p.y,p.type==="ammo"?"#ffd84d":"#ff5364",12);
        }
    }
}

/* =========================
   PARTÍCULAS
========================= */

function burst(x,y,color,n){
    for(let i=0;i<n;i++){
        const a=Math.random()*Math.PI*2;
        const s=1+Math.random()*5;

        particles.push({
            x,y,
            vx:Math.cos(a)*s,
            vy:Math.sin(a)*s,
            life:.5+Math.random()*.6,
            size:2+Math.random()*5,
            color
        });
    }
}

function updateParticles(){
    for(let i=particles.length-1;i>=0;i--){
        const p=particles[i];

        p.x+=p.vx;
        p.y+=p.vy;
        p.vx*=.97;
        p.vy*=.97;
        p.life-=.025;

        if(p.life<=0)particles.splice(i,1);
    }
}

/* =========================
   CÂMERA
========================= */

function updateCamera(){
    const tx=player.x-W*.42;
    const ty=player.y-H*.52;

    game.cameraX+=(tx-game.cameraX)*.09;
    game.cameraY+=(ty-game.cameraY)*.09;

    game.cameraX=Math.max(0,Math.min(WORLD_W-W,game.cameraX));
    game.cameraY=Math.max(430,Math.min(700,game.cameraY));
}

/* =========================
   DESENHO DO MUNDO
========================= */

function screenX(x){return x-game.cameraX}
function screenY(y){return y-game.cameraY}

function drawBackground(){
    const sky=ctx.createLinearGradient(0,0,0,H);
    sky.addColorStop(0,"#182b38");
    sky.addColorStop(.55,"#31565b");
    sky.addColorStop(1,"#101918");

    ctx.fillStyle=sky;
    ctx.fillRect(0,0,W,H);

    /* lua */
    ctx.fillStyle="#dce9bf";
    ctx.shadowColor="#b9ffbf";
    ctx.shadowBlur=30;
    ctx.beginPath();
    ctx.arc(W-130,100,42,0,Math.PI*2);
    ctx.fill();
    ctx.shadowBlur=0;

    /* neblina */
    for(let i=0;i<8;i++){
        const x=(i*350-game.cameraX*.15)%1500-200;
        ctx.fillStyle="rgba(170,220,204,.07)";
        ctx.beginPath();
        ctx.ellipse(x,H*.55,250,70,0,0,Math.PI*2);
        ctx.fill();
    }
}

function drawGround(){
    ctx.fillStyle="#151d1b";
    ctx.fillRect(0,screenY(540),W,H);

    for(let x=-100;x<W+100;x+=90){
        const wx=x+game.cameraX;
        const y=screenY(850+Math.sin(wx*.01)*10);

        ctx.fillStyle="rgba(0,0,0,.16)";
        ctx.fillRect(x,y,55,4);
    }

    /* estrada */
    ctx.fillStyle="#222b28";
    ctx.fillRect(0,screenY(760),W,270);

    ctx.strokeStyle="#c3a950";
    ctx.lineWidth=5;
    ctx.setLineDash([35,30]);
    ctx.beginPath();
    ctx.moveTo(0,screenY(890));
    ctx.lineTo(W,screenY(890));
    ctx.stroke();
    ctx.setLineDash([]);
}

function drawDecorations(){
    for(const d of decorations){
        const x=screenX(d.x);
        const y=screenY(d.y);

        if(x<-80||x>W+80)continue;

        if(d.type==="grass"){
            ctx.strokeStyle="#42734c";
            ctx.lineWidth=3;
            ctx.beginPath();
            ctx.moveTo(x,y);
            ctx.lineTo(x-5,y-18*d.s);
            ctx.moveTo(x,y);
            ctx.lineTo(x+6,y-22*d.s);
            ctx.stroke();
        }else{
            ctx.fillStyle="#333c38";
            ctx.beginPath();
            ctx.ellipse(x,y,18*d.s,9*d.s,0,0,Math.PI*2);
            ctx.fill();
        }
    }
}

/* =========================
   PRÉDIOS
========================= */

function drawBuildings(){
    const start=Math.floor(game.cameraX/170)-2;

    for(let i=start;i<start+Math.ceil(W/170)+4;i++){
        const x=i*170-game.cameraX;
        const h=100+Math.abs(i*73%230);

        ctx.fillStyle=i%2?"#1b292d":"#202e32";
        ctx.fillRect(x,H*.55-h,130,h);

        for(let yy=H*.55-h+25;yy<H*.55-15;yy+=35){
            for(let xx=x+18;xx<x+115;xx+=30){
                ctx.fillStyle=(i+Math.floor(yy))%3===0?
                    "#d1b85b":"#34494b";
                ctx.fillRect(xx,yy,13,17);
            }
        }

        ctx.fillStyle="#11191a";
        ctx.fillRect(x+45,H*.55-h-35,40,35);
    }
}

/* =========================
   PLAYER
========================= */

function drawPlayer(){
    if(player.inv>0&&Math.floor(player.inv/4)%2===0)return;

    const x=screenX(player.x);
    const y=screenY(player.y);

    const moving=Math.abs(player.vx)+Math.abs(player.vy)>.4;
    const bob=moving?Math.sin(game.time*12)*2:0;

    ctx.save();
    ctx.translate(x,y+bob);

    /* sombra */
    ctx.fillStyle="#0008";
    ctx.beginPath();
    ctx.ellipse(0,27,22,7,0,0,Math.PI*2);
    ctx.fill();

    /* pernas */
    ctx.fillStyle="#25333a";
    ctx.fillRect(-14,5,11,22);
    ctx.fillRect(4,5,11,22);

    /* botas */
    ctx.fillStyle="#101416";
    ctx.fillRect(-18,22,17,8);
    ctx.fillRect(3,22,19,8);

    /* corpo */
    const body=ctx.createLinearGradient(-20,-15,20,18);
    body.addColorStop(0,"#2d9a63");
    body.addColorStop(1,"#13533d");

    ctx.fillStyle=body;
    ctx.beginPath();
    ctx.roundRect(-20,-17,40,34,9);
    ctx.fill();

    ctx.strokeStyle="#102d24";
    ctx.lineWidth=3;
    ctx.stroke();

    /* mochila */
    ctx.fillStyle="#59472b";
    ctx.fillRect(14,-10,10,25);

    /* pescoço */
    ctx.fillStyle="#b87855";
    ctx.fillRect(-7,-23,14,9);

    /* cabeça */
    ctx.fillStyle="#c98b68";
    ctx.beginPath();
    ctx.ellipse(0,-31,18,20,0,0,Math.PI*2);
    ctx.fill();

    ctx.strokeStyle="#59392e";
    ctx.stroke();

    /* cabelo */
    ctx.fillStyle="#172d35";
    ctx.beginPath();
    ctx.arc(-11,-45,10,0,Math.PI*2);
    ctx.arc(0,-48,12,0,Math.PI*2);
    ctx.arc(12,-44,10,0,Math.PI*2);
    ctx.fill();

    /* olhos */
    ctx.fillStyle="#101416";
    ctx.beginPath();
    ctx.arc(-6,-31,2.5,0,Math.PI*2);
    ctx.arc(7,-31,2.5,0,Math.PI*2);
    ctx.fill();

    /* arma */
    ctx.save();
    ctx.rotate(player.angle);

    ctx.fillStyle="#161b1c";
    ctx.fillRect(9,-5,28,9);
    ctx.fillStyle="#7c8580";
    ctx.fillRect(25,-7,15,5);
    ctx.fillStyle="#573f2d";
    ctx.fillRect(12,3,9,10);

    ctx.restore();

    ctx.restore();
}

/* =========================
   ZUMBI
========================= */

function drawZombie(e){
    const x=screenX(e.x);
    const y=screenY(e.y);
    const wob=Math.sin(e.phase)*3;

    ctx.save();
    ctx.translate(x,y+wob);

    /* sombra */
    ctx.fillStyle="#0008";
    ctx.beginPath();
    ctx.ellipse(0,e.h*.45,e.w*.42,7,0,0,Math.PI*2);
    ctx.fill();

    if(e.boss){
        ctx.fillStyle="#6c1e25";
        ctx.beginPath();
        ctx.roundRect(-35,-38,70,70,18);
        ctx.fill();

        ctx.strokeStyle="#e04c42";
        ctx.lineWidth=4;
        ctx.stroke();

        ctx.fillStyle="#8f3029";
        ctx.fillRect(-25,10,50,22);

        ctx.fillStyle="#c98a65";
        ctx.beginPath();
        ctx.arc(0,-28,23,0,Math.PI*2);
        ctx.fill();

        ctx.fillStyle="#ffcc45";
        ctx.beginPath();
        ctx.arc(-8,-30,4,0,Math.PI*2);
        ctx.arc(9,-30,4,0,Math.PI*2);
        ctx.fill();
    }else{
        ctx.fillStyle=e.hit?"#d8ff91":"#6b9d55";
        ctx.beginPath();
        ctx.roundRect(-21,-15,42,38,13);
        ctx.fill();

        ctx.strokeStyle="#304d2d";
        ctx.lineWidth=3;
        ctx.stroke();

        ctx.fillStyle="#9dbd75";
        ctx.beginPath();
        ctx.arc(0,-25,17,0,Math.PI*2);
        ctx.fill();

        ctx.fillStyle="#151818";
        ctx.beginPath();
        ctx.arc(-6,-27,3,0,Math.PI*2);
        ctx.arc(7,-27,3,0,Math.PI*2);
        ctx.fill();

        ctx.strokeStyle="#481e1c";
        ctx.lineWidth=3;
        ctx.beginPath();
        ctx.moveTo(-7,-17);
        ctx.lineTo(8,-13);
        ctx.stroke();

        /* braços */
        ctx.strokeStyle="#6f9b5b";
        ctx.lineWidth=8;
        ctx.beginPath();
        ctx.moveTo(-17,-2);
        ctx.lineTo(-31,9);
        ctx.moveTo(17,-2);
        ctx.lineTo(31,9);
        ctx.stroke();
    }

    /* barra de vida */
    if(e.hp<e.maxHp){
        const bw=e.boss?80:52;

        ctx.fillStyle="#000b";
        ctx.fillRect(-bw/2,-e.h/2-15,bw,6);

        ctx.fillStyle=e.boss?"#ff3f42":"#7cff61";
        ctx.fillRect(
            -bw/2,
            -e.h/2-15,
            bw*Math.max(0,e.hp/e.maxHp),
            6
        );
    }

    ctx.restore();
}

/* =========================
   BALAS / ITENS
========================= */

function drawBullets(){
    for(const b of bullets){
        ctx.strokeStyle="#ffe56b";
        ctx.lineWidth=4;
        ctx.beginPath();
        ctx.moveTo(screenX(b.x-b.vx*.5),screenY(b.y-b.vy*.5));
        ctx.lineTo(screenX(b.x),screenY(b.y));
        ctx.stroke();
    }
}

function drawPickups(){
    for(const p of pickups){
        if(p.taken)continue;

        const x=screenX(p.x);
        const y=screenY(p.y+Math.sin(game.time*3+p.x)*4);

        ctx.fillStyle=p.type==="ammo"?"#ffd34d":"#ff5364";
        ctx.shadowColor=ctx.fillStyle;
        ctx.shadowBlur=15;

        ctx.beginPath();
        ctx.arc(x,y,12,0,Math.PI*2);
        ctx.fill();
        ctx.shadowBlur=0;

        ctx.fillStyle="#18201c";
        ctx.font="bold 13px Arial";
        ctx.textAlign="center";
        ctx.textBaseline="middle";
        ctx.fillText(p.type==="ammo"?"A":"+ ",x,y);
    }
}

function drawParticles(){
    for(const p of particles){
        ctx.globalAlpha=Math.max(0,p.life);
        ctx.fillStyle=p.color;

        ctx.beginPath();
        ctx.arc(screenX(p.x),screenY(p.y),p.size,0,Math.PI*2);
        ctx.fill();
    }
    ctx.globalAlpha=1;
}

/* =========================
   SAÍDA
========================= */

function drawExit(){
    const x=screenX(WORLD_W-220);
    const y=screenY(790);

    ctx.fillStyle="#11191a";
    ctx.fillRect(x-65,y-100,130,170);

    ctx.strokeStyle="#55d67c";
    ctx.lineWidth=5;
    ctx.strokeRect(x-65,y-100,130,170);

    ctx.fillStyle="#55d67c";
    ctx.font="bold 18px Arial";
    ctx.textAlign="center";
    ctx.fillText("ABRIGO",x,y-115);

    if(player.x>WORLD_W-280){
        endGame(true);
    }
}

/* =========================
   RENDER
========================= */

function render(){
    ctx.clearRect(0,0,W,H);

    drawBackground();
    drawBuildings();
    drawGround();
    drawDecorations();
    drawPickups();
    drawExit();

    for(const e of enemies){
        if(e.alive)drawZombie(e);
    }

    drawBullets();
    drawPlayer();
    drawParticles();
}

/* =========================
   HUD
========================= */

function updateHUD(){
    document.getElementById("healthText").textContent=
        Math.max(0,Math.ceil(player.health));

    document.getElementById("ammoText").textContent=
        player.ammo+"/"+player.reserve;

    document.getElementById("waveText").textContent=
        game.wave;

    document.getElementById("scoreText").textContent=
        game.score;
}

/* =========================
   FINAL
========================= */

function endGame(win){
    if(!game.running)return;

    game.running=false;

    document.getElementById("endTitle").textContent=
        win?"ABRIGO ALCANÇADO! 🏆":"NICO FOI DERROTADO 💀";

    document.getElementById("endText").textContent=
        win
        ?`Você sobreviveu ao apocalipse! Pontuação: ${game.score}. Zumbis derrotados: ${game.kills}.`
        :`A cidade venceu desta vez. Pontuação: ${game.score}. Zumbis derrotados: ${game.kills}.`;

    document.getElementById("endScreen").classList.remove("hidden");
}

/* =========================
   UPDATE
========================= */

function update(){
    if(!game.running)return;

    game.time+=1/60;

    updatePlayer();

    if(input.firing)shoot();

    updateBullets();
    updateEnemies();
    updatePickups();
    updateParticles();
    updateCamera();

    game.spawnTimer++;

    if(
        enemies.filter(e=>e.alive).length<2&&
        game.spawnTimer>100
    ){
        game.wave++;

        if(game.wave>12){
            endGame(true);
            return;
        }

        spawnWave();
        sound(330,.18,"sine",.035);
    }

    if(game.shake>0){
        game.shake*=.88;
        if(game.shake<.2)game.shake=0;
    }

    updateHUD();
}

/* =========================
   LOOP
========================= */

let last=0;

function loop(t){
    const dt=Math.min((t-last)/16.67,2);
    last=t;

    if(game.running){
        const steps=Math.max(1,Math.round(dt));
        for(let i=0;i<steps;i++)update();
    }

    ctx.save();

    if(game.shake){
        ctx.translate(
            (Math.random()-.5)*game.shake,
            (Math.random()-.5)*game.shake
        );
    }

    render();
    ctx.restore();

    requestAnimationFrame(loop);
}

/* =========================
   START
========================= */

function startGame(){
    try{
        if(audio&&audio.state==="suspended")audio.resume();
    }catch(_){}

    resetGame();

    game.running=true;

    document.getElementById("startScreen").classList.add("hidden");
    document.getElementById("endScreen").classList.add("hidden");

    sound(440,.12,"sine",.035);
}

document.getElementById("startBtn").addEventListener("click",startGame);
document.getElementById("againBtn").addEventListener("click",startGame);

resetGame();
requestAnimationFrame(loop);
</script>

</body>
</html>