<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Missão Movimento</title>

<style>

*{
    box-sizing:border-box;
}

body{
    margin:0;
    font-family:Arial,sans-serif;
    background:#101827;
    color:white;
}

header{
    text-align:center;
    padding:22px;
    background:linear-gradient(135deg,#00c853,#00a8ff);
}

header h1{
    margin:0;
    font-size:42px;
}

header p{
    margin:8px 0 0;
}

#inicio{
    max-width:550px;
    margin:45px auto;
    background:#182235;
    padding:30px;
    border-radius:20px;
    box-shadow:0 10px 30px #0008;
    text-align:center;
}

input,select{
    width:90%;
    padding:13px;
    margin:7px;
    border:0;
    border-radius:10px;
    font-size:16px;
}

button{
    border:0;
    border-radius:10px;
    padding:12px 18px;
    background:#00c853;
    color:white;
    font-weight:bold;
    cursor:pointer;
    margin:5px;
}

button:hover{
    background:#00a844;
    transform:translateY(-2px);
}

#game{
    display:none;
    max-width:1250px;
    margin:20px auto;
    padding:10px;
}

.layout{
    display:grid;
    grid-template-columns:1fr 330px;
    gap:20px;
}

.board{
    display:grid;
    grid-template-columns:repeat(6,1fr);
    gap:7px;
    background:#0b111d;
    padding:10px;
    border-radius:20px;
}

.cell{
    min-height:105px;
    border-radius:14px;
    padding:8px;
    border:2px solid #29364c;
    background:#1c2738;
    text-align:center;
    position:relative;
}

.cell strong{
    display:block;
    font-size:12px;
}

.cell .symbol{
    font-size:30px;
    margin:7px;
}

.cell small{
    color:#b9c7d9;
}

.start{
    background:#064d35;
}

.challenge{
    background:#513d00;
}

.question{
    background:#073f5f;
}

.event{
    background:#45206b;
}

.rest{
    background:#343b49;
}

.finish{
    background:#725300;
}

.players{
    position:absolute;
    bottom:5px;
    left:0;
    right:0;
}

.token{
    display:inline-block;
    font-size:20px;
    margin:1px;
}

.panel{
    background:#182235;
    padding:20px;
    border-radius:20px;
    box-shadow:0 8px 25px #0006;
}

.turn{
    color:#00e676;
    font-size:20px;
    margin-bottom:15px;
}

.stats{
    background:#111927;
    padding:12px;
    border-radius:12px;
    margin:8px 0;
}

.stat{
    display:flex;
    justify-content:space-between;
    margin:5px 0;
}

.dice{
    text-align:center;
    font-size:65px;
    margin:15px;
}

.message{
    margin-top:15px;
    padding:15px;
    background:#111927;
    border-radius:12px;
    min-height:70px;
}

#modal{
    display:none;
    position:fixed;
    inset:0;
    background:#000b;
    justify-content:center;
    align-items:center;
    z-index:99;
}

.modal{
    background:#182235;
    max-width:520px;
    width:90%;
    padding:25px;
    border-radius:20px;
    text-align:center;
}

.answer{
    display:block;
    width:100%;
    background:#26364e;
}

.answer:hover{
    background:#00a8ff;
}

.ability{
    background:#233149;
    padding:10px;
    border-radius:10px;
    margin-top:10px;
}

@media(max-width:850px){

    .layout{
        grid-template-columns:1fr;
    }

    .board{
        grid-template-columns:repeat(4,1fr);
    }

}

@media(max-width:500px){

    header h1{
        font-size:30px;
    }

    .board{
        grid-template-columns:repeat(3,1fr);
    }

    .cell{
        min-height:90px;
    }

}

</style>
</head>

<body>

<header>

<h1>⚡ MISSÃO MOVIMENTO</h1>

<p>
Uma aventura para descobrir como cuidar melhor do corpo e da mente!
</p>

</header>


<section id="inicio">

<h2>🎮 Preparar missão</h2>

<p>Escolha de 2 a 4 participantes.</p>

<select id="qtd">

<option value="2">2 jogadores</option>
<option value="3">3 jogadores</option>
<option value="4">4 jogadores</option>

</select>

<div id="names"></div>

<button onclick="startGame()">
🚀 INICIAR MISSÃO
</button>

</section>


<section id="game">

<div class="layout">

<div class="board" id="board"></div>


<div class="panel">

<div class="turn" id="turn"></div>

<div class="dice" id="dice">🎲</div>

<button id="roll" onclick="rollDice()">
🎲 AVANÇAR
</button>

<button onclick="rules()">
📘 COMO JOGAR
</button>

<div id="message" class="message">
Prepare-se para começar!
</div>

<div id="playersInfo"></div>

</div>

</div>

</section>


<div id="modal">

<div class="modal" id="modalContent"></div>

</div>


<script>

const spaces=[

["BASE DE ENERGIA","🏠","start"],
["AQUECIMENTO","🔥","challenge"],
["QUIZ DO CORPO","🧠","question"],
["CAMINHO DA CORRIDA","🏃","challenge"],
["EVENTO SAUDÁVEL","✨","event"],
["ZONA DO ESPORTE","⚽","challenge"],

["PAUSA DIGITAL","📵","rest"],
["DESAFIO DE FORÇA","💪","challenge"],
["QUIZ DA MENTE","🧠","question"],
["TRILHA","🌳","challenge"],
["EVENTO SURPRESA","🎁","event"],
["HIDRATAÇÃO","💧","rest"],

["DESAFIO CARDIO","❤️","challenge"],
["QUIZ DA SAÚDE","🩺","question"],
["MOVIMENTO LIVRE","🤸","challenge"],
["EVENTO POSITIVO","⭐","event"],
["TEMPO DE DESCANSO","😴","rest"],
["DESAFIO EM EQUIPE","🤝","challenge"],

["QUIZ FINAL","🏆","question"],
["CAMINHO DA DANÇA","💃","challenge"],
["EVENTO ESPECIAL","🌟","event"],
["DESAFIO RELÂMPAGO","⚡","challenge"],
["ÚLTIMA ETAPA","🚀","challenge"],
["META FINAL","🏁","finish"]

];


const questions=[

[
"Qual é um benefício da atividade física?",
["Melhora a saúde","Aumenta o sedentarismo","Diminui a disposição"],
0
],

[
"Qual destes é um exemplo de atividade física?",
["Assistir televisão","Caminhar","Ficar sentado"],
1
],

[
"O sedentarismo pode aumentar o risco de:",
["Problemas de saúde","Mais energia","Melhor condicionamento"],
0
],

[
"A atividade física pode contribuir para:",
["Melhorar o humor","Aumentar o estresse","Diminuir a socialização"],
0
],

[
"Qual atitude ajuda a combater o sedentarismo?",
["Passar mais tempo sentado","Praticar exercícios","Evitar qualquer movimento"],
1
],

[
"Esportes em grupo podem desenvolver:",
["Cooperação","Isolamento","Desinteresse"],
0
],

[
"Qual destes ajuda na saúde dos ossos?",
["Atividade física","Ficar o dia inteiro sentado","Evitar movimentos"],
0
]

];


const challenges=[

"Faça 10 polichinelos.",
"Faça 8 agachamentos.",
"Corra parado durante 15 segundos.",
"Fique 10 segundos equilibrado em uma perna.",
"Faça uma mímica de um esporte.",
"Cite 5 atividades físicas.",
"Faça 5 movimentos de alongamento.",
"Crie um grito de guerra para sua equipe."

];


const abilities=[

{
name:"⚡ Energia Extra",
text:"Uma vez por partida, avance 2 casas extras."
},

{
name:"🧠 Mente Rápida",
text:"Uma vez por partida, pode tentar novamente uma pergunta errada."
},

{
name:"❤️ Vida Saudável",
text:"Recebe 2 pontos de saúde quando completa um desafio."
},

{
name:"🏃 Atleta",
text:"Uma vez por partida, pode ignorar uma casa de descanso."
}

];


let players=[];
let current=0;
let usedAbility=false;
let moved=false;


document.getElementById("qtd").addEventListener("change",makeNames);

makeNames();


function makeNames(){

    let qtd=Number(document.getElementById("qtd").value);

    let area=document.getElementById("names");

    area.innerHTML="";

    for(let i=0;i<qtd;i++){

        area.innerHTML+=`

        <input
        id="name${i}"
        value="Jogador ${i+1}"
        placeholder="Nome do jogador">

        `;

    }

}


function startGame(){

    let qtd=Number(document.getElementById("qtd").value);

    players=[];

    for(let i=0;i<qtd;i++){

        players.push({

            name:
            document.getElementById("name"+i).value
            ||"Jogador "+(i+1),

            position:0,

            energy:10,

            health:5,

            knowledge:0,

            ability:
            abilities[i],

            finished:false

        });

    }

    current=0;

    document.getElementById("inicio").style.display="none";

    document.getElementById("game").style.display="block";

    createBoard();

    update();

}


function createBoard(){

    let board=document.getElementById("board");

    board.innerHTML="";

    spaces.forEach((s,i)=>{

        let div=document.createElement("div");

        div.className="cell "+s[2];

        div.innerHTML=`

        <strong>${s[0]}</strong>

        <div class="symbol">${s[1]}</div>

        <div class="players" id="p${i}"></div>

        `;

        board.appendChild(div);

    });

}


function update(){

    let p=players[current];

    document.getElementById("turn").innerHTML=
    `🎯 Vez de <b>${p.name}</b>`;

    let info=document.getElementById("playersInfo");

    info.innerHTML="";

    players.forEach((p,i)=>{

        info.innerHTML+=`

        <div class="stats">

        <b>${p.name}</b>

        <div class="stat">
        ⚡ Energia <span>${p.energy}</span>
        </div>

        <div class="stat">
        ❤️ Saúde <span>${p.health}</span>
        </div>

        <div class="stat">
        🧠 Conhecimento <span>${p.knowledge}</span>
        </div>

        <div class="ability">
        ${p.ability.name}<br>
        <small>${p.ability.text}</small>
        </div>

        </div>

        `;

    });

    drawPlayers();

}


function drawPlayers(){

    document.querySelectorAll(".players")
    .forEach(x=>x.innerHTML="");

    players.forEach((p,i)=>{

        let area=document.getElementById("p"+p.position);

        let token=document.createElement("span");

        token.className="token";

        token.innerHTML=["🔴","🔵","🟡","🟣"][i];

        area.appendChild(token);

    });

}


function rollDice(){

    if(moved)return;

    moved=true;

    let p=players[current];

    let roll=Math.floor(Math.random()*6)+1;

    document.getElementById("dice").innerText=roll;

    p.position+=roll;

    if(p.position>=spaces.length-1){

        p.position=spaces.length-1;

        update();

        setTimeout(finish,500);

        return;

    }

    p.energy--;

    if(p.energy<0)p.energy=0;

    update();

    setTimeout(resolveSpace,500);

}


function resolveSpace(){

    let p=players[current];

    let type=spaces[p.position][2];

    if(type==="challenge"){

        challenge();

    }

    else if(type==="question"){

        question();

    }

    else if(type==="event"){

        event();

    }

    else if(type==="rest"){

        rest();

    }

    else{

        nextTurn();

    }

}


function challenge(){

    let text=
    challenges[
    Math.floor(Math.random()*challenges.length)
    ];

    openModal(`

    <h2>💪 DESAFIO</h2>

    <p>${text}</p>

    <p>Complete a atividade para ganhar energia!</p>

    <button onclick="completeChallenge()">
    ✅ COMPLETEI
    </button>

    `);

}


function completeChallenge(){

    let p=players[current];

    p.energy+=2;

    p.health+=1;

    closeModal();

    message(
    `💪 Excelente! ${p.name} completou o desafio e ganhou energia e saúde.`
    );

    update();

    nextTurn();

}


function question(){

    let q=
    questions[
    Math.floor(Math.random()*questions.length)
    ];

    let html=`

    <h2>🧠 QUIZ</h2>

    <p><b>${q[0]}</b></p>

    `;

    q[1].forEach((answer,i)=>{

        html+=`

        <button class="answer"
        onclick="answer(${i},${q[3]})">

        ${answer}

        </button>

        `;

    });

    openModal(html);

}


function answer(choice,correct){

    let p=players[current];

    if(choice===correct){

        p.knowledge+=2;

        p.health+=1;

        message(
        "🧠 Resposta correta! Você ganhou conhecimento e saúde."
        );

    }else{

        p.knowledge+=0;

        message(
        "❌ Não foi dessa vez. Continue aprendendo!"
        );

    }

    closeModal();

    update();

    nextTurn();

}


function event(){

    let p=players[current];

    let events=[

    ["🌟 Você ajudou um colega a praticar esporte!","health",2],

    ["💧 Você lembrou de se hidratar!","energy",2],

    ["📵 Você ficou menos tempo no celular hoje!","health",1],

    ["🎵 Você descobriu uma atividade que gosta!","energy",3],

    ["😴 Você descansou adequadamente!","health",1]

    ];

    let e=events[Math.floor(Math.random()*events.length)];

    p[e[1]]+=e[2];

    message(`${e[0]} +${e[2]} ponto(s)!`);

    update();

    nextTurn();

}


function rest(){

    let p=players[current];

    p.energy+=3;

    message(
    `😴 Momento de descanso! ${p.name} recuperou 3 pontos de energia.`
    );

    update();

    nextTurn();

}


function nextTurn(){

    setTimeout(()=>{

        current++;

        if(current>=players.length)
        current=0;

        moved=false;

        document.getElementById("dice").innerText="🎲";

        update();

    },1200);

}


function finish(){

    let ranking=[...players];

    ranking.sort((a,b)=>{

        let scoreA=
        a.health*3+
        a.energy+
        a.knowledge*2;

        let scoreB=
        b.health*3+
        b.energy+
        b.knowledge*2;

        return scoreB-scoreA;

    });

    let html=`

    <h2>🏆 MISSÃO CONCLUÍDA!</h2>

    <p>Todos chegaram ao final da jornada.</p>

    `;

    ranking.forEach((p,i)=>{

        let score=
        p.health*3+
        p.energy+
        p.knowledge*2;

        html+=`

        <p>
        ${i+1}º <b>${p.name}</b>
        — ${score} pontos
        </p>

        `;

    });

    html+=`

    <button onclick="location.reload()">
    🔄 NOVA MISSÃO
    </button>

    `;

    openModal(html);

    document.getElementById("roll").disabled=true;

}


function message(text){

    document.getElementById("message").innerHTML=text;

}


function openModal(html){

    document.getElementById("modalContent").innerHTML=html;

    document.getElementById("modal").style.display="flex";

}


function closeModal(){

    document.getElementById("modal").style.display="none";

}


function rules(){

    openModal(`

    <h2>📘 COMO JOGAR</h2>

    <p>🎲 Jogue o dado e avance pelo caminho.</p>

    <p>⚡ A energia diminui quando você avança.</p>

    <p>💪 Os desafios recuperam energia.</p>

    <p>🧠 As perguntas aumentam seu conhecimento.</p>

    <p>❤️ Eventos e desafios ajudam sua saúde.</p>

    <p>🏁 Ao chegar ao final, os pontos são calculados.</p>

    <p>
    <b>Saúde × 3 + Energia + Conhecimento × 2
    = pontuação final.</b>
    </p>

    <p>
    Portanto, chegar primeiro não garante a vitória!
    </p>

    <button onclick="closeModal()">FECHAR</button>

    `);

}

</script>

</body>
</html>