
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
            padding: 15px;
            border-radius: 6px;
            margin: 15px 0;
        }

        .sucesso {
            background: #C6F6D5;
            color: #276749;
        }

        .erro {
            background: #FED7D7;
            color: #C53030;
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
    </style>
</head>
<body>
    <div class="card">
        <h1>FAST HUG - Formulário de Avaliação</h1>
        
        <form id="fastHugForm" onsubmit="salvarFormulario(event)">
            <div class="form-group">
                <label for="data">Data da Avaliação:</label>
                <input type="date" id="data" name="data" required>
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
                <label for="data_intubacao">Data da Intubação:</label>
                <input type="date" id="data_intubacao" name="data_intubacao" onchange="calcularTempoIntubacao()">
                <div id="tempo_intubacao" class="mt-2 text-sm text-gray-600"></div>
            </div>

            <h2>FAST HUG</h2>

            <div class="form-group">
                <label for="alimentacao">F - Alimentação:</label>
                <select id="alimentacao" name="alimentacao" required>
                    <option value="">Selecione...</option>
                    <option value="via_oral">Via Oral</option>
                    <option value="sng">Sonda Nasogástrica</option>
                    <option value="sne">Sonda Nasoenteral</option>
                    <option value="gastrostomia">Gastrostomia</option>
                    <option value="jejunostomia">Jejunostomia</option>
                    <option value="npo">NPO</option>
                </select>
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

        function calcularTempoIntubacao() {
            const dataIntubacao = document.getElementById('data_intubacao').value;
            const divTempo = document.getElementById('tempo_intubacao');
            
            if (dataIntubacao) {
                const dataInicio = new Date(dataIntubacao);
                const dataAtual = new Date();
                
                // Ajusta as datas para meio-dia para evitar problemas com horário de verão
                dataInicio.setHours(12, 0, 0, 0);
                dataAtual.setHours(12, 0, 0, 0);
                
                const diffTempo = dataAtual - dataInicio;
                const dias = Math.floor(diffTempo / (1000 * 60 * 60 * 24));
                
                if (dias < 0) {
                    divTempo.innerHTML = '<span style="color: red;">Data de intubação não pode ser futura</span>';
                    document.getElementById('data_intubacao').value = '';
                } else {
                    divTempo.innerHTML = `<strong>Tempo de Intubação:</strong> ${dias} dia(s)`;
                }
            } else {
                divTempo.innerHTML = '';
            }
        }

        function salvarFormulario(event) {
            event.preventDefault();
            const formData = new FormData(event.target);
            const dados = Object.fromEntries(formData.entries());
            
            // Calcular e adicionar tempo de intubação
            if (dados.data_intubacao) {
                const dataInicio = new Date(dados.data_intubacao);
                const dataAtual = new Date();
                dataInicio.setHours(12, 0, 0, 0);
                dataAtual.setHours(12, 0, 0, 0);
                const diffTempo = dataAtual - dataInicio;
                dados.tempo_intubacao = Math.floor(diffTempo / (1000 * 60 * 60 * 24));
            }
            
            dados.timestamp = new Date().toISOString();
            dados.tromboprofilaxia = formData.getAll('tromboprofilaxia').join(';');
            dados.ulcera = formData.getAll('ulcera').join(';');

            avaliacoes.push(dados);
            localStorage.setItem('avaliacoesFastHug', JSON.stringify(avaliacoes));
            atualizarTabela();
            mostrarMensagem('Avaliação salva com sucesso!', 'sucesso');
            event.target.reset();
        }

        function mostrarMensagem(texto, tipo) {
            const status = document.getElementById('status');
            status.innerHTML = `<div class="status ${tipo}">${texto}</div>`;
            setTimeout(() => status.innerHTML = '', 3000);
        }

        function baixarCSV() {
            const cabecalhos = [
                'Data da Avaliação',
                'Paciente',
                'Leito',
                'Data da Intubação',
                'Tempo de Intubação (dias)',
                'Alimentação',
                'Dor',
                'RASS',
                'Tromboprofilaxia',
                'Cabeceira',
                'Profilaxia Úlcera',
                'Glicemia',
                'Evacuação',
                'Diurese',
                'Volume Diurese (mL)',
                'Observações',
                'Data/Hora Registro'
            ];

            let csvContent = cabecalhos.join(',') + '\n';

            avaliacoes.forEach(av => {
                const linha = [
                    av.data,
                    `"${av.paciente}"`,
                    `"${av.leito}"`,
                    av.data_intubacao || 'N/A',
                    av.tempo_intubacao || 'N/A',
                    `"${av.alimentacao}"`,
                    av.dor,
                    `"${av.rass}"`,
                    `"${av.tromboprofilaxia}"`,
                    av.cabeceira,
                    `"${av.ulcera}"`,
                    av.glicemia,
                    av.evacuacao,
                    av.diurese_presente,
                    av.volume_diurese || 'N/A',
                    `"${av.observacoes || ''}"`,
                    `"${av.timestamp}"`
                ];
                csvContent += linha.join(',') + '\n';
            });

            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            const dataAtual = new Date().toISOString().split('T')[0];
            const nomeArquivo = `fasthug_registros_${dataAtual}.csv`;

            if (navigator.msSaveBlob) {
                navigator.msSaveBlob(blob, nomeArquivo);
            } else {
                link.href = URL.createObjectURL(blob);
                link.setAttribute("download", nomeArquivo);
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            }
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
                Data da Intubação: ${av.data_intubacao || 'Não informada'}
                Tempo de Intubação: ${av.tempo_intubacao ? av.tempo_intubacao + ' dias' : 'N/A'}
                Alimentação: ${av.alimentacao}
                Dor: ${av.dor}
                RASS: ${av.rass}
                Tromboprofilaxia: ${av.tromboprofilaxia}
                Cabeceira: ${av.cabeceira}º
                Profilaxia Úlcera: ${av.ulcera}
                Glicemia: ${av.glicemia}
                Evacuação: ${av.evacuacao}
                Diurese: ${av.diurese_presente}
                Volume Diurese: ${av.volume_diurese ? av.volume_diurese + ' mL' : 'N/A'}
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

        function limparDados() {
            if (confirm('Tem certeza que deseja limpar todos os registros?')) {
                avaliacoes = [];
                localStorage.removeItem('avaliacoesFastHug');
                atualizarTabela();
                mostrarMensagem('Todos os registros foram removidos.', 'sucesso');
            }
        }
    </script>
