<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulário FAST HUG</title>
    <style>
        /* ... mantido o mesmo CSS anterior ... */
    </style>
</head>
<body>
    <div class="card">
        <h1>Formulário FAST HUG</h1>
        
        <form id="fastHugForm" onsubmit="salvarFormulario(event)">
            <!-- ... mantido o mesmo formulário anterior ... -->
        </form>
        <div id="status"></div>
        
        <!-- Botão para baixar CSV -->
        <button onclick="baixarCSV()" class="botao" style="background-color: #2C5282;">
            Baixar Registros (CSV)
        </button>
    </div>

    <script>
        // Array para armazenar todas as entradas
        let avaliacoes = [];
        
        // Carregar dados existentes ao iniciar
        window.onload = function() {
            const dadosSalvos = localStorage.getItem('avaliacoesFastHug');
            if (dadosSalvos) {
                avaliacoes = JSON.parse(dadosSalvos);
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
            
            // Salvar no localStorage como backup
            localStorage.setItem('avaliacoesFastHug', JSON.stringify(avaliacoes));

            // Mostrar mensagem de sucesso
            const status = document.getElementById('status');
            status.innerHTML = '<div class="status sucesso">Avaliação salva com sucesso!</div>';
            
            // Limpar formulário
            event.target.reset();

            // Limpar mensagem após 3 segundos
            setTimeout(() => {
                status.innerHTML = '';
            }, 3000);
        }

        function baixarCSV() {
            // Definir cabeçalho do CSV
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

            // Converter dados para formato CSV
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

            // Criar arquivo CSV e fazer download
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            
            // Criar nome do arquivo com data atual
            const dataAtual = new Date().toISOString().split('T')[0];
            const nomeArquivo = `fasthug_registros_${dataAtual}.csv`;

            if (navigator.msSaveBlob) { // Para IE 10+
                navigator.msSaveBlob(blob, nomeArquivo);
            } else {
                link.href = URL.createObjectURL(blob);
                link.setAttribute("download", nomeArquivo);
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            }
        }

        // Função para limpar todos os dados (opcional)
        function limparDados() {
            if (confirm('Tem certeza que deseja limpar todos os registros?')) {
                avaliacoes = [];
                localStorage.removeItem('avaliacoesFastHug');
                alert('Todos os registros foram removidos.');
            }
        }
    </script>
</body>
</html>
ricardo
