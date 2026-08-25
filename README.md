<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Futebol Retrô Arcade</title>
<style>
  body {
    margin: 0;
    background: #0d0d11;
    color: white;
    font-family: 'Courier New', Courier, monospace;
    text-align: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
  }

  h1 {
    color: #00ff66;
    margin: 10px 0;
    text-shadow: 0 0 10px #00ff66;
  }

  canvas {
    background: #116729;
    border: 4px solid #fff;
    box-shadow: 0 0 20px rgba(0, 255, 102, 0.3);
    max-width: 95vw;
    height: auto;
    image-rendering: pixelated;
  }

  p {
    color: #aaa;
    margin-top: 10px;
  }

  span.highlight {
    color: #ffcc00;
  }
</style>
</head>

<body>
<h1>⚽ FUTEBOL RETRÔ ARCADE ⚽</h1>
<canvas id="campo" width="800" height="500"></canvas>
<p>
  <b>WASD/Setas</b>: Mover | <span class="highlight"><b>SHIFT</b>: Correr (Sprint)</span> | <b>Espaço</b>: Chutar
</p>

<script>
const canvas = document.getElementById("campo");
const ctx = canvas.getContext("2d");

const jogador = {
  x: 180,
  y: 250,
  tamanho: 24,
  velocidadeBase: 3.5,
  velocidade: 3.5,
  estamina: 100,
  cor: "#2196ff"
};

const adversario = {
  x: 620,
  y: 250,
  tamanho: 24,
  velocidade: 2.8,
  cor: "#ff3030"
};

const bola = {
  x: 400,
  y: 250,
  raio: 9,
  vx: 0,
  vy: 0,
  rastro: []
};

let teclas = {};
let golsJogador = 0;
let golsAdversario = 0;
let textoGolTempo = 0;

document.addEventListener("keydown", e => {
  teclas[e.key.toLowerCase()] = true;
  if (e.code === "Space") chutar();
});

document.addEventListener("keyup", e => {
  teclas[e.key.toLowerCase()] = false;
});

function distancia(a, b) {
  return Math.hypot(a.x - b.x, a.y - b.y);
}

function chutar() {
  if (distancia(jogador, bola) < 45) {
    const angulo = Math.atan2(bola.y - jogador.y, bola.x - jogador.x);
    const forca = 12;
    bola.vx = Math.cos(angulo) * forca;
    bola.vy = Math.sin(angulo) * forca;
  }
}

function moverJogador() {
  // Controle de Sprint
  if (teclas["shift"] && jogador.estamina > 5) {
    jogador.velocidade = jogador.velocidadeBase * 1.6;
    jogador.estamina = Math.max(0, jogador.estamina - 1.2);
  } else {
    jogador.velocidade = jogador.velocidadeBase;
    jogador.estamina = Math.min(100, jogador.estamina + 0.4);
  }

  if (teclas["arrowup"] || teclas["w"]) jogador.y -= jogador.velocidade;
  if (teclas["arrowdown"] || teclas["s"]) jogador.y += jogador.velocidade;
  if (teclas["arrowleft"] || teclas["a"]) jogador.x -= jogador.velocidade;
  if (teclas["arrowright"] || teclas["d"]) jogador.x += jogador.velocidade;

  jogador.x = Math.max(20, Math.min(780, jogador.x));
  jogador.y = Math.max(20, Math.min(480, jogador.y));

  verificarColisao(jogador);
}

function moverAdversario() {
  if (adversario.y < bola.y) adversario.y += adversario.velocidade;
  if (adversario.y > bola.y) adversario.y -= adversario.velocidade;

  if (adversario.x < bola.x && adversario.x < 720) adversario.x += adversario.velocidade * 0.6;
  if (adversario.x > bola.x && adversario.x > 380) adversario.x -= adversario.velocidade * 0.6;

  verificarColisao(adversario);

  // Chute da IA
  if (distancia(adversario, bola) < 40 && Math.random() < 0.08) {
    const angulo = Math.atan2(bola.y - adversario.y, bola.x - adversario.x);
    const forca = 9;
    bola.vx = Math.cos(angulo) * forca;
    bola.vy = Math.sin(angulo) * forca;
  }
}

function verificarColisao(p) {
  if (distancia(p, bola) < p.tamanho / 2 + bola.raio) {
    const angulo = Math.atan2(bola.y - p.y, bola.x - p.x);
    bola.vx = Math.cos(angulo) * 3.5;
    bola.vy = Math.sin(angulo) * 3.5;
  }
}

function atualizarBola() {
  // Rastro visual da bola
  if (Math.hypot(bola.vx, bola.vy) > 2) {
    bola.rastro.push({ x: bola.x, y: bola.y, alpha: 0.6 });
  }
  if (bola.rastro.length > 5) bola.rastro.shift();

  bola.x += bola.vx;
  bola.y += bola.vy;

  bola.vx *= 0.985;
  bola.vy *= 0.985;

  // Bordas Topo / Baixo
  if (bola.y - bola.raio < 5 || bola.y + bola.raio > 495) {
    bola.vy *= -1;
  }

  // Gol do jogador
  if (bola.x > 790 && bola.y > 190 && bola.y < 310) {
    golsJogador++;
    textoGolTempo = 60;
    verificarVitoria();
    reiniciar();
  }

  // Gol do adversário
  if (bola.x < 10 && bola.y > 190 && bola.y < 310) {
    golsAdversario++;
    textoGolTempo = 60;
    verificarVitoria();
    reiniciar();
  }

  // Traves / Paredes laterais
  if ((bola.x - bola.raio < 5 || bola.x + bola.raio > 795) && (bola.y <= 190 || bola.y >= 310)) {
    bola.vx *= -1;
  }
}

function verificarVitoria() {
  if (golsJogador >= 5 || golsAdversario >= 5) {
    setTimeout(() => {
      alert(golsJogador >= 5 ? "🏆 VOCÊ FOI CAMPEÃO!" : "❌ O ADVERSÁRIO VENCEU!");
      golsJogador = 0;
      golsAdversario = 0;
    }, 150);
  }
}

function reiniciar() {
  bola.x = 400;
  bola.y = 250;
  bola.vx = 0;
  bola.vy = 0;
  bola.rastro = [];

  jogador.x = 180;
  jogador.y = 250;

  adversario.x = 620;
  adversario.y = 250;
}

function desenharCampo() {
  ctx.clearRect(0, 0, 800, 500);

  // Listras do Gramado
  for (let i = 0; i < 800; i += 80) {
    ctx.fillStyle = (i / 80) % 2 === 0 ? "#146126" : "#166a2a";
    ctx.fillRect(i, 0, 80, 500);
  }

  // Linhas do campo
  ctx.strokeStyle = "rgba(255, 255, 255, 0.8)";
  ctx.lineWidth = 3;

  ctx.strokeRect(5, 5, 790, 490);

  ctx.beginPath();
  ctx.moveTo(400, 5);
  ctx.lineTo(400, 495);
  ctx.stroke();

  ctx.beginPath();
  ctx.arc(400, 250, 70, 0, Math.PI * 2);
  ctx.stroke();

  // Áreas
  ctx.strokeRect(5, 150, 110, 200);
  ctx.strokeRect(685, 150, 110, 200);

  // Redes
  ctx.fillStyle = "#fff";
  ctx.fillRect(0, 190, 8, 120);
  ctx.fillRect(792, 190, 8, 120);
}

function desenharJogador(obj) {
  // Sombra
  ctx.fillStyle = "rgba(0,0,0,0.2)";
  ctx.beginPath();
  ctx.ellipse(obj.x, obj.y + 12, 12, 5, 0, 0, Math.PI * 2);
  ctx.fill();

  // Corpo
  ctx.fillStyle = obj.cor;
  ctx.fillRect(obj.x - obj.tamanho / 2, obj.y - obj.tamanho / 2, obj.tamanho, obj.tamanho);
  ctx.strokeStyle = "#fff";
  ctx.lineWidth = 1;
  ctx.strokeRect(obj.x - obj.tamanho / 2, obj.y - obj.tamanho / 2, obj.tamanho, obj.tamanho);
}

function desenharBola() {
  // Desenhar Rastro
  bola.rastro.forEach(p => {
    ctx.fillStyle = `rgba(255, 255, 255, ${p.alpha})`;
    ctx.beginPath();
    ctx.arc(p.x, p.y, bola.raio * 0.8, 0, Math.PI * 2);
    ctx.fill();
    p.alpha -= 0.1;
  });

  // Bola
  ctx.fillStyle = "#fff";
  ctx.beginPath();
  ctx.arc(bola.x, bola.y, bola.raio, 0, Math.PI * 2);
  ctx.fill();

  ctx.strokeStyle = "#000";
  ctx.lineWidth = 1.5;
  ctx.stroke();
}

function desenharUI() {
  // Placar
  ctx.fillStyle = "#fff";
  ctx.font = "bold 32px 'Courier New', monospace";
  ctx.fillText(`${golsJogador}  -  ${golsAdversario}`, 350, 45);

  // Barra de Estamina do Jogador
  ctx.fillStyle = "rgba(255, 255, 255, 0.3)";
  ctx.fillRect(20, 20, 100, 10);
  ctx.fillStyle = jogador.estamina > 20 ? "#00ff66" : "#ff3333";
  ctx.fillRect(20, 20, jogador.estamina, 10);
  ctx.strokeStyle = "#fff";
  ctx.strokeRect(20, 20, 100, 10);

  // Animação de GOL
  if (textoGolTempo > 0) {
    ctx.fillStyle = "#ffcc00";
    ctx.font = "bold 60px 'Courier New', monospace";
    ctx.fillText("G O L !", 300, 260);
    textoGolTempo--;
  }
}

function jogo() {
  moverJogador();
  moverAdversario();
  atualizarBola();

  desenharCampo();
  desenharJogador(jogador);
  desenharJogador(adversario);
  desenharBola();
  desenharUI();

  requestAnimationFrame(jogo);
}

jogo();
</script>
</body>
</html>