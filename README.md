<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FAST HUG - Sistema Integrado</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background: #f5f5f5;
        }
        .card {
            background: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        input, select, textarea {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        .checkbox-group {
            margin: 8px 0;
        }
        .checkbox-group input[type="radio"],
        .checkbox-group input[type="checkbox"] {
            width: auto;
            margin-right: 8px;
        }
        .checkbox-group label {
            display: inline;
            font-weight: normal;
        }
        .botao {
            background: #4C51BF;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
        }
        .tabela-avaliacoes {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        .tabela-avaliacoes th, 
        .tabela-avaliacoes td {
            padding: 8px;
            border: 1px solid #ddd;
            text-align: left;
        }
        .status {
            padding: 10px;
            margin: 10px 0;
            border-radius: 4px;
        }
        .status.erro {
            background: #FEE2E2;
            color: #991B1B;
        }
        .status.sucesso {
            background: #DCFCE7;
            color: #166534;
        }
    </style>
</head>
<body>
    <div class="card">
        <h1>FAST HUG - Avaliação</h1>
        
        <form id="fastHugForm">
            <div class="form-group">
                <label>Data da Avaliação:</label>
                <input type="date" id="data" required>
            </div>

            <div class="form-group">
                <label>Nome do Paciente:</label>
                <input type="text" id="paciente" required>
            </div>

            <div class="form-group">
                <label>Leito:</label>
                <input type="text" id="leito" required>
            </div>

            <div class="form-group">
                <label>Intubado(a)?</label>
                <div class="checkbox-group">
                    <input type="radio" id="intubado_sim" name="intubado" value="sim" onchange="toggleIntubacao()">
                    <label for="intubado_sim">Sim</label>
                    <input type="radio" id="intubado_nao" name="intubado" value="nao" onchange="toggleIntubacao()">
                    <label for="intubado_nao">Não</label>
                </div>
            </div>

            <div id="intubacao_info" style="display:none">
                <div class="form-group">
                    <label>Data da Intubação:</label>
                    <input type="date" id="data_intubacao">
                    <div id="tempo_intubacao"></div>
                </div>
            </div>

            <div class="form-group">
                <label>Uso de Cateter:</label>
                <div class="checkbox-group">
                    <input type="radio" id="cateter_sim" name="cateter" value="sim" onchange="toggleCateter()">
                    <label for="cateter_sim">Sim</label>
                    <input type="radio" id="cateter_nao" name="cateter" value="nao" onchange="toggleCateter()">
                    <label for="cateter_nao">Não</label>
                </div>
            </div>

            <div id="cateter_info" style="display:none">
                <div class="form-group">
                    <label>Data de Inserção do Cateter:</label>
                    <input type="date" id="data_cateter">
                    <div id="tempo_cateter"></div>
                </div>
                <div class="form-group">
                    <label>Pode ser retirado?</label>
                    <div class="checkbox-group">
                        <input type="radio" id="retirar_sim" name="retirar_cateter" value="sim">
                        <label for="retirar_sim">Sim</label>
                        <input type="radio" id="retirar_nao" name="retirar_cateter" value="nao">
                        <label for="retirar_nao">Não</label>
                    </div>
                </div>
            </div>

            <h2>FAST HUG</h2>

            <div class="form-group">
                <label>F - Alimentação:</label>
                <select id="alimentacao" required onchange="toggleJejum()">
                    <option value="">Selecione...</option>
                    <option value="jejum">Jejum</option>
                    <option value="via_oral">Via Oral</option>
                    <option value="sng">Sonda Nasogástrica</option>
                    <option value="sne">Sonda Nasoenteral</option>
                    <option value="gastrostomia">Gastrostomia</option>
                    <option value="jejunostomia">Jejunostomia</option>
                    <option value="npo">NPO</option>
                </select>
            </div>

            <div id="jejum_info" style="display:none">
                <div class="form-group">
                    <label>Última Alimentação:</label>
                    <input type="datetime-local" id="ultima_alimentacao">
                    <div id="tempo_jejum"></div>
                </div>
            </div>

            <div class="form-group">
                <label>A - Analgesia (0-10):</label>
                <input type="number" id="dor" min="0" max="10" required>
            </div>

            <div class="form-group">
                <label>S - Sedação (RASS):</label>
                <select id="rass" required>
                    <option value="">Selecione...</option>
                    <option value="+4">+4 Combativo</option>
                    <option value="+3">+3 Muito Agitado</option>
                    <option value="+2">+2 Agitado</option>
                    <option value="+1">+1 Inquieto</option>
                    <option value="0">0 Alerta/Calmo</option>
                    <option value="-1">-1 Sonolento</option>
                    <option value="-2">-2 Sedação Leve</option>
                    <option value="-3">-3 Sedação Moderada</option>
                    <option value="-4">-4 Sedação Profunda</option>
                    <option value="-5">-5 Não Despertável</option>
                </select>
            </div>

            <div class="form-group">
                <label>T - Tromboprofilaxia:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="heparina">
                    <label for="heparina">Heparina</label>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="hbpm">
                    <label for="hbpm">HBPM</label>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="anticoagulante">
                    <label for="anticoagulante">Anticoagulante Oral</label>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="meias">
                    <label for="meias">Meias Compressivas</label>
                </div>
            </div>

            <div class="form-group">
                <label>H - Cabeceira (graus):</label>
                <input type="number" id="cabeceira" min="0" max="90" required>
            </div>

            <div class="form-group">
                <label>U - Profilaxia Úlcera:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="omeprazol">
                    <label for="omeprazol">Omeprazol</label>
                    <input type="checkbox" id="ranitidina">
                    <label for="ranitidina">Ranitidina</label>
                </div>
            </div>

            <div class="form-group">
                <label>G - Glicemia (mg/dL):</label>
                <input type="number" id="glicemia" required>
            </div>

            <button type="submit" class="botao">Salvar Avaliação</button>
        </form>
    </div>

    <div id="avaliacoes" class="card">
        <h2>Avaliações Realizadas</h2>
        <table class="tabela-avaliacoes">
            <thead>
                <tr>
                    <th>Data</th>
                    <th>Paciente</th>
                    <th>Leito</th>
                    <th>Ações</th>
                </tr>
            </thead>
            <tbody id="tabela-body"></tbody>
        </table>
        <button onclick="exportarCSV()" class="botao" style="margin-top: 10px">Exportar CSV</button>
    </div>

    <script>
        let avaliacoes = [];

        function toggleIntubacao() {
            const div = document.getElementById('intubacao_info');
            div.style.display = document.getElementById('intubado_sim').checked ? 'block' : 'none';
        }

        function toggleCateter() {
            const div = document.getElementById('cateter_info');
            div.style.display = document.getElementById('cateter_sim').checked ? 'block' : 'none';
        }

        function toggleJejum() {
            const div = document.getElementById('jejum_info');
            div.style.display = document.getElementById('alimentacao').value === 'jejum' ? 'block' : 'none';
        }

        function calcularTempo(dataInicio, elemento) {
            if (!dataInicio) return;
            const inicio = new Date(dataInicio);
            const agora = new Date();
            const diff = Math.floor((agora - inicio) / (1000 * 60 * 60 * 24));
            elemento.textContent = `Tempo: ${diff} dia(s)`;
        }

        document.getElementById('data_intubacao').addEventListener('change', function() {
            calcularTempo(this.value, document.getElementById('tempo_intubacao'));
        });

        document.getElementById('data_cateter').addEventListener('change', function() {
            calcularTempo(this.value, document.getElementById('tempo_cateter'));
        });

        document.getElementById('ultima_alimentacao').addEventListener('change', function() {
            calcularTempo(this.value, document.getElementById('tempo_jejum'));
        });

        document.getElementById('fastHugForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const avaliacao = {
                data: document.getElementById('data').value,
                paciente: document.getElementById('paciente').value,
                leito: document.getElementById('leito').value,
                intubado: document.getElementById('intubado_sim').checked,
                data_intubacao: document.getElementById('data_intubacao').value,
                cateter: document.getElementById('cateter_sim').checked,
                data_cateter: document.getElementById('data_cateter').value,
                retirar_cateter: document.querySelector('input[name="retirar_cateter"]:checked')?.value,
                alimentacao: document.getElementById('alimentacao').value,
                ultima_alimentacao: document.getElementById('ultima_alimentacao').value,
                dor: document.getElementById('dor').value,
                rass: document.getElementById('rass').value,
                tromboprofilaxia: {
                    heparina: document.getElementById('heparina').checked,
                    hbpm: document.getElementById('hbpm').checked,
                    anticoagulante: document.getElementById('anticoagulante').checked,
                    meias: document.getElementById('meias').checked
                },
                cabeceira: document.getElementById('cabeceira').value,
                ulcera: {
                    omeprazol: document.getElementById('omeprazol').checked,
                    ranitidina: document.getElementById('ranitidina').checked
                },
                glicemia: document.getElementById('glicemia').value
            };

            avaliacoes.push(avaliacao);
            atualizarTabela();
            this.reset();

            const status = document.createElement('div');
            status.className = 'status sucesso';
            status.textContent = 'Avaliação salva com sucesso!';
            this.appendChild(status);
            setTimeout(() => status.remove(), 3000);
        });

        function atualizarTabela() {
            const tbody = document.getElementById('tabela-body');
            tbody.innerHTML = '';
            
            avaliacoes.forEach((av, index) => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${av.data}</td>
                    <td>${av.paciente}</td>
                    <td>${av.leito}</td>
                    <td>
                        <button onclick="verAvaliacao(${index})" class="botao">Ver</button>
                        <button onclick="excluirAvaliacao(${index})" class="botao">Excluir</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function verAvaliacao(index) {
            const av = avaliacoes[index];
            alert(JSON.stringify(av, null, 2));
        }

        function excluirAvaliacao(index) {
            if (confirm('Confirma exclusão?')) {
                avaliacoes.splice(index, 1);
                atualizarTabela();
            }
        }

        function exportarCSV() {
            const csv = [
                ['Data', 'Paciente', 'Leito', 'Intubado', 'Data Intubação', 'Cateter', 
                 'Data Cateter', 'Retirar Cateter', 'Alimentação', 'Última Alimentação',
                 'Dor', 'RASS', 'Cabeceira', 'Glicemia'].join(',')
            ];

            avaliacoes.forEach(av => {
                csv.push([
                    av.data,
                    av.paciente,
                    av.leito,
                    av.intubado,
                    av.data_intubacao,
                    av.cateter,
                    av.data_cateter,
                    av.retirar_cateter,
                    av.alimentacao,
                    av.ultima_alimentacao,
                    av.dor,
                    av.rass,
                    av.cabeceira,
                    av.glicemia
                ].join(','));
            });

            const blob = new Blob([csv.join('\n')], { type: 'text/csv' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'avaliacoes.csv';
            a.click();
        }
    </script>
</body>
</html>
