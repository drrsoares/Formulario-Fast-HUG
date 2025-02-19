<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FAST HUG - Sistema Integrado de Avaliação</title>

    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
            line-height: 1.6;
        }

        .card {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .form-group {
            margin-bottom: 15px;
            padding: 10px 0;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #333333;
        }

        input, 
        select, 
        textarea {
            width: 100%;
            padding: 8px;
            border: 1px solid #dddddd;
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 14px;
        }

        textarea {
            resize: vertical;
            min-height: 60px;
        }

        .checkbox-group {
            margin: 8px 0;
            padding: 5px 0;
        }

        .checkbox-group input[type="radio"],
        .checkbox-group input[type="checkbox"] {
            width: auto;
            margin-right: 8px;
            cursor: pointer;
        }

        .checkbox-group label {
            display: inline;
            font-weight: normal;
            cursor: pointer;
        }

        .botao {
            background-color: #4C51BF;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
            margin-right: 10px;
            transition: background-color 0.3s ease;
        }

        .botao:hover {
            background-color: #434190;
        }

        .tabela-avaliacoes {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            background-color: white;
        }

        .tabela-avaliacoes th, 
        .tabela-avaliacoes td {
            padding: 12px;
            border: 1px solid #dddddd;
            text-align: left;
        }

        .tabela-avaliacoes th {
            background-color: #f8f8f8;
            font-weight: bold;
        }

        .status {
            padding: 15px;
            margin: 15px 0;
            border-radius: 4px;
            font-weight: bold;
        }

        .status.erro {
            background-color: #FEE2E2;
            color: #991B1B;
            border: 1px solid #991B1B;
        }

        .status.sucesso {
            background-color: #DCFCE7;
            color: #166534;
            border: 1px solid #166534;
        }

        select[multiple] {
            height: auto;
            min-height: 100px;
        }

        h1, h2 {
            color: #1a202c;
            margin-bottom: 20px;
        }

        h1 {
            font-size: 24px;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 10px;
        }

        h2 {
            font-size: 20px;
            margin-top: 30px;
        }
    </style>
</head>
<body>
    <div class="card">
        <h1>FAST HUG - Avaliação Diária</h1>
        
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
                <label>Uso de Cateter Venoso Central:</label>
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
                    <label>Tipo de Antibiótico (múltipla escolha):</label>
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
                    <textarea id="motivo_antibiotico" rows="2" placeholder="Descreva o motivo do uso do antibiótico"></textarea>
                </div>
            </div>

            <div class="form-group">
                <label>Sonda Vesical de Demora (SVD):</label>
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
                        <option value="">Selecione a indicação...</option>
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

            <h2>Checklist FAST HUG</h2>
<div class="form-group">
                <label>F - Feeding (Alimentação):</label>
                <select id="feeding" required>
                    <option value="">Selecione o tipo de alimentação...</option>
                    <option value="oral">Via Oral</option>
                    <option value="sng">Sonda Nasogástrica (SNG)</option>
                    <option value="sne">Sonda Nasoenteral (SNE)</option>
                    <option value="gastrostomia">Gastrostomia</option>
                    <option value="jejunostomia">Jejunostomia</option>
                    <option value="npo">Nada Por Via Oral (NPO)</option>
                </select>
            </div>

            <div class="form-group">
                <label>A - Analgesia:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="analgesia_adequada">
                    <label for="analgesia_adequada">Analgesia Adequada</label>
                </div>
                <textarea 
                    id="analgesia_obs" 
                    placeholder="Observações sobre analgesia: medicações em uso, escala de dor, etc."
                    rows="3">
                </textarea>
            </div>

            <div class="form-group">
                <label>S - Sedação:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="sedacao_adequada">
                    <label for="sedacao_adequada">Sedação Adequada</label>
                </div>
                <select id="rass">
                    <option value="">Selecione o nível RASS...</option>
                    <option value="+4">+4 Combativo, violento</option>
                    <option value="+3">+3 Muito agitado, agressivo</option>
                    <option value="+2">+2 Agitado, não cooperativo</option>
                    <option value="+1">+1 Inquieto, ansioso</option>
                    <option value="0">0 Alerta e calmo</option>
                    <option value="-1">-1 Sonolento, desperta ao chamado</option>
                    <option value="-2">-2 Sedação leve</option>
                    <option value="-3">-3 Sedação moderada</option>
                    <option value="-4">-4 Sedação profunda</option>
                    <option value="-5">-5 Não desperta</option>
                </select>
            </div>

            <div class="form-group">
                <label>T - Tromboprofilaxia:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="tromboprofilaxia">
                    <label for="tromboprofilaxia">Profilaxia Adequada</label>
                </div>
                <textarea 
                    id="tromboprofilaxia_obs" 
                    placeholder="Observações sobre tromboprofilaxia: método utilizado, contraindicações, etc."
                    rows="3">
                </textarea>
            </div>

            <div class="form-group">
                <label>H - Head Elevation (Cabeceira Elevada):</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="cabeceira">
                    <label for="cabeceira">Cabeceira elevada entre 30-45 graus</label>
                </div>
            </div>

            <div class="form-group">
                <label>U - Úlcera por Pressão:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="upp_prevencao">
                    <label for="upp_prevencao">Medidas de Prevenção Implementadas</label>
                </div>
                <textarea 
                    id="upp_obs" 
                    placeholder="Localização e estágio das úlceras por pressão, se presentes. Medidas preventivas em uso."
                    rows="3">
                </textarea>
            </div>

            <div class="form-group">
                <label>G - Glicemia:</label>
                <input 
                    type="number" 
                    id="glicemia" 
                    placeholder="Valor da glicemia em mg/dL"
                    min="0" 
                    max="999">
                <div class="checkbox-group">
                    <input type="checkbox" id="glicemia_controlada">
                    <label for="glicemia_controlada">Controle Glicêmico Adequado</label>
                </div>
            </div>

            <div class="form-group" style="margin-top: 30px;">
                <button type="submit" class="botao">Salvar Avaliação</button>
                <button type="button" class="botao" onclick="exportarCSV()">Exportar para CSV</button>
            </div>
        </form>
    </div>

    <div id="status"></div>
    <div id="avaliacoes"></div>

    <script>
        // Array para armazenar todas as avaliações
        let avaliacoes = [];

        // Funções para mostrar/ocultar campos condicionais
        function toggleIntubacao() {
            const divIntubacao = document.getElementById('intubacao_info');
            divIntubacao.style.display = document.getElementById('intubado_sim').checked ? 'block' : 'none';
        }

        function toggleCateter() {
            const divCateter = document.getElementById('cateter_info');
            divCateter.style.display = document.getElementById('cateter_sim').checked ? 'block' : 'none';
        }

        function toggleAntibiotico() {
            const divAntibiotico = document.getElementById('antibiotico_info');
            divAntibiotico.style.display = document.getElementById('antibiotico_sim').checked ? 'block' : 'none';
        }

        function toggleSVD() {
            const divSVD = document.getElementById('svd_info');
            divSVD.style.display = document.getElementById('svd_sim').checked ? 'block' : 'none';
        }

        // Função para calcular o tempo de uso de dispositivos
        function calcularTempo(dataInicio, elementoDestino) {
            if (!dataInicio) return;
            
            const dataInicioObj = new Date(dataInicio);
            const dataAtual = new Date();
            const diferencaDias = Math.floor((dataAtual - dataInicioObj) / (1000 * 60 * 60 * 24));
            
            elementoDestino.textContent = `Tempo de uso: ${diferencaDias} dias`;
        }

        // Funções específicas para cálculo de tempo de cada dispositivo
        function calcularTempoIntubacao() {
            const dataIntubacao = document.getElementById('data_intubacao').value;
            calcularTempo(dataIntubacao, document.getElementById('tempo_intubacao'));
        }

        function calcularTempoCateter() {
            const dataCateter = document.getElementById('data_cateter').value;
            calcularTempo(dataCateter, document.getElementById('tempo_cateter'));
        }

        function calcularTempoAntibiotico() {
            const dataInicioAntibiotico = document.getElementById('data_inicio_antibiotico').value;
            calcularTempo(dataInicioAntibiotico, document.getElementById('tempo_antibiotico'));
        }

        function calcularTempoSVD() {
            const dataSVD = document.getElementById('data_svd').value;
            calcularTempo(dataSVD, document.getElementById('tempo_svd'));
        }

        // Manipulação do formulário
        document.getElementById('fastHugForm').addEventListener('submit', function(evento) {
            evento.preventDefault();
            
            const novaAvaliacao = {
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
                tipo_antibiotico: Array.from(document.getElementById('tipo_antibiotico').selectedOptions).map(opcao => opcao.value),
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
            
            avaliacoes.push(novaAvaliacao);
            atualizarTabela();
            mostrarStatus('Avaliação salva com sucesso!', 'sucesso');
            this.reset();
        });

        // Função para mostrar mensagens de status
        function mostrarStatus(mensagem, tipo) {
            const elementoStatus = document.getElementById('status');
            elementoStatus.textContent = mensagem;
            elementoStatus.className = `status ${tipo}`;
            setTimeout(() => elementoStatus.textContent = '', 3000);
        }

        // Função para atualizar a tabela de avaliações
        function atualizarTabela() {
            const divAvaliacoes = document.getElementById('avaliacoes');
            if (avaliacoes.length === 0) {
                divAvaliacoes.innerHTML = '';
                return;
            }

            let htmlTabela = '<table class="tabela-avaliacoes">';
            htmlTabela += '<tr><th>Data</th><th>Paciente</th><th>Leito</th><th>Ações</th></tr>';
            
            avaliacoes.forEach((avaliacao, indice) => {
                htmlTabela += `
                    <tr>
                        <td>${avaliacao.data}</td>
                        <td>${avaliacao.paciente}</td>
                        <td>${avaliacao.leito}</td>
                        <td>
                            <button onclick="removerAvaliacao(${indice})" class="botao">Remover</button>
                        </td>
                    </tr>
                `;
            });
            
            htmlTabela += '</table>';
            divAvaliacoes.innerHTML = htmlTabela;
        }

        // Função para remover uma avaliação
        function removerAvaliacao(indice) {
            avaliacoes.splice(indice, 1);
            atualizarTabela();
            mostrarStatus('Avaliação removida com sucesso!', 'sucesso');
        }

        // Função para exportar dados para CSV
        function exportarCSV() {
            if (avaliacoes.length === 0) {
                mostrarStatus('Não há avaliações para exportar', 'erro');
                return;
            }

            const cabecalhoCSV = [
                'Data,Paciente,Leito,Intubado,Data Intubação,' +
                'Cateter,Data Cateter,Retirar Cateter,' +
                'Antibiótico,Data Início ATB,Data Fim ATB,Tipo ATB,Motivo ATB,' +
                'SVD,Data SVD,Indicação SVD,Remover SVD,' +
                'Feeding,Analgesia,Obs Analgesia,Sedação,RASS,' +
                'Tromboprofilaxia,Obs Tromboprofilaxia,Cabeceira,' +
                'UPP Prevenção,UPP Obs,Glicemia,Glicemia Controlada'
            ];

            const linhasCSV = avaliacoes.map(avaliacao => 
                `${avaliacao.data},${avaliacao.paciente},${avaliacao.leito},${avaliacao.intubado},${avaliacao.data_intubacao},` +
                `${avaliacao.cateter},${avaliacao.data_cateter},${avaliacao.retirar_cateter},` +
                `${avaliacao.antibiotico},${avaliacao.data_inicio_antibiotico},${avaliacao.data_fim_antibiotico},${avaliacao.tipo_antibiotico},${avaliacao.motivo_antibiotico},` +
                `${avaliacao.svd},${avaliacao.data_svd},${avaliacao.indicacao_svd},${avaliacao.remover_svd},` +
                `${avaliacao.feeding},${avaliacao.analgesia_adequada},${avaliacao.analgesia_obs},${avaliacao.sedacao_adequada},${avaliacao.rass},` +
                `${avaliacao.tromboprofilaxia},${avaliacao.tromboprofilaxia_obs},${avaliacao.cabeceira},` +
                `${avaliacao.upp_prevencao},${avaliacao.upp_obs},${avaliacao.glicemia},${avaliacao.glicemia_controlada}`
            );

            const conteudoCSV = [cabecalhoCSV, ...linhasCSV].join('\n');
            const blob = new Blob([conteudoCSV], { type: 'text/csv' });
            const url = window.URL.createObjectURL(blob);
            const linkDownload = document.createElement('a');
            linkDownload.href = url;
            linkDownload.download = 'avaliacoes_fast_hug.csv';
            linkDownload.click();
            window.URL.revokeObjectURL(url);
        }
    </script>
</body>
</html>
