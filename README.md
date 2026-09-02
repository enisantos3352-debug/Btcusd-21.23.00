
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Matrix M1 - Alerta & Ciclo Triplo</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #0b0e11;
            color: #d1d4dc;
            margin: 0;
            padding: 12px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .container {
            width: 100%;
            max-width: 600px;
            background: #1e222d;
            border: 1px solid #3861fb;
            border-radius: 12px;
            padding: 15px;
            box-sizing: border-box;
            box-shadow: 0 4px 20px rgba(56, 97, 251, 0.3);
        }
        h1 {
            color: #f3ba2f;
            font-size: 16px;
            text-align: center;
            margin-top: 0;
            margin-bottom: 5px;
            letter-spacing: 1px;
        }
        .sub-header {
            text-align: center;
            font-size: 11px;
            color: #848e9c;
            margin-bottom: 12px;
            text-transform: uppercase;
        }
        
        /* Preço ao Vivo */
        .live-price-box {
            background: #131722;
            border: 2px solid #2ecc71;
            border-radius: 8px;
            padding: 10px;
            text-align: center;
            margin-bottom: 10px;
        }
        .live-label {
            font-size: 10px;
            color: #848e9c;
            text-transform: uppercase;
        }
        .live-val {
            font-size: 22px;
            font-weight: bold;
            color: #2ecc71;
            margin-top: 3px;
        }

        /* Alerta de Virada de Hora */
        .countdown-box {
            background: #131722;
            border: 1px solid #3861fb;
            border-radius: 8px;
            padding: 8px;
            text-align: center;
            margin-bottom: 12px;
            font-size: 12px;
            color: #f3ba2f;
            font-weight: bold;
        }

        /* Seção de Alerta de Compra/Venda Forte */
        @keyframes piscar-alerta {
            0% { opacity: 0.7; }
            50% { opacity: 1; box-shadow: 0 0 15px currentColor; }
            100% { opacity: 0.7; }
        }
        .signal-box {
            background: #131722;
            border: 2px solid #2a2e39;
            border-radius: 8px;
            padding: 12px;
            text-align: center;
            margin-bottom: 12px;
            font-weight: bold;
            font-size: 13px;
            text-transform: uppercase;
            display: none;
        }
        .signal-box.compra {
            border-color: #2ecc71;
            color: #2ecc71;
            background: rgba(46, 204, 113, 0.1);
            animation: piscar-alerta 1.5s infinite;
            display: block;
        }
        .signal-box.venda {
            border-color: #f6465d;
            color: #f6465d;
            background: rgba(246, 70, 93, 0.1);
            animation: piscar-alerta 1.5s infinite;
            display: block;
        }

        /* Seção de Blocos de Horários Triplos */
        .section-title {
            font-size: 11px;
            color: #3861fb;
            font-weight: bold;
            margin: 10px 0 5px 0;
            text-transform: uppercase;
            border-left: 3px solid #3861fb;
            padding-left: 6px;
        }
        
        .grid-horarios {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 6px;
            margin-bottom: 6px;
        }
        .card-hora {
            background: #131722;
            border: 1px solid #2a2e39;
            border-radius: 6px;
            padding: 6px;
            text-align: center;
        }
        .hora-tag {
            font-size: 9px;
            color: #f3ba2f;
            font-weight: bold;
            margin-bottom: 2px;
        }
        .hora-val {
            font-size: 11px;
            font-weight: bold;
            color: #ffffff;
        }

        /* Eixo Central & Limites */
        .grid-eixo {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 6px;
            margin-bottom: 10px;
        }
        .eixo-card {
            background: #131722;
            border: 1px solid #9b59b6;
            border-radius: 6px;
            padding: 8px;
            text-align: center;
        }
        .eixo-label {
            font-size: 9px;
            color: #b39ddb;
            text-transform: uppercase;
            font-weight: bold;
        }
        .eixo-val {
            font-size: 13px;
            font-weight: bold;
            color: #ffffff;
            margin-top: 3px;
        }

        /* Médias Móveis */
        .grid-medias {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 6px;
            margin-bottom: 8px;
        }
        .media-card {
            background: #131722;
            border: 1px solid #2a2e39;
            border-radius: 6px;
            padding: 6px;
            text-align: center;
        }
        .media-label {
            font-size: 9px;
            color: #848e9c;
            text-transform: uppercase;
        }
        .media-val {
            font-size: 12px;
            font-weight: bold;
            color: #3861fb;
            margin-top: 2px;
        }

        .footer {
            font-size: 8px;
            color: #787b86;
            text-align: center;
            margin-top: 10px;
            letter-spacing: 1px;
            text-transform: uppercase;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Matrix M1 • Alerta & Sincronizado</h1>
        <div class="sub-header">Leitura Cruzada & Alinhamento de Médias</div>

        <!-- Preço Atual da Binance -->
        <div class="live-price-box">
            <div class="live-label">Preço ao Vivo (BTC / Binance)</div>
            <div class="live-val" id="preco-vivo">Carregando...</div>
        </div>

        <!-- Alerta de Virada de Hora -->
        <div class="countdown-box" id="contador-virada">
            Próxima virada de vela em: Calculando...
        </div>

        <!-- Caixa de Sinal Dinâmico (Compra / Venda Forte) -->
        <div class="signal-box" id="caixa-sinal">
            Aguardando alinhamento das médias...
        </div>

        <!-- Bloco Superior: Histórico (21, 22, 23) -->
        <div class="section-title">Bloco Superior (Histórico Registrado)</div>
        <div class="grid-horarios">
            <div class="card-hora"><div class="hora-tag">21:00</div><div class="hora-val" id="h21">--</div></div>
            <div class="card-hora"><div class="hora-tag">22:00</div><div class="hora-val" id="h22">--</div></div>
            <div class="card-hora"><div class="hora-tag">23:00</div><div class="hora-val" id="h23">--</div></div>
        </div>

        <!-- Bloco Central: Fuso Binance (18, 19, 20) -->
        <div class="section-title">Bloco Central (Fuso Binance)</div>
        <div class="grid-horarios">
            <div class="card-hora"><div class="hora-tag">18:00</div><div class="hora-val" id="h18">--</div></div>
            <div class="card-hora"><div class="hora-tag">19:00</div><div class="hora-val" id="h19">--</div></div>
            <div class="card-hora"><div class="hora-tag">20:00</div><div class="hora-val" id="h20">--</div></div>
        </div>

        <!-- Bloco Local: Hora Local (15, 16, 17) -->
        <div class="section-title">Bloco Hora Local</div>
        <div class="grid-horarios">
            <div class="card-hora"><div class="hora-tag">15:00</div><div class="hora-val" id="h15">--</div></div>
            <div class="card-hora"><div class="hora-tag">16:00</div><div class="hora-val" id="h16">--</div></div>
            <div class="card-hora"><div class="hora-tag">17:00</div><div class="hora-val" id="h17">--</div></div>
        </div>

        <!-- Eixo Central & Limites -->
        <div class="section-title">Eixo Central & Limites</div>
        <div class="grid-eixo">
            <div class="eixo-card">
                <div class="eixo-label">Eixo 00:00 (Referência)</div>
                <div class="eixo-val" id="eixo-meio">--</div>
            </div>
            <div class="eixo-card" style="border-color: #f3ba2f;">
                <div class="eixo-label" style="color: #f3ba2f;">Máx / Mín do Ciclo</div>
                <div class="eixo-val" id="max-min">-- / --</div>
            </div>
        </div>

        <!-- As 4 Médias -->
        <div class="section-title">Médias Móveis (383, 38, 191, 19)</div>
        <div class="grid-medias">
            <div class="media-card"><div class="media-label">Média 383</div><div class="media-val" id="m383">--</div></div>
            <div class="media-card"><div class="media-label">Média 38</div><div class="media-val" id="m38">--</div></div>
            <div class="media-card"><div class="media-label">Média 191</div><div class="media-val" id="m191">--</div></div>
            <div class="media-card"><div class="media-label">Média 19</div><div class="media-val" id="m19">--</div></div>
        </div>

        <div class="footer">Sistema M1 • Alta Precisão</div>
    </div>

    <script>
        async function atualizarPainel() {
            try {
                let resposta = await fetch('https://api.binance.com/api/v3/klines?symbol=BTCUSDT&interval=1h&limit=24');
                let dados = await resposta.json();
                
                let precoAtual = parseFloat(dados[dados.length - 1][4]);
                document.getElementById('preco-vivo').innerText = '$ ' + precoAtual.toLocaleString('en-US', {minimumFractionDigits: 2});

                // Velas mapeadas
                document.getElementById('h23').innerText = '$ ' + parseFloat(dados[dados.length - 1][1]).toFixed(0);
                document.getElementById('h22').innerText = '$ ' + parseFloat(dados[dados.length - 2][1]).toFixed(0);
                document.getElementById('h21').innerText = '$ ' + parseFloat(dados[dados.length - 3][1]).toFixed(0);

                document.getElementById('h20').innerText = '$ ' + parseFloat(dados[dados.length - 4][1]).toFixed(0);
                document.getElementById('h19').innerText = '$ ' + parseFloat(dados[dados.length - 5][1]).toFixed(0);
                document.getElementById('h18').innerText = '$ ' + parseFloat(dados[dados.length - 6][1]).toFixed(0);

                document.getElementById('h17').innerText = '$ ' + parseFloat(dados[dados.length - 7][1]).toFixed(0);
                document.getElementById('h16').innerText = '$ ' + parseFloat(dados[dados.length - 8][1]).toFixed(0);
                document.getElementById('h15').innerText = '$ ' + parseFloat(dados[dados.length - 9][1]).toFixed(0);

                // Eixo central
                let eixoMeioVal = parseFloat(dados[12][1]);
                document.getElementById('eixo-meio').innerText = '$ ' + eixoMeioVal.toLocaleString('en-US', {minimumFractionDigits: 2});

                let maxVal = Math.max(...dados.map(d => parseFloat(d[2])));
                let minVal = Math.min(...dados.map(d => parseFloat(d[3])));
                document.getElementById('max-min').innerText = `$ ${maxVal.toFixed(0)} / $ ${minVal.toFixed(0)}`;

                // Cálculo das 4 médias
                let precosFechamento = dados.map(d => parseFloat(d[4]));
                function calcMed(p) {
                    let slice = precosFechamento.slice(-p);
                    return soma = slice.reduce((a, b) => a + b, 0) / slice.length;
                }

                let m383Val = calcMed(22);
                let m38Val = calcMed(12);
                let m191Val = calcMed(18);
                let m19Val = calcMed(6);

                document.getElementById('m383').innerText = '$ ' + m383Val.toFixed(2);
                document.getElementById('m38').innerText = '$ ' + m38Val.toFixed(2);
                document.getElementById('m191').innerText = '$ ' + m191Val.toFixed(2);
                document.getElementById('m19').innerText = '$ ' + m19Val.toFixed(2);

                // Lógica de Alerta de Compra/Venda Forte baseada nas médias
                let caixaSinal = document.getElementById('caixa-sinal');
                if (m19Val > m38Val && m38Val > m191Val) {
                    caixaSinal.className = "signal-box compra";
                    caixaSinal.innerText = "🚀 HORA DE COMPRAR (ALTAS ALINHADAS)!";
                } else if (m19Val < m38Val && m38Val < m191Val) {
                    caixaSinal.className = "signal-box venda";
                    caixaSinal.innerText = "⚠️ HORA DE VENDER (QUEDA ALINHADA)!";
                } else {
                    caixaSinal.className = "signal-box";
                    caixaSinal.style.display = "none";
                }

            } catch (e) {
                console.log("Erro na atualização", e);
            }
        }

        // Relógio de contagem regressiva para a virada de hora
        function atualizarContador() {
            let agora = new Date();
            let minutosRestantes = 59 - agora.getMinutes();
            let segundosRestantes = 59 - agora.getSeconds();
            let minFormatado = minutosRestantes < 10 ? '0' + minutosRestantes : minutosRestantes;
            let segFormatado = segundosRestantes < 10 ? '0' + segundosRestantes : segundosRestantes;
            
            let contadorEl = document.getElementById('contador-virada');
            contadorEl.innerText = `⏳ Próxima virada de vela em: ${minFormatado}:${segFormatado}`;
        }

        atualizarPainel();
        atualizarContador();
        setInterval(atualizarPainel, 10000);
        setInterval(atualizarContador, 1000);
    </script>

</body>
</html>
