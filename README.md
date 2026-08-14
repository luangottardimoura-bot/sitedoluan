<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Mini GTA</title>
<style>
    * { box-sizing: border-box; }
    body {
        margin: 0;
        overflow: hidden;
        background: #111;
        font-family: Arial, sans-serif;
    }

    canvas {
        display: block;
        background: #397a3b;
    }

    #hud {
        position: fixed;
        top: 15px;
        left: 15px;
        color: white;
        font-size: 18px;
        background: rgba(0,0,0,.65);
        padding: 12px;
        border-radius: 8px;
        z-index: 2;
    }

    #help {
        position: fixed;
        bottom: 15px;
        left: 15px;
        color: white;
        background: rgba(0,0,0,.65);
        padding: 10px;
        border-radius: 8px;
        z-index: 2;
    }
</style>
</head>
<body>

<div id="hud">
    ❤️ Vida: <span id="vida">100</span><br>
    💰 Dinheiro: $<span id="dinheiro">500</span><br>
    ⭐ Procurado: <span id="estrelas">0</span>
</div>

<div id="help">
    WASD = andar/dirigir | E = entrar no carro | ESPAÇO = atirar
</div>

<canvas id="game"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const keys = {};

document.addEventListener("keydown", e => {
    keys[e.key.toLowerCase()] = true;

    if (e.key.toLowerCase() === "e") {
        entrarNoCarro();
    }

    if (e.code === "Space") {
        atirar();
    }
});

document.addEventListener("keyup", e => {
    keys[e.key.toLowerCase()] = false;
});

let player = {
    x: 600,
    y: 500,
    size: 18,
    speed: 3,
    vida: 100,
    dinheiro: 500,
    noCarro: false
};

let camera = {
    x: 0,
    y: 0
};

let carros = [
    {x: 450, y: 400, cor: "#e63946", ocupado:false},
    {x: 800, y: 550, cor: "#3498db", ocupado:false},
    {x: 1000, y: 350, cor: "#f1c40f", ocupado:false}
];

let policiais = [
    {x: 1300, y: 700, speed: 1.5},
    {x: 300, y: 900, speed: 1.3}
];

let balas = [];

let predios = [];

// Cria a cidade
for (let x = 0; x < 2500; x += 250) {
    for (let y = 0; y < 1800; y += 220) {
        predios.push({
            x: x + 25,
            y: y + 25,
            w: 170,
            h: 130,
            cor: ["#777","#888","#996633","#555"][Math.floor(Math.random()*4)]
        });
    }
}

function atualizar() {

    let velocidade = player.noCarro ? 7 : player.speed;

    let dx = 0;
    let dy = 0;

    if (keys["w"]) dy -= velocidade;
    if (keys["s"]) dy += velocidade;
    if (keys["a"]) dx -= velocidade;
    if (keys["d"]) dx += velocidade;

    player.x += dx;
    player.y += dy;

    player.x = Math.max(20, Math.min(2480, player.x));
    player.y = Math.max(20, Math.min(1780, player.y));

    // Câmera seguindo jogador
    camera.x = player.x - canvas.width / 2;
    camera.y = player.y - canvas.height / 2;

    camera.x = Math.max(0, Math.min(2500 - canvas.width, camera.x));
    camera.y = Math.max(0, Math.min(1800 - canvas.height, camera.y));

    // Polícia persegue jogador
    policiais.forEach(policial => {

        if (player.noCarro || estrelas() > 0) {

            let dx = player.x - policial.x;
            let dy = player.y - policial.y;
            let distancia = Math.hypot(dx, dy);

            if (distancia > 35) {
                policial.x += dx / distancia * policial.speed;
                policial.y += dy / distancia * policial.speed;
            }

            if (distancia < 40) {
                player.vida -= 0.2;
            }
        }
    });

    // Atualiza balas
    balas.forEach(b => {
        b.x += b.dx;
        b.y += b.dy;
        b.tempo--;
    });

    balas = balas.filter(b => b.tempo > 0);

    // Bala acerta policial
    balas.forEach(b => {
        policiais.forEach(p => {

            if (Math.hypot(b.x-p.x, b.y-p.y) < 25) {
                p.x = Math.random()*2400;
                p.y = Math.random()*1700;

                player.dinheiro += 100;
            }

        });
    });

    if (player.vida <= 0) {
        alert("Você morreu!");
        location.reload();
    }

    document.getElementById("vida").textContent =
        Math.floor(player.vida);

    document.getElementById("dinheiro").textContent =
        player.dinheiro;

    document.getElementById("estrelas").textContent =
        "★".repeat(estrelas());
}

function estrelas() {
    return player.noCarro ? 2 : 1;
}

function entrarNoCarro() {

    let carroMaisProximo = null;
    let menorDistancia = 60;

    carros.forEach(carro => {

        let d = Math.hypot(
            player.x - carro.x,
            player.y - carro.y
        );

        if (d < menorDistancia) {
            menorDistancia = d;
            carroMaisProximo = carro;
        }
    });

    if (carroMaisProximo) {

        player.noCarro = !player.noCarro;

        if (player.noCarro) {
            carroMaisProximo.ocupado = true;
        } else {
            carroMaisProximo.ocupado = false;
        }
    }
}

function atirar() {

    let velocidade = 12;

    // Tiro para a direita
    balas.push({
        x: player.x,
        y: player.y,
        dx: velocidade,
        dy: 0,
        tempo: 80
    });

    player.dinheiro -= 1;
}

function desenharCidade() {

    // Fundo
    ctx.fillStyle = "#3d8c40";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // Estradas horizontais
    ctx.fillStyle = "#333";

    for (let y = 180; y < 1800; y += 400) {
        ctx.fillRect(
            -camera.x,
            y-camera.y,
            2500,
            90
        );
    }

    // Estradas verticais
    for (let x = 180; x < 2500; x += 500) {
        ctx.fillRect(
            x-camera.x,
            -camera.y,
            90,
            1800
        );
    }

    // Prédios
    predios.forEach(p => {

        ctx.fillStyle = p.cor;

        ctx.fillRect(
            p.x-camera.x,
            p.y-camera.y,
            p.w,
            p.h
        );

        // Janelas
        ctx.fillStyle = "#f5d76e";

        for (let wx = 0; wx < 4; wx++) {
            for (let wy = 0; wy < 3; wy++) {

                ctx.fillRect(
                    p.x + 20 + wx*38-camera.x,
                    p.y + 20 + wy*35-camera.y,
                    15,
                    15
                );

            }
        }
    });
}

function desenharCarros() {

    carros.forEach(carro => {

        ctx.fillStyle = carro.cor;

        ctx.fillRect(
            carro.x-camera.x-30,
            carro.y-camera.y-18,
            60,
            36
        );

        ctx.fillStyle = "#111";

        ctx.fillRect(
            carro.x-camera.x-20,
            carro.y-camera.y-14,
            40,
            10
        );

        // rodas
        ctx.fillStyle = "#050505";

        ctx.fillRect(
            carro.x-camera.x-25,
            carro.y-camera.y+14,
            14,
            8
        );

        ctx.fillRect(
            carro.x-camera.x+11,
            carro.y-camera.y+14,
            14,
            8
        );
    });
}

function desenharPolicia() {

    policiais.forEach(p => {

        ctx.fillStyle = "#111";

        ctx.fillRect(
            p.x-camera.x-12,
            p.y-camera.y-15,
            24,
            30
        );

        ctx.fillStyle = "#3498db";

        ctx.beginPath();
        ctx.arc(
            p.x-camera.x,
            p.y-camera.y-20,
            8,
            0,
            Math.PI*2
        );
        ctx.fill();
    });
}

function desenharPlayer() {

    ctx.fillStyle = player.noCarro ? "#ffcc00" : "#00ff66";

    ctx.beginPath();

    ctx.arc(
        player.x-camera.x,
        player.y-camera.y,
        player.noCarro ? 20 : player.size,
        0,
        Math.PI*2
    );

    ctx.fill();

    // Vida acima do personagem
    ctx.fillStyle = "red";

    ctx.fillRect(
        player.x-camera.x-20,
        player.y-camera.y-35,
        40,
        5
    );

    ctx.fillStyle = "lime";

    ctx.fillRect(
        player.x-camera.x-20,
        player.y-camera.y-35,
        40 * (player.vida/100),
        5
    );
}

function desenharBalas() {

    balas.forEach(b => {

        ctx.fillStyle = "yellow";

        ctx.beginPath();

        ctx.arc(
            b.x-camera.x,
            b.y-camera.y,
            4,
            0,
            Math.PI*2
        );

        ctx.fill();
    });
}

function desenhar() {

    ctx.clearRect(0,0,canvas.width,canvas.height);

    desenharCidade();
    desenharCarros();
    desenharPolicia();
    desenharBalas();
    desenharPlayer();
}

function loop() {

    atualizar();
    desenhar();

    requestAnimationFrame(loop);
}

loop();

window.addEventListener("resize", () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
});
</script>

</body>
</html>