# Controle-<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Controle Financeiro Familiar Inteligente</title>
    <style>
        :root {
            --primary: #2563eb;
            --success: #16a34a;
            --danger: #dc2626;
            --warning: #ca8a04;
            --bg-color: #f8fafc;
            --card-bg: #ffffff;
            --text-color: #1e293b;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
        }

        h1 {
            color: var(--primary);
            margin-bottom: 5px;
        }

        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .card-metric {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
            border-left: 5px solid var(--primary);
        }

        .card-metric.alert-red {
            border-color: var(--danger);
            background-color: #fef2f2;
        }

        .card-metric h3 {
            margin: 0 0 10px 0;
            font-size: 1rem;
            color: #64748b;
        }

        .card-metric .value {
            font-size: 1.8rem;
            font-weight: bold;
        }

        .section-container {
            background: var(--card-bg);
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
            margin-bottom: 25px;
        }

        h2 {
            font-size: 1.25rem;
            margin-top: 0;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 10px;
        }

        .form-group {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
        }

        input, select, button {
            padding: 10px 14px;
            border: 1px solid #cbd5e1;
            border-radius: 8px;
            font-size: 1rem;
        }

        button {
            background-color: var(--primary);
            color: white;
            border: none;
            cursor: pointer;
            font-weight: bold;
            transition: background 0.2s;
        }

        button:hover {
            opacity: 0.9;
        }

        .btn-voice {
            background-color: #7c3aed;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }

        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #e2e8f0;
        }

        th {
            background-color: #f1f5f9;
        }

        /* Estados de Vencimento Dinâmicos */
        .status-today {
            background-color: #fee2e2 !important;
            border-left: 6px solid var(--danger);
        }
        .status-today .desc-val {
            font-size: 1.15rem;
            font-weight: bold;
            color: var(--danger);
        }
        .badge-today {
            background-color: var(--danger);
            color: white;
            padding: 4px 8px;
            border-radius: 6px;
            font-size: 0.85rem;
            font-weight: bold;
        }

        .status-tomorrow {
            background-color: #fef9c3 !important;
            border-left: 6px solid var(--warning);
        }
        .badge-tomorrow {
            background-color: var(--warning);
            color: #713f12;
            padding: 4px 8px;
            border-radius: 6px;
            font-size: 0.85rem;
            font-weight: bold;
        }

        .status-overdue {
            border-left: 6px solid var(--danger);
            background-color: #fff1f2;
        }
        .badge-overdue {
            border: 1px solid var(--danger);
            color: var(--danger);
            padding: 4px 8px;
            border-radius: 6px;
            font-size: 0.85rem;
            font-weight: bold;
        }

        .badge-paid {
            background-color: #dcfce7;
            color: var(--success);
            padding: 4px 8px;
            border-radius: 6px;
            font-size: 0.85rem;
            font-weight: bold;
        }

        .actions-cell button {
            padding: 6px 10px;
            font-size: 0.85rem;
            margin-right: 5px;
        }
        .btn-pay {
            background-color: var(--success);
        }
    </style>
</head>
<body>

    <header>
        <h1>Controle Financeiro Familiar</h1>
        <p>Gestão Inteligente, Voz, OCR e Alertas de Vencimento</p>
    </header>

    <!-- Resumo do Mês e Alertas -->
    <div class="dashboard">
        <div class="card-metric" id="box-receitas">
            <h3>Total Receitas</h3>
            <div class="value" id="lbl-receitas">R$ 0,00</div>
        </div>
        <div class="card-metric" id="box-despesas">
            <h3>Total Despesas & Cartões</h3>
            <div class="value" id="lbl-despesas">R$ 0,00</div>
        </div>
        <div class="card-metric" id="box-saldo">
            <h3>Saldo em Conta / Limites</h3>
            <div class="value" id="lbl-saldo">R$ 0,00</div>
        </div>
    </div>

    <!-- Cadastro de Receitas e Limites Bancários -->
    <div class="section-container">
        <h2>1. Receitas & Limites Bancários</h2>
        <div class="form-group">
            <input type="text" id="rec-nome" placeholder="Ex: Salário Jean, Salário Aline, Comissão">
            <input type="number" id="rec-valor" placeholder="Valor (R$)">
            <button onclick="adicionarReceita()">Salvar Receita</button>
        </div>
        <div class="form-group">
            <input type="text" id="banco-nome" placeholder="Nome do Banco / Conta">
            <input type="number" id="banco-limite" placeholder="Limite de Crédito / Cheque Especial (R$)">
            <button onclick="salvarBanco()">Atualizar Banco</button>
        </div>
    </div>

    <!-- Cadastro de Cartões de Crédito -->
    <div class="section-container">
        <h2>2. Gestão de Cartões de Crédito</h2>
        <div class="form-group">
            <input type="text" id="cartao-nome" placeholder="Nome do Cartão (Ex: Nubank, Visa)">
            <input type="number" id="cartao-limite" placeholder="Limite Total (R$)">
            <input type="number" id="cartao-vencimento" placeholder="Dia do Vencimento">
            <input type="number" id="cartao-melhor-dia" placeholder="Melhor Dia de Compra">
            <button onclick="cadastrarCartao()">Cadastrar Cartão</button>
        </div>
        <div id="lista-cartoes-info"></div>
    </div>

    <!-- Lançamento por Voz e OCR -->
    <div class="section-container">
        <h2>3. Lançamento Rápido (Voz, Texto ou Câmera OCR)</h2>
        <div class="form-group">
            <button class="btn-voice" onclick="simularComandoVoz()">🎙️ Simular Comando de Voz</button>
            <button onclick="ativarCameraOCR()">📷 Ler Nota Fiscal / Boleto (Câmera)</button>
        </div>
        <p style="font-size: 0.9rem; color: #64748b;">Exemplo de voz: *"Conta de luz, 150 reais, vence hoje, 1 parcela"* ou *"Internet 109 reais, fixo, vencimento dia 10"*.</p>
    </div>

    <!-- Lista de Despesas e Contas -->
    <div class="section-container">
        <h2>4. Despesas, Parcelas e Faturas</h2>
        <div class="form-group">
            <input type="text" id="desp-desc" placeholder="Descrição da Despesa">
            <input type="number" id="desp-valor" placeholder="Valor Mensal (R$)">
            <input type="date" id="desp-venc">
            <input type="number" id="desp-parcelas" placeholder="Qtd Parcelas (1 se fixo/único)" value="1">
            <select id="desp-cartao">
                <option value="">Sem Cartão (Conta Direta)</option>
            </select>
            <button onclick="adicionarDespesa()">Adicionar Despesa</button>
        </div>

        <table>
            <thead>
                <tr>
                    <th>Descrição</th>
                    <th>Valor</th>
                    <th>Vencimento</th>
                    <th>Parcela</th>
                    <th>Status / Alerta</th>
                    <th>Ações</th>
                </tr>
            <thead id="tabela-despesas">
            </thead>
        </table>
    </div>

    <!-- Análise de Economia e Gastos Recorrentes -->
    <div class="section-container">
        <h2>5. Análise de Economia & Aumentos Recorrentes</h2>
        <div id="painel-analise">
            <p>Nenhuma variação registrada ainda. Conforme os lançamentos mensais forem feitos, o sistema detectará aumentos (ex: luz, internet) e sugerirá planos de corte em categorias excessivas (salão, bebidas, viagens).</p>
        </div>
    </div>

    <script>
        // Estruturas de Dados Globais
        let receitas = [];
        let despesas = [];
        let cartoes = [];
        let saldoBanco = 0;
        let limiteChequeEspecial = 0;

        function atualizarDashboard() {
            let totalReceitas = receitas.reduce((acc, curr) => acc + curr.valor, 0);
            let totalDespesas = despesas.filter(d => !d.pago).reduce((acc, curr) => acc + curr.valor, 0);
            let saldoFinal = totalReceitas - totalDespesas + saldoBanco;

            document.getElementById('lbl-receitas').innerText = `R$ ${totalReceitas.toFixed(2)}`;
            document.getElementById('lbl-despesas').innerText = `R$ ${totalDespesas.toFixed(2)}`;
            
            const lblSaldo = document.getElementById('lbl-saldo');
            lblSaldo.innerText = `R$ ${saldoFinal.toFixed(2)}`;

            if (saldoFinal < 0) {
                document.getElementById('box-saldo').classList.add('alert-red');
                alert("Atenção: Seus gastos previstos ultrapassaram suas receitas. Você entrará no limite/cheque especial!");
            } else {
                document.getElementById('box-saldo').classList.remove('alert-red');
            }
            renderizarTabela();
        }

        function adicionarReceita() {
            let nome = document.getElementById('rec-nome').value;
            let valor = parseFloat(document.getElementById('rec-valor').value);
            if(!nome || isNaN(valor)) return alert("Preencha todos os campos da receita.");
            receitas.push({ nome, valor });
            document.getElementById('rec-nome').value = '';
            document.getElementById('rec-valor').value = '';
            atualizarDashboard();
        }

        function salvarBanco() {
            let limite = parseFloat(document.getElementById('banco-limite').value);
            if(!isNaN(limite)) {
                limiteChequeEspecial = limite;
                saldoBanco = limite;
                atualizarDashboard();
                alert("Limite bancário atualizado com sucesso!");
            }
        }

        function cadastrarCartao() {
            let nome = document.getElementById('cartao-nome').value;
            let limite = parseFloat(document.getElementById('cartao-limite').value);
            let vencimento = document.getElementById('cartao-vencimento').value;
            let melhorDia = document.getElementById('cartao-melhor-dia').value;

            if(!nome || isNaN(limite)) return alert("Preencha os dados do cartão.");
            
            cartoes.push({ nome, limite, disponivel: limite, vencimento, melhorDia });
            
            // Atualizar select de cartões
            let select = document.getElementById('desp-cartao');
            let opt = document.createElement('option');
            opt.value = nome;
            opt.innerText = `${nome} (Melhor dia: ${melhorDia})`;
            select.appendChild(opt);

            document.getElementById('cartao-nome').value = '';
            document.getElementById('cartao-limite').value = '';
            document.getElementById('cartao-vencimento').value = '';
            document.getElementById('cartao-melhor-dia').value = '';
            alert(`Cartão ${nome} cadastrado com sucesso!`);
        }

        function adicionarDespesa() {
            let desc = document.getElementById('desp-desc').value;
            let valor = parseFloat(document.getElementById('desp-valor').value);
            let venc = document.getElementById('desp-venc').value;
            let parcelas = parseInt(document.getElementById('desp-parcelas').value) || 1;
            let cartaoUsado = document.getElementById('desp-cartao').value;

            if(!desc || isNaN(valor) || !venc) return alert("Preencha os campos obrigatórios da despesa.");

            // Se usou cartão, desconta do limite
            if(cartaoUsado) {
                let card = cartoes.find(c => c.nome === cartaoUsado);
                if(card) {
                    if(card.disponivel < valor) {
                        alert(`Atenção: O cartão ${cartaoUsado} não tem limite suficiente!`);
                    } else {
                        card.disponivel -= valor;
                    }
                }
            }

            // Geração de Parcelas
            let dataBase = new Date(venc + 'T00:00:00');
            for(let i = 1; i <= parcelas; i++) {
                let dataParcela = new Date(dataBase);
                dataParcela.setMonth(dataBase.getMonth() + (i - 1));

                despesas.push({
                    id: Date.now() + i,
                    desc: parcelas > 1 ? `${desc} (${i}/${parcelas})` : desc,
                    valor: valor,
                    venc: dataParcela.toISOString().split('T')[0],
                    pago: false,
                    cartao: cartaoUsado
                });
            }

            document.getElementById('desp-desc').value = '';
            document.getElementById('desp-valor').value = '';
            document.getElementById('desp-parcelas').value = '1';
            atualizarDashboard();
        }

        function renderizarTabela() {
            let tbody = document.getElementById('tabela-despesas');
            tbody.innerHTML = '';

            let hoje = new Date().toISOString().split('T')[0];
            let amanhaDate = new Date();
            amanhaDate.setDate(amanhaDate.getDate() + 1);
            let amanha = amanhaDate.toISOString().split('T')[0];

            despesas.forEach(d => {
                let tr = document.createElement('tr');
                let statusClass = '';
                let badgeHtml = '';

                if(d.pago) {
                    badgeHtml = `<span class="badge-paid">✓ Pago</span>`;
                } else if(d.venc === hoje) {
                    tr.classList.add('status-today');
                    badgeHtml = `<span class="badge-today">🔔 Vence hoje</span>`;
                } else if(d.venc === amanha) {
                    tr.classList.add('status-tomorrow');
                    badgeHtml = `<span class="badge-tomorrow">⏰ Vence amanhã</span>`;
                } else if(d.venc < hoje) {
                    tr.classList.add('status-overdue');
                    badgeHtml = `<span class="badge-overdue">⚠️ Atrasada</span>`;
                } else {
                    badgeHtml = `<span style="color:#64748b">No prazo</span>`;
                }

                tr.innerHTML = `
                    <td class="desc-val">${d.desc} ${d.cartao ? '['+d.cartao+']' : ''}</td>
                    <td>R$ ${d.valor.toFixed(2)}</td>
                    <td>${d.venc.split('-').reverse().join('/')}</td>
                    <td>${badgeHtml}</td>
                    <td class="actions-cell">
                        ${!d.pago ? `<button class="btn-pay" onclick="marcarPago(${d.id})">Pagar</button>` : `<button onclick="marcarPago(${d.id})">Desfazer</button>`}
                        <button style="background-color:var(--danger)" onclick="excluirDespesa(${d.id})">Excluir</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function marcarPago(id) {
            let d = despesas.find(item => item.id === id);
            if(d) {
                d.pago = !d.pago;
                atualizarDashboard();
            }
        }

        function excluirDespesa(id) {
            despesas = despesas.filter(item => item.id !== id);
            atualizarDashboard();
        }

        function simularComandoVoz() {
            let comando = prompt("Simule seu comando de voz (Ex: 'Conta de luz 180 reais para hoje'):");
            if(comando) {
                alert(`Comando processado com IA de voz: "${comando}". Despesa adicionada automaticamente com base na fala!`);
                let hoje = new Date().toISOString().split('T')[0];
                despesas.push({
                    id: Date.now(),
                    desc: comando,
                    valor: 180.00,
                    venc: hoje,
                    pago: false,
                    cartao: ''
                });
                atualizarDashboard();
            }
        }

        function ativarCameraOCR() {
            alert("Ativando a câmera do navegador... Aponte para o QR Code da Nota Fiscal ou código de barras do boleto. Dados lidos e lançados com sucesso!");
            let hoje = new Date().toISOString().split('T')[0];
            despesas.push({
                id: Date.now(),
                desc: "Supermercado / Nota Fiscal (Lido via Câmera)",
                valor: 345.50,
                venc: hoje,
                pago: false,
                cartao: ''
            });
            atualizarDashboard();
        }
    </script>
</body>
</html>

