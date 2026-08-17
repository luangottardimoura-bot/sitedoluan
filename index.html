<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Banco Ativo</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #0b7a45, #21b66f);
    color: #222;
}

h1 {
    text-align: center;
    color: white;
    margin: 20px 0 5px;
    font-size: 42px;
}

.subtitulo {
    text-align: center;
    color: white;
    margin-bottom: 20px;
}

#configuracao {
    background: white;
    width: min(500px, 90%);
    margin: 30px auto;
    padding: 25px;
    border-radius: 18px;
    box-shadow: 0 8px 25px #0005;
    text-align: center;
}

#configuracao input,
#configuracao select {
    width: 90%;
    padding: 12px;
    margin: 7px;
    border: 2px solid #ddd;
    border-radius: 10px;
    font-size: 16px;
}

button {
    border: none;
    padding: 12px 18px;
    border-radius: 10px;
    background: #087f45;
    color: white;
    font-weight: bold;
    cursor: pointer;
    transition: .2s;
}

button:hover {
    transform: scale(1.05);
    background: #065f34;
}

#jogo {
    display: none;
    width: 95%;
    max-width: 1300px;
    margin: auto;
}

.area-jogo {
    display: grid;
    grid-template-columns: 1fr 330px;
    gap: 20px;
}

.tabuleiro {
    background: #eee;
    border: 8px solid #174b32;
    border-radius: 15px;
    padding: 8px;
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    grid-template-rows: repeat(5, 100px);
    gap: 5px;
    min-height: 520px;
}

.casa {
    background: white;
    border: 2px solid #333;
    border-radius: 8px;
    padding: 5px;
    text-align: center;
    position: relative;
    font-size: 12px;
    overflow: hidden;
}

.casa strong {
    display: block;
    font-size: 13px;
    margin-bottom: 5px;
}

.casa .icone {
    font-size: 25px;
}

.esporte {
    background: #d9f7e7;
}

.pergunta {
    background: #dcecff;
}

.desafio {
    background: #fff0c2;
}

.perigo {
    background: #ffd6d6;
}

.bonus {
    background: #e7d8ff;
}

.inicio {
    background: #b9f5cb;
}

.chegada {
    background: #ffd86b;
}

.player {
    position: absolute;
    bottom: 4px;
    right: 4px;
    font-size: 22px;
}

.painel {
    background: white;
    border-radius: 15px;
    padding: 18px;
    box-shadow: 0 5px 20px #0004;
}

#turno {
    font-size: 20px;
    font-weight: bold;
    color: #087f45;
    margin-bottom: 10px;
}

.dado {
    font-size: 65px;
    text-align: center;
    margin: 10px;
}

.info-jogadores {
    margin-top: 15px;
}

.jogador-info {
    padding: 10px;
    border-radius: 10px;
    margin-bottom: 8px;
    color: white;
}

.controles {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-top: 15px;
}

#mensagem {
    margin-top: 15px;
    background: #f1f1f1;
    padding: 12px;
    border-radius: 10px;
    min-height: 50px;
}

#modal {
    display: none;
    position: fixed;
    inset: 0;
    background: #0009;
    justify-content: center;
    align-items: center;
    z-index: 10;
}

.modal-box {
    width: min(500px, 90%);
    background: white;
    padding: 25px;
    border-radius: 18px;
    text-align: center;
    box-shadow: 0 10px 30px #0008;
}

.modal-box h2 {
    color: #087f45;
}

.opcoes button {
    display: block;
    width: 100%;
    margin: 8px 0;
    background: #eee;
    color: #222;
}

.opcoes button:hover {
    background: #c9f3db;
}

@media(max-width: 900px) {
    .area-jogo {
        grid-template-columns: 1fr;
    }

    .tabuleiro {
        grid-template-rows: repeat(5, 75px);
    }

    .casa {
        font-size: 9px;
    }

    .casa strong {
        font-size: 10px;
    }

    .casa .icone {
        font-size: 18px;
    }
}
</style>
</head>

<body>

<h1>🏃 BANCO ATIVO 🏃</h1>
<div class="subtitulo">
    O jogo que transforma atividade física em diversão!
</div>

<div id="configuracao">
    <h2>🎮 Começar jogo</h2>

    <label>Quantidade de jogadores:</label>
    <select id="quantidade">
        <option value="2">2 jogadores</option>
        <option value="3">3 jogadores</option>
        <option value="4">4 jogadores</option>
    </select>

    <div id="nomes"></div>

    <button onclick="iniciarJogo()">COMEÇAR JOGO</button>
</div>

<div id="jogo">

    <div class="area-jogo">

        <div class="tabuleiro" id="tabuleiro"></div>

        <div class="painel">

            <div id="turno">Turno</div>

            <div class="dado" id="dado">🎲</div>

            <button onclick="jogarDado()" id="btnDado">
                🎲 JOGAR DADO
            </button>

            <div class="controles">
                <button onclick="mostrarRegras()">📖 Regras</button>
                <button onclick="novoJogo()">🔄 Novo jogo</button>
            </div>

            <div id="mensagem">
                Bem-vindo ao Banco Ativo!
            </div>

            <div class="info-jogadores" id="infoJogadores"></div>

        </div>
    </div>
</div>

<div id="modal">
    <div class="modal-box" id="modalConteudo"></div>
</div>

<script>

const casas = [
    {nome:"INÍCIO", icone:"🏁", tipo:"inicio"},
    {nome:"Quadra de Futebol", icone:"⚽", tipo:"esporte", preco:300},
    {nome:"Pergunta", icone:"❓", tipo:"pergunta"},
    {nome:"Pista de Corrida", icone:"🏃", tipo:"esporte", preco:350},
    {nome:"Desafio Físico", icone:"💪", tipo:"desafio"},
    {nome:"Academia", icone:"🏋️", tipo:"esporte", preco:400},

    {nome:"Sedentarismo", icone:"📱", tipo:"perigo"},
    {nome:"Quadra de Vôlei", icone:"🏐", tipo:"esporte", preco:300},
    {nome:"Pergunta", icone:"❓", tipo:"pergunta"},
    {nome:"Parque", icone:"🌳", tipo:"esporte", preco:250},
    {nome:"Desafio Físico", icone:"💪", tipo:"desafio"},
    {nome:"Descanso", icone:"😴", tipo:"bonus"},

    {nome:"Piscina", icone:"🏊", tipo:"esporte", preco:400},
    {nome:"Pergunta", icone:"❓", tipo:"pergunta"},
    {nome:"Pista de Ciclismo", icone:"🚴", tipo:"esporte", preco:350},
    {nome:"Sedentarismo", icone:"📱", tipo:"perigo"},
    {nome:"Basquete", icone:"🏀", tipo:"esporte", preco:300},
    {nome:"Desafio Físico", icone:"💪", tipo:"desafio"},

    {nome:"Pergunta", icone:"❓", tipo:"pergunta"},
    {nome:"Academia ao ar livre", icone:"🤸", tipo:"esporte", preco:250},
    {nome:"Bônus Saúde", icone:"❤️", tipo:"bonus"},
    {nome:"Desafio Físico", icone:"💪", tipo:"desafio"},
    {nome:"Pergunta", icone:"❓", tipo:"pergunta"},
    {nome:"CHEGADA", icone:"🏆", tipo:"chegada"}
];

let jogadores = [];
let jogadorAtual = 0;
let jogou = false;

const perguntas = [
    {
        pergunta:"Qual é um benefício da atividade física?",
        opcoes:["Melhora a saúde","Causa sedentarismo","Diminui a disposição"],
        correta:0
    },
    {
        pergunta:"O que é sedentarismo?",
        opcoes:[
            "Praticar esportes todos os dias",
            "Fazer pouca ou nenhuma atividade física",
            "Dormir cedo"
        ],
        correta:1
    },
    {
        pergunta:"Qual atividade fortalece os músculos?",
        opcoes:["Musculação","Assistir TV","Jogar videogame"],
        correta:0
    },
    {
        pergunta:"A atividade física pode ajudar a:",
        opcoes:[
            "Aumentar o estresse",
            "Melhorar o humor",
            "Diminuir a interação social"
        ],
        correta:1
    },
    {
        pergunta:"Qual destes é um esporte?",
        opcoes:["Futebol","Celular","Televisão"],
        correta:0
    },
    {
        pergunta:"O esporte em equipe ajuda a desenvolver:",
        opcoes:[
            "Respeito e cooperação",
            "Isolamento",
            "Sedentarismo"
        ],
        correta:0
    }
];

const desafios = [
    "Faça 10 polichinelos!",
    "Faça 5 agachamentos!",
    "Fique 10 segundos em uma perna!",
    "Faça 10 segundos de corrida parada!",
    "Cite 5 esportes rapidamente!",
    "Faça uma mímica de um esporte!"
];

const cores = [
    "#e74c3c",
    "#3498db",
    "#f39c12",
    "#9b59b6"
];

document.getElementById("quantidade").addEventListener("change", criarNomes);

function criarNomes() {

    const qtd = Number(document.getElementById("quantidade").value);
    const div = document.getElementById("nomes");

    div.innerHTML = "";

    for(let i=0;i<qtd;i++){

        div.innerHTML += `
            <input
                id="nome${i}"
                placeholder="Nome do jogador ${i+1}"
                value="Jogador ${i+1}">
        `;
    }
}

criarNomes();

function iniciarJogo(){

    const qtd = Number(document.getElementById("quantidade").value);

    jogadores = [];

    for(let i=0;i<qtd;i++){

        jogadores.push({
            nome:document.getElementById("nome"+i).value || "Jogador "+(i+1),
            pos:0,
            pontos:1000,
            propriedades:[]
        });
    }

    jogadorAtual = 0;

    document.getElementById("configuracao").style.display="none";
    document.getElementById("jogo").style.display="block";

    montarTabuleiro();
    atualizar();
}

function montarTabuleiro(){

    const tabuleiro = document.getElementById("tabuleiro");

    tabuleiro.innerHTML="";

    casas.forEach((casa,i)=>{

        const div = document.createElement("div");

        div.className = "casa "+casa.tipo;

        div.innerHTML = `
            <strong>${casa.nome}</strong>
            <div class="icone">${casa.icone}</div>
            ${casa.preco ? `<small>💰 ${casa.preco} PS</small>` : ""}
            <div id="players-${i}"></div>
        `;

        tabuleiro.appendChild(div);
    });

    atualizarJogadoresNoTabuleiro();
}

function atualizarJogadoresNoTabuleiro(){

    document.querySelectorAll("[id^='players-']").forEach(e=>{
        e.innerHTML="";
    });

    jogadores.forEach((j,i)=>{

        const lugar = document.getElementById("players-"+j.pos);

        if(lugar){

            const span=document.createElement("span");

            span.className="player";
            span.style.color=cores[i];

            span.innerHTML="●";

            lugar.appendChild(span);
        }
    });
}

function atualizar(){

    const j = jogadores[jogadorAtual];

    document.getElementById("turno").innerHTML =
        `🎮 Vez de: <b>${j.nome}</b>`;

    const info=document.getElementById("infoJogadores");

    info.innerHTML="";

    jogadores.forEach((j,i)=>{

        info.innerHTML += `
            <div class="jogador-info"
                 style="background:${cores[i]}">
                <b>${j.nome}</b><br>
                ❤️ ${j.pontos} Pontos de Saúde<br>
                🏟️ ${j.propriedades.length} propriedades
            </div>
        `;
    });

    atualizarJogadoresNoTabuleiro();
}

function jogarDado(){

    if(jogou) return;

    jogou=true;

    const j=jogadores[jogadorAtual];

    const numero=Math.floor(Math.random()*6)+1;

    document.getElementById("dado").innerHTML=numero;

    document.getElementById("mensagem").innerHTML =
        `${j.nome} tirou ${numero}!`;

    let novaPos=j.pos+numero;

    if(novaPos>=casas.length-1){

        novaPos=casas.length-1;

        j.pos=novaPos;

        atualizar();

        setTimeout(()=>vitoria(j),500);

        return;
    }

    if(novaPos>=casas.length){

        novaPos=casas.length-1;
    }

    j.pos=novaPos;

    atualizar();

    setTimeout(()=>acaoCasa(casas[novaPos]),400);
}

function acaoCasa(casa){

    const j=jogadores[jogadorAtual];

    if(casa.tipo==="esporte"){

        comprarPropriedade(casa);

    }else if(casa.tipo==="pergunta"){

        fazerPergunta();

    }else if(casa.tipo==="desafio"){

        fazerDesafio();

    }else if(casa.tipo==="perigo"){

        j.pontos-=100;

        if(j.pontos<0) j.pontos=0;

        document.getElementById("mensagem").innerHTML =
            `📱 ${j.nome} caiu no sedentarismo e perdeu 100 PS!`;

        finalizarTurno();

    }else if(casa.tipo==="bonus"){

        j.pontos+=200;

        document.getElementById("mensagem").innerHTML =
            `❤️ Bônus de saúde! ${j.nome} ganhou 200 PS.`;

        finalizarTurno();

    }else{

        finalizarTurno();
    }

    atualizar();
}

function comprarPropriedade(casa){

    const j=jogadores[jogadorAtual];

    if(casa.dono!==undefined){

        if(casa.dono===jogadorAtual){

            document.getElementById("mensagem").innerHTML =
                "🏟️ Você já possui este espaço!";

            finalizarTurno();
            return;
        }

        const dono=jogadores[casa.dono];

        j.pontos-=100;
        dono.pontos+=100;

        if(j.pontos<0) j.pontos=0;

        document.getElementById("mensagem").innerHTML =
            `🏟️ ${j.nome} pagou 100 PS para ${dono.nome}.`;

        finalizarTurno();
        return;
    }

    if(j.pontos<casa.preco){

        document.getElementById("mensagem").innerHTML =
            `❌ Você não tem pontos suficientes para comprar ${casa.nome}.`;

        finalizarTurno();
        return;
    }

    abrirModal(`
        <h2>🏟️ ${casa.nome}</h2>
        <p>Preço: <b>${casa.preco} Pontos de Saúde</b></p>
        <p>Você quer comprar este espaço?</p>

        <button onclick="confirmarCompra()">COMPRAR</button>
        <button onclick="fecharModal()">NÃO COMPRAR</button>
    `);

    window.propriedadeAtual=casa;
}

function confirmarCompra(){

    const casa=window.propriedadeAtual;
    const j=jogadores[jogadorAtual];

    j.pontos-=casa.preco;

    casa.dono=jogadorAtual;

    j.propriedades.push(casa.nome);

    fecharModal();

    document.getElementById("mensagem").innerHTML =
        `🏆 ${j.nome} comprou ${casa.nome}!`;

    atualizar();

    finalizarTurno();
}

function fazerPergunta(){

    const p=perguntas[Math.floor(Math.random()*perguntas.length)];

    let html=`
        <h2>❓ Pergunta</h2>
        <p><b>${p.pergunta}</b></p>
        <div class="opcoes">
    `;

    p.opcoes.forEach((op,i)=>{

        html+=`
            <button onclick="responder(${i},${p.correta})">
                ${op}
            </button>
        `;
    });

    html+=`</div>`;

    abrirModal(html);
}

function responder(escolha,correta){

    const j=jogadores[jogadorAtual];

    if(escolha===correta){

        j.pontos+=100;

        document.getElementById("mensagem").innerHTML =
            `✅ Muito bem, ${j.nome}! Você ganhou 100 PS.`;

    }else{

        document.getElementById("mensagem").innerHTML =
            `❌ Resposta incorreta! Continue aprendendo sobre atividade física.`;
    }

    fecharModal();

    atualizar();

    finalizarTurno();
}

function fazerDesafio(){

    const desafio=desafios[Math.floor(Math.random()*desafios.length)];

    abrirModal(`
        <h2>💪 DESAFIO!</h2>
        <p>${desafio}</p>
        <p>Faça o desafio e clique em concluir.</p>

        <button onclick="concluirDesafio()">
            ✅ CONCLUÍ O DESAFIO
        </button>
    `);
}

function concluirDesafio(){

    const j=jogadores[jogadorAtual];

    j.pontos+=100;

    fecharModal();

    document.getElementById("mensagem").innerHTML =
        `💪 Parabéns, ${j.nome}! Você ganhou 100 PS.`;

    atualizar();

    finalizarTurno();
}

function finalizarTurno(){

    atualizar();

    setTimeout(()=>{

        jogadorAtual++;

        if(jogadorAtual>=jogadores.length){
            jogadorAtual=0;
        }

        jogou=false;

        document.getElementById("dado").innerHTML="🎲";

        atualizar();

    },1200);
}

function vitoria(j){

    document.getElementById("btnDado").disabled=true;

    abrirModal(`
        <h2>🏆 FIM DE JOGO!</h2>

        <p>
            <b>${j.nome}</b> chegou primeiro à chegada!
        </p>

        <h3>❤️ Pontos de Saúde</h3>

        ${jogadores
            .sort((a,b)=>b.pontos-a.pontos)
            .map((p,i)=>`
                <p>${i+1}º - ${p.nome}: ${p.pontos} PS</p>
            `).join("")}

        <button onclick="novoJogo()">🔄 JOGAR NOVAMENTE</button>
    `);
}

function abrirModal(conteudo){

    document.getElementById("modalConteudo").innerHTML=conteudo;

    document.getElementById("modal").style.display="flex";
}

function fecharModal(){

    document.getElementById("modal").style.display="none";
}

function mostrarRegras(){

    abrirModal(`
        <h2>📖 REGRAS</h2>

        <p>🎲 Jogue o dado e avance pelo tabuleiro.</p>

        <p>🏟️ Compre espaços esportivos usando Pontos de Saúde.</p>

        <p>❓ Responda perguntas para ganhar pontos.</p>

        <p>💪 Faça desafios físicos para ganhar pontos.</p>

        <p>📱 Cuidado com o sedentarismo!</p>

        <p>❤️ Bônus aumentam seus Pontos de Saúde.</p>

        <p>🏆 Quem chegar primeiro à chegada termina o jogo.</p>

        <p><b>Vence quem tiver mais Pontos de Saúde!</b></p>

        <button onclick="fecharModal()">FECHAR</button>
    `);
}

function novoJogo(){

    location.reload();
}

</script>

</body>
</html>