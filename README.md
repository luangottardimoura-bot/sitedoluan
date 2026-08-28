<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Tudo Sobre Futebol</title>

    <style>

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        body {
            background: #eef2f0;
            color: #222;
        }

        /* TOPO */

        header {
            background: linear-gradient(135deg, #006b3c, #003d24);
            color: white;
            padding: 22px;
            text-align: center;
        }

        header h1 {
            font-size: 32px;
        }

        header p {
            margin-top: 8px;
            opacity: .9;
        }

        /* PESQUISA */

        .pesquisa {
            background: white;
            padding: 30px 20px;
            text-align: center;
        }

        .pesquisa h2 {
            color: #006b3c;
            margin-bottom: 18px;
        }

        .barra {
            max-width: 650px;
            margin: auto;
            display: flex;
        }

        .barra input {
            flex: 1;
            padding: 16px;
            border: 2px solid #006b3c;
            border-radius: 10px 0 0 10px;
            font-size: 16px;
            outline: none;
        }

        .barra button {
            padding: 16px 25px;
            border: none;
            background: #006b3c;
            color: white;
            font-size: 16px;
            font-weight: bold;
            border-radius: 0 10px 10px 0;
            cursor: pointer;
        }

        .barra button:hover {
            background: #004d2b;
        }

        /* CONTEÚDO */

        main {
            max-width: 1100px;
            margin: 35px auto;
            padding: 0 20px;
        }

        #carregando {
            text-align: center;
            color: #006b3c;
            font-weight: bold;
            display: none;
        }

        #erro {
            text-align: center;
            color: #c62828;
            font-weight: bold;
            margin: 20px;
        }

        /* PERFIL */

        .perfil {
            background: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 5px 20px rgba(0,0,0,.08);
            margin-bottom: 35px;
        }

        .perfil img {
            width: 110px;
            height: 110px;
            object-fit: contain;
            margin-bottom: 15px;
        }

        .perfil h2 {
            color: #006b3c;
            font-size: 30px;
        }

        .pais {
            color: #777;
            margin-top: 8px;
        }

        /* SEÇÕES */

        .titulo {
            color: #006b3c;
            margin-bottom: 18px;
            border-left: 5px solid #ffd700;
            padding-left: 10px;
        }

        .lista {
            display: grid;
            gap: 15px;
            margin-bottom: 40px;
        }

        /* JOGO */

        .jogo {
            background: white;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 15px rgba(0,0,0,.07);
        }

        .campeonato {
            color: #777;
            font-size: 14px;
            margin-bottom: 12px;
        }

        .data {
            color: #555;
            font-size: 14px;
            margin-bottom: 18px;
        }

        .times {
            display: grid;
            grid-template-columns: 1fr auto 1fr;
            align-items: center;
            text-align: center;
            gap: 15px;
        }

        .time {
            font-weight: bold;
        }

        .placar {
            font-size: 25px;
            font-weight: bold;
            color: #006b3c;
        }

        .proximo {
            color: #1565c0;
        }

        /* RODAPÉ */

        footer {
            background: #003d24;
            color: white;
            text-align: center;
            padding: 25px;
            margin-top: 50px;
        }

        /* CELULAR */

        @media(max-width: 600px) {

            header h1 {
                font-size: 25px;
            }

            .barra {
                flex-direction: column;
                gap: 10px;
            }

            .barra input {
                border-radius: 10px;
            }

            .barra button {
                border-radius: 10px;
            }

            .times {
                grid-template-columns: 1fr;
            }

            .placar {
                order: 2;
            }

        }

    </style>

</head>

<body>


<header>

    <h1>⚽ Tudo Sobre Futebol</h1>

    <p>
        Seu time, seus jogos e seus resultados.
    </p>

</header>


<section class="pesquisa">

    <h2>🔎 Pesquise seu time</h2>

    <div class="barra">

        <input
            id="campoTime"
            type="text"
            placeholder="Ex: Flamengo"
        >

        <button onclick="pesquisarTime()">
            Pesquisar
        </button>

    </div>

</section>


<main>

    <div id="carregando">
        🔄 Procurando informações...
    </div>

    <div id="erro"></div>

    <div id="conteudo"></div>

</main>


<footer>

    <p>
        ⚽ Tudo Sobre Futebol © 2026
    </p>

</footer>


<script>

    /*
       COLOQUE SUA CHAVE DA API AQUI
    */

    const API_KEY = "COLOQUE_SUA_CHAVE_AQUI";


    const API =
        "https://live-football-api.com/api/v1";


    async function pesquisarTime() {

        const nome =
            document
            .getElementById("campoTime")
            .value
            .trim();


        const erro =
            document.getElementById("erro");

        const carregando =
            document.getElementById("carregando");

        const conteudo =
            document.getElementById("conteudo");


        erro.innerHTML = "";
        conteudo.innerHTML = "";


        if (!nome) {

            erro.innerHTML =
                "Digite o nome de