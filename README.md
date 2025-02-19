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

            <div class="form-group">
                <label>Uso de Antibióticos:</label>
                <div class="checkbox-group">
                    <input type="radio" id="antibiotico_sim" name="antibiotico" value="sim" onchange="toggleAntibiotico()">
                    <label for="antibiotico_sim">Sim</label>
                    <input type="radio" id="antibiotico_nao" name="antibiotico" value="nao" onchange="toggleAntibiotico()">
                    <label for="antibiotico_nao">Não</label>
                </div>
            </div>

            <div id="antibiotico_info" style="display:none">
                <div class="form-group">
                    <label>Data de Início do Antibiótico:</label>
                    <input type="date" id="data_inicio_antibiotico" onchange="calcularTempoAntibiotico()">
                    <div id="tempo_antibiotico"></div>
                </div>
                
                <div class="form-group">
                    <label>Previsão de Término:</label>
                    <input type="date" id="data_fim_antibiotico">
                </div>
                
                <div class="form-group">
                    <label>Tipo de Antibiótico:</label>
                    <select id="tipo_antibiotico" multiple>
                        <option value="amoxicilina">Amoxicilina</option>
                        <option value="azitromicina">Azitromicina</option>
                        <option value="cefalexina">Cefalexina</option>
                        <option value="ceftriaxona">Ceftriaxona</option>
                        <option value="ciprofloxacino">Ciprofloxacino</option>
                        <option value="clindamicina">Clindamicina</option>
                        <option value="meropenem">Meropenem</option>
                        <option value="metronidazol">Metronidazol</option>
                        <option value="outro">Outro</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label>Motivo do Uso:</label>
                    <textarea id="motivo_antibiotico" rows="2"></textarea>
                </div>
            </div>

            <div class="form-group">
                <label>Sonda Vesical de Demora:</label>
                <div class="checkbox-group">
                    <input type="radio" id="svd_sim" name="svd" value="sim" onchange="toggleSVD()">
                    <label for="svd_sim">Sim</label>
                    <input type="radio" id="svd_nao" name="svd" value="nao" onchange="toggleSVD()">
                    <label for="svd_nao">Não</label>
                </div>
            </div>

            <div id="svd_info" style="display:none">
                <div class="form-group">
                    <label>Data de Inserção da SVD:</label>
                    <input type="date" id="data_svd" onchange="calcularTempoSVD()">
                    <div id="tempo_svd"></div>
                </div>
                
                <div class="form-group">
                    <label>Indicação para Manutenção:</label>
                    <select id="indicacao_svd">
                        <option value="">Selecione...</option>
                        <option value="controle_diurese">Controle Rigoroso de Diurese</option>
                        <option value="retencao">Retenção Urinária</option>
                        <option value="bexigaNeurogenica">Bexiga Neurogênica</option>
                        <option value="cirurgia">Pós-operatório</option>
                        <option value="lesao">Lesão/Úlcera Sacral</option>
                        <option value="outro">Outro</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label>Pode ser removida?</label>
                    <div class="checkbox-group">
                        <input type="radio" id="remover_svd_sim" name="remover_svd" value="sim">
                        <label for="remover_svd_sim">Sim</label>
                        <input type="radio" id="remover_svd_nao" name="remover_svd" value="nao">
                        <label for="remover_svd_nao">Não</label>
                    </div>
                </div>
            </div>

            <h2>FAST HUG</h2>
<div class="form-group">
                <label>F - Feeding (Alimentação):</label>
                <select id="feeding" required>
                    <option value="">Selecione...</option>
                    <option value="oral">Via Oral</option>
                    <option value="sng">SNG</option>
                    <option value="sne">SNE</option>
                    <option value="gastrostomia">Gastrostomia</option>
                    <option value="jejunostomia">Jejunostomia</option>
                    <option value="npo">NPO</option>
                </select>
            </div>

            <div class="form-group">
                <label>A - Analgesia:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="analgesia_adequada">
                    <label for="analgesia_adequada">Analgesia Adequada</label>
                </div>
                <textarea id="analgesia_obs" placeholder="Observações sobre analgesia"></textarea>
            </div>

            <div class="form-group">
                <label>S - Sedação:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="sedacao_adequada">
                    <label for="sedacao_adequada">Sedação Adequada</label>
                </div>
                <select id="rass">
                    <option value="">Selecione RASS...</option>
                    <option value="+4">+4 Combativo</option>
                    <option value="+3">+3 Muito Agitado</option>
                    <option value="+2">+2 Agitado</option>
                    <option value="+1">+1 Inquieto</option>
                    <option value="0">0 Alerta e Calmo</option>
                    <option value="-1">-1 Sonolento</option>
                    <option value="-2">-2 Sedação Leve</option>
                    <option value="-3">-3 Sedação Moderada</option>
                    <option value="-4">-4 Sedação Profunda</option>
                    <option value="-5">-5 Não Desperta</option>
                </select>
            </div>

            <div class="form-group">
                <label>T - Tromboprofilaxia:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="tromboprofilaxia">
                    <label for="tromboprofilaxia">Profilaxia Adequada</label>
                </div>
                <textarea id="tromboprofilaxia_obs" placeholder="Observações sobre tromboprofilaxia"></textarea>
            </div>

            <div class="form-group">
                <label>H - Head Elevation (Cabeceira Elevada):</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="cabeceira">
                    <label for="cabeceira">Cabeceira 30-45°</label>
                </div>
            </div>

            <div class="form-group">
                <label>U - Úlcera por Pressão:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="upp_prevencao">
                    <label for="upp_prevencao">Medidas de Prevenção</label>
                </div>
                <textarea id="upp_obs" placeholder="Localização e estágio das UPPs, se presentes"></textarea>
            </div>

            <div class="form-group">
                <label>G - Glicemia:</label>
                <input type="number" id="glicemia" placeholder="Valor da glicemia">
                <div class="checkbox-group">
                    <input type="checkbox" id="glicemia_controlada">
                    <label for="glicemia_controlada">Controle Adequado</label>
                </div>
            </div>

            <button type="submit" class="botao">Salvar Avaliação</button>
            <button type="button" class="botao" onclick="exportarCSV()">Exportar CSV</button>
        </form>
    </div>

    <div id="status"></div>
    <div id="avaliacoes"></div>

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

        function toggleAntibiotico() {
            const div = document.getElementById('antibiotico_info');
            div.style.display = document.getElementById('antibiotico_sim').checked ? 'block' : 'none';
        }

        function toggleSVD() {
            const div = document.getElementById('svd_info');
            div.style.display = document.getElementById('svd_sim').checked ? 'block' : 'none';
        }

        function calcularTempo(dataInicio, elementoDestino) {
            if (!dataInicio) return;
            
            const inicio = new Date(dataInicio);
            const hoje = new Date();
            const diff = Math.floor((hoje - inicio) / (1000 * 60 * 60 * 24));
            
            elementoDestino.textContent = `Tempo: ${diff} dias`;
        }

        function calcularTempoIntubacao() {
            const dataInicio = document.getElementById('data_intubacao').value;
            calcularTempo(dataInicio, document.getElementById('tempo_intubacao'));
        }

        function calcularTempoCateter() {
            const dataInicio = document.getElementById('data_cateter').value;
            calcularTempo(dataInicio, document.getElementById('tempo_cateter'));
        }

        function calcularTempoAntibiotico() {
            const dataInicio = document.getElementById('data_inicio_antibiotico').value;
            calcularTempo(dataInicio, document.getElementById('tempo_antibiotico'));
        }

        function calcularTempoSVD() {
            const dataInicio = document.getElementById('data_svd').value;
            calcularTempo(dataInicio, document.getElementById('tempo_svd'));
        }

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
                
                antibiotico: document.getElementById('antibiotico_sim').checked,
                data_inicio_antibiotico: document.getElementById('data_inicio_antibiotico').value,
                data_fim_antibiotico: document.getElementById('data_fim_antibiotico').value,
                tipo_antibiotico: Array.from(document.getElementById('tipo_antibiotico').selectedOptions).map(opt => opt.value),
                motivo_antibiotico: document.getElementById('motivo_antibiotico').value,
                
                svd: document.getElementById('svd_sim').checked,
                data_svd: document.getElementById('data_svd').value,
                indicacao_svd: document.getElementById('indicacao_svd').value,
                remover_svd: document.querySelector('input[name="remover_svd"]:checked')?.value,
                
                feeding: document.getElementById('feeding').value,
                analgesia_adequada: document.getElementById('analgesia_adequada').checked,
                analgesia_obs: document.getElementById('analgesia_obs').value,
                sedacao_adequada: document.getElementById('sedacao_adequada').checked,
                rass: document.getElementById('rass').value,
                tromboprofilaxia: document.getElementById('tromboprofilaxia').checked,
                tromboprofilaxia_obs: document.getElementById('tromboprofilaxia_obs').value,
                cabeceira: document.getElementById('cabeceira').checked,
                upp_prevencao: document.getElementById('upp_prevencao').checked,
                upp_obs: document.getElementById('upp_obs').value,
                glicemia: document.getElementById('glicemia').value,
                glicemia_controlada: document.getElementById('glicemia_controlada').checked
            };
            
            avaliacoes.push(avaliacao);
            atualizarTabela();
            mostrarStatus('Avaliação salva com sucesso!', 'sucesso');
            this.reset();
        });

        function mostrarStatus(mensagem, tipo) {
            const status = document.getElementById('status');
            status.textContent = mensagem;
            status.className = `status ${tipo}`;
            setTimeout(() => status.textContent = '', 3000);
        }

        function atualizarTabela() {
            const div = document.getElementById('avaliacoes');
            if (avaliacoes.length === 0) {
                div.innerHTML = '';
                return;
            }

            let html = '<table class="tabela-avaliacoes">';
            html += '<tr><th>Data</th><th>Paciente</th><th>Leito</th><th>Ações</th></tr>';
            
            avaliacoes.forEach((av, index) => {
                html += `
                    <tr>
                        <td>${av.data}</td>
                        <td>${av.paciente}</td>
                        <td>${av.leito}</td>
                        <td>
                            <button onclick="removerAvaliacao(${index})" class="botao">Remover</button>
                        </td>
                    </tr>
                `;
            });
            
            html += '</table>';
            div.innerHTML = html;
        }

        function removerAvaliacao(index) {
            avaliacoes.splice(index, 1);
            atualizarTabela();
            mostrarStatus('Avaliação removida com sucesso!', 'sucesso');
        }

        function exportarCSV() {
            if (avaliacoes.length === 0) {
                mostrarStatus('Não há avaliações para exportar', 'erro');
                return;
            }

            const csv = [
                'Data,Paciente,Leito,Intubado,Data Intubação,' +
                'Cateter,Data Cateter,Retirar Cateter,' +
                'Antibiótico,Data Início ATB,Data Fim ATB,Tipo ATB,Motivo ATB,' +
                'SVD,Data SVD,Indicação SVD,Remover SVD,' +
                'Feeding,Analgesia,Obs Analgesia,Sedação,RASS,' +
                'Tromboprofilaxia,Obs Tromboprofilaxia,Cabeceira,' +
                'UPP Prevenção,UPP Obs,Glicemia,Glicemia Controlada'
            ];

            avaliacoes.forEach(av => {
                csv.push(`${av.data},${av.paciente},${av.leito},${av.intubado},${av.data_intubacao},` +
                    `${av.cateter},${av.data_cateter},${av.retirar_cateter},` +
                    `${av.antibiotico},${av.data_inicio_antibiotico},${av.data_fim_antibiotico},${av.tipo_
