<!DOCTYPE html>
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

        .botao.api {
            background: #38A169;
        }

        .botao.api:hover {
            background: #2F855A;
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
                    <label for="heparina">Heparina</label>
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
                <label for="observacoes">Observações Adicionais:</label>
                <textarea id="observacoes" name="observacoes" rows="4"></textarea>
            </div>

            <div class="acoes">
                <button type="submit" class="botao">Salvar Avaliação</button>
                <button type="button" class="botao exportar" onclick="baixarCSV()">Exportar CSV</button>
                <button type="button" class="botao api" onclick="salvarAPI()">Enviar para API</button>
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

        function salvarFormulario(event) {
            event.preventDefault();
            const formData = new FormData(event.target);
            const dados = Object.fromEntries(formData.entries());
            
            // Adicionar data e hora atual
            dados.timestamp = new Date().toISOString();
            
            // Tratamento especial para checkboxes
            dados.tromboprofilaxia = formData.getAll('tromboprofilaxia').join(';');
            dados.ulcera = formData.getAll('ulcera').join(';');

            // Adicionar ao array de avaliações
            avaliacoes.push(dados);
            
            // Salvar no localStorage
            localStorage.setItem('avaliacoesFastHug', JSON.stringify(avaliacoes));

            // Atualizar tabela
            atualizarTabela();

            // Mostrar mensagem de sucesso
            mostrarMensagem('Avaliação salva com sucesso!', 'sucesso');
            
            // Limpar formulário
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
                'Alimentação',
                'Dor',
                'RASS',
                'Tromboprofilaxia',
                'Cabeceira',
                'Profilaxia Úlcera',
                'Glicemia',
                'Observações',
                'Data/Hora Registro'
            ];

            let csvContent = cabecalhos.join(',') + '\n';

            avaliacoes.forEach(av => {
                const linha = [
                    av.data,
                    `"${av.paciente}"`,
                    `"${av.leito}"`,
                    `"${av.alimentacao}"`,
                    av.dor,
                    `"${av.rass}"`,
                    `"${av.tromboprofilaxia}"`,
                    av.cabeceira,
                    `"${av.ulcera}"`,
                    av.glicemia,
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

        async function salvarAPI() {
            try {
                const ultimaAvaliacao = avaliacoes[avaliacoes.length - 1];
                const response = await fetch('https://seu-servidor.com/api/fasthug', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(ultimaAvaliacao)
                });

                if (response.ok) {
                    mostrarMensagem('Dados enviados para API com sucesso!', 'sucesso');
                } else {
                    throw new Error('Erro ao enviar para API');
                }
            } catch (erro) {
                mostrarMensagem('Erro ao enviar dados: ' + erro.message, 'erro');
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
                Alimentação: ${av.alimentacao}
                Dor: ${av.dor}
                RASS: ${av.rass}
                Tromboprofilaxia: ${av.tromboprofilaxia}
                Cabeceira: ${av.cabeceira}º
                Profilaxia Úlcera: ${av.ulcera}
                Glicemia: ${av.glicemia}
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
</body>
</html>
