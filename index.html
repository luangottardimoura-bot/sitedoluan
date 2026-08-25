<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Futebol Retrô - Física Realista</title>
<style>
  * { box-sizing: border-box; }
  body {
    margin: 0;
    background: #090c10;
    color: #fff;
    font-family: 'Courier New', Courier, monospace;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
  }

  h1 {
    color: #00ff66;
    margin: 10px 0 5px;
    text-shadow: 0 0 10px rgba(0, 255, 102, 0.4);
    font-size: 24px;
  }

  canvas {
    background: #125424;
    border: 4px solid #fff;
    box-shadow: 0 0 25px rgba(0, 0, 0, 0.7);
    image-rendering: pixelated;
  }

  .hud {
    margin-top: 10px;
    font-size: 13px;
    color: #aaa;
  }

  .destaque { color: #ffcc00; font-weight: bold; }
</style>
</head>
<body>

<h1>⚽ FUTEBOL RETRÔ REALISTA ⚽</h1>
<canvas id="campo" width="800" height="500"></canvas>
<div class="hud">
  WASD: Mover | <span class="destaque">Segure ESPAÇO: Carregar Chute</span> | <span class="destaque">E: Passe Rápido</span>
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

// Entidade com física de inércia
class Atleta {
  constructor(x, y, cor, eGoleiro = false) {
    this.x = x;
    this.y = y;
    this.vx = 0;
    this.vy = 0;
    this.massa = 1.2;
    this.tam = 22;
    this.acel = 0.45;
    this.atrito = 0.86;
    this.cor = cor;
    this.eGoleiro = eGoleiro;
  }

  atualizar() {
    this.vx *= this.atrito;
    this.vy *= this.atrito;
    this.x += this.vx;
    this.y += this.vy;

    // Limites do campo
    this.x = Math.max(20, Math.min(780, this.x));
    this.y = Math.max(20, Math.min(480, this.y));
  }
}

const jogador = new Atleta(220, 250, "#00a2ff");
const goleiroAzul = new Atleta(30, 250, "#00f3ff", true);

const oponente = new Atleta(580, 250, "#ff3344");
const goleiroVermelho = new Atleta(770, 250, "#ff8800", true);

const bola = {
  x: 400,
  y: 250,
  vx: 0,
  vy: 0,
  raio: 7,
  massa: 0.4,
  atrito: 0.982,
  rastro: []
};

let teclas = {};
let golsJogador = 0;
let golsOponente = 0;
let forcaChute = 0;
let carregandoChute = false;
let textoGolTempo = 0;

document.addEventListener("keydown", e => {
  teclas[e.key.toLowerCase()] = true;

  if (e.code === "Space" && !carregandoChute) {
    carregandoChute = true;
    forcaChute = 0;
  }
  if (e.key.toLowerCase() === "e") {
    executarChute(6);
  }
});

document.addEventListener("keyup", e => {
  teclas[e.key.toLowerCase()] = false;

  if (e.code === "Space" && carregandoChute) {
    executarChute(4 + (forcaChute / 100) * 12);
    carregandoChute = false;
    forcaChute = 0;
  }
});

function dist(a, b) {
  return Math.hypot(a.x - b.x, a.y - b.y);
}

function executarChute(potencia) {
  if (dist(jogador, bola) < 38) {
    const ang = Math.atan2(bola.y - jogador.y, bola.x - jogador.x);
    bola.vx = Math.cos(ang) * potencia + jogador.vx * 0.5;
    bola.vy = Math.sin(ang) * potencia + jogador.vy * 0.5;
    tocarSom(180 + potencia * 15, "square", 0.12, 0.3);
  }
}

function resolverColisao(p, b) {
  const d = dist(p, b);
  const raioMin = p.tam / 2 + b.raio;

  if (d < raioMin) {
    const ang = Math.atan2(b.y - p.y, b.x - p.x);
    const sobreposicao = raioMin - d;

    // Empurra a bola para fora do corpo do jogador
    b.x += Math.cos(ang) * sobreposicao;
    b.y += Math.sin(ang) * sobreposicao;

    // Transferência de momento linear (física realista de impacto)
    const velRelativaX = b.vx - p.vx;
    const velRelativaY = b.vy - p.vy;
    const velEmAngulo = velRelativaX * Math.cos(ang) + velRelativaY * Math.sin(ang);

    if (velEmAngulo < 0) {
      const impulso = (2 * velEmAngulo) / (p.massa + b.massa);
      b.vx -= impulso * p.massa * Math.cos(ang) * 0.8;
      b.vy -= impulso * p.massa * Math.sin(ang) * 0.8;
      p.vx += impulso * b.massa * Math.cos(ang) * 0.2;
      p.vy += impulso * b.massa * Math.sin(ang) * 0.2;
      tocarSom(120, "sine", 0.04, 0.1);
    }
  }
}

function IA_Goleiro(goleiro, xFixa) {
  goleiro.x = xFixa;
  const alvoY = Math.max(180, Math.min(320, bola.y));
  const dy = alvoY - goleiro.y;
  goleiro.vy += Math.sign(dy) * 0.3;
}

function IA_Oponente(p) {
  const dy = bola.y - p.y;
  const dx = bola.x - p.x;
  p.vx += Math.sign(dx) * 0.25;
  p.vy += Math.sign(dy) * 0.25;

  if (dist(p, bola) < 32 && Math.random() < 0.08) {
    const ang = Math.atan2(250 - p.y, 0 - p.x);
    bola.vx = Math.cos(ang) * 11;
    bola.vy = Math.sin(ang) * 11;
    tocarSom(220, "square", 0.1, 0.2);
  }
}

function atualizar() {
  // Carregamento da barra de força
  if (carregandoChute) {
    forcaChute = Math.min(100, forcaChute + 3);
  }

  // Controles de movimento do Jogador (Aceleração contínua)
  if (teclas["w"] || teclas["arrowup"]) jogador.vy -= jogador.acel;
  if (teclas["s"] || teclas["arrowdown"]) jogador.vy += jogador.acel;
  if (teclas["a"] || teclas["arrowleft"]) jogador.vx -= jogador.acel;
  if (teclas["d"] || teclas["arrowright"]) jogador.vx += jogador.acel;

  // Atualizar Entidades
  [jogador, oponente, goleiroAzul, goleiroVermelho].forEach(p => p.atualizar());

  IA_Oponente(oponente);
  IA_Goleiro(goleiroAzul, 25);
  IA_Goleiro(goleiroVermelho, 775);

  // Colisões Físicas
  [jogador, oponente, goleiroAzul, goleiroVermelho].forEach(p => resolverColisao(p, bola));

  // Física da Bola
  if (Math.hypot(bola.vx, bola.vy) > 1.5) {
    bola.rastro.push({ x: bola.x, y: bola.y, a: 0.5 });
  }
  if (bola.rastro.length > 6) bola.rastro.shift();

  bola.x += bola.vx;
  bola.y += bola.vy;
  bola.vx *= bola.atrito;
  bola.vy *= bola.atrito;

  // Paredes
  if (bola.y - bola.raio < 5 || bola.y + bola.raio > 495) {
    bola.vy *= -0.8;
    tocarSom(150, "triangle", 0.05, 0.15);
  }

  // Traves laterais
  if ((bola.x - bola.raio < 5 || bola.x + bola.raio > 795) && (bola.y <= 180 || bola.y >= 320)) {
    bola.vx *= -0.8;
    tocarSom(150, "triangle", 0.05, 0.15);
  }

  // Detecção de Gol
  if (bola.y > 180 && bola.y < 320) {
    if (bola.x > 792) {
      golsJogador++;
      textoGolTempo = 50;
      tocarSom(523, "sine", 0.25, 0.3);
      reset();
    }
    if (bola.x < 8) {
      golsOponente++;
      textoGolTempo = 50;
      tocarSom(130, "sawtooth", 0.25, 0.3);
      reset();
    }
  }
}

function reset() {
  bola.x = 400; bola.y = 250; bola.vx = 0; bola.vy = 0; bola.rastro = [];
  jogador.x = 220; jogador.y = 250; jogador.vx = 0; jogador.vy = 0;
  oponente.x = 580; oponente.y = 250; oponente.vx = 0; oponente.vy = 0;
}

function desenhar() {
  ctx.clearRect(0, 0, 800, 500);

  // Gramado com textura de listras
  for (let i = 0; i < 800; i += 80) {
    ctx.fillStyle = (i / 80) % 2 === 0 ? "#125424" : "#145d28";
    ctx.fillRect(i, 0, 80, 500);
  }

  // Marcações de campo
  ctx.strokeStyle = "rgba(255, 255, 255, 0.7)";
  ctx.lineWidth = 3;
  ctx.strokeRect(5, 5, 790, 490);

  ctx.beginPath();
  ctx.moveTo(400, 5); ctx.lineTo(400, 495);
  ctx.arc(400, 250, 65, 0, Math.PI * 2);
  ctx.stroke();

  ctx.strokeRect(5, 150, 90, 200);
  ctx.strokeRect(705, 150, 90, 200);

  // Traves
  ctx.fillStyle = "#fff";
  ctx.fillRect(0, 180, 6, 140);
  ctx.fillRect(794, 180, 6, 140);

  // Rastro
  bola.rastro.forEach(p => {
    ctx.fillStyle = `rgba(255, 255, 255, ${p.a})`;
    ctx.beginPath();
    ctx.arc(p.x, p.y, bola.raio * 0.7, 0, Math.PI * 2);
    ctx.fill();
    p.a -= 0.08;
  });

  // Bola com sombra
  ctx.fillStyle = "rgba(0,0,0,0.25)";
  ctx.beginPath();
  ctx.ellipse(bola.x, bola.y + 6, 7, 3, 0, 0, Math.PI * 2);
  ctx.fill();

  ctx.fillStyle = "#fff";
  ctx.beginPath();
  ctx.arc(bola.x, bola.y, bola.raio, 0, Math.PI * 2);
  ctx.fill();
  ctx.strokeStyle = "#000";
  ctx.lineWidth = 1;
  ctx.stroke();

  // Jogadores com sombras dinâmicas
  [jogador, oponente, goleiroAzul, goleiroVermelho].forEach(p => {
    ctx.fillStyle = "rgba(0,0,0,0.3)";
    ctx.beginPath();
    ctx.ellipse(p.x, p.y + 10, 10, 4, 0, 0, Math.PI * 2);
    ctx.fill();

    ctx.fillStyle = p.cor;
    ctx.fillRect(p.x - p.tam / 2, p.y - p.tam / 2, p.tam, p.tam);
    ctx.strokeStyle = "#fff";
    ctx.strokeRect(p.x - p.tam / 2, p.y - p.tam / 2, p.tam, p.tam);
  });

  // Placar HUD
  ctx.fillStyle = "#fff";
  ctx.font = "bold 26px monospace";
  ctx.fillText(`${golsJogador}  -  ${golsOponente}`, 355, 40);

  // Barra de carregamento do chute
  if (carregandoChute) {
    ctx.fillStyle = "rgba(0,0,0,0.5)";
    ctx.fillRect(jogador.x - 20, jogador.y - 25, 40, 6);
    ctx.fillStyle = forcaChute > 70 ? "#ff3300" : "#ffcc00";
    ctx.fillRect(jogador.x - 20, jogador.y - 25, (forcaChute / 100) * 40, 6);
  }

  // Texto de Gol
  if (textoGolTempo > 0) {
    ctx.fillStyle = "#ffcc00";
    ctx.font = "bold 55px monospace";
    ctx.fillText("G O L !", 310, 260);
    textoGolTempo--;
  }
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