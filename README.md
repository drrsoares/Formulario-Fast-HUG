<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FAST HUG - Sistema Integrado</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Arial, sans-serif;
            line-height: 1.6;
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

        h1, h2 {
            color: #2c5282;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        input[type="text"],
        input[type="number"],
        input[type="date"],
        input[type="datetime-local"],
        textarea,
        select {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }

        .checkbox-group {
            margin: 10px 0;
        }

        .checkbox-group label {
            font-weight: normal;
        }

        .botao {
            background: #4C51BF;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
            margin: 10px 5px;
            transition: background-color 0.3s;
        }

        .botao:hover {
            background: #434190;
        }

        .botao.exportar {
            background: #2C5282;
        }

        .botao.exportar:hover {
            background: #2A4365;
        }

        .status {
            padding: 12px 15px;
            margin: 10px 0;
            border-radius: 4px;
            position: relative;
            animation: slideIn 0.3s ease-out;
        }

        .status.erro {
            background: #FEE2E2;
            color: #991B1B;
            border-left: 4px solid #DC2626;
        }

        .status.sucesso {
            background: #DCFCE7;
            color: #166534;
            border-left: 4px solid #16A34A;
        }

        .mensagem-conteudo {
            margin-right: 25px;
        }

        .fechar-mensagem {
            position: absolute;
            right: 10px;
            top: 50%;
            transform: translateY(-50%);
            background: none;
            border: none;
            font-size: 20px;
            cursor: pointer;
            color: inherit;
            opacity: 0.7;
        }

        .fechar-mensagem:hover {
            opacity: 1;
        }

        .error-message {
            color: #DC2626;
            font-weight: 500;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }

        th {
            background-color: #f8f9fa;
            font-weight: bold;
        }

        .acoes {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }

        @media (max-width: 600px) {
            body {
                padding: 10px;
            }
            
            .acoes {
                flex-direction: column;
            }
            
            .botao {
                width: 100%;
                margin: 5px 0;
            }
        }

        @keyframes slideIn {
            from {
                transform: translateY(-10px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }
    </style>
</head>
<body>
    <div class="card">
        <h1>FAST HUG - Formulário de Avaliação</h1>
        
        <form id="fastHugForm" onsubmit="salvarFormulario(event)">
            <div class="form-group">
                <label for="data">Data da Avaliação:</label>
                <input type="date" id="data" name="data" required onchange="validarDataAvaliacao()">
            </div>

            <div class="form-group">
                <label for="paciente">Nome do Paciente:</label>
                <input type="text" id="paciente" name="paciente" required>
            </div>

            <div class="form-group">
                <label for="leito">Leito:</label>
                <input type="text" id="leito" name="leito" required>
            </div>

            <div class="form-group">
                <label>Paciente está intubado(a)?</label>
                <div class="checkbox-group">
                    <input type="radio" id="intubado_sim" name="esta_intubado" value="sim" required 
                           onchange="toggleIntubacaoFields()">
                    <label for="intubado_sim">Sim</label>
                </div>
                <div class="checkbox-group">
                    <input type="radio" id="intubado_nao" name="esta_intubado" value="nao" required
                           onchange="toggleIntubacaoFields()">
                    <label for="intubado_nao">Não</label>
                </div>
            </div>

            <div id="intubacao_fields" style="display: none;">
                <div class="form-group">
                    <label for="data_intubacao">Data da Intubação:</label>
                    <input type="date" id="data_intubacao" name="data_intubacao" onchange="calcularTempoIntubacao()">
                    <div id="tempo_intubacao" class="mt-2 text-sm text-gray-600"></div>
                </div>
            </div>

            <h2>FAST HUG</h2>

            <div class="form-group">
                <label for="alimentacao">F - Alimentação:</label>
                <select id="alimentacao" name="alimentacao" required onchange="toggleJejumFields()">
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

            <div id="jejum_fields" style="display: none;">
                <div class="form-group">
                    <label for="data_ultima_alimentacao">Data da Última Alimentação:</label>
                    <input type="datetime-local" 
                           id="data_ultima_alimentacao" 
                           name="data_ultima_alimentacao"
                           onchange="calcularTempoJejum()">
                    <div id="tempo_jejum" class="mt-2 text-sm text-gray-600"></div>
                </div>
            </div>

            <div class="form-group">
                <label for="dor">A - Analgesia (Escala de Dor 0-10):</label>
                <input type="number" id="dor" name="dor" min="0" max="10" required>
            </div>

            <div class="form-group">
                <label for="rass">S - Sedação (RASS):</label>
                <select id="rass" name="rass" required>
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
                    <input type="checkbox" id="heparina" name="tromboprofilaxia" value="heparina">
                    <label for="heparina">Heparina Não Fracionada</label>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="hbpm" name="tromboprofilaxia" value="hbpm">
                    <label for="hbpm">HBPM (Heparina Baixo Peso Molecular)</label>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="anticoagulante" name="tromboprofilaxia" value="anticoagulante">
                    <label for="anticoagulante">Anticoagulante Oral</label>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="meias" name="tromboprofilaxia" value="meias">
                    <label for="meias">Meias Compressivas</label>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="compressao" name="tromboprofilaxia" value="compressao">
                    <label for="compressao">Compressão Pneumática</label>
                </div>
            </div>

            <div class="form-group">
                <label for="cabeceira">H - Cabeceira (graus):</label>
                <input type="number" id="cabeceira" name="cabeceira" min="0" max="90" required>
            </div>

            <div class="form-group">
                <label>U - Profilaxia de Úlcera:</label>
                <div class="checkbox-group">
                    <input type="checkbox" id="omeprazol" name="ulcera" value="omeprazol">
                    <label for="omeprazol">Omeprazol</label>
                </div>
                <div class="checkbox-group">
                    <input type="checkbox" id="ranitidina" name="ulcera" value="ranitidina">
                    <label for="ranitidina">Ranitidina</label>
                </div>
            </div>

            <div class="form-group">
                <label for="glicemia">G - Glicemia (mg/dL):</label>
                <input type="number" id="glicemia" name="glicemia" required>
            </div>

            <div class="form-group">
                <label>Evacuação:</label>
                <div class="checkbox-group">
                    <input type="radio" id="evacuacao_sim" name="evacuacao" value="sim" required>
                    <label for="evacuacao_sim">Sim</label>
                </div>
                <div class="checkbox-group">
                    <input type="radio" id="evacuacao_nao" name="evacuacao" value="nao" required>
                    <label for="evacuacao_nao">Não</label>
                </div>
            </div>

            <div class="form-group">
                <label>Diurese:</label>
                <div class="checkbox-group">
                    <input type="radio" id="diurese_sim" name="diurese_presente" value="sim" required 
                           onchange="document.getElementById('volume_diurese').style.display = this.checked ? 'block' : 'none'">
                    <label for="diurese_sim">Sim</label>
                </div>
                <div class="checkbox-group">
                    <input type="radio" id="diurese_nao" name="diurese_presente" value="nao" required
                           onchange="document.getElementById('volume_diurese').style.display = 'none'">
                    <label for="diurese_nao">Não</label>
                </div>
                
                <div id="volume_diurese" style="display: none; margin-top: 10px;">
                    <label for="volume_diurese_input">Volume da Diurese (mL):</label>
                    <input type="number" 
                           id="volume_diurese_input" 
                           name="volume_diurese" 
                           min="0" 
                           max="10000"
                           class="form-control"
                           placeholder="Informe o volume em mL">
                </div>
            </div>

<!-- Campos para Cateter -->
            <div class="form-group">
                <label>Uso de Cateter:</label>
                <div class="checkbox-group">
                    <input type="radio" id="cateter_sim" name="uso_cateter" value="sim" required 
                           onchange="toggleCateterFields()">
                    <label for="cateter_sim">Sim</label>
                </div>
                <div class="checkbox-group">
                    <input type="radio" id="cateter_nao" name="uso_cateter" value="nao" required
                           onchange="toggleCateterFields()">
                    <label for="cateter_nao">Não</label>
                </div>
            </div>

            <div id="cateter_fields" style="display: none;">
                <div class="form-group">
                    <label for="data_insercao_cateter">Data de Inserção do Cateter:</label>
                    <input type="date" id="data_insercao_cateter" name="data_insercao_cateter" 
                           onchange="calcularTempoCateter()">
                    <div id="tempo_cateter" class="mt-2 text-sm text-gray-600"></div>
                </div>
                <div class="form-group">
                    <label>Pode ser retirado?</label>
                    <div class="checkbox-group">
                        <input type="radio" id="retirar_cateter_sim" name="pode_retirar_cateter" value="sim">
                        <label for="retirar_cateter_sim">Sim</label>
                    </div>
                    <div class="checkbox-group">
                        <input type="radio" id="retirar_cateter_nao" name="pode_retirar_cateter" value="nao">
                        <label for="retirar_cateter_nao">Não</label>
                    </div>
                </div>
            </div>

            <!-- Campos para Sonda Vesical -->
            <div class="form-group">
                <label>Uso de Sonda Vesical de Demora:</label>
                <div class="checkbox-group">
                    <input type="radio" id="svd_sim" name="uso_svd" value="sim" required 
                           onchange="toggleSVDFields()">
                    <label for="svd_sim">Sim</label>
                </div>
                <div class="checkbox-group">
                    <input type="radio" id="svd_nao" name="uso_svd" value="nao" required
                           onchange="toggleSVDFields()">
                    <label for="svd_nao">Não</label>
                </div>
            </div>

            <div id="svd_fields" style="display: none;">
                <div class="form-group">
                    <label for="data_instalacao_svd">Data de Instalação da SVD:</label>
                    <input type="date" id="data_instalacao_svd" name="data_instalacao_svd" 
                           onchange="calcularTempoSVD()">
                    <div id="tempo_svd" class="mt-2 text-sm text-gray-600"></div>
                </div>
                <div class="form-group">
                    <label>Pode ser retirada?</label>
                    <div class="checkbox-group">
                        <input type="radio" id="retirar_svd_sim" name="pode_retirar_svd" value="sim">
                        <label for="retirar_svd_sim">Sim</label>
                    </div>
                    <div class="checkbox-group">
                        <input type="radio" id="retirar_svd_nao" name="pode_retirar_svd" value="nao">
                        <label for="retirar_svd_nao">Não</label>
                    </div>
                </div>
            </div>

            <!-- Campos para Antibiótico -->
            <div class="form-group">
                <label>Uso de Antibiótico:</label>
                <div class="checkbox-group">
                    <input type="radio" id="antibiotico_sim" name="uso_antibiotico" value="sim" required 
                           onchange="toggleAntibioticoFields()">
                    <label for="antibiotico_sim">Sim</label>
                </div>
                <div class="checkbox-group">
                    <input type="radio" id="antibiotico_nao" name="uso_antibiotico" value="nao" required
                           onchange="toggleAntibioticoFields()">
                    <label for="antibiotico_nao">Não</label>
                </div>
            </div>

            <div id="antibiotico_fields" style="display: none;">
                <div class="form-group">
                    <label>Pode ser suspenso?</label>
                    <div class="checkbox-group">
                        <input type="radio" id="suspender_antibiotico_sim" name="pode_suspender_antibiotico" value="sim">
                        <label for="suspender_antibiotico_sim">Sim</label>
                    </div>
                    <div class="checkbox-group">
                        <input type="radio" id="suspender_antibiotico_nao" name="pode_suspender_antibiotico" value="nao">
                        <label for="suspender_antibiotico_nao">Não</label>
                    </div>
                </div>
            </div>

            <div class="form-group">
                <label for="observacoes">Observações Adicionais:</label>
                <textarea id="observacoes" name="observacoes" rows="4"></textarea>
            </div>

            <div class="acoes">
                <button type="submit" class="botao">Salvar Avaliação</button>
                <button type="button" class="botao exportar" onclick="baixarCSV()">Exportar CSV</button>
            </div>
        </form>
        <div id="status"></div>
    </div>

    <div class="card">
        <h2>Avaliações Realizadas</h2>
        <table id="avaliacoesTable">
            <thead>
                <tr>
                    <th>Data</th>
                    <th>Paciente</th>
                    <th>Leito</th>
                    <th>Ações</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
    </div>

    <script>
        // Objeto utilitário para manipulação de datas
        const DateUtils = {
            isValidDate(date) {
                return date instanceof Date && !isNaN(date);
            },

            parseDate(dateString) {
                const parsedDate = new Date(dateString);
                return this.isValidDate(parsedDate) ? parsedDate : null;
            },

            formatDate(date) {
                if (!this.isValidDate(date)) return 'Data inválida';
                return date.toLocaleDateString('pt-BR');
            },

            getDifferenceInDays(startDate, endDate) {
                const diffTime = Math.abs(endDate - startDate);
                return Math.floor(diffTime / (1000 * 60 * 60 * 24));
            },

            getDifferenceInHoursAndMinutes(startDate, endDate) {
                const diffTime = Math.abs(endDate - startDate);
                const hours = Math.floor(diffTime / (1000 * 60 * 60));
                const minutes = Math.floor((diffTime % (1000 * 60 * 60)) / (1000 * 60));
                return { hours, minutes };
            }
        };

        // Array para armazenar todas as entradas
        let avaliacoes = [];
        
        // Carregar dados existentes ao iniciar
        window.onload = function() {
            const dadosSalvos = localStorage.getItem('avaliacoesFastHug');
            if (dadosSalvos) {
                avaliacoes = JSON.parse(dadosSalvos);
                atualizarTabela();
            }
        };

        function validarData(data, tipo) {
            const parsedDate = DateUtils.parseDate(data);
            if (!parsedDate) {
                throw new Error(`${tipo}: formato de data inválido`);
            }
            
            const hoje = new Date();
            hoje.setHours(23, 59, 59, 999);

            if (parsedDate > hoje) {
                throw new Error(`${tipo} não pode ser uma data futura`);
            }

            return parsedDate;
        }

        function toggleIntubacaoFields() {
            const estaIntubado = document.getElementById('intubado_sim').checked;
            const intubacaoFields = document.getElementById('intubacao_fields');
            const dataIntubacao = document.getElementById('data_intubacao');
            
            if (estaIntubado) {
                intubacaoFields.style.display = 'block';
                dataIntubacao.required = true;
            } else {
                intubacaoFields.style.display = 'none';
                dataIntubacao.required = false;
                dataIntubacao.value = '';
                document.getElementById('tempo_intubacao').innerHTML = '';
            }
        }

        function toggleJejumFields() {
            const alimentacao = document.getElementById('alimentacao').value;
            const jejumFields = document.getElementById('jejum_fields');
            const dataUltimaAlimentacao = document.getElementById('data_ultima_alimentacao');
            
            if (alimentacao === 'jejum') {
                jejumFields.style.display = 'block';
                dataUltimaAlimentacao.required = true;
            } else {
                jejumFields.style.display = 'none';
                dataUltimaAlimentacao.required = false;
                dataUltimaAlimentacao.value = '';
                document.getElementById('tempo_jejum').innerHTML = '';
            }
        }

        function toggleCateterFields() {
            const usaCateter = document.getElementById('cateter_sim').checked;
            const cateterFields = document.getElementById('cateter_fields');
            const dataInsercao = document.getElementById('data_insercao_cateter');
            
            if (usaCateter) {
                cateterFields.style.display = 'block';
                dataInsercao.required = true;
            } else {
                cateterFields.style.display = 'none';
                dataInsercao.required = false;
                dataInsercao.value = '';
                document.getElementById('tempo_cateter').innerHTML = '';
                document.querySelectorAll('input[name="pode_retirar_cateter"]').forEach(radio => radio.checked = false);
            }
        }

        function toggleSVDFields() {
            const usaSVD = document.getElementById('svd_sim').checked;
            const svdFields = document.getElementById('svd_fields');
            const dataInstalacao = document.getElementById('data_instalacao_svd');
            
            if (usaSVD) {
                svdFields.style.display = 'block';
                dataInstalacao.required = true;
            } else {
                svdFields.style.display = 'none';
                dataInstalacao.required = false;
                dataInstalacao.value = '';
                document.getElementById('tempo_svd').innerHTML = '';
                document.querySelectorAll('input[name="pode_retirar_svd"]').forEach(radio => radio.checked = false);
            }
        }

        function toggleAntibioticoFields() {
            const usaAntibiotico = document.getElementById('antibiotico_sim').checked;
            const antibioticoFields = document.getElementById('antibiotico_fields');
            
            if (usaAntibiotico) {
                antibioticoFields.style.display = 'block';
            } else {
                antibioticoFields.style.display = 'none';
                document.querySelectorAll('input[name="pode_suspender_antibiotico"]').forEach(radio => radio.checked = false);
            }
        }

        function calcularTempoIntubacao() {
            const dataIntubacaoInput = document.getElementById('data_intubacao');
            const dataAvaliacaoInput = document.getElementById('data');
            const divTempo = document.getElementById('tempo_intubacao');
            
            try {
                if (!dataIntubacaoInput.value || !dataAvaliacaoInput.value) {
                    divTempo.innerHTML = '';
                    return;
                }

                const dataIntubacao = validarData(dataIntubacaoInput.value, 'Data de intubação');
                const dataAvaliacao = validarData(dataAvaliacaoInput.value, 'Data da avaliação');

                if (dataIntubacao > dataAvaliacao) {
                    throw new Error('Data de intubação não pode ser posterior à data da avaliação');
                }

                const dias = DateUtils.getDifferenceInDays(dataIntubacao, dataAvaliacao);
                divTempo.innerHTML = `<strong>Tempo de Intubação:</strong> ${dias} dia(s)`;
                divTempo.className = 'mt-2 text-sm text-green-600';
                
            } catch (error) {
                divTempo.innerHTML = `<span class="error-message">${error.message}</span>`;
                divTempo.className = 'mt-2 text-sm text-red-600';
                dataIntubacaoInput.value = '';
            }
        }

        function calcularTempoJejum() {
            const dataUltimaAlimentacaoInput = document.getElementById('data_ultima_alimentacao');
            const dataAvaliacaoInput = document.getElementById('data');
            const divTempo = document.getElementById('tempo_jejum');
            
            try {
                if (!dataUltimaAlimentacaoInput.value || !dataAvaliacaoInput.value) {
                    divTempo.innerHTML = '';
                    return;
                }

                const dataUltimaAlimentacao = validarData(dataUltimaAlimentacaoInput.value, 'Data da última alimentação');
                const dataAvaliacao = validarData(dataAvaliacaoInput.value, 'Data da avaliação');

                if (dataUltimaAlimentacao > dataAvaliacao) {
                    throw new Error('Data da última alimentação não pode ser posterior à data da avaliação');
                }

                const { hours, minutes } = DateUtils.getDifferenceInHoursAndMinutes(dataUltimaAlimentacao, dataAvaliacao);
                divTempo.innerHTML = `<strong>Tempo de Jejum:</strong> ${hours} horas e ${minutes} minutos`;
                divTempo.className = 'mt-2 text-sm text-green-600';
                
            } catch (error) {
                divTempo.innerHTML = `<span class="error-message">${error.message}</span>`;
                divTempo.className = 'mt-2 text-sm text-red-600';
                dataUltimaAlimentacaoInput.value = '';
            }
        }

        function calcularTempoCateter() {
            const dataInsercaoInput = document.getElementById('data_insercao_cateter');
            const dataAvaliacaoInput = document.getElementById('data');
            const divTempo = document.getElementById('tempo_cateter');
            
            try {
                if (!dataInsercaoInput.value || !dataAvaliacaoInput.value) {
                    divTempo.innerHTML = '';
                    return;
                }

                const dataInsercao = validarData(dataInsercaoInput.value, 'Data de inserção do cateter');
                const dataAvaliacao = validarData(dataAvaliacaoInput.value, 'Data da avaliação');

                if (dataInsercao > dataAvaliacao) {
                    throw new Error('Data de inserção do cateter não pode ser posterior à data da avaliação');
                }

                const dias = DateUtils.getDifferenceInDays(dataInsercao, dataAvaliacao);
                divTempo.innerHTML = `<strong>Tempo de uso do cateter:</strong> ${dias} dia(s)`;
                divTempo.className = 'mt-2 text-sm text-green-600';
                
            } catch (error) {
                divTempo.innerHTML = `<span class="error-message">${error.message}</span>`;
                divTempo.className = 'mt-2 text-sm text-red-600';
                dataInsercaoInput.value = '';
            }
        }

        function calcularTempoSVD() {
            const dataInstalacaoInput = document.getElementById('data_instalacao_svd');
            const dataAvaliacaoInput = document.getElementById('data');
            const divTempo = document.getElementById('tempo_svd');
            
            try {
                if (!dataInstalacaoInput.value || !dataAvaliacaoInput.value) {
                    divTempo.innerHTML = '';
                    return;
                }

                const dataInstalacao = validarData(dataInstalacaoInput.value, 'Data de instalação da SVD');
                const dataAvaliacao = validarData(dataAvaliacaoInput.value, 'Data da avaliação');

                if (dataInstalacao > dataAvaliacao) {
                    throw new Error('Data de instalação da SVD não pode ser posterior à data da avaliação');
                }

                const dias = DateUtils.getDifferenceInDays(dataInstalacao, dataAvaliacao);
                divTempo.innerHTML = `<strong>Tempo de uso da SVD:</strong> ${dias} dia(s)`;
                divTempo.className = 'mt-2 text-sm text-green-600';
                
            } catch (error) {
                divTempo.innerHTML = `<span class="error-message">${error.message}</span>`;
                divTempo.className = 'mt-2 text-sm text-red-600';
                dataInstalacaoInput.value = '';
            }
        }

        function validarDataAvaliacao() {
            const dataAvaliacaoInput = document.getElementById('data');
            try {
                if (dataAvaliacaoInput.value) {
                    validarData(dataAvaliacaoInput.value, 'Data da avaliação');
                    if (document.getElementById('intubado_sim').checked) {
                        calcularTempoIntubacao();
                    }
                    if (document.getElementById('alimentacao').value === 'jejum') {
                        calcularTempoJejum();
                    }
                    if (document.getElementById('cateter_sim').checked) {
                        calcularTempoCateter();
                    }
                    if (document.getElementById('svd_sim').checked) {
                        calcularTempoSVD();
                    }
                }
            } catch (error) {
                mostrarMensagem(error.message, 'erro');
                dataAvaliacaoInput.value = '';
            }
        }

        function salvarFormulario(event) {
            event.preventDefault();
            
            try {
                const formData = new FormData(event.target);
                const dados = Object.fromEntries(formData.entries());

                // Validar data da avaliação
                const dataAvaliacao = validarData(dados.data, 'Data da avaliação');

                // Validar data de intubação se aplicável
                if (dados.esta_intubado === 'sim') {
                    if (!dados.data_intubacao) {
                        throw new Error('Data de intubação é obrigatória para pacientes intubados');
                    }
                    const dataIntubacao = validarData(dados.data_intubacao, 'Data de intubação');
                    if (dataIntubacao > dataAvaliacao) {
                        throw new Error('Data de intubação não pode ser posterior à data da avaliação');
                    }
                    dados.tempo_intubacao = DateUtils.getDifferenceInDays(dataIntubacao, dataAvaliacao);
                }

                // Validar data da última alimentação se em jejum
                if (dados.alimentacao === 'jejum') {
                    if (!dados.data_ultima_alimentacao) {
                        throw new Error('Data da última alimentação é obrigatória para pacientes em jejum');
                    }
                    const dataUltimaAlimentacao = validarData(dados.data_ultima_alimentacao, 'Data da última alimentação');
                    if (dataUltimaAlimentacao > dataAvaliacao) {
                        throw new Error('Data da última alimentação não pode ser posterior à data da avaliação');
                    }
                    const tempoJejum = DateUtils.getDifferenceInHoursAndMinutes(dataUltimaAlimentacao, dataAvaliacao);
                    dados.tempo_jejum_horas = tempoJejum.hours;
                    dados.tempo_jejum_minutos = tempoJejum.minutes;
                }

                // Validar data de inserção do cateter se aplicável
                if (dados.uso_cateter === 'sim') {
                    if (!dados.data_insercao_cateter) {
                        throw new Error('Data de inserção do cateter é obrigatória');
                    }
                    const dataInsercao = validarData(dados.data_insercao_cateter, 'Data de inserção do cateter');
                    if (dataInsercao > dataAvaliacao) {
                        throw new Error('Data de inserção do cateter não pode ser posterior à data da avaliação');
                    }
                    dados.tempo_cateter = DateUtils.getDifferenceInDays(dataInsercao, dataAvaliacao);
                }

                // Validar data de instalação da SVD se aplicável
                if (dados.uso_svd === 'sim') {
                    if (!dados.data_instalacao_svd) {
                        throw new Error('Data de instalação da SVD é obrigatória');
                    }
                    const dataInstalacao = validarData(dados.data_instalacao_svd, 'Data de instalação da SVD');
                    if (dataInstalacao > dataAvaliacao) {
                        throw new Error('Data de instalação da SVD não pode ser posterior à data da avaliação');
                    }
                    dados.tempo_svd = DateUtils.getDifferenceInDays(dataInstalacao, dataAvaliacao);
                }

                dados.timestamp = new Date().toISOString();
                dados.tromboprofilaxia = formData.getAll('tromboprofilaxia').join(';');
                dados.ulcera = formData.getAll('ulcera').join(';');

                avaliacoes.push(dados);
                localStorage.setItem('avaliacoesFastHug', JSON.stringify(avaliacoes));
                atualizarTabela();
                mostrarMensagem('Avaliação salva com sucesso!', 'sucesso');
                event.target.reset();
                
            } catch (error) {
                mostrarMensagem(error.message, 'erro');
            }
        }

        function mostrarMensagem(texto, tipo) {
            const status = document.getElementById('status');
            const mensagem = document.createElement('div');
            mensagem.className = `status ${tipo}`;
            mensagem.innerHTML = `
                <div class="mensagem-conteudo">
                    <strong>${tipo === 'erro' ? 'Erro: ' : 'Sucesso: '}</strong>
                    ${texto}
                </div>
                ${tipo === 'erro' ? '<button class="fechar-mensagem">&times;</button>' : ''}
            `;
            
            if (tipo === 'erro') {
                mensagem.querySelector('.fechar-mensagem').onclick = () => status.removeChild(mensagem);
            } else {
                setTimeout(() => {
                    if (status.contains(mensagem)) {
                        status.removeChild(mensagem);
                    }
                }, 3000);
            }
            
            status.appendChild(mensagem);
        }

        function atualizarTabela() {
            const tbody = document.querySelector('#avaliacoesTable tbody');
            tbody.innerHTML = avaliacoes.map((av, index) => `
                <tr>
                    <td>${new Date(av.data).toLocaleDateString()}</td>
                    <td>${av.paciente}</td>
                    <td>${av.leito}</td>
                    <td>
                        <button onclick="verDetalhes(${index})" class="botao">Ver</button>
                        <button onclick="excluirAvaliacao(${index})" class="botao">Excluir</button>
                    </td>
                </tr>
            `).join('');
        }

        function verDetalhes(index) {
            const av = avaliacoes[index];
            alert(`
                Detalhes da Avaliação:
                
                Paciente: ${av.paciente}
                Data: ${av.data}
                Leito: ${av.leito}
                Intubado(a): ${av.esta_intubado === 'sim' ? 'Sim' : 'Não'}
                ${av.esta_intubado === 'sim' ? `Data da Intubação: ${av.data_intubacao || 'Não informada'}
                Tempo de Intubação: ${av.tempo_intubacao ? av.tempo_intubacao + ' dias' : 'N/A'}` : ''}
                Alimentação: ${av.alimentacao}
                ${av.alimentacao === 'jejum' ? `Data Última Alimentação: ${av.data_ultima_alimentacao}
                Tempo de Jejum: ${av.tempo_jejum_horas} horas e ${av.tempo_jejum_minutos} minutos` : ''}
                Dor: ${av.dor}
                RASS: ${av.rass}
                Tromboprofilaxia: ${av.tromboprofilaxia}
                Cabeceira: ${av.cabeceira}º
                Profilaxia Úlcera: ${av.ulcera}
                Glicemia: ${av.glicemia}
                Evacuação: ${av.evacuacao}
                Diurese: ${av.diurese_presente}
                Volume Diurese: ${av.volume_diurese ? av.volume_diurese + ' mL' : 'N/A'}
                
                Uso de Cateter: ${av.uso_cateter === 'sim' ? 'Sim' : 'Não'}
                ${av.uso_cateter === 'sim' ? `Data de Inserção: ${av.data_insercao_cateter}
                Tempo de Uso: ${av.tempo_cateter} dias
                Pode ser retirado: ${av.pode_retirar_cateter === 'sim' ? 'Sim' : 'Não'}` : ''}
                
                Uso de SVD: ${av.uso_svd === 'sim' ? 'Sim' : 'Não'}
                ${av.uso_svd === 'sim' ? `Data de Instalação: ${av.data_instalacao_svd}
                Tempo de Uso: ${av.tempo_svd} dias
                Pode ser retirada: ${av.pode_retirar_svd === 'sim' ? 'Sim' : 'Não'}` : ''}
                
                Uso de Antibiótico: ${av.uso_antibiotico === 'sim' ? 'Sim' : 'Não'}
                ${av.uso_antibiotico === 'sim' ? `Pode ser suspenso: ${av.pode_suspender_antibiotico === 'sim' ? 'Sim' : 'Não'}` : ''}
                
                Observações: ${av.observacoes || 'Nenhuma'}
            `);
        }

        function excluirAvaliacao(index) {
            if (confirm('Tem certeza que deseja excluir esta avaliação?')) {
                avaliacoes.splice(index, 1);
                localStorage.setItem('avaliacoesFastHug', JSON.stringify(avaliacoes));
                atualizarTabela();
                mostrarMensagem('Avaliação excluída com sucesso!', 'sucesso');
            }
        }

        function baixarCSV() {
            const cabecalhos = [
                'Data da Avaliação',
                'Paciente',
                'Leito',
                'Intubado(a)',
                'Data da Intubação',
                'Tempo de Intubação (dias)',
                'Alimentação',
                'Data Última Alimentação',
                'Tempo de Jejum (horas)',
                'Dor',
                'RASS',
                'Tromboprofilaxia',
                'Cabeceira',
                'Profilaxia Úlcera',
                'Glicemia',
                'Evacuação',
                'Diurese',
                'Volume Diurese (mL)',
                'Uso de Cateter',
                'Data Inserção Cateter',
                'Tempo de Uso Cateter (dias)',
                'Pode Retirar Cateter',
                'Uso de SVD',
                'Data Instalação SVD',
                'Tempo de Uso SVD (dias)',
                'Pode Retirar SVD',
                'Uso de Antibiótico',
                'Pode Suspender Antibiótico',
                'Observações',
                'Data/Hora Registro'
            ];

            let csvContent = cabecalhos.join(',') + '\n';

            avaliacoes.forEach(av => {
                const linha = [
                    av.data,
                    `"${av.paciente}"`,
                    `"${av.leito}"`,
                    av.esta_intubado,
                    av.data_intubacao || 'N/A',
                    av.tempo_intubacao || 'N/A',
                    `"${av.alimentacao}"`,
                    av.data_ultima_alimentacao || 'N/A',
                    av.tempo_jejum_horas ? `${av.tempo_jejum_horas}:${av.tempo_jejum_minutos}` : 'N/A',
                    av.dor,
                    `"${av.rass}"`,
                    `"${av.tromboprofilaxia}"`,
                    av.cabeceira,
                    `"${av.ulcera}"`,
                    av.glicemia,
                    av.evacuacao,
                    av.diurese_presente,
                    av.volume_diurese || 'N/A',
                    av.uso_cateter,
                    av.data_insercao_cateter || 'N/A',
                    av.tempo_cateter || 'N/A',
                    av.pode_retirar_cateter || 'N/A',
                    av.uso_svd,
                    av.data_instalacao_svd || 'N/A',
                    av.tempo_svd || 'N/A',
                    av.pode_retirar_svd || 'N/A',
                    av.uso_antibiotico,
                    av.pode_suspender_antibiotico || 'N/A',
                    `"${av.observacoes || ''}"`,
                    `"${av.timestamp}"`
                ];
                csvContent += linha.join(',') + '\n';
            });

            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            const dataAtual = new Date().toISOString().split('T')[0];
            const nomeArquivo = `fasthug_registros_${dataAt
