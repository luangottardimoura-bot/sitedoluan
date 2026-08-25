<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Arena de Tiro 2P - Turbo</title>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #06060c;
  color: white;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  user-select: none;
}

h1 {
  color: #00f3ff;
  text-shadow: 0 0 15px #00f3ff;
  margin: 15px 0 5px;
  letter-spacing: 2px;
}

canvas {
  background: #0d0d1a;
  border: 3px solid #00f3ff;
  box-shadow: 0 0 35px rgba(0, 243, 255, 0.3);
  max-width: 95vw;
}

.info {
  margin-bottom: 10px;
  color: #aaa;
  font-size: 14px;
}

.azul { color: #00f3ff; font-weight: bold; }
.vermelho { color: #ff1744; font-weight: bold; }
</style>
</head>

<body>

<h1>ARENA DE TIRO 2P - TURBO</h1>

<div class="info">
  <span class="azul">P1: WASD + F</span>
  &nbsp; | &nbsp;
  <span class="vermelho">P2: SETAS + L</span>
</div>

<canvas id="game" width="900" height="550"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

// --- SISTEMA DE ÁUDIO SINTETIZADO ---
const AudioCtx = window.AudioContext || window.webkitAudioContext;
let audioCtx = null;

function initAudio() {
  if (!audioCtx) audioCtx = new AudioCtx();
  if (audioCtx.state === 'suspended') audioCtx.resume();
}

function tocarSom(freq, tipo, duracao, vol = 0.1) {
  if (!audioCtx) return;
  try {
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
  } catch(e) {}
}

const teclas = {};

document.addEventListener("keydown", e => {
  initAudio();
  teclas[e.key.toLowerCase()] = true;

  if (e.key.toLowerCase() === "f") atirar(jogador1);
  if (e.key.toLowerCase() === "l") atirar(jogador2);
  if (e.key.toLowerCase() === "r" && jogoAcabou) reiniciar();
});

document.addEventListener("keyup", e => {
  teclas[e.key.toLowerCase()] = false;
});

const jogador1 = {
  id: 1, x: 120, y: 275, tamanho: 24, velocidade: 4,
  cor: "#00f3ff", pontos: 0, direcaoX: 1, direcaoY: 0,
  cooldown: 0, recuo: 0, escudos: 0, tiroTriplo: 0
};

const jogador2 = {
  id: 2, x: 780, y: 275, tamanho: 24, velocidade: 4,
  cor: "#ff1744", pontos: 0, direcaoX: -1, direcaoY: 0,
  cooldown: 0, recuo: 0, escudos: 0, tiroTriplo: 0
};

let balas = [];
let particulas = [];
let powerups = [];
let jogoAcabou = false;
let vencedor = "";
let screenShake = 0;
let tempoPowerup = 0;

const obstaculos = [
  {x: 400, y: 150, w: 100, h: 30},
  {x: 400, y: 370, w: 100, h: 30},
  {x: 270, y: 225, w: 30, h: 100},
  {x: 600, y: 225, w: 30, h: 100}
];

function moverJogador(p, controles) {
  let dx = 0, dy = 0;

  if (teclas[controles.cima]) dy--;
  if (teclas[controles.baixo]) dy++;
  if (teclas[controles.esquerda]) dx--;
  if (teclas[controles.direita]) dx++;

  if (dx !== 0 || dy !== 0) {
    const tamanho = Math.hypot(dx, dy);
    dx /= tamanho;
    dy /= tamanho;

    p.x += dx * p.velocidade;
    p.y += dy * p.velocidade;

    p.direcaoX = dx;
    p.direcaoY = dy;
  }

  tratarColisaoObstaculos(p);

  p.x = Math.max(15, Math.min(canvas.width - 15, p.x));
  p.y = Math.max(15, Math.min(canvas.height - 15, p.y));
}

function tratarColisaoObstaculos(p) {
  const raio = p.tamanho / 2;
  for (const o of obstaculos) {
    if (p.x + raio > o.x && p.x - raio < o.x + o.w &&
        p.y + raio > o.y && p.y - raio < o.y + o.h) {
      const centroX = o.x + o.w / 2;
      const centroY = o.y + o.h / 2;
      const diffX = p.x - centroX;
      const diffY = p.y - centroY;
      const overlapX = (o.w / 2 + raio) - Math.abs(diffX);
      const overlapY = (o.h / 2 + raio) - Math.abs(diffY);

      if (overlapX < overlapY) {
        p.x += diffX > 0 ? overlapX : -overlapX;
      } else {
        p.y += diffY > 0 ? overlapY : -overlapY;
      }
    }
  }
}

function atirar(p) {
  if (jogoAcabou || p.cooldown > 0) return;

  const criarBala = (angleOffset = 0) => {
    const cos = Math.cos(angleOffset);
    const sin = Math.sin(angleOffset);
    const vx = p.direcaoX * cos - p.direcaoY * sin;
    const vy = p.direcaoX * sin + p.direcaoY * cos;

    balas.push({
      x: p.x + vx * 18,
      y: p.y + vy * 18,
      vx: vx * 10,
      vy: vy * 10,
      cor: p.cor,
      dono: p,
      ricochete: 1,
      rastro: []
    });
  };

  if (p.tiroTriplo > 0) {
    criarBala(-0.25);
    criarBala(0);
    criarBala(0.25);
    p.tiroTriplo--;
  } else {
    criarBala(0);
  }

  p.cooldown = 12;
  p.recuo = 6;
  tocarSom(300, 'square', 0.08, 0.15);
}

function gerarPowerup() {
  if (powerups.length >= 2) return;
  const tipos = ['escudo', 'triplo'];
  const tipo = tipos[Math.floor(Math.random() * tipos.length)];
  const x = 100 + Math.random() * (canvas.width - 200);
  const y = 100 + Math.random() * (canvas.height - 200);

  // Evitar nascer dentro de obstáculos
  for (const o of obstaculos) {
    if (x > o.x - 20 && x < o.x + o.w + 20 && y > o.y - 20 && y < o.y + o.h + 20) return;
  }

  powerups.push({ x, y, tipo, tempo: 400 });
}

function criarExplosao(x, y, cor, qtd = 20) {
  for (let i = 0; i < qtd; i++) {
    particulas.push({
      x: x, y: y,
      vx: (Math.random() - 0.5) * 8,
      vy: (Math.random() - 0.5) * 8,
      vida: 25 + Math.random() * 15,
      cor: cor,
      tamanho: 2 + Math.random() * 3
    });
  }
}

function atualizarBalas() {
  for (let i = balas.length - 1; i >= 0; i--) {
    const b = balas[i];

    b.rastro.push({x: b.x, y: b.y});
    if (b.rastro.length > 5) b.rastro.shift();

    b.x += b.vx;
    b.y += b.vy;

    // Colisão com bordas da tela (Ricochete)
    if (b.x < 5 || b.x > canvas.width - 5) {
      if (b.ricochete > 0) { b.vx *= -1; b.ricochete--; tocarSom(150, 'sine', 0.05); }
      else { balas.splice(i, 1); continue; }
    }
    if (b.y < 5 || b.y > canvas.height - 5) {
      if (b.ricochete > 0) { b.vy *= -1; b.ricochete--; tocarSom(150, 'sine', 0.05); }
      else { balas.splice(i, 1); continue; }
    }

    // Colisão com obstáculos
    for (const o of obstaculos) {
      if (b.x > o.x && b.x < o.x + o.w && b.y > o.y && b.y < o.y + o.h) {
        if (b.ricochete > 0) {
          // Determina o lado do impacto para rebater corretamente
          const prevX = b.x - b.vx;
          if (prevX <= o.x || prevX >= o.x + o.w) b.vx *= -1;
          else b.vy *= -1;
          b.ricochete--;
          tocarSom(150, 'sine', 0.05);
        } else {
          criarExplosao(b.x, b.y, "#888", 5);
          balas.splice(i, 1);
          break;
        }
      }
    }

    if (!balas[i]) continue;

    // Colisão com Jogadores
    const alvo = b.dono === jogador1 ? jogador2 : jogador1;
    if (Math.hypot(b.x - alvo.x, b.y - alvo.y) < 18) {
      if (alvo.escudos > 0) {
        alvo.escudos--;
        criarExplosao(alvo.x, alvo.y, "#00ff88", 15);
        tocarSom(500, 'triangle', 0.15);
      } else {
        b.dono.pontos++;
        screenShake = 12;
        criarExplosao(alvo.x, alvo.y, alvo.cor, 30);
        tocarSom(100, 'sawtooth', 0.3, 0.3);
        resetarPosicoes();

        if (b.dono.pontos >= 10) {
          jogoAcabou = true;
          vencedor = b.dono === jogador1 ? "JOGADOR 1" : "JOGADOR 2";
        }
      }
      balas.splice(i, 1);
    }
  }
}

function atualizarPowerups() {
  tempoPowerup++;
  if (tempoPowerup > 300) {
    gerarPowerup();
    tempoPowerup = 0;
  }

  for (let i = powerups.length - 1; i >= 0; i--) {
    const pw = powerups[i];
    pw.tempo--;

    [jogador1, jogador2].forEach(p => {
      if (Math.hypot(pw.x - p.x, pw.y - p.y) < 25) {
        if (pw.tipo === 'escudo') p.escudos = 1;
        if (pw.tipo === 'triplo') p.tiroTriplo = 5;
        tocarSom(600, 'sine', 0.2, 0.2);
        powerups.splice(i, 1);
      }
    });

    if (pw.tempo <= 0) powerups.splice(i, 1);
  }
}

function atualizarParticulas() {
  for (let i = particulas.length - 1; i >= 0; i--) {
    const p = particulas[i];
    p.x += p.vx;
    p.y += p.vy;
    p.vx *= 0.92;
    p.vy *= 0.92;
    p.vida--;
    if (p.vida <= 0) particulas.splice(i, 1);
  }
}

function resetarPosicoes() {
  jogador1.x = 120; jogador1.y = 275; jogador1.direcaoX = 1; jogador1.direcaoY = 0;
  jogador2.x = 780; jogador2.y = 275; jogador2.direcaoX = -1; jogador2.direcaoY = 0;
  jogador1.escudos = 0; jogador2.escudos = 0;
  jogador1.tiroTriplo = 0; jogador2.tiroTriplo = 0;
  balas = [];
}

function reiniciar() {
  jogador1.pontos = 0;
  jogador2.pontos = 0;
  particulas = [];
  powerups = [];
  jogoAcabou = false;
  vencedor = "";
  resetarPosicoes();
}

function desenharArena() {
  ctx.fillStyle = "#0b0b18";
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  // Grade Neon
  ctx.strokeStyle = "rgba(255, 255, 255, 0.03)";
  ctx.lineWidth = 1;
  for (let x = 0; x < canvas.width; x += 40) {
    ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
  }
  for (let y = 0; y < canvas.height; y += 40) {
    ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
  }

  // Obstáculos
  for (const o of obstaculos) {
    ctx.fillStyle = "#1e1e38";
    ctx.fillRect(o.x, o.y, o.w, o.h);
    ctx.strokeStyle = "#4d4dff";
    ctx.lineWidth = 2;
    ctx.strokeRect(o.x, o.y, o.w, o.h);
  }

  // Moldura
  ctx.strokeStyle = "#00f3ff";
  ctx.lineWidth = 3;
  ctx.strokeRect(2, 2, canvas.width - 4, canvas.height - 4);
}

function desenharJogador(p) {
  ctx.save();
  ctx.translate(p.x, p.y);

  // Animação de Recuo
  const offsetArma = Math.max(0, p.recuo);

  // Escudo Ativo
  if (p.escudos > 0) {
    ctx.strokeStyle = "#00ff88";
    ctx.lineWidth = 3;
    ctx.shadowColor = "#00ff88";
    ctx.shadowBlur = 10;
    ctx.beginPath();
    ctx.arc(0, 0, p.tamanho / 2 + 6, 0, Math.PI * 2);
    ctx.stroke();
  }

  // Corpo
  ctx.shadowColor = p.cor;
  ctx.shadowBlur = 15;
  ctx.fillStyle = p.cor;
  ctx.beginPath();
  ctx.arc(0, 0, p.tamanho / 2, 0, Math.PI * 2);
  ctx.fill();

  // Arma
  ctx.strokeStyle = "#ffffff";
  ctx.lineWidth = 4;
  ctx.beginPath();
  ctx.moveTo(0, 0);
  ctx.lineTo(
    (p.direcaoX * 22) - (p.direcaoX * offsetArma),
    (p.direcaoY * 22) - (p.direcaoY * offsetArma)
  );
  ctx.stroke();

  ctx.restore();
}

function desenharBalas() {
  for (const b of balas) {
    // Rastro
    ctx.strokeStyle = b.cor;
    ctx.lineWidth = 2;
    ctx.beginPath();
    for (let i = 0; i < b.rastro.length; i++) {
      ctx.lineTo(b.rastro[i].x, b.rastro[i].y);
    }
    ctx.stroke();

    // Bala
    ctx.save();
    ctx.shadowColor = b.cor;
    ctx.shadowBlur = 10;
    ctx.fillStyle = "#fff";
    ctx.beginPath();
    ctx.arc(b.x, b.y, 4, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
  }
}

function desenharPowerups() {
  for (const pw of powerups) {
    ctx.save();
    ctx.translate(pw.x, pw.y);
    ctx.shadowBlur = 12;

    if (pw.tipo === 'escudo') {
      ctx.fillStyle = "#00ff88";
      ctx.shadowColor = "#00ff88";
      ctx.beginPath(); ctx.arc(0, 0, 10, 0, Math.PI * 2); ctx.fill();
    } else {
      ctx.fillStyle = "#ff00ea";
      ctx.shadowColor = "#ff00ea";
      ctx.fillRect(-8, -8, 16, 16);
    }
    ctx.restore();
  }
}

function desenharParticulas() {
  for (const p of particulas) {
    ctx.globalAlpha = p.vida / 40;
    ctx.fillStyle = p.cor;
    ctx.fillRect(p.x, p.y, p.tamanho, p.tamanho);
  }
  ctx.globalAlpha = 1;
}

function desenharHUD() {
  ctx.font = "bold 24px monospace";

  ctx.fillStyle = jogador1.cor;
  ctx.fillText("P1: " + jogador1.pontos + (jogador1.tiroTriplo ? " [TRIPLO]" : ""), 30, 40);

  ctx.fillStyle = jogador2.cor;
  ctx.fillText("P2: " + jogador2.pontos + (jogador2.tiroTriplo ? " [TRIPLO]" : ""), 680, 40);

  ctx.font = "14px monospace";
  ctx.fillStyle = "#aaa";
  ctx.fillText("PRIMEIRO A 10 PONTOS VENCE", 350, 35);
}

function desenharFim() {
  if (!jogoAcabou) return;

  ctx.fillStyle = "rgba(0, 0, 0, 0.85)";
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  ctx.textAlign = "center";
  ctx.fillStyle = "#fff";
  ctx.font = "bold 50px monospace";
  ctx.fillText(vencedor + " VENCEU!", canvas.width / 2, 230);

  ctx.font = "20px monospace";
  ctx.fillStyle = "#00f3ff";
  ctx.fillText("Pressione 'R' para Jogar Novamente", canvas.width / 2, 290);
  ctx.textAlign = "left";
}

function atualizar() {
  if (jogoAcabou) return;

  moverJogador(jogador1, { cima: "w", baixo: "s", esquerda: "a", direita: "d" });
  moverJogador(jogador2, { cima: "arrowup", baixo: "arrowdown", esquerda: "arrowleft", direita: "arrowright" });

  if (jogador1.cooldown > 0) jogador1.cooldown--;
  if (jogador2.cooldown > 0) jogador2.cooldown--;

  if (jogador1.recuo > 0) jogador1.recuo--;
  if (jogador2.recuo > 0) jogador2.recuo--;

  atualizarBalas();
  atualizarPowerups();
  atualizarParticulas();

  if (screenShake > 0) screenShake--;
}

function desenhar() {
  ctx.save();

  // Aplica tremor de tela quando atingido
  if (screenShake > 0) {
    const rx = (Math.random() - 0.5) * screenShake;
    const ry = (Math.random() - 0.5) * screenShake;
    ctx.translate(rx, ry);
  }

  desenharArena();
  desenharPowerups();
  desenharParticulas();
  desenharBalas();
  desenharJogador(jogador1);
  desenharJogador(jogador2);
  desenharHUD();
  desenharFim();

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