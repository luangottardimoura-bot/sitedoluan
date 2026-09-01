<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FPS Retro 2.5D Simples - Sem Three.js</title>
    <style>
        body { margin: 0; overflow: hidden; font-family: sans-serif; background: #222; }
        #canvas-container { position: relative; width: 100vw; height: 100vh; }
        canvas { display: block; background-color: #000; }
        
        /* UI Elements */
        #ui { position: absolute; bottom: 20px; left: 20px; color: white; text-shadow: 2px 2px 0 #000; font-size: 24px; pointer-events: none; }
        #crosshair { 
            position: absolute; top: 50%; left: 50%; 
            width: 10px; height: 10px; border: 2px solid white; border-radius: 50%;
            transform: translate(-50%, -50%); pointer-events: none; 
        }
        #instructions {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8); color: white;
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            cursor: pointer; text-align: center;
        }
    </style>
</head>
<body>

<div id="canvas-container">
    <canvas id="gameCanvas"></canvas>
    <div id="crosshair"></div>
    <div id="ui">
        Vida: <span id="health">100</span> | Pontos: <span id="score">0</span>
    </div>
    <div id="instructions">
        <h1>Clique para Jogar</h1>
        <p>Mover: **WASD** | Olhar: **Mouse** | Atirar: **Clique**</p>
        <p>(Necessário navegador moderno)</p>
    </div>
</div>

<script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const uiHealth = document.getElementById('health');
    const uiScore = document.getElementById('score');
    const instructions = document.getElementById('instructions');

    // Configuração do Canvas (resolução interna baixa para visual retro)
    const WIDTH = 640; 
    const HEIGHT = 480;
    canvas.width = WIDTH;
    canvas.height = HEIGHT;

    // --- ESTADO DO JOGO ---
    let gameState = {
        running: false,
        health: 100,
        score: 0,
        lastShotTime: 0,
        fireRate: 200 // ms entre tiros
    };

    // --- CONFIGURAÇÃO DO JOGADOR ---
    let player = {
        x: 100,
        y: HEIGHT / 2,
        angle: 0, // em radianos
        speed: 3,
        rotationSpeed: 0.003, // sensibilidade do mouse
        fov: Math.PI / 3, // 60 graus de campo de visão
        bobbing: 0 // para efeito de andar
    };

    // --- OBJETOS (INIMIGOS/ZUMBIS) ---
    // Representados como círculos 2D que projetaremos
    let entities = [
        { x: 500, y: 150, radius: 20, color: '#0f0', type: 'zombie', health: 30 },
        { x: 450, y: 350, radius: 25, color: '#0a0', type: 'zombie', health: 50 }
    ];

    let keys = {};

    // --- CONTROLES (Mover) ---
    window.addEventListener('keydown', e => keys[e.code] = true);
    window.addEventListener('keyup', e => keys[e.code] = false);

    // --- CONTROLES (Pointer Lock e Olhar) ---
    instructions.addEventListener('click', () => {
        canvas.requestPointerLock();
    });

    document.addEventListener('pointerlockchange', () => {
        if (document.pointerLockElement === canvas) {
            instructions.style.display = 'none';
            gameState.running = true;
        } else {
            instructions.style.display = 'flex';
            gameState.running = false;
        }
    });

    document.addEventListener('mousemove', e => {
        if (gameState.running) {
            player.angle += e.movementX * player.rotationSpeed;
        }
    });

    // --- CONTROLES (Atirar) ---
    window.addEventListener('mousedown', e => {
        if (gameState.running && e.button === 0) {
            atirar();
        }
    });

    function atirar() {
        const agora = Date.now();
        if (agora - gameState.lastShotTime < gameState.fireRate) return;
        gameState.lastShotTime = agora;

        // Efeito visual simples de "flash" no centro
        ctx.fillStyle = 'rgba(255, 255, 255, 0.5)';
        ctx.fillRect(WIDTH/2 - 20, HEIGHT/2 - 20, 40, 40);

        // Verificação de acerto (Lógica simplificada 2D)
        // Verificamos quais entidades estão próximas da linha de visão do jogador
        entities.forEach((entity, index) => {
            if (entity.type !== 'zombie' || entity.health <= 0) return;

            // Calcula o ângulo relativo da entidade para o jogador
            const dx = entity.x - player.x;
            const dy = entity.y - player.y;
            const angleToEntity = Math.atan2(dy, dx);

            // Diferença de ângulo (normalizada entre -PI e PI)
            let angleDiff = angleToEntity - player.angle;
            while (angleDiff < -Math.PI) angleDiff += 2 * Math.PI;
            while (angleDiff > Math.PI) angleDiff -= 2 * Math.PI;

            // Se a entidade estiver dentro do FOV e próxima do centro da mira
            const distancia = Math.sqrt(dx*dx + dy*dy);
            const margemAcerto = (entity.radius / distancia) * 1.2; // Aumenta margem com a distância

            if (Math.abs(angleDiff) < margemAcerto && distancia < 600) {
                entity.health -= 20;
                entity.color = '#fff'; // Feedback visual de dano
                setTimeout(() => entity.color = (entity.health > 0) ? (entity.radius > 22 ? '#0a0' : '#0f0') : '#333', 100);

                if (entity.health <= 0) {
                    gameState.score += (entity.radius > 22 ? 20 : 10);
                    uiScore.innerText = gameState.score;
                    entity.color = '#333'; // Cor de "morto"
                }
            }
        });
    }

    // --- LOOP PRINCIPAL DO JOGO ---
    function gameLoop() {
        if (gameState.running) {
            update();
            draw();
        }
        requestAnimationFrame(gameLoop);
    }

    // --- ATUALIZAÇÃO DA LÓGICA ---
    function update() {
        // Movimento (WASD) - Sem colisões com paredes neste exemplo simples
        let moveX = 0;
        let moveY = 0;

        if (keys['KeyW']) { moveX += Math.cos(player.angle); moveY += Math.sin(player.angle); }
        if (keys['KeyS']) { moveX -= Math.cos(player.angle); moveY -= Math.sin(player.angle); }
        if (keys['KeyA']) { moveX += Math.sin(player.angle); moveY -= Math.cos(player.angle); }
        if (keys['KeyD']) { moveX -= Math.sin(player.angle); moveY += Math.cos(player.angle); }

        // Normaliza movimento diagonal
        const len = Math.sqrt(moveX*moveX + moveY*moveY);
        if (len > 0) {
            player.x += (moveX / len) * player.speed;
            player.y += (moveY / len) * player.speed;
            player.bobbing += 0.2; // Efeito de andar
        }
    }

    // --- DESENHAR GRÁFICOS (A "Mágica" do Faux-3D) ---
    function draw() {
        // Limpa o canvas
        ctx.fillStyle = '#000'; // Fundo preto
        ctx.fillRect(0, 0, WIDTH, HEIGHT);

        // Desenha Chão e Teto (Gradientes simples para profundidade)
        const gradientCeil = ctx.createLinearGradient(0, 0, 0, HEIGHT/2);
        gradientCeil.addColorStop(0, '#111'); gradientCeil.addColorStop(1, '#222');
        ctx.fillStyle = gradientCeil;
        ctx.fillRect(0, 0, WIDTH, HEIGHT/2);

        const gradientFloor = ctx.createLinearGradient(0, HEIGHT/2, 0, HEIGHT);
        gradientFloor.addColorStop(0, '#333'); gradientFloor.addColorStop(1, '#111');
        ctx.fillStyle = gradientFloor;
        ctx.fillRect(0, HEIGHT/2, WIDTH, HEIGHT/2);

        // --- PROJEÇÃO DE ENTIDADES (Zumbis) ---
        // Precisamos ordenar as entidades por distância (Z-sorting) para desenhar as mais distantes primeiro
        const sortedEntities = entities
            .map(e => {
                const dx = e.x - player.x;
                const dy = e.y - player.y;
                return { ...e, distance: Math.sqrt(dx*dx + dy*dy), angleTo: Math.atan2(dy, dx) };
            })
            .sort((a, b) => b.distance - a.distance); // Ordena decrescente

        sortedEntities.forEach(entity => {
            if (entity.distance < 10) return; // Muito perto ou atrás

            // Diferença de ângulo entre o jogador e a entidade (normalizada)
            let angleDiff = entity.angleTo - player.angle;
            while (angleDiff < -Math.PI) angleDiff += 2 * Math.PI;
            while (angleDiff > Math.PI) angleDiff -= 2 * Math.PI;

            // Se estiver fora do FOV (com uma margem), não desenha
            if (Math.abs(angleDiff) > player.fov / 1.5) return;

            // Projeção 3D simplificada
            const projScale = (WIDTH / 2) / Math.tan(player.fov / 2); // Fator de escala de projeção
            const screenX = (WIDTH / 2) + (angleDiff * projScale / entity.distance); // Posição X na tela
            
            // Corrige a distância para evitar o efeito "olho de peixe"
            const correctedDistance = entity.distance * Math.cos(angleDiff);
            const spriteHeight = (entity.radius * 2 * HEIGHT) / correctedDistance; // Altura na tela baseada na distância
            const spriteWidth = spriteHeight * 0.8; // Proporção simples
            
            const bobOffset = Math.sin(player.bobbing) * 5 * (100 / entity.distance); // Inclui efeito de andar
            const screenY = (HEIGHT / 2) - (spriteHeight / 2) + bobOffset;

            // Desenha o "Zumbi" como uma elipse preenchida
            ctx.fillStyle = entity.color;
            ctx.beginPath();
            // x, y, radiusX, radiusY, rotation, startAngle, endAngle
            ctx.ellipse(screenX, screenY + spriteHeight/2, Math.abs(spriteWidth/2), Math.abs(spriteHeight/2), 0, 0, Math.PI * 2);
            ctx.fill();
            
            // Adiciona uma "sombra" ou detalhe simples
            ctx.fillStyle = 'rgba(0,0,0,0.5)';
            ctx.beginPath();
            ctx.ellipse(screenX, screenY + spriteHeight/2, Math.abs(spriteWidth/4), Math.abs(spriteHeight/3), 0, 0, Math.PI * 2);
            ctx.fill();
        });
        
        // Efeito visual de vinheta simples nas bordas
        const vignette = ctx.createRadialGradient(WIDTH/2, HEIGHT/2, WIDTH/4, WIDTH/2, HEIGHT/2, WIDTH/1.5);
        vignette.addColorStop(0, 'rgba(0,0,0,0)');
        vignette.addColorStop(1, 'rgba(0,0,0,0.7)');
        ctx.fillStyle = vignette;
        ctx.fillRect(0, 0, WIDTH, HEIGHT);
    }

    // Inicia o loop
    gameLoop();

    // Ajusta o canvas se a janela for redimensionada (mantendo o aspect ratio)
    window.addEventListener('resize', () => {
        // Opcional: implementar lógica para manter aspect ratio fixo
    });

</script>

</body>
</html>