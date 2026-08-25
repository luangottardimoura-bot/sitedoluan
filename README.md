<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cyber Soccer 3D Perspective</title>
<style>
  * { box-sizing: border-box; }
  body {
    margin: 0;
    background: #050508;
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
    color: #00ffcc;
    margin: 10px 0 5px;
    text-shadow: 0 0 10px #00ffcc;
    font-size: 22px;
  }

  canvas {
    background: #020b12;
    border: 3px solid #00ffcc;
    box-shadow: 0 0 30px rgba(0, 255, 204, 0.2);
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

<h1>⚽ CYBER SOCCER 3D CAM ⚽</h1>
<canvas id="campo" width="800" height="450"></canvas>
<div class="hud">
  WASD: Mover | <span class="k">ESPAÇO: Chutar</span> | <span class="k">SHIFT: Correr</span>
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

// Câmera Dinâmica
const cam = { x: 0, y: 0, zoom: 1 };

class Entidade3D {
  constructor(x, z, cor, eGoleiro = false) {
    this.x = x;     // Posição Horizontal no mundo
    this.z = z;     // Profundidade (Distância da tela)
    this.vx = 0;
    this.vz = 0;
    this.cor = cor;
    this.eGoleiro = eGoleiro;
    this.tamBase = 28;
  }

  atualizar() {
    this.vx *= 0.85;
    this.vz *= 0.85;
    this.x += this.vx;
    this.z += this.vz;

    // Limites do Campo em 3D
    this.x = Math.max(-380, Math.min(380, this.x));
    this.z = Math.max(100, Math.min(900, this.z));
  }

  // Mapeamento de Coordenadas 3D para Projeção 2D na Tela
  getProjecao() {
    const escala = 300 / this.z; // Efeito de perspectiva (quanto mais longe, menor)
    const telaX = canvas.width / 2 + (this.x - cam.x) * escala;
    const telaY = canvas.height / 2 + (this.z - cam.y) * escala * 0.35 - 50;
    const tamanho = this.tamBase * escala;
    return { x: telaX, y: telaY, tamanho, escala };
  }
}

const p1 = new Entidade3D(0, 300, "#00ffcc");
const rival = new Entidade3D(0, 600, "#ff0055");
const goleiroRival = new Entidade3D(0, 850, "#ffcc00", true);

const bola = {
  x: 0, z: 450, y: 0,
  vx: 0, vz: 0, vy: 0,
  atrito: 0.98,
  getProjecao() {
    const escala = 300 / this.z;
    const telaX = canvas.width / 2 + (this.x - cam.x) * escala;
    const telaY = canvas.height / 2 + (this.z - cam.y) * escala * 0.35 - 50 - (this.y * escala);
    return { x: telaX, y: telaY, tamanho: 12 * escala, escala };
  }
};

let teclas = {};
let golsP1 = 0;
let golsRival = 0;
let textoGol = 0;

document.addEventListener("keydown", e => teclas[e.key.toLowerCase()] = true);
document.addEventListener("keyup", e => teclas[e.key.toLowerCase()] = false);

function dist3D(a, b) {
  return Math.hypot(a.x - b.x, a.z - b.z);
}

function processarControles() {
  let vel = teclas["shift"] ? 4.5 : 2.8;

  if (teclas["w"]) p1.vz += vel * 0.3;
  if (teclas["s"]) p1.vz -= vel * 0.3;
  if (teclas["a"]) p1.vx -= vel * 0.4;
  if (teclas["d"]) p1.vx += vel * 0.4;

  if (teclas[" "] && dist3D(p1, bola) < 45) {
    bola.vx = (bola.x - p1.x) * 0.3;
    bola.vz = 14;
    bola.vy = 6; // Elevação em 3D
    tocarSom(300, "square", 0.1, 0.3);
  }
}

function IA_Controlar() {
  // Defender / Atacar da IA
  const dz = bola.z - rival.z;
  const dx = bola.x - rival.x;
  rival.vx += Math.sign(dx) * 0.25;
  rival.vz += Math.sign(dz) * 0.25;

  if (dist3D(rival, bola) < 40 && Math.random() < 0.1) {
    bola.vz = -12;
    bola.vx = (Math.random() - 0.5) * 8;
    tocarSom(200, "sawtooth", 0.1, 0.2);
  }

  // Goleiro
  goleiroRival.x += (bola.x - goleiroRival.x) * 0.1;
  goleiroRival.x = Math.max(-120, Math.min(120, goleiroRival.x));
}

function atualizarFisica() {
  p1.atualizar();
  rival.atualizar();
  goleiroRival.atualizar();

  // Movimento da bola
  bola.x += bola.vx;
  bola.z += bola.vz;
  bola.y += bola.vy;

  bola.vx *= bola.atrito;
  bola.vz *= bola.atrito;

  if (bola.y > 0) {
    bola.vy -= 0.4; // Gravidade
  } else {
    bola.y = 0;
    if (Math.abs(bola.vy) > 1) bola.vy = -bola.vy * 0.5;
    else bola.vy = 0;
  }

  // Colisão física simples entre jogadores e bola
  [p1, rival, goleiroRival].forEach(p => {
    if (dist3D(p, bola) < 30 && bola.y < 20) {
      bola.vx = (bola.x - p.x) * 0.4;
      bola.vz = (bola.z - p.z) * 0.4;
    }
  });

  // Câmera segue a bola suavemente
  cam.x += (bola.x - cam.x) * 0.08;
  cam.y += (bola.z - 200 - cam.y) * 0.08;

  // Gol no Fundo (Z alto)
  if (bola.z > 860 && Math.abs(bola.x) < 130 && bola.y < 60) {
    golsP1++;
    textoGol = 50;
    tocarSom(500, "sine", 0.3, 0.3);
    reset();
  }

  // Out de Fundo / Lateral
  if (bola.z < 80 || bola.z > 920 || Math.abs(bola.x) > 400) {
    reset();
  }
}

function reset() {
  bola.x = 0; bola.z = 450; bola.y = 0; bola.vx = 0; bola.vz = 0; bola.vy = 0;
  p1.x = 0; p1.z = 300;
  rival.x = 0; rival.z = 600;
}

function desenhar() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Desenhar Campo em Perspectiva (Linhas de Profundidade)
  ctx.strokeStyle = "rgba(0, 255, 204, 0.15)";
  ctx.lineWidth = 2;

  for (let z = 100; z <= 900; z += 80) {
    const pEsq = (new Entidade3D(-380, z)).getProjecao();
    const pDir = (new Entidade3D(380, z)).getProjecao();
    ctx.beginPath();
    ctx.moveTo(pEsq.x, pEsq.y);
    ctx.lineTo(pDir.x, pDir.y);
    ctx.stroke();
  }

  // Trave 3D no Fundo
  const tEsq = (new Entidade3D(-130, 870)).getProjecao();
  const tDir = (new Entidade3D(130, 870)).getProjecao();
  
  ctx.strokeStyle = "#ff0055";
  ctx.lineWidth = 4;
  ctx.strokeRect(tEsq.x, tEsq.y - 50 * tEsq.escala, tDir.x - tEsq.x, 50 * tEsq.escala);

  // Ordenar entidades por profundidade (Z-Index rendering)
  const listaProjecao = [
    { tipo: 'p', obj: p1 },
    { tipo: 'p', obj: rival },
    { tipo: 'p', obj: goleiroRival },
    { tipo: 'b', obj: bola }
  ].sort((a, b) => (b.obj.z || b.obj.z) - (a.obj.z || a.obj.z));

  // Renderizar entidades ordenadas
  listaProjecao.forEach(item => {
    if (item.tipo === 'p') {
      const p = item.obj.getProjecao();
      
      // Sombra
      ctx.fillStyle = "rgba(0,0,0,0.4)";
      ctx.beginPath();
      ctx.ellipse(p.x, p.y, p.tamanho * 0.6, p.tamanho * 0.2, 0, 0, Math.PI * 2);
      ctx.fill();

      // Corpo
      ctx.fillStyle = item.obj.cor;
      ctx.fillRect(p.x - p.tamanho / 2, p.y - p.tamanho, p.tamanho, p.tamanho);
    } else {
      const b = bola.getProjecao();

      // Sombra
      ctx.fillStyle = "rgba(0,0,0,0.5)";
      ctx.beginPath();
      ctx.ellipse(b.x, b.y + (bola.y * b.escala), b.tamanho * 0.8, b.tamanho * 0.3, 0, 0, Math.PI * 2);
      ctx.fill();

      // Bola
      ctx.fillStyle = "#fff";
      ctx.beginPath();
      ctx.arc(b.x, b.y, b.tamanho, 0, Math.PI * 2);
      ctx.fill();
    }
  });

  // HUD
  ctx.fillStyle = "#00ffcc";
  ctx.font = "bold 26px monospace";
  ctx.fillText(`AZUL: ${golsP1}  |  VERMELHO: ${golsRival}`, 240, 40);

  if (textoGol > 0) {
    ctx.fillStyle = "#ff0055";
    ctx.font = "bold 60px monospace";
    ctx.fillText("G O L !", 300, 230);
    textoGol--;
  }
}

function loop() {
  processarControles();
  IA_Controlar();
  atualizarFisica();
  desenhar();
  requestAnimationFrame(loop);
}

loop();
</script>
</body>
</html>ok