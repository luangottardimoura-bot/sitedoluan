<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Zombie Survival 1v1</title>
<style>
  * { box-sizing: border-box; }
  body {
    margin: 0;
    background: #0a0a0c;
    color: #fff;
    font-family: 'Courier New', Courier, monospace;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    overflow: hidden;
  }

  h1 {
    color: #ff3333;
    margin: 10px 0 5px;
    text-shadow: 0 0 10px #ff0000;
    font-size: 22px;
  }

  canvas {
    background: #15181c;
    border: 3px solid #ff3333;
    box-shadow: 0 0 20px rgba(255, 0, 0, 0.3);
  }

  .hud {
    margin-top: 10px;
    font-size: 13px;
    color: #aaa;
    display: flex;
    gap: 20px;
  }

  .p1-text { color: #00a2ff; font-weight: bold; }
  .p2-text { color: #ffaa00; font-weight: bold; }
</style>
</head>
<body>

<h1>🧟 ZOMBIE SURVIVAL - MODO 1v1 🧟</h1>
<canvas id="gameCanvas" width="800" height="500"></canvas>
<div class="hud">
  <div><span class="p1-text">P1 (Azul):</span> WASD: Mover | ESPAÇO: Atirar</div>
  <div><span class="p2-text">P2 (Laranja):</span> Setas: Mover | ENTER: Atirar</div>
</div>

<script>
const canvas = document.getElementById("gameCanvas");
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
  constructor(x, y, cor, id) {
    this.x = x;
    this.y = y;
    this.cor = cor;
    this.id = id;
    this.raio = 15;
    this.velocidade = 3.5;
    this.angulo = 0;
    this.vida = 100;
    this.pontos = 0;
    this.vivo = true;
    this.recarregando = 0;
  }

  desenhar() {
    if (!this.vivo) return;
    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate(this.angulo);

    // Arma
    ctx.fillStyle = "#888";
    ctx.fillRect(0, -3, 22, 6);

    // Corpo
    ctx.fillStyle = this.cor;
    ctx.beginPath();
    ctx.arc(0, 0, this.raio, 0, Math.PI * 2);
    ctx.fill();

    ctx.restore();
  }
}

const p1 = new Jogador(200, 250, "#00a2ff", 1);
const p2 = new Jogador(600, 250, "#ffaa00", 2);

let tiros = [];
let zumbis = [];
let teclas = {};
let tempoGerarZumbi = 0;
let vencedor = null;

document.addEventListener("keydown", e => {
  teclas[e.code] = true;

  if (e.code === "Space" && p1.vivo && p1.recarregando <= 0) {
    atirar(p1);
    p1.recarregando = 10;
  }
  if (e.code === "Enter" && p2.vivo && p2.recarregando <= 0) {
    atirar(p2);
    p2.recarregando = 10;
  }
  if (vencedor && e.code === "KeyR") {
    reiniciarJogo();
  }
});

document.addEventListener("keyup", e => teclas[e.code] = false);

function atirar(jogador) {
  tiros.push({
    x: jogador.x + Math.cos(jogador.angulo) * jogador.raio,
    y: jogador.y + Math.sin(jogador.angulo) * jogador.raio,
    vx: Math.cos(jogador.angulo) * 10,
    vy: Math.sin(jogador.angulo) * 10,
    raio: 4,
    donoId: jogador.id
  });
  tocarSom(400, "square", 0.08, 0.2);
}

function criarZumbi() {
  let x, y;
  if (Math.random() < 0.5) {
    x = Math.random() < 0.5 ? -20 : canvas.width + 20;
    y = Math.random() * canvas.height;
  } else {
    x = Math.random() * canvas.width;
    y = Math.random() < 0.5 ? -20 : canvas.height + 20;
  }

  zumbis.push({
    x: x,
    y: y,
    raio: 14,
    velocidade: 1 + Math.random() * 1.5,
    vida: 2
  });
}

function atualizar() {
  if (vencedor) return;

  // Movimento P1 (WASD)
  let vx1 = 0, vy1 = 0;
  if (teclas["KeyW"] && p1.y > p1.raio) vy1--;
  if (teclas["KeyS"] && p1.y < canvas.height - p1.raio) vy1++;
  if (teclas["KeyA"] && p1.x > p1.raio) vx1--;
  if (teclas["KeyD"] && p1.x < canvas.width - p1.raio) vx1++;

  if (vx1 !== 0 || vy1 !== 0) {
    p1.x += vx1 * p1.velocidade;
    p1.y += vy1 * p1.velocidade;
    p1.angulo = Math.atan2(vy1, vx1);
  }

  // Movimento P2 (Setas)
  let vx2 = 0, vy2 = 0;
  if (teclas["ArrowUp"] && p2.y > p2.raio) vy2--;
  if (teclas["ArrowDown"] && p2.y < canvas.height - p2.raio) vy2++;
  if (teclas["ArrowLeft"] && p2.x > p2.raio) vx2--;
  if (teclas["ArrowRight"] && p2.x < canvas.width - p2.raio) vx2++;

  if (vx2 !== 0 || vy2 !== 0) {
    p2.x += vx2 * p2.velocidade;
    p2.y += vy2 * p2.velocidade;
    p2.angulo = Math.atan2(vy2, vx2);
  }

  if (p1.recarregando > 0) p1.recarregando--;
  if (p2.recarregando > 0) p2.recarregando--;

  // Criar zumbis
  tempoGerarZumbi++;
  if (tempoGerarZumbi > 30) {
    criarZumbi();
    tempoGerarZumbi = 0;
  }

  // Atualizar Tiros
  for (let i = tiros.length - 1; i >= 0; i--) {
    let t = tiros[i];
    t.x += t.vx;
    t.y += t.vy;

    if (t.x < 0 || t.x > canvas.width || t.y < 0 || t.y > canvas.height) {
      tiros.splice(i, 1);
      continue;
    }

    // Tiro acerta oponente
    [p1, p2].forEach(p => {
      if (p.vivo && p.id !== t.donoId) {
        if (Math.hypot(p.x - t.x, p.y - t.y) < p.raio + t.raio) {
          p.vida -= 15;
          tiros.splice(i, 1);
          tocarSom(120, "sawtooth", 0.1, 0.2);
        }
      }
    });
  }

  // Atualizar Zumbis
  for (let i = zumbis.length - 1; i >= 0; i--) {
    let z = zumbis[i];

    // Zumbi persegue o jogador vivo mais próximo
    let alvo = null;
    let dist1 = p1.vivo ? Math.hypot(p1.x - z.x, p1.y - z.y) : Infinity;
    let dist2 = p2.vivo ? Math.hypot(p2.x - z.x, p2.y - z.y) : Infinity;

    if (dist1 < dist2) alvo = p1;
    else if (dist2 < dist1) alvo = p2;

    if (alvo) {
      const dx = alvo.x - z.x;
      const dy = alvo.y - z.y;
      const dist = Math.hypot(dx, dy);
      z.x += (dx / dist) * z.velocidade;
      z.y += (dy / dist) * z.velocidade;

      if (dist < alvo.raio + z.raio) {
        alvo.vida -= 0.6;
        tocarSom(100, "sawtooth", 0.05, 0.05);
      }
    }

    // Colisão com Tiros
    for (let j = tiros.length - 1; j >= 0; j--) {
      let t = tiros[j];
      if (Math.hypot(z.x - t.x, z.y - t.y) < z.raio + t.raio) {
        z.vida--;
        tiros.splice(j, 1);
        tocarSom(250, "sine", 0.05, 0.1);

        if (z.vida <= 0) {
          if (t.donoId === 1) p1.pontos += 10;
          if (t.donoId === 2) p2.pontos += 10;
          zumbis.splice(i, 1);
        }
        break;
      }
    }
  }

  // Checar mortes e fim de jogo
  if (p1.vida <= 0) { p1.vivo = false; p1.vida = 0; }
  if (p2.vida <= 0) { p2.vivo = false; p2.vida = 0; }

  if (!p1.vivo && !p2.vivo) vencedor = "EMPATE!";
  else if (!p1.vivo) vencedor = "JOGADOR 2 VENCEU!";
  else if (!p2.vivo) vencedor = "JOGADOR 1 VENCEU!";
}

function desenharBarraVida(x, y, vida, cor, nome) {
  ctx.fillStyle = "#fff";
  ctx.font = "bold 14px monospace";
  ctx.fillText(nome, x, y - 5);

  ctx.fillStyle = "#333";
  ctx.fillRect(x, y, 150, 12);
  ctx.fillStyle = cor;
  ctx.fillRect(x, y, Math.max(0, vida * 1.5), 12);
  ctx.strokeStyle = "#fff";
  ctx.strokeRect(x, y, 150, 12);
}

function desenhar() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Grade do piso
  ctx.strokeStyle = "rgba(255, 255, 255, 0.03)";
  ctx.lineWidth = 1;
  for (let x = 0; x < canvas.width; x += 40) {
    ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
  }
  for (let y = 0; y < canvas.height; y += 40) {
    ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
  }

  // Tiros
  ctx.fillStyle = "#ffff00";
  tiros.forEach(t => {
    ctx.beginPath();
    ctx.arc(t.x, t.y, t.raio, 0, Math.PI * 2);
    ctx.fill();
  });

  // Zumbis
  zumbis.forEach(z => {
    ctx.fillStyle = "#33cc55";
    ctx.beginPath();
    ctx.arc(z.x, z.y, z.raio, 0, Math.PI * 2);
    ctx.fill();

    ctx.fillStyle = "#ff0000";
    ctx.beginPath();
    ctx.arc(z.x - 3, z.y - 3, 2, 0, Math.PI * 2);
    ctx.arc(z.x + 3, z.y - 3, 2, 0, Math.PI * 2);
    ctx.fill();
  });

  // Jogadores
  p1.desenhar();
  p2.desenhar();

  // Interface HUD
  desenharBarraVida(20, 25, p1.vida, "#00a2ff", `P1: ${p1.pontos} pts`);
  desenharBarraVida(canvas.width - 170, 25, p2.vida, "#ffaa00", `P2: ${p2.pontos} pts`);

  // Tela de Vitória
  if (vencedor) {
    ctx.fillStyle = "rgba(0, 0, 0, 0.8)";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    ctx.fillStyle = "#ff3333";
    ctx.font = "bold 40px monospace";
    ctx.fillText(vencedor, 200, 230);

    ctx.fillStyle = "#fff";
    ctx.font = "16px monospace";
    ctx.fillText("Pressione 'R' para Jogar Novamente", 230, 280);
  }
}

function reiniciarJogo() {
  p1.x = 200; p1.y = 250; p1.vida = 100; p1.vivo = true; p1.pontos = 0;
  p2.x = 600; p2.y = 250; p2.vida = 100; p2.vivo = true; p2.pontos = 0;
  zumbis = [];
  tiros = [];
  vencedor = null;
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