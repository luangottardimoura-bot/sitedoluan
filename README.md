<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Futebol Retrô</title>
<style>
  body {
    margin: 0;
    background: #111;
    color: white;
    font-family: monospace;
    text-align: center;
  }

  h1 {
    color: #00ff66;
  }

  canvas {
    background: #168a35;
    border: 5px solid white;
    max-width: 95vw;
    height: auto;
    image-rendering: pixelated;
  }

  p {
    color: #ccc;
  }
</style>
</head>

<body>
<h1>⚽ FUTEBOL RETRÔ ⚽</h1>
<canvas id="campo" width="800" height="500"></canvas>
<p>Use WASD ou as setas para jogar • Espaço para chutar</p>

<script>
const canvas = document.getElementById("campo");
const ctx = canvas.getContext("2d");

const jogador = {
  x: 180,
  y: 250,
  tamanho: 25,
  velocidade: 4,
  cor: "#2196ff"
};

const adversario = {
  x: 620,
  y: 250,
  tamanho: 25,
  velocidade: 2.5,
  cor: "#ff3030"
};

const bola = {
  x: 400,
  y: 250,
  raio: 10,
  vx: 0,
  vy: 0
};

let teclas = {};
let golsJogador = 0;
let golsAdversario = 0;

document.addEventListener("keydown", e => {
  teclas[e.key.toLowerCase()] = true;

  if (e.code === "Space") {
    chutar();
  }
});

document.addEventListener("keyup", e => {
  teclas[e.key.toLowerCase()] = false;
});

function distancia(a, b) {
  return Math.hypot(a.x - b.x, a.y - b.y);
}

function chutar() {
  if (distancia(jogador, bola) < 50) {
    bola.vx = 9;
    bola.vy = (bola.y - jogador.y) * 0.15;
  }
}

function moverJogador() {
  if (teclas["arrowup"] || teclas["w"]) jogador.y -= jogador.velocidade;
  if (teclas["arrowdown"] || teclas["s"]) jogador.y += jogador.velocidade;
  if (teclas["arrowleft"] || teclas["a"]) jogador.x -= jogador.velocidade;
  if (teclas["arrowright"] || teclas["d"]) jogador.x += jogador.velocidade;

  jogador.x = Math.max(20, Math.min(780, jogador.x));
  jogador.y = Math.max(20, Math.min(480, jogador.y));
}

function moverAdversario() {
  if (adversario.y < bola.y) adversario.y += adversario.velocidade;
  if (adversario.y > bola.y) adversario.y -= adversario.velocidade;

  if (distancia(adversario, bola) < 45) {
    bola.vx = -7;
    bola.vy = (bola.y - adversario.y) * 0.12;
  }
}

function atualizarBola() {
  bola.x += bola.vx;
  bola.y += bola.vy;

  bola.vx *= 0.985;
  bola.vy *= 0.985;

  if (bola.y < 10 || bola.y > 490) {
    bola.vy *= -1;
  }

  // Gol do jogador
  if (bola.x > 790 && bola.y > 190 && bola.y < 310) {
    golsJogador++;
    reiniciar();
  }

  // Gol do adversário
  if (bola.x < 10 && bola.y > 190 && bola.y < 310) {
    golsAdversario++;
    reiniciar();
  }

  // Laterais
  if (bola.x < 10 || bola.x > 790) {
    bola.vx *= -1;
  }
}

function reiniciar() {
  bola.x = 400;
  bola.y = 250;
  bola.vx = 0;
  bola.vy = 0;

  jogador.x = 180;
  jogador.y = 250;

  adversario.x = 620;
  adversario.y = 250;
}

function desenharCampo() {
  ctx.clearRect(0, 0, 800, 500);

  // Campo
  ctx.fillStyle = "#168a35";
  ctx.fillRect(0, 0, 800, 500);

  // Linhas
  ctx.strokeStyle = "white";
  ctx.lineWidth = 4;

  ctx.strokeRect(5, 5, 790, 490);

  ctx.beginPath();
  ctx.moveTo(400, 5);
  ctx.lineTo(400, 495);
  ctx.stroke();

  ctx.beginPath();
  ctx.arc(400, 250, 70, 0, Math.PI * 2);
  ctx.stroke();

  // Áreas
  ctx.strokeRect(5, 160, 100, 180);
  ctx.strokeRect(695, 160, 100, 180);

  // Gols
  ctx.fillStyle = "#ddd";
  ctx.fillRect(0, 190, 10, 120);
  ctx.fillRect(790, 190, 10, 120);
}

function desenharJogador(obj) {
  ctx.fillStyle = obj.cor;
  ctx.fillRect(
    obj.x - obj.tamanho / 2,
    obj.y - obj.tamanho / 2,
    obj.tamanho,
    obj.tamanho
  );
}

function desenharBola() {
  ctx.fillStyle = "white";
  ctx.beginPath();
  ctx.arc(bola.x, bola.y, bola.raio, 0, Math.PI * 2);
  ctx.fill();

  ctx.strokeStyle = "black";
  ctx.stroke();
}

function desenharPlacar() {
  ctx.fillStyle = "white";
  ctx.font = "bold 28px monospace";
  ctx.fillText(
    `${golsJogador}  -  ${golsAdversario}`,
    365,
    40
  );
}

function jogo() {
  moverJogador();
  moverAdversario();
  atualizarBola();

  desenharCampo();
  desenharJogador(jogador);
  desenharJogador(adversario);
  desenharBola();
  desenharPlacar();

  requestAnimationFrame(jogo);
}

jogo();
</script>

</body>
</html>
