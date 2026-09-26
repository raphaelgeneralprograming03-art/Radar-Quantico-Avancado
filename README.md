
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulação de Guerra: Radar Quântico</title>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            margin: 0;
            padding: 20px;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #030712;
            color: #f3f4f6;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
        }

        h1 {
            margin-bottom: 5px;
            font-size: 26px;
            text-align: center;
            color: #38bdf8;
            text-transform: uppercase;
            letter-spacing: 2px;
            text-shadow: 0 0 12px rgba(56, 189, 248, 0.4);
        }

        p.subtitle {
            color: #64748b;
            margin-top: 0;
            margin-bottom: 25px;
            font-size: 13px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .container {
            display: flex;
            gap: 25px;
            max-width: 1100px;
            width: 100%;
            flex-wrap: wrap;
            justify-content: center;
        }

        /* PAINEL TÁTICO MODERNO */
        .panel {
            background: linear-gradient(145deg, #0f172a, #090d16);
            border-radius: 12px;
            padding: 22px;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.6), 0 0 0 1px rgba(56, 189, 248, 0.2);
            width: 480px;
            box-sizing: border-box;
            position: relative;
        }

        .panel-title {
            font-size: 13px;
            font-weight: 800;
            text-transform: uppercase;
            letter-spacing: 0.1em;
            margin-bottom: 15px;
            color: #38bdf8;
            border-bottom: 1px solid rgba(56, 189, 248, 0.3);
            padding-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .display-box {
            background-color: #020617;
            background-image: 
                radial-gradient(rgba(56, 189, 248, 0.08) 1px, transparent 0),
                linear-gradient(to right, rgba(255, 255, 255, 0.02) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(255, 255, 255, 0.02) 1px, transparent 1px);
            background-size: 20px 20px, 20px 20px, 20px 20px;
            border-radius: 8px;
            height: 350px;
            position: relative;
            overflow: hidden;
            border: 1px solid #1e293b;
            box-shadow: inset 0 0 20px rgba(0, 0, 0, 0.8);
        }

        /* TELA DO RADAR TÁTICO */
        #radarView {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            padding: 25px;
            align-content: center;
            justify-items: center;
            box-sizing: border-box;
        }

        .controls {
            margin-top: 18px;
            display: flex;
            flex-direction: column;
            gap: 14px;
            width: 100%;
        }

        .btn-group {
            display: flex;
            gap: 12px;
        }

        /* NOVOS BOTÕES TÁTICOS CIBERPNUK */
        button {
            background: linear-gradient(180deg, #1e293b 0%, #0f172a 100%);
            color: #38bdf8;
            border: 1px solid #38bdf8;
            padding: 12px 16px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 700;
            font-size: 12px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            transition: all 0.25s ease;
            flex: 1;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }

        button:hover {
            background: #38bdf8;
            color: #020617;
            box-shadow: 0 0 18px rgba(56, 189, 248, 0.6);
            transform: translateY(-1px);
        }

        button#scan-quantum {
            color: #f43f5e;
            border-color: #f43f5e;
        }

        button#scan-quantum:hover {
            background: #f43f5e;
            color: #ffffff;
            box-shadow: 0 0 18px rgba(244, 63, 94, 0.6);
        }

        .slider-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
            font-size: 12px;
            color: #94a3b8;
            font-weight: bold;
        }

        .slider-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 12px;
        }

        input[type="range"] {
            flex: 1;
            accent-color: #38bdf8;
            cursor: pointer;
        }

        .stats {
            margin-top: 14px;
            font-size: 13px;
            color: #94a3b8;
            font-weight: bold;
            display: flex;
            justify-content: space-between;
            background: rgba(15, 23, 42, 0.6);
            padding: 10px 14px;
            border-radius: 6px;
            border: 1px solid #1e293b;
        }

        /* UNIDADES MILITARES */
        .military-unit {
            width: 44px;
            height: 44px;
            background-color: #0f172a;
            border: 1px solid #334155;
            border-radius: 6px;
            transition: all 0.25s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            font-weight: 800;
            color: #475569;
            font-family: monospace;
        }

        /* SINAL ENCONTRADO PELO RADAR CONVENCIONAL */
        .radar-hit {
            background-color: #0284c7 !important;
            border-color: #38bdf8 !important;
            color: #ffffff !important;
            box-shadow: 0 0 25px #38bdf8;
            transform: scale(1.15);
        }

        /* ANOMALIA DE ASSINATURA ESCURA / STEALTH */
        .stealth-hit {
            background-color: #e11d48 !important;
            border-color: #f43f5e !important;
            color: #ffffff !important;
            box-shadow: 0 0 25px #f43f5e;
            transform: scale(1.15);
        }

        /* GRÁFICO DE ESPECTRO EM GUERRA */
        .chart-zone-top {
            position: absolute;
            left: 50px;
            top: 30px;
            width: 330px;
            height: 130px;
            background-color: rgba(56, 189, 248, 0.08);
            border-left: 2px solid #334155;
        }

        .chart-zone-bottom {
            position: absolute;
            left: 50px;
            top: 162px;
            width: 330px;
            height: 130px;
            background-color: rgba(244, 63, 94, 0.08);
            border-left: 2px solid #334155;
            border-bottom: 2px solid #334155;
        }

        .chart-line {
            position: absolute;
            left: 50px;
            top: 160px;
            width: 330px;
            height: 2px;
            background-color: #38bdf8;
            box-shadow: 0 0 8px #38bdf8;
        }

        .chart-label {
            position: absolute;
            font-size: 11px;
            font-weight: bold;
            font-family: monospace;
        }

        .dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            position: absolute;
            border: 2px solid #ffffff;
            transform: translate(-50%, -50%);
            animation: pop 0.3s ease-out;
        }

        @keyframes pop {
            0% { transform: translate(-50%, -50%) scale(0); }
            100% { transform: translate(-50%, -50%) scale(1); }
        }
    </style>
</head>
<body>

    <h1>Simulador de Campo de Batalha: Radar Quântico</h1>
    <p class="subtitle">Estratégias de Reconhecimento Eletrônico contra Camuflagem de Absorção Total</p>

    <div class="container">
        <!-- Painel Esquerdo: Varredura de Campo -->
        <div class="panel">
            <div class="panel-title">📡 Varredura Tática de Frequência</div>
            <div id="radarView" class="display-box">
                <!-- Setores militares gerados dinamicamente -->
            </div>
            <div class="controls">
                <div class="btn-group">
                    <button id="scan-normal">Varredura de Radar</button>
                    <button id="scan-quantum">Pulso Quântico (Anti-Ocultação)</button>
                </div>
                <div class="slider-group">
                    <div class="slider-row">
                        <label>Potência do Pulso:</label>
                        <input type="range" id="power-slider" min="10" max="200" value="100">
                        <span id="power-val" style="color:#38bdf8;">100 MW</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- Painel Direito: Análise de Assinatura -->
        <div class="panel">
            <div class="panel-title">📊 Análise de Espectro: Ruído vs Assinatura Térmica</div>
            <div id="chartView" class="display-box">
                <div class="chart-zone-top"></div>
                <div class="chart-zone-bottom"></div>
                <div class="chart-line"></div>
                
                <div class="chart-label" style="left: 200px; top: 40px; color: #38bdf8;">Alvos Convencionais (RF)</div>
                <div class="chart-label" style="left: 170px; top: 250px; color: #f43f5e;">Assinaturas Ocultas (Invisíveis)</div>
                <div class="chart-label" style="left: 140px; top: 315px; color: #64748b;">Frequência de Retorno de Onda &rarr;</div>
            </div>
            <div class="stats">
                <span>Alvos Revelados: <strong id="count-normal" style="color:#38bdf8">0</strong></span>
                <span>Ocultações Rompidas: <strong id="count-stealth" style="color:#f43f5e">0</strong></span>
            </div>
        </div>
    </div>

    <script>
        const radarView = document.getElementById('radarView');
        const chartView = document.getElementById('chartView');
        const powerSlider = document.getElementById('power-slider');
        const powerVal = document.getElementById('power-val');

        let counters = { normal: 0, stealth: 0 };
        let currentPower = 100;

        powerSlider.addEventListener('input', function(e) {
            currentPower = parseInt(e.target.value);
            powerVal.innerText = currentPower + " MW";
        });

        // Cria a matriz de setores do mapa militar tático (12 setores)
        const totalSectors = 12;
        for (let i = 0; i < totalSectors; i++) {
            let unit = document.createElement('div');
            unit.className = 'military-unit';
            unit.innerText = "SET-" + (i + 1);
            radarView.appendChild(unit);
        }

        const unitList = document.querySelectorAll('.military-unit');

        // Ações de Varredura Tática
        document.getElementById('scan-normal').addEventListener('click', function() {
            triggerRadar('normal');
        });

        document.getElementById('scan-quantum').addEventListener('click', function() {
            triggerRadar('stealth');
        });

        function triggerRadar(mode) {
            let randomIndex = Math.floor(Math.random() * unitList.length);
            let selectedSector = unitList[randomIndex];

            let dot = document.createElement('div');
            dot.className = 'dot';

            let posX, posY;

            if (mode === 'normal') {
                // Animação visual no setor detectado
                selectedSector.classList.add('radar-hit');
                setTimeout(() => selectedSector.classList.remove('radar-hit'), 400);

                // Posição no gráfico (Zona Superior - Alvos Convencionais)
                posY = Math.floor(Math.random() * 95) + 45;
                posX = Math.floor(Math.random() * 280) + 70;

                dot.style.backgroundColor = '#38bdf8';
                dot.style.borderColor = '#0284c7';
                dot.style.boxShadow = '0 0 10px #38bdf8';

                counters.normal++;
                document.getElementById('count-normal').innerText = counters.normal;
            } else if (mode === 'stealth') {
                // Animação visual no setor com unidade oculta
                selectedSector.classList.add('stealth-hit');
                setTimeout(() => selectedSector.classList.remove('stealth-hit'), 400);

                // Posição no gráfico (Zona Inferior - Assinaturas Ocultas)
                posY = Math.floor(Math.random() * 90) + 175;

                // Fator de deslocamento baseado na Potência do Pulso (MW)
                let powerRatio = currentPower / 200;
                let minX = 60 + (powerRatio * 30);
                let rangeX = 160 + (powerRatio * 120);
                posX = Math.floor(Math.random() * rangeX) + minX;
                if (posX > 360) posX = 360;

                dot.style.backgroundColor = '#f43f5e';
                dot.style.borderColor = '#e11d48';
                dot.style.boxShadow = '0 0 10px #f43f5e';

                counters.stealth++;
                document.getElementById('count-stealth').innerText = counters.stealth;
            }

            dot.style.left = posX + 'px';
            dot.style.top = posY + 'px';

            chartView.appendChild(dot);
        }
    </script>
</body>
</html>
