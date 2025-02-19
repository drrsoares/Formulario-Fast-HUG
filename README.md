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
