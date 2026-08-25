<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Super Soccer Arcade 90s</title>
<style>
  * { box-sizing: border-box; }
  body {
    margin: 0;
    background: #0d0f12;
    color: #fff;
    font-family: 'Courier New', Courier, monospace;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
  }

  h1 {
    color: #ffea00;
    margin: 10px 0 5px;
    text-shadow: 2px 2px #ff0055;
    font-size: 24px;
  }

  canvas {
    background: #1e8535;
    border: 4px solid #fff;
    box-shadow: 0 0 30px rgba(0,0,0,0.8);
    image-rendering: pixelated;
  }

  .hud {
    margin-top: 10px;
    font-size: 13px;
    color: #ccc;
  }

  .destaque { color: #ffea00; font-weight: bold; }
</style>
</head>
<body>

<h1>⚡ SUPER SOCCER ARCADE ⚡</h1>
<canvas id="campo" width="800" height="500"></canvas>
<div class="hud">
  WASD: Mover | <span class="destaque">SHIFT: Pique</span> | <span class="destaque">ESPAÇO: Chute Direcionado</span> | <span class="destaque">K: Carrinho / Roubar Bola</span>
</div>

<script>
const canvas = document.getElementById("campo");
const ctx = canvas.getContext("2d");

const AudioContext = window.AudioContext || window.webkitAudioContext;
const audioCtx = new AudioContext();

function tocarSom(freq, tipo = 'sine', duracao = 0.1, vol = 0.2) {
  if (audioCtx.state === 'suspended') audioCtx.resume();
  const osc = audioCtx.createOscillator();
  const gain = audioCtx.createGain();
  osc.type = tipo;
  osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
  gain.gain.setValueAtTime(vol, audioCtx.currentTime);
  gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duracao);
  osc.connect(gain);
  gain.connect(audioCtx.destination);
  osc.start();
  osc.stop(audioCtx.currentTime + duracao);
}

class Jogador {
  constructor(x, y, cor, eJogador = false) {
    this.x = x;
    this.y = y;
    this.vx = 0;
    this.vy = 0;
    this.tam = 18;
    this.cor = cor;
    this.eJogador = eJogador;
    this.estamina = 100;
    this.emCarrinho = false;
    this.tempoCarrinho = 0;
    this.miraAngulo = 0;
  }

  atualizar() {
    if (this.emCarrinho) {
      this.tempoCarrinho--;
      this.x += this.vx * 1.5;
      this.y += this.vy * 1.5;
      if (this.tempoCarrinho <= 0) this.emCarrinho = false;
    } else {
      this.vx *= 0.85;
      this.vy *= 0.85;
      this.x += this.vx;
      this.y += this.vy;
    }

    this.x = Math.max(25, Math.min(775, this.x));
    this.y = Math.max(25, Math.min(475, this.y));
  }
}

const p1 = new Jogador(250, 250, "#00e1ff", true);
const rival = new Jogador(550, 250, "#ff0055", false);
const goleiroEsq = new Jogador(35, 250, "#00ff66", false);
const goleiroDir = new Jogador(765, 250, "#ffaa00", false);

const bola = {
  x: 400, y: 250, vx: 0, vy: 0, raio: 6, atrito: 0.981
};

let teclas = {};
let placarA = 0;
let placarB = 0;
let tremorCam = 0;
let textoGolTempo = 0;

document.addEventListener("keydown", e => {
  teclas[e.key.toLowerCase()] = true;

  if (e.code === "Space" && Math.hypot(p1.x - bola.x, p1.y - bola.y) < 35) {
    bola.vx = Math.cos(p1.miraAngulo) * 14;
    bola.vy = Math.sin(p1.miraAngulo) * 14;
    tocarSom(300, "square", 0.12, 0.3);
  }

  if (e.key.toLowerCase() === "k" && !p1.emCarrinho && p1.estamina > 20) {
    p1.emCarrinho = true;
    p1.tempoCarrinho = 18;
    p1.estamina -= 20;
    tocarSom(150, "sawtooth", 0.15, 0.2);
  }
});

document.addEventListener("keyup", e => {
  teclas[e.key.toLowerCase()] = false;
});

function IA_Controlar(bot, alvoX, alvoY, velMax) {
  if (bot.emCarrinho) return;
  const dx = alvoX - bot.x;
  const dy = alvoY - bot.y;
  const d = Math.hypot(dx, dy);

  if (d > 5) {
    bot.vx += (dx / d) * 0.35;
    bot.vy += (dy / d) * 0.35;
    const vAtual = Math.hypot(bot.vx, bot.vy);
    if (vAtual > velMax) {
      bot.vx = (bot.vx / vAtual) * velMax;
      bot.vy = (bot.vy / vAtual) * velMax;
    }
  }

  // Carrinho da IA
  if (d < 30 && Math.random() < 0.03 && !bot.emCarrinho) {
    bot.emCarrinho = true;
    bot.tempoCarrinho = 15;
    tocarSom(140, "sawtooth", 0.1, 0.2);
  }
}

function resolverColisoes() {
  const todos = [p1, rival, goleiroEsq, goleiroDir];

  todos.forEach(p => {
    const d = Math.hypot(p.x - bola.x, p.y - bola.y);
    const limite = p.tam + bola.raio;

    if (d < limite) {
      const ang = Math.atan2(bola.y - p.y, bola.x - p.x);
      const forca = p.emCarrinho ? 8 : 3.5;
      bola.vx = Math.cos(ang) * forca;
      bola.vy = Math.sin(ang) * forca;
      tocarSom(180, "sine", 0.05, 0.1);
    }
  });
}

function atualizar() {
  // Controle P1 + Pique
  let velAcel = 0.45;
  let velMax = 3.5;

  if (teclas["shift"] && p1.estamina > 1) {
    velMax = 5.2;
    p1.estamina -= 0.8;
  } else if (p1.estamina < 100) {
    p1.estamina += 0.4;
  }

  if (!p1.emCarrinho) {
    if (teclas["w"]) p1.vy -= velAcel;
    if (teclas["s"]) p1.vy += velAcel;
    if (teclas["a"]) p1.vx -= velAcel;
    if (teclas["d"]) p1.vx += velAcel;

    const vP1 = Math.hypot(p1.vx, p1.vy);
    if (vP1 > velMax) {
      p1.vx = (p1.vx / vP1) * velMax;
      p1.vy = (p1.vy / vP1) * velMax;
    }
  }

  // Ângulo de mira baseado no movimento ou na bola
  p1.miraAngulo = Math.atan2(250 - p1.y, 800 - p1.x);

  // IAs
  IA_Controlar(rival, bola.x, bola.y, 3.2);
  IA_Controlar(goleiroEsq, 35, Math.max(170, Math.min(330, bola.y)), 2.8);
  IA_Controlar(goleiroDir, 765, Math.max(170, Math.min(330, bola.y)), 2.8);

  p1.atualizar();
  rival.atualizar();
  goleiroEsq.atualizar();
  goleiroDir.atualizar();

  resolverColisoes();

  // Movimento Bola
  bola.x += bola.vx;
  bola.y += bola.vy;
  bola.vx *= bola.atrito;
  bola.vy *= bola.atrito;

  // Bordas
  if (bola.y < 15 || bola.y > 485) bola.vy *= -0.8;
  if ((bola.x < 15 || bola.x > 785) && (bola.y < 170 || bola.y > 330)) bola.vx *= -0.8;

  // Gol
  if (bola.y >= 170 && bola.y <= 330) {
    if (bola.x > 790) { placarA++; pontuar(); }
    if (bola.x < 10) { placarB++; pontuar(); }
  }

  if (tremorCam > 0) tremorCam--;
}

function pontuar() {
  textoGolTempo = 50;
  tremorCam = 15;
  tocarSom(587, "square", 0.3, 0.4);
  bola.x = 400; bola.y = 250; bola.vx = 0; bola.vy = 0;
  p1.x = 250; p1.y = 250;
  rival.x = 550; rival.y = 250;
}

function desenhar() {
  ctx.save();
  if (tremorCam > 0) {
    ctx.translate((Math.random() - 0.5) * 8, (Math.random() - 0.5) * 8);
  }

  ctx.clearRect(0, 0, 800, 500);

  // Gramado Retro
  ctx.fillStyle = "#1e8535";
  ctx.fillRect(0, 0, 800, 500);
  ctx.fillStyle = "#186e2b";
  for (let i = 0; i < 800; i += 100) {
    ctx.fillRect(i, 0, 50, 500);
  }

  // Linhas
  ctx.strokeStyle = "#fff";
  ctx.lineWidth = 3;
  ctx.strokeRect(10, 10, 780, 480);
  ctx.beginPath();
  ctx.moveTo(400, 10); ctx.lineTo(400, 490);
  ctx.arc(400, 250, 60, 0, Math.PI * 2);
  ctx.stroke();

  // Traves
  ctx.strokeRect(10, 170, 50, 160);
  ctx.strokeRect(740, 170, 50, 160);

  // Bola e Sombra
  ctx.fillStyle = "rgba(0,0,0,0.3)";
  ctx.beginPath();
  ctx.arc(bola.x + 3, bola.y + 4, bola.raio, 0, Math.PI * 2);
  ctx.fill();

  ctx.fillStyle = "#fff";
  ctx.beginPath();
  ctx.arc(bola.x, bola.y, bola.raio, 0, Math.PI * 2);
  ctx.fill();

  // Jogadores
  [p1, rival, goleiroEsq, goleiroDir].forEach(p => {
    ctx.fillStyle = "rgba(0,0,0,0.3)";
    ctx.beginPath();
    ctx.ellipse(p.x, p.y + 8, p.tam, 5, 0, 0, Math.PI * 2);
    ctx.fill();

    ctx.fillStyle = p.cor;
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.tam, 0, Math.PI * 2);
    ctx.fill();
    ctx.lineWidth = 2;
    ctx.strokeStyle = "#fff";
    ctx.stroke();

    // Efeito Carrinho
    if (p.emCarrinho) {
      ctx.fillStyle = "rgba(255,255,255,0.4)";
      ctx.beginPath();
      ctx.arc(p.x - p.vx * 2, p.y - p.vy * 2, p.tam * 0.8, 0, Math.PI * 2);
      ctx.fill();
    }
  });

  // Guia de mira do P1
  if (Math.hypot(p1.x - bola.x, p1.y - bola.y) < 35) {
    ctx.strokeStyle = "#ffea00";
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.moveTo(p1.x, p1.y);
    ctx.lineTo(p1.x + Math.cos(p1.miraAngulo) * 40, p1.y + Math.sin(p1.miraAngulo) * 40);
    ctx.stroke();
  }

  // HUD / Placar
  ctx.fillStyle = "#ffea00";
  ctx.font = "bold 28px monospace";
  ctx.fillText(`${placarA} - ${placarB}`, 360, 45);

  // Barra de Estamina
  ctx.fillStyle = "#333";
  ctx.fillRect(20, 20, 100, 8);
  ctx.fillStyle = p1.estamina > 20 ? "#00e1ff" : "#ff0055";
  ctx.fillRect(20, 20, p1.estamina, 8);

  if (textoGolTempo > 0) {
    ctx.fillStyle = "#ffea00";
    ctx.font = "bold 60px monospace";
    ctx.fillText("GOOOOL!", 270, 260);
    textoGolTempo--;
  }

  ctx.restore();
}

function loop() {
  atualizar();
  desenhar();
  requestAnimationFrame(loop);
}

loop();
</script>
</body>
</html>