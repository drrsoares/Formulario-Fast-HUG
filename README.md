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
