<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Craques da Bola 1B</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    background: #101010;
    color: white;
    font-family: monospace;
    text-align: center;
}

h1 {
    color: #ffd700;
    font-size: 38px;
    margin: 15px;
    text-shadow: 4px 4px #000;
}

#menu {
    width: 90%;
    max-width: 700px;
    margin: 30px auto;
    padding: 25px;
    background: #222;
    border: 6px solid #555;
    box-shadow: 8px 8px #000;
}

#menu h2 {
    color: #00ff55;
    font-size: 30px;
}

button {
    background: #ffd000;
    color: #111;
    border: 4px solid white;
    padding: 12px 30px;
    font-family: monospace;
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
    box-shadow: 5px 5px #000;
}

button:hover {
    background: #fff;
}

#placar {
    font-size: 28px;
    margin: 10px;
    font-weight: bold;
}

#placarJogador {
    color: #2d6cff;
}

#placarCPU {
    color: #ff3030;
}

canvas {
    width: 900px;
    max-width: 95vw;
    border: 6px solid #777;
    display: block;
    margin: auto;
    image-rendering: pixelated;
}

.instrucoes {
    font-size: 17px;
    color: #ddd;
}
</style>
</head>

<body>

<h1>⚽ CRAQUES DA BOLA 1B ⚽</h1>

<div id="menu">

    <h2>🏆 CRAQUES DA BOLA 1B 🏆</h2>

    <p>O clássico futebol retrô!</p>

    <p class="instrucoes">
        🎮 SETAS ou WASD = MOVER<br>
        ⚽ ESPAÇO = CHUTAR
    </p>

    <button onclick="iniciarJogo()">JOGAR</button>

</div>

<div id="jogo" style="display:none;">

    <div id="placar">
        🔵 VOCÊ
        <span id="placarJogador">0</span>
        X
        <span id="placarCPU">0</span>
        🔴 CPU
    </div>

    <canvas id="campo" width="900" height="500"></canvas>

    <p class="instrucoes">
        SETAS / WASD = MOVER &nbsp; | &nbsp; ESPAÇO = CHUTAR
    </p>

    <button onclick="voltarMenu()">MENU</button>

</div>

<script>

const canvas = document.getElementById("campo");
const ctx = canvas.getContext("2d");

const teclas = {};

let jogoAtivo = false;

let placarJogador = 0;
let placarCPU = 0;

const jogador = {
    x: 180,
    y: 250,
    raio: 18,
    velocidade: 4,
    cor: "#2455ff"
};

const cpu = {
    x: 720,
    y: 250,
    raio: 18,
    velocidade: 2.4,
    cor: "#ed3030"
};

const bola = {
    x: 450,
    y: 250,
    raio: 10,
    vx: 0,
    vy: 0
};

document.addEventListener("keydown", function(e) {

    teclas[e.key.toLowerCase()] = true;

    if (e.code === "Space") {
        teclas.space = true;
        e.preventDefault();
    }
});

document.addEventListener("keyup", function(e) {

    teclas[e.key.toLowerCase()] = false;

    if (e.code === "Space") {
        teclas.space = false;
    }
});

function iniciarJogo() {

    document.getElementById("menu").style.display = "none";
    document.getElementById("jogo").style.display = "block";

    placarJogador = 0;
    placarCPU = 0;

    atualizarPlacar();

    jogoAtivo = true;

    reiniciar();
}

function voltarMenu() {

    jogoAtivo = false;

    document.getElementById("jogo").style.display = "none";
    document.getElementById("menu").style.display = "block";
}

function atualizarPlacar() {

    document.getElementById("placarJogador").textContent =
        placarJogador;

    document.getElementById("placarCPU").textContent =
        placarCPU;
}

function distancia(a, b) {

    const dx = a.x - b.x;
    const dy = a.y - b.y;

    return Math.sqrt(dx * dx + dy * dy);
}

function moverJogador() {

    let dx = 0;
    let dy = 0;

    if (teclas.w || teclas.arrowup) dy--;
    if (teclas.s || teclas.arrowdown) dy++;
    if (teclas.a || teclas.arrowleft) dx--;
    if (teclas.d || teclas.arrowright) dx++;

    if (dx !== 0 || dy !== 0) {

        const tamanho =
            Math.sqrt(dx * dx + dy * dy);

        dx /= tamanho;
        dy /= tamanho;

        jogador.x +=
            dx * jogador.velocidade;

        jogador.y +=
            dy * jogador.velocidade;
    }

    jogador.x =
        Math.max(30, Math.min(870, jogador.x));

    jogador.y =
        Math.max(30, Math.min(470, jogador.y));
}

function moverCPU() {

    const dx = bola.x - cpu.x;
    const dy = bola.y - cpu.y;

    const d = Math.sqrt(dx * dx + dy * dy);

    if (d > 5) {

        cpu.x +=
            (dx / d) * cpu.velocidade;

        cpu.y +=
            (dy / d) * cpu.velocidade;
    }

    cpu.x =
        Math.max(30, Math.min(870, cpu.x));

    cpu.y =
        Math.max(30, Math.min(470, cpu.y));
}

function chutar() {

    if (distancia(jogador, bola) < 48) {

        let dx = bola.x - jogador.x;
        let dy = bola.y - jogador.y;

        let d = Math.sqrt(dx * dx + dy * dy);

        if (d === 0) d = 1;

        bola.vx =
            (dx / d) * 10;

        bola.vy =
            (dy / d) * 10;
    }
}

function chuteCPU() {

    if (distancia(cpu, bola) < 40) {

        let dx = 20 - bola.x;
        let dy = 250 - bola.y;

        let d = Math.sqrt(dx * dx + dy * dy);

        if (d === 0) d = 1;

        bola.vx =
            (dx / d) * 6;

        bola.vy =
            (dy / d) * 6;
    }
}

function atualizarBola() {

    bola.x += bola.vx;
    bola.y += bola.vy;

    bola.vx *= 0.985;
    bola.vy *= 0.985;

    // Parte superior
    if (bola.y < bola.raio) {

        bola.y = bola.raio;
        bola.vy *= -0.8;
    }

    // Parte inferior
    if (bola.y > canvas.height - bola.raio) {

        bola.y =
            canvas.height - bola.raio;

        bola.vy *= -0.8;
    }

    // Parede esquerda
    if (
        bola.x < bola.raio &&
        !(bola.y > 180 && bola.y < 320)
    ) {

        bola.x = bola.raio;
        bola.vx *= -0.8;
    }

    // Parede direita
    if (
        bola.x > canvas.width - bola.raio &&
        !(bola.y > 180 && bola.y < 320)
    ) {

        bola.x =
            canvas.width - bola.raio;

        bola.vx *= -0.8;
    }

    // Gol do CPU
    if (bola.x < -5) {

        placarCPU++;

        atualizarPlacar();

        verificarFim();

        reiniciar();
    }

    // Gol do jogador
    if (bola.x > canvas.width + 5) {

        placarJogador++;

        atualizarPlacar();

        verificarFim();

        reiniciar();
    }
}

function verificarFim() {

    if (placarJogador >= 5) {

        jogoAtivo = false;

        setTimeout(() => {

            alert(
                "🏆 CRAQUES DA BOLA 1B 🏆\n\n" +
                "VOCÊ VENCEU!"
            );

            placarJogador = 0;
            placarCPU = 0;

            atualizarPlacar();

            jogoAtivo = true;

        }, 100);
    }

    if (placarCPU >= 5) {

        jogoAtivo = false;

        setTimeout(() => {

            alert(
                "⚽ CRAQUES DA BOLA 1B ⚽\n\n" +
                "A CPU VENCEU!"
            );

            placarJogador = 0;
            placarCPU = 0;

            atualizarPlacar();

            jogoAtivo = true;

        }, 100);
    }
}

function reiniciar() {

    jogador.x = 180;
    jogador.y = 250;

    cpu.x = 720;
    cpu.y = 250;

    bola.x = 450;
    bola.y = 250;

    bola.vx = 0;
    bola.vy = 0;
}

function colisao(j) {

    const d = distancia(j, bola);

    if (d < j.raio + bola.raio) {

        let dx = bola.x - j.x;
        let dy = bola.y - j.y;

        let tamanho =
            Math.sqrt(dx * dx + dy * dy);

        if (tamanho === 0) tamanho = 1;

        bola.x =
            j.x +
            (dx / tamanho) *
            (j.raio + bola.raio);

        bola.y =
            j.y +
            (dy / tamanho) *
            (j.raio + bola.raio);

        bola.vx +=
            (dx / tamanho) * 0.6;

        bola.vy +=
            (dy / tamanho) * 0.6;
    }
}

function desenharCampo() {

    // Gramado
    ctx.fillStyle = "#168a35";

    ctx.fillRect(
        0,
        0,
        canvas.width,
        canvas.height
    );

    // Faixas
    for (let x = 0; x < 900; x += 100) {

        ctx.fillStyle =
            x % 200 === 0
            ? "#1b913b"
            : "#168a35";

        ctx.fillRect(
            x,
            0,
            100,
            500
        );
    }

    // Linhas
    ctx.strokeStyle = "white";
    ctx.lineWidth = 5;

    ctx.strokeRect(
        10,
        10,
        880,
        480
    );

    // Linha central
    ctx.beginPath();

    ctx.moveTo(450, 10);
    ctx.lineTo(450, 490);

    ctx.stroke();

    // Círculo
    ctx.beginPath();

    ctx.arc(
        450,
        250,
        75,
        0,
        Math.PI * 2
    );

    ctx.stroke();

    // Área esquerda
    ctx.strokeRect(
        10,
        140,
        130,
        220
    );

    // Área direita
    ctx.strokeRect(
        760,
        140,
        130,
        220
    );

    // Gols
    ctx.fillStyle = "#ddd";

    ctx.fillRect(
        0,
        180,
        35,
        140
    );

    ctx.fillRect(
        865,
        180,
        35,
        140
    );
}

function desenharJogador(j) {

    // Sombra
    ctx.fillStyle =
        "rgba(0,0,0,0.35)";

    ctx.beginPath();

    ctx.ellipse(
        j.x,
        j.y + 20,
        22,
        7,
        0,
        0,
        Math.PI * 2
    );

    ctx.fill();

    // Corpo
    ctx.fillStyle = j.cor;

    ctx.fillRect(
        j.x - 15,
        j.y - 15,
        30,
        30
    );

    // Cabeça
    ctx.fillStyle = "#ffd0a0";

    ctx.fillRect(
        j.x - 10,
        j.y - 29,
        20,
        14
    );

    // Cabelo
    ctx.fillStyle = "#222";

    ctx.fillRect(
        j.x - 10,
        j.y - 30,
        20,
        6
    );

    // Pernas
    ctx.fillStyle = "#111";

    ctx.fillRect(
        j.x - 12,
        j.y + 14,
        8,
        13
    );

    ctx.fillRect(
        j.x + 4,
        j.y + 14,
        8,
        13
    );
}

function desenharBola() {

    // Sombra
    ctx.fillStyle =
        "rgba(0,0,0,0.3)";

    ctx.beginPath();

    ctx.ellipse(
        bola.x,
        bola.y + 9,
        11,
        5,
        0,
        0,
        Math.PI * 2
    );

    ctx.fill();

    // Bola
    ctx.fillStyle = "white";

    ctx.beginPath();

    ctx.arc(
        bola.x,
        bola.y,
        bola.raio,
        0,
        Math.PI * 2
    );

    ctx.fill();

    // Detalhe
    ctx.fillStyle = "#111";

    ctx.fillRect(
        bola.x - 3,
        bola.y - 3,
        6,
        6
    );
}

function desenhar() {

    desenharCampo();

    desenharJogador(jogador);

    desenharJogador(cpu);

    desenharBola();

    ctx.fillStyle = "white";
    ctx.font = "bold 14px monospace";

    ctx.fillText(
        "VOCÊ",
        jogador.x - 20,
        jogador.y - 40
    );

    ctx.fillText(
        "CPU",
        cpu.x - 15,
        cpu.y - 40
    );
}

function atualizar() {

    if (!jogoAtivo) return;

    moverJogador();

    moverCPU();

    if (teclas.space) {
        chutar();
    }

    chuteCPU();

    colisao(jogador);

    colisao(cpu);

    atualizarBola();
}

function loop() {

    atualizar();

    if (jogoAtivo) {
        desenhar();
    }

    requestAnimationFrame(loop);
}

loop();

</script>

</body>
</html>