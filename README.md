"Por favor, digite o nome de um time.";
            return;
        }

        carregando.style.display = "block";

        try {
            // 1. Busca o time pelo nome digitado
            const resTime = await fetch(`${API}/search/teams?query=${encodeURIComponent(nome)}&api_key=${API_KEY}`);
            const dataTime = await resTime.json();

            if (!dataTime.data || dataTime.data.length === 0) {
                carregando.style.display = "none";
                erro.innerHTML = "Time não encontrado. Tente novamente.";
                return;
            }

            const time = dataTime.data[0];

            // 2. Busca jogos anteriores e próximos jogos em paralelo
            const [resPassados, resFuturos] = await Promise.all([
                fetch(`${API}/teams/${time.id}/matches?type=past&api_key=${API_KEY}`),
                fetch(`${API}/teams/${time.id}/matches?type=future&api_key=${API_KEY}`)
            ]);

            const dataPassados = await resPassados.json();
            const dataFuturos = await resFuturos.json();

            carregando.style.display = "none";

            // 3. Monta a seção do perfil do time
            let html = `
                <div class="perfil">
                    ${time.logo ? `<img src="${time.logo}" alt="${time.name}">` : ""}
                    <h2>${time.name}</h2>
                    <p class="pais">📍 ${time.country || "País não especificado"}</p>
                </div>
            `;

            // 4. Seção de Jogos Anteriores
            html += `<h3 class="titulo">Últimos Jogos</h3><div class="lista">`;
            if (dataPassados.data && dataPassados.data.length > 0) {
                dataPassados.data.slice(0, 5).forEach(jogo => {
                    html += criarCardJogo(jogo, false);
                });
            } else {
                html += `<p>Nenhum jogo recente encontrado.</p>`;
            }
            html += `</div>`;

            // 5. Seção de Próximos Jogos
            html += `<h3 class="titulo">Próximos Jogos</h3><div class="lista">`;
            if (dataFuturos.data && dataFuturos.data.length > 0) {
                dataFuturos.data.slice(0, 5).forEach(jogo => {
                    html += criarCardJogo(jogo, true);
                });
            } else {
                html += `<p>Nenhum jogo futuro agendado.</p>`;
            }
            html += `</div>`;

            conteudo.innerHTML = html;

        } catch (e) {
            carregando.style.display = "none";
            erro.innerHTML = "Erro ao carregar dados. Verifique a API ou tente novamente.";
            console.error(e);
        }
    }

    function criarCardJogo(jogo, ehFuturo) {
        const dataFormatada = new Date(jogo.date).toLocaleDateString("pt-BR", {
            day: "2-digit",
            month: "2-digit",
            year: "numeric",
            hour: "2-digit",
            minute: "2-digit"
        });

        const placar = ehFuturo 
            ? `<span class="proximo">VS</span>` 
            : `${jogo.home_score ?? 0} - ${jogo.away_score ?? 0}`;

        return `
            <div class="jogo">
                <div class="campeonato">🏆 ${jogo.league_name || "Campeonato"}</div>
                <div class="data">📅 ${dataFormatada}</div>
                <div class="times">
                    <div class="time">${jogo.home_team_name}</div>
                    <div class="placar">${placar}</div>
                    <div class="time">${jogo.away_team_name}</div>
                </div>
            </div>
        `;
    }

    // Atalho para pesquisar ao pressionar Enter
    document.getElementById("campoTime").addEventListener("keypress", function(e) {
        if (e.key === "Enter") {
            pesquisarTime();
        }
    });
</script>
</body>
</html>