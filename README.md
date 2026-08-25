<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Retro Cyber Soccer 2v2</title>
<style>
  body {
    margin: 0;
    background: #080811;
    color: #fff;
    font-family: 'Courier New', Courier, monospace;
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
  }

  h1 {
    color: #00f3ff;
    margin: 10px 0 5px 0;
    text-shadow: 0 0 10px #00f3ff, 0 0 20px #00f3ff;
  }

  canvas {
    background: #0a131a;
    border: 3px solid #00f3ff;
    box-shadow: 0 0 25px rgba(0, 243, 255, 0.3);
    image-rendering: pixelated;
  }

  .hud {
    margin-top: 10px;
    font-size: 13px;
    color: #aaa;
  }

  .k { color: #ff0055; font-weight: bold; }
</style>
</head>
<body>

<h1>⚽ CYBER SOCCER 2v2 ⚽</h1>
<canvas id="campo" width="800" height="500"></canvas>
<div class="hud">
  WASD: Mover | <span class="k">SHIFT: Pique</span> | <span class="k">ESPAÇO: Chute Forte</span> | <span class="k">E: Passe/Toque Fraco</span>
</div>

<script>
const canvas = document.getElementById("campo");
const ctx = canvas.getContext("2d");

// Time Azul (Jogador + Companheiro IA)
const timeAzul = [
  { x: 200, y: 250, tam: 22, velBase: 3.8, vel: 3.8, estamina: 100, cor: "#00f3ff", eJogador: true },
  { x: 300, y: 150, tam: 22, velBase: 2.8, vel: 2.8, estamina: 100, cor: "#0088ff", eJogador: false }
];

// Time Vermelho (Inimigos IA)
const timeVermelho = [
  { x: 600, y: 250, tam: 22, velBase: 2.7, vel: 2.7, estamina: 100, cor: "#ff0055", eJogador: false },
  { x: 500, y: 350, tam: 22, velBase: 2.5, vel: 2.5, estamina: 100, cor: "#ff5500", eJogador: false }
];

const bola = {
  x: 400, y: 250, raio: 8, vx: 0, vy: 0, rastro: []
};

let teclas = {};
let golsAzul = 0;
let golsVermelho = 0;
let textoGolTempo = 0;

document.addEventListener("keydown", e => {
  teclas[e.key.toLowerCase()] = true;
  if (e.code === "Space") chutar(14); // Chute forte
  if (e.key.toLowerCase() === "e") chutar(6); // Passe curto
});

document.addEventListener("keyup", e => {
  teclas[e.key.toLowerCase()] = false;
});

function dist(a, b) {
  return Math.hypot(a.x - b.x, a.y - b.y);
}

function chutar(forca) {
  const p = timeAzul[0];
  if (dist(p, bola) < 42) {
    const ang = Math.atan2(bola.y - p.y, bola.x - p.x);
    bola.vx = Math.cos(ang) * forca;
    bola.vy = Math.sin(ang) * forca;
  }
}

function IA_Mover(p, alvoX, alvoY, limiteXMin, limiteXMax) {
  if (p.y < alvoY) p.y += p.vel;
  if (p.y > alvoY) p.y -= p.vel;
  if (p.x < alvoX && p.x < limiteXMax) p.x += p.vel;
  if (p.x > alvoX && p.x > limiteXMin) p.x -= p.vel;

  // Chute automático da IA
  if (dist(p, bola) < 35 && Math.random() < 0.08) {
    const direcao = p.cor.includes("ff0055") || p.cor.includes("ff5500") ? 0 : 800; // Ataca para o gol oposto
    const ang = Math.atan2(250 - p.y, direcao - p.x);
    bola.vx = Math.cos(ang) * 10;
    bola.vy = Math.sin(ang) * 10;
  }
}

function atualizar() {
  const p1 = timeAzul[0];

  // Pique (Sprint) do jogador
  if (teclas["shift"] && p1.estamina > 5) {
    p1.vel = p1.velBase * 1.6;
    p1.estamina -= 1.8;
  } else {
    p1.vel = p1.velBase;
    if (p1.estamina < 100) p1.estamina += 0.6;
  }

  // Controles P1
  if (teclas["w"] || teclas["arrowup"]) p1.y -= p1.vel;
  if (teclas["s"] || teclas["arrowdown"]) p1.y += p1.vel;
  if (teclas["a"] || teclas["arrowleft"]) p1.x -= p1.vel;
  if (teclas["d"] || teclas["arrowright"]) p1.x += p1.vel;

  // Manter dentro de campo
  [...timeAzul, ...timeVermelho].forEach(p => {
    p.x = Math.max(20, Math.min(780, p.x));
    p.y = Math.max(20, Math.min(480, p.y));
  });

  // Movimento de IA dos parceiros e oponentes
  IA_Mover(timeAzul[1], bola.x - 50, bola.y, 50, 400); // Companheiro apoia no ataque
  IA_Mover(timeVermelho[0], bola.x, bola.y, 350, 750); // Atacante Vermelho
  IA_Mover(timeVermelho[1], bola.x, bola.y, 550, 780); // Zagueiro Vermelho

  // Colisões com bola (Dribles)
  [...timeAzul, ...timeVermelho].forEach(p => {
    if (dist(p, bola) < p.tam / 2 + bola.raio) {
      const ang = Math.atan2(bola.y - p.y, bola.x - p.x);
      bola.vx = Math.cos(ang) * 3.5;
      bola.vy = Math.sin(ang) * 3.5;
    }
  });

  // Física da Bola
  if (Math.hypot(bola.vx, bola.vy) > 1.5) {
    bola.rastro.push({ x: bola.x, y: bola.y, a: 0.6 });
  }
  if (bola.rastro.length > 8) bola.rastro.shift();

  bola.x += bola.vx;
  bola.y += bola.vy;
  bola.vx *= 0.981; // Fricção
  bola.vy *= 0.981;

  // Rebote Topo/Fundo
  if (bola.y - bola.raio < 5 || bola.y + bola.raio > 495) bola.vy *= -1;

  // Rebote Lateral Fora da Trave
  if ((bola.x - bola.raio < 5 || bola.x + bola.raio > 795) && (bola.y <= 180 || bola.y >= 320)) {
    bola.vx *= -1;
  }

  // Gols
  if (bola.y > 180 && bola.y < 320) {
    if (bola.x > 792) { golsAzul++; textoGolTempo = 50; reset(); }
    if (bola.x < 8) { golsVermelho++; textoGolTempo = 50; reset(); }
  }
}

function reset() {
  bola.x = 400; bola.y = 250; bola.vx = 0; bola.vy = 0; bola.rastro = [];
  timeAzul[0].x = 200; timeAzul[0].y = 250;
  timeAzul[1].x = 300; timeAzul[1].y = 150;
  timeVermelho[0].x = 600; timeVermelho[0].y = 250;
  timeVermelho[1].x = 500; timeVermelho[1].y = 350;
}

function desenhar() {
  ctx.clearRect(0, 0, 800, 500);

  // Grid Cyberpunk (Fundo do Campo)
  ctx.strokeStyle = "rgba(0, 243, 255, 0.05)";
  ctx.lineWidth = 1;
  for (let x = 0; x < 800; x += 40) {
    ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, 500); ctx.stroke();
  }
  for (let y = 0; y < 500; y += 40) {
    ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(800, y); ctx.stroke();
  }

  // Linhas do campo
  ctx.strokeStyle = "#00f3ff";
  ctx.lineWidth = 2;
  ctx.strokeRect(5, 5, 790, 490);
  ctx.beginPath();
  ctx.moveTo(400, 5); ctx.lineTo(400, 495);
  ctx.arc(400, 250, 60, 0, Math.PI * 2);
  ctx.stroke();

  // Traves Neon
  ctx.strokeStyle = "#ff0055";
  ctx.strokeRect(5, 180, 10, 140);
  ctx.strokeStyle = "#00f3ff";
  ctx.strokeRect(785, 180, 10, 140);

  // Rastro da bola
  bola.rastro.forEach(p => {
    ctx.fillStyle = `rgba(255, 255, 255, ${p.a})`;
    ctx.beginPath();
    ctx.arc(p.x, p.y, bola.raio * 0.8, 0, Math.PI * 2);
    ctx.fill();
    p.a -= 0.06;
  });

  // Bola
  ctx.fillStyle = "#fff";
  ctx.beginPath();
  ctx.arc(bola.x, bola.y, bola.raio, 0, Math.PI * 2);
  ctx.fill();

  // Jogadores
  [...timeAzul, ...timeVermelho].forEach(p => {
    ctx.fillStyle = p.cor;
    ctx.shadowColor = p.cor;
    ctx.shadowBlur = 10;
    ctx.fillRect(p.x - p.tam / 2, p.y - p.tam / 2, p.tam, p.tam);
    ctx.shadowBlur = 0; // Reseta brilho
  });

  // Placar HUD
  ctx.fillStyle = "#fff";
  ctx.font = "bold 26px 'Courier New', monospace";
  ctx.fillText(`${golsAzul}  -  ${golsVermelho}`, 355, 40);

  // Barra de Estamina P1
  ctx.fillStyle = "rgba(255,255,255,0.2)";
  ctx.fillRect(20, 20, 100, 6);
  ctx.fillStyle = timeAzul[0].estamina > 20 ? "#00f3ff" : "#ff0055";
  ctx.fillRect(20, 20, timeAzul[0].estamina, 6);

  // Efeito de Gol
  if (textoGolTempo > 0) {
    ctx.fillStyle = "#ff0055";
    ctx.shadowColor = "#ff0055";
    ctx.shadowBlur = 20;
    ctx.font = "bold 60px monospace";
    ctx.fillText("GOOOOOOL!", 250, 260);
    ctx.shadowBlur = 0;
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