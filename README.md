<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>FAST HUG - Exportar para OneNote</title>
    <style>
        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: -apple-system, system-ui, sans-serif;
            margin: 0;
            padding: 16px;
            background: #f0f0f5;
            line-height: 1.5;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
        }

        .card {
            background: white;
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 16px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        h1 {
            color: #333;
            font-size: 24px;
            margin: 0 0 20px 0;
            text-align: center;
        }

        h2 {
            color: #2c5282;
            font-size: 20px;
            margin: 20px 0 10px 0;
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
            color: #444;
        }

        input[type="text"],
        input[type="number"],
        input[type="date"],
        select,
        textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            background: white;
            margin-bottom: 8px;
        }

        .checkbox-container {
            background: #f8f9fa;
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 10px;
        }

        .checkbox-container label {
            display: flex;
            align-items: center;
            margin: 8px 0;
            font-weight: normal;
        }

        .checkbox-container input[type="checkbox"] {
            margin-right: 10px;
            width: 18px;
            height: 18px;
        }

        button {
            background: #4C51BF;
            color: white;
            border: none;
            border-radius: 8px;
            padding: 15px 25px;
            font-size: 16px;
            font-weight: 600;
            width: 100%;
            cursor: pointer;
            margin-top: 20px;
            transition: background 0.3s ease;
        }

        button:hover {
            background: #434190;
        }

        .export-status {
            text-align: center;
            margin-top: 20px;
            padding: 10px;
            border-radius: 8px;
            display: none;
        }

        .success {
            background: #C6F6D5;
            color: #2F855A;
        }

        .error {
            background: #FED7D7;
            color: #C53030;
        }

        @media (prefers-color-scheme: dark) {
            body {
                background: #1a1a1a;
                color: #fff;
            }

            .card {
                background: #2d2d2d;
            }

            h1, h2 {
                color: #fff;
            }

            input[type="text"],
            input[type="number"],
            input[type="date"],
            select,
            textarea {
                background: #333;
                border-color: #444;
                color: #fff;
            }

            .checkbox-container {
                background: #333;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="card">
            <h1>FAST HUG - UTI</h1>
            
            <form id="fastHugForm">
                <div class="form-group">
                    <label>Data da Avaliação</label>
                    <input type="date" name="data" required>
                </div>

                <div class="form-group">
                    <label>Identificação do Paciente</label>
                    <input type="text" name="paciente" placeholder="Nome do Paciente" required>
                    <input type="text" name="leito" placeholder="Número do Leito" required>
                    <input type="text" name="medico" placeholder="Médico Responsável" required>
                </div>

                <h2>F - Feeding (Alimentação)</h2>
                <div class="form-group">
                    <select name="alimentacao" required>
                        <option value="">Selecione a via de alimentação</option>
                        <option>Oral</option>
                        <option>Enteral</option>
                        <option>Parenteral</option>
                        <option>Mista</option>
                        <option>Jejum</option>
                    </select>
                    <input type="number" name="calorias" placeholder="Meta calórica (kcal/dia)">
                    <input type="number" name="proteinas" placeholder="Meta proteica (g/dia)">
                </div>

                <h2>A - Analgesia</h2>
                <div class="form-group">
                    <input type="number" name="dor" placeholder="Escala de dor (0-10)" min="0" max="10">
                    <div class="checkbox-container">
                        <label><input type="checkbox" name="analgesicos" value="Dipirona"> Dipirona</label>
                        <label><input type="checkbox" name="analgesicos" value="Tramadol"> Tramadol</label>
                        <label><input type="checkbox" name="analgesicos" value="Morfina"> Morfina</label>
                        <label><input type="checkbox" name="analgesicos" value="Fentanil"> Fentanil</label>
                    </div>
                </div>

                <h2>S - Sedação</h2>
                <div class="form-group">
                    <select name="rass" required>
                        <option value="">Escala RASS</option>
                        <option>+4 Combativo</option>
                        <option>+3 Muito agitado</option>
                        <option>+2 Agitado</option>
                        <option>+1 Inquieto</option>
                        <option>0 Alerta/calmo</option>
                        <option>-1 Sonolento</option>
                        <option>-2 Sedação leve</option>
                        <option>-3 Sedação moderada</option>
                        <option>-4 Sedação profunda</option>
                        <option>-5 Não despertável</option>
                    </select>
                    <div class="checkbox-container">
                        <label><input type="checkbox" name="sedativos" value="Midazolam"> Midazolam</label>
                        <label><input type="checkbox" name="sedativos" value="Propofol"> Propofol</label>
                        <label><input type="checkbox" name="sedativos" value="Dexmedetomidina"> Dexmedetomidina</label>
                    </div>
                </div>

                <h2>T - Tromboprofilaxia</h2>
                <div class="checkbox-container">
                    <label><input type="checkbox" name="tromboprofilaxia" value="Heparina"> Heparina não fracionada</label>
                    <label><input type="checkbox" name="tromboprofilaxia" value="Enoxaparina"> Enoxaparina</label>
                    <label><input type="checkbox" name="tromboprofilaxia" value="Meias"> Meias compressivas</label>
                    <label><input type="checkbox" name="tromboprofilaxia" value="Compressao"> Compressão pneumática</label>
                </div>

                <h2>H - Head of bed (Cabeceira)</h2>
                <div class="form-group">
                    <input type="number" name="cabeceira" min="0" max="90" placeholder="Ângulo da cabeceira (graus)">
                </div>

                <h2>U - Úlcera de estresse</h2>
                <div class="checkbox-container">
                    <label><input type="checkbox" name="ulcera" value="Omeprazol"> Omeprazol</label>
                    <label><input type="checkbox" name="ulcera" value="Ranitidina"> Ranitidina</label>
                    <label><input type="checkbox" name="ulcera" value="Pantoprazol"> Pantoprazol</label>
                </div>

                <h2>G - Glicemia</h2>
                <div class="form-group">
                    <input type="number" name="glicemia" placeholder="Valor da glicemia (mg/dL)">
                    <input type="number" name="insulina" placeholder="Doses de insulina (UI)">
                </div>

                <button type="button" onclick="exportToOneNote()">Exportar para OneNote</button>
            </form>

            <div id="exportStatus" class="export-status"></div>
        </div>
    </div>

    <script src="https://alcdn.msauth.net/browser/2.30.0/js/msal-browser.min.js"></script>
    <script>
        const msalConfig = {
            auth: {
                clientId: "SEU_CLIENT_ID_AQUI",
                authority: "https://login.microsoftonline.com/common",
                redirectUri: window.location.origin
            }
        };

        const msalInstance = new msal.PublicClientApplication(msalConfig);

        async function exportToOneNote() {
            try {
                const loginResponse = await msalInstance.loginPopup({
                    scopes: ["Notes.Create"]
                });

                const form = document.getElementById('fastHugForm');
                const formData = new FormData(form);
                
                // Formatar dados para o OneNote
                const pageContent = `
                    <html>
                    <head>
                        <title>FAST HUG - ${formData.get('data')}</title>
                    </head>
                    <body>
                        <h1>Avaliação FAST HUG</h1>
                        <h2>Dados do Paciente</h2>
                        <p>Data: ${formData.get('data')}</p>
                        <p>Paciente: ${formData.get('paciente')}</p>
                        <p>Leito: ${formData.get('leito')}</p>
                        <p>Médico: ${formData.get('medico')}</p>

                        <h2>F - Feeding</h2>
                        <p>Via de alimentação: ${formData.get('alimentacao')}</p>
                        <p>Meta calórica: ${formData.get('calorias')} kcal/dia</p>
                        <p>Meta proteica: ${formData.get('proteinas')} g/dia</p>

                        <h2>A - Analgesia</h2>
                        <p>Escala de dor: ${formData.get('dor')}</p>
                        <p>Medicações: ${Array.from(formData.getAll('analgesicos')).join(', ')}</p>

                        <h2>S - Sedação</h2>
                        <p>RASS: ${formData.get('rass')}</p>
                        <p>Sedativos: ${Array.from(formData.getAll('sedativos')).join(', ')}</p>

                        <h2>T - Tromboprofilaxia</h2>
                        <p>${Array.from(formData.getAll('tromboprofilaxia')).join(', ')}</p>

                        <h2>H - Head of bed</h2>
                        <p>Cabeceira: ${formData.get('cabeceira')}°</p>

                        <h2>U - Úlcera de estresse</h2>
                        <p>Profilaxia: ${Array.from(formData.getAll('ulcera')).join(', ')}</p>

                        <h2>G - Glicemia</h2>
                        <p>Glicemia: ${formData.get('glicemia')} mg/dL</p>
                        <p>Insulina: ${formData.get('insulina')} UI</p>
                    </body>
                    </html>
                `;

                const token = loginResponse.accessToken;
                const response = await fetch('https://graph.microsoft.com/v1.0/me/onenote/pages', {
                    method: 'POST',
                    headers: {
                        'Authorization': `Bearer ${token}`,
                        'Content-Type': 'application/xhtml+xml'
                    },
                    body: pageContent
                });

                if (response.ok) {
                    showStatus('Exportado com sucesso para o OneNote!', 'success');
                } else {
                    throw new Error('Erro ao exportar para o OneNote');
                }
            } catch (error) {
                console.error(error);
                showStatus('Erro ao exportar para o OneNote. Tente novamente.', 'error');
            }
        }

        function showStatus(message, type) {
            const statusDiv = document.getElementById('exportStatus');
            statusDiv.textContent = message;
            statusDiv.className = `export-status ${type}`;
            statusDiv.style.display = 'block';
            setTimeout(() => {
                statusDiv.style.display = 'none';
            }, 3000);
        }

        // Preencher data atual ao carregar
        window.addEventListener('load', () => {
            const hoje = new Date().toISOString().split('T')[0];
            document.querySelector('input[type="date"]').value = hoje;
        });
    </script>
</body>
</html>
Por Ricardo Soares
