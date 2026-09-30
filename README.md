<!DOCTYPE html>
<html lang="pt-PT">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dashboard de Gestão de Ferramentaria e Calibração</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Segoe+UI:wght@300;400;600;700&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-main: #f0f2f5;
      --card-bg: #ffffff;
      --primary-teal: #1f8a70;
      --primary-teal-dark: #166b56;
      --primary-teal-light: #2da88b;
      --accent-card: #20937a;
      --accent-kpi-bg: #21967d;
      --bar-color: #486581;
      --text-dark: #243b53;
      --text-muted: #627d98;
      --border-color: #d9e2ec;
      --hover-bg: #f8fafc;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, 'Inter', Roboto, sans-serif;
      background-color: var(--bg-main);
      color: var(--text-dark);
      padding: 16px;
      line-height: 1.4;
    }

    .dashboard-container {
      max-width: 1440px;
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      gap: 20px;
    }

    /* Secção */
    .section-block {
      background: var(--card-bg);
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.06);
      overflow: hidden;
      border: 1px solid var(--border-color);
    }

    /* Cabeçalho da Seção */
    .section-banner {
      background: linear-gradient(90deg, #1b7a63 0%, #2a9d80 100%);
      color: #ffffff;
      padding: 10px 20px;
      font-size: 1.15rem;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.8px;
      text-align: center;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
    }

    .section-banner svg {
      width: 22px;
      height: 22px;
      fill: currentColor;
    }

    .section-content {
      padding: 16px;
      display: grid;
      gap: 16px;
    }

    /* Grid para Secção 1 (Devedores): Filtros (220px) | Matriz (1fr) | Ranking (340px) */
    .grid-devedores {
      grid-template-columns: 210px minmax(360px, 1fr) 340px;
    }

    /* Grid para Secção 2 (Manutenção): Filtros (210px) | Tabela Principal (1fr) */
    .grid-manutencao {
      grid-template-columns: 210px 1fr;
    }

    @media (max-width: 1100px) {
      .grid-devedores {
        grid-template-columns: 1fr;
      }
      .grid-manutencao {
        grid-template-columns: 1fr;
      }
    }

    /* Painel Lateral de Filtros */
    .filter-panel {
      display: flex;
      flex-direction: column;
      gap: 14px;
    }

    .kpi-card {
      background: linear-gradient(135deg, #1f8a70, #2eb090);
      color: white;
      border-radius: 8px;
      padding: 20px 14px;
      text-align: center;
      box-shadow: inset 0 0 10px rgba(0,0,0,0.1), 0 4px 6px rgba(0,0,0,0.07);
    }

    .kpi-card .kpi-value {
      font-size: 3.5rem;
      font-weight: 700;
      line-height: 1;
      letter-spacing: -1px;
    }

    .kpi-card .kpi-label {
      font-size: 0.85rem;
      margin-top: 6px;
      text-transform: uppercase;
      opacity: 0.9;
      font-weight: 600;
      letter-spacing: 0.5px;
    }

    .filter-group {
      background: #f8fafc;
      border: 1px solid var(--border-color);
      border-radius: 6px;
      padding: 12px;
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .filter-group label {
      font-size: 0.78rem;
      font-weight: 700;
      color: #486581;
      text-transform: uppercase;
      letter-spacing: 0.3px;
    }

    .filter-group select, .filter-group input {
      width: 100%;
      padding: 7px 10px;
      border: 1px solid #bcccdc;
      border-radius: 4px;
      font-size: 0.85rem;
      color: #334e68;
      background-color: #ffffff;
      outline: none;
      transition: border-color 0.2s;
    }

    .filter-group select:focus, .filter-group input:focus {
      border-color: var(--primary-teal);
    }

    .btn-reset {
      background: #e2e8f0;
      border: none;
      padding: 8px;
      border-radius: 4px;
      color: #475569;
      font-size: 0.8rem;
      font-weight: 600;
      cursor: pointer;
      transition: background 0.2s;
    }

    .btn-reset:hover {
      background: #cbd5e1;
    }

    /* Tabelas e Matrizes */
    .table-card {
      border: 1px solid var(--border-color);
      border-radius: 6px;
      overflow: hidden;
      display: flex;
      flex-direction: column;
      background: #ffffff;
    }

    .table-responsive {
      overflow-x: auto;
      max-height: 480px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.84rem;
      text-align: left;
    }

    th {
      background: #f1f5f9;
      color: #334e68;
      font-weight: 600;
      padding: 8px 10px;
      border-bottom: 2px solid var(--border-color);
      position: sticky;
      top: 0;
      z-index: 10;
      white-space: nowrap;
    }

    td {
      padding: 7px 10px;
      border-bottom: 1px solid #e2e8f0;
      color: #243b53;
      white-space: nowrap;
    }

    tr:hover td {
      background-color: var(--hover-bg);
    }

    tr.row-total td {
      background: #f8fafc;
      font-weight: 700;
      border-top: 2px solid #cbd5e1;
      border-bottom: none;
      color: #102a43;
    }

    .col-num {
      text-align: right;
    }

    .expander-btn {
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 18px;
      height: 18px;
      border: 1px solid #94a3b8;
      border-radius: 3px;
      margin-right: 6px;
      font-size: 11px;
      font-weight: bold;
      color: #475569;
      background: #ffffff;
      user-select: none;
    }

    .expander-btn:hover {
      background: #e2e8f0;
    }

    .subrow-content {
      background: #f8fafc;
      padding: 8px 14px;
      font-size: 0.78rem;
      border-left: 3px solid var(--primary-teal);
      margin: 4px 0;
      border-radius: 0 4px 4px 0;
    }

    /* Gráfico de Barras Laterais */
    .chart-card {
      border: 1px solid var(--border-color);
      border-radius: 6px;
      padding: 14px;
      background: #ffffff;
      display: flex;
      flex-direction: column;
    }

    .chart-header {
      font-size: 0.85rem;
      font-weight: 700;
      color: #334e68;
      margin-bottom: 12px;
      text-transform: uppercase;
      border-bottom: 1px solid #f1f5f9;
      padding-bottom: 6px;
    }

    .bar-list {
      display: flex;
      flex-direction: column;
      gap: 10px;
      overflow-y: auto;
      max-height: 440px;
      padding-right: 4px;
    }

    .bar-item {
      display: flex;
      flex-direction: column;
      gap: 3px;
    }

    .bar-label-row {
      display: flex;
      justify-content: space-between;
      font-size: 0.8rem;
      color: #334e68;
      font-weight: 500;
    }

    .bar-track {
      background: #e2e8f0;
      border-radius: 3px;
      height: 22px;
      width: 100%;
      overflow: hidden;
      position: relative;
    }

    .bar-fill {
      background: #486581;
      height: 100%;
      border-radius: 3px;
      display: flex;
      align-items: center;
      justify-content: flex-end;
      padding-right: 8px;
      color: #ffffff;
      font-size: 0.75rem;
      font-weight: 600;
      transition: width 0.4s ease;
    }

    .badge-status {
      display: inline-block;
      padding: 2px 7px;
      border-radius: 10px;
      font-size: 0.74rem;
      font-weight: 600;
    }

    .badge-atrasado {
      background: #fee2e2;
      color: #991b1b;
    }

    .badge-ok {
      background: #dcfce7;
      color: #166534;
    }

    .badge-warn {
      background: #fef3c7;
      color: #92400e;
    }

    /* Rodapé Informativo */
    .dashboard-footer {
      text-align: center;
      font-size: 0.8rem;
      color: var(--text-muted);
      padding: 10px 0;
    }
  </style>
</head>
<body>

  <div class="dashboard-container">

    <!-- ==================== SEÇÃO 1: USUÁRIO COM DÉBITO DE DEVOLUÇÃO ==================== -->
    <div class="section-block">
      <div class="section-banner">
        <span>Listagem Usuário com Débito de Devolução</span>
      </div>
      <div class="section-content grid-devedores">
        <!-- Filtros Esquerda -->
        <div class="filter-panel">
          <div class="kpi-card">
            <div class="kpi-value" id="kpi-devedores-total">0</div>
            <div class="kpi-label">Itens Pendentes</div>
          </div>

          <div class="filter-group">
            <label for="filtro-dev-apelido">Apelido Principal Ferramenta</label>
            <select id="filtro-dev-apelido" onchange="renderDevedores()">
              <option value="Todos">Todos</option>
            </select>
          </div>

          <div class="filter-group">
            <label for="filtro-dev-ano">Ano</label>
            <select id="filtro-dev-ano" onchange="renderDevedores()">
              <option value="Todos">Todos</option>
            </select>
          </div>

          <div class="filter-group">
            <label for="filtro-dev-mes">Mês</label>
            <select id="filtro-dev-mes" onchange="renderDevedores()">
              <option value="Todos">Todos</option>
            </select>
          </div>

          <button class="btn-reset" onclick="resetFiltrosDevedores()">Repor Filtros</button>
        </div>

        <!-- Matriz / Tabela Central -->
        <div class="table-card">
          <div class="table-responsive">
            <table id="tabela-matriz-devedores">
              <thead>
                <tr id="header-matriz-devedores">
                  <th>Nome Usuário Solicitante</th>
                  <!-- Meses dinâmicos -->
                  <th class="col-num">Total</th>
                </tr>
              </thead>
              <tbody id="body-matriz-devedores">
                <!-- Linhas inseridas via JS -->
              </tbody>
            </table>
          </div>
        </div>

        <!-- Gráfico de Ranking Direita -->
        <div class="chart-card">
          <div class="chart-header">Top Devedores (Itens Pendentes)</div>
          <div class="bar-list" id="bar-list-devedores">
            <!-- Barras inseridas via JS -->
          </div>
        </div>
      </div>
    </div>

    <!-- ==================== SEÇÃO 2: MANUTENÇÃO / CALIBRAÇÃO A VENCER ==================== -->
    <div class="section-block">
      <div class="section-banner">
        <span>Listagem Manutenção/Calibração a Vencer</span>
      </div>
      <div class="section-content grid-manutencao">
        <!-- Filtros Esquerda -->
        <div class="filter-panel">
          <div class="kpi-card">
            <div class="kpi-value" id="kpi-manutencao-total">0</div>
            <div class="kpi-label">Equipamentos a Vencer</div>
          </div>

          <div class="filter-group">
            <label for="filtro-inv-ano">Ano</label>
            <select id="filtro-inv-ano" onchange="renderManutencao()">
              <option value="Todos">Todos</option>
            </select>
          </div>

          <div class="filter-group">
            <label for="filtro-inv-mes">Mês</label>
            <select id="filtro-inv-mes" onchange="renderManutencao()">
              <option value="Todos">Todos</option>
            </select>
          </div>

          <div class="filter-group">
            <label for="filtro-inv-busca">Pesquisar Equipamento</label>
            <input type="text" id="filtro-inv-busca" placeholder="Digite o apelido..." oninput="renderManutencao()">
          </div>

          <button class="btn-reset" onclick="resetFiltrosManutencao()">Repor Filtros</button>
        </div>

        <!-- Tabela Direita -->
        <div class="table-card">
          <div class="table-responsive">
            <table id="tabela-manutencao">
              <thead>
                <tr>
                  <th>Apelido</th>
                  <th class="col-num">Contagem de Apelido</th>
                  <th>Ano</th>
                  <th>Mês</th>
                  <th>Status</th>
                </tr>
              </thead>
              <tbody id="body-manutencao">
                <!-- Linhas inseridas via JS -->
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>

    <div class="dashboard-footer">
      Dashboard Interativo de Gestão de Ferramentaria • Dados extraídos de Relatório Devedor e Inventário
    </div>

  </div>

  <script>
    // Dados carregados dos arquivos enviados
    const DADOS_DEVEDORES = [{"solicitante": "Fabriciane Vieira", "apelido": "TRAVA QUEDA RETRÁTIL", "descricao": "TRAVA QUEDA AUTO RETRATIL ABS AC", "data_emp": "17/04/2025 09:14:44", "data_dev": "22/04/2025 09:14:44", "dias_atraso": 447, "protocolo": "495 - 2025", "cod_sap": "13027488", "serie": "3101696B - 3M"}, {"solicitante": "Fabriciane Vieira", "apelido": "TRAVA QUEDA RETRÁTIL", "descricao": "TRAVA QUEDA AUTO RETRATIL ABS AC", "data_emp": "17/04/2025 09:14:44", "data_dev": "22/04/2025 09:14:44", "dias_atraso": 447, "protocolo": "495 - 2025", "cod_sap": "13027488", "serie": "3101696B - 3M"}, {"solicitante": "Bruno Gomes Fidelis", "apelido": "CINTA", "descricao": "CINTA ELEV 150X3000MM 5T", "data_emp": "16/10/2025 11:10:19", "data_dev": "21/10/2025 11:10:19", "dias_atraso": 265, "protocolo": "902 - 2025", "cod_sap": "15321813", "serie": ""}, {"solicitante": "Bruno Gomes Fidelis", "apelido": "CINTA", "descricao": "CINTA ELEV 150X3000MM 5T", "data_emp": "16/10/2025 11:10:47", "data_dev": "21/10/2025 11:10:47", "dias_atraso": 265, "protocolo": "903 - 2025", "cod_sap": "15321813", "serie": ""}, {"solicitante": "Bruno Gomes Fidelis", "apelido": "CINTA", "descricao": "CINTA ELEV 150X3000MM 5T", "data_emp": "16/10/2025 11:11:12", "data_dev": "21/10/2025 11:11:12", "dias_atraso": 265, "protocolo": "904 - 2025", "cod_sap": "15321813", "serie": ""}, {"solicitante": "Bruno Gomes Fidelis", "apelido": "CINTA", "descricao": "CINTA ELEV 150X3000MM 5T", "data_emp": "16/10/2025 11:11:30", "data_dev": "21/10/2025 11:11:30", "dias_atraso": 265, "protocolo": "905 - 2025", "cod_sap": "15321813", "serie": ""}, {"solicitante": "Bruno Gomes Fidelis", "apelido": "CINTA", "descricao": "CINTA ELEV 150X3000MM 5T", "data_emp": "17/10/2025 11:21:12", "data_dev": "22/10/2025 11:21:12", "dias_atraso": 264, "protocolo": "915 - 2025", "cod_sap": "15321813", "serie": ""}, {"solicitante": "Bruno Gomes Fidelis", "apelido": "TALHA MANUAL", "descricao": "TALHA MANUAL ALAV", "data_emp": "17/10/2025 14:30:19", "data_dev": "22/10/2025 14:30:19", "dias_atraso": 264, "protocolo": "924 - 2025", "cod_sap": "13349689", "serie": ""}, {"solicitante": "Marcos Aurelio Cavallares Mattos", "apelido": "PENTE DE ROSCA", "descricao": "CALIBRADOR ROSCA; TIPO: PENTE; ROSCA: ME", "data_emp": "12/01/2026 13:34:13", "data_dev": "17/01/2026 13:34:13", "dias_atraso": 177, "protocolo": "31 - 2026", "cod_sap": "15252676", "serie": ""}, {"solicitante": "Marcos Aurelio Cavallares Mattos", "apelido": "PAQUIMETRO ANALÓGICO 300MM", "descricao": "PAQUIMETRO 300MM 0,05MM", "data_emp": "12/01/2026 13:34:13", "data_dev": "17/01/2026 13:34:13", "dias_atraso": 177, "protocolo": "31 - 2026", "cod_sap": "13252398", "serie": ""}, {"solicitante": "Ricardo Goncalves De Mello Leite", "apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "descricao": "CINTO PARAQUEDISTA;PARAQUEDISTA", "data_emp": "13/04/2026 11:09:39", "data_dev": "18/04/2026 11:09:39", "dias_atraso": 86, "protocolo": "152 - 2026", "cod_sap": "15042959", "serie": "HL005KV"}, {"solicitante": "Antonio Andrade", "apelido": "Notebook do Laser Scan", "descricao": "NOTEBOOK INTEL CORE I7 LCD FULL HD", "data_emp": "25/05/2026 15:54:28", "data_dev": "26/05/2026 15:54:28", "dias_atraso": 48, "protocolo": "220 - 2026", "cod_sap": "13059999", "serie": ""}, {"solicitante": "Antonio Andrade", "apelido": "NIVEL SCANNER FARO", "descricao": "SCANNER 400082-45 PROGRESS RAIL", "data_emp": "25/05/2026 15:54:28", "data_dev": "26/05/2026 15:54:28", "dias_atraso": 48, "protocolo": "220 - 2026", "cod_sap": "15328831", "serie": ""}, {"solicitante": "Antonio Andrade", "apelido": "TRIPE DO SCANNER FARO", "descricao": "TRIPE 2,2M 150KG HL3F220 HERCULES", "data_emp": "25/05/2026 15:54:28", "data_dev": "26/05/2026 15:54:28", "dias_atraso": 48, "protocolo": "220 - 2026", "cod_sap": "15348096", "serie": ""}, {"solicitante": "Wanderson Teixeira", "apelido": "TRANSFERIDOR DE GRAU", "descricao": "TRANSFERIDOR MED ANG 0-90GR 175-300MM", "data_emp": "15/06/2026 09:22:58", "data_dev": "20/06/2026 09:22:58", "dias_atraso": 23, "protocolo": "235 - 2026", "cod_sap": "13037587", "serie": "CTSS0123"}, {"solicitante": "Wanderson Teixeira", "apelido": "TRAVA QUEDA RETRÁTIL", "descricao": "TRAVA QUEDA AUTO RETRATIL ABS AC", "data_emp": "15/06/2026 09:24:29", "data_dev": "20/06/2026 09:24:29", "dias_atraso": 23, "protocolo": "236 - 2026", "cod_sap": "13027488", "serie": "HWR025"}, {"solicitante": "Wanderson Teixeira", "apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "descricao": "CINTO PARAQUEDISTA;PARAQUEDISTA", "data_emp": "15/06/2026 09:24:29", "data_dev": "20/06/2026 09:24:29", "dias_atraso": 23, "protocolo": "236 - 2026", "cod_sap": "15042959", "serie": "HL005KV"}, {"solicitante": "Wanderson Teixeira", "apelido": "LIXADEIRA 7\"", "descricao": "ESMERILHADEIRA TRILH MAN 535MM HIDR 2 LA", "data_emp": "15/06/2026 09:24:29", "data_dev": "20/06/2026 09:24:29", "dias_atraso": 23, "protocolo": "236 - 2026", "cod_sap": "15733921", "serie": ""}, {"solicitante": "Wanderson Teixeira", "apelido": "MANOMETRO OXIGENIO", "descricao": "MANOMETRO CIRCULAR; DIAMETRO: 100MM; ESC", "data_emp": "15/06/2026 13:30:15", "data_dev": "25/06/2026 13:30:15", "dias_atraso": 18, "protocolo": "241 - 2026", "cod_sap": "15433206", "serie": "CTSS1106"}, {"solicitante": "Wanderson Teixeira", "apelido": "MANOMETRO OXIGENIO", "descricao": "MANOMETRO CIRCULAR; DIAMETRO: 100MM; ESC", "data_emp": "15/06/2026 13:30:15", "data_dev": "25/06/2026 13:30:15", "dias_atraso": 18, "protocolo": "241 - 2026", "cod_sap": "15433206", "serie": "CTSS1107"}, {"solicitante": "Thiago Barbosa Dos Santos", "apelido": "PAQUIMETRO DIGITAL DE FIBRA DE CARBONO 150MM", "descricao": "PAQUIMETRO 150MM 0,05MM", "data_emp": "16/06/2026 16:00:41", "data_dev": "26/06/2026 16:00:41", "dias_atraso": 17, "protocolo": "245 - 2026", "cod_sap": "13293628", "serie": "CTSS0204"}, {"solicitante": "Robson De Souza Mendonca", "apelido": "DETECTOR DE TENSAO POR APROXIMAÇÃO ", "descricao": "DETECTOR TENSAO; TIPO: MEDIA TENSAO; MED", "data_emp": "18/06/2026 08:24:05", "data_dev": "23/06/2026 08:24:05", "dias_atraso": 20, "protocolo": "248 - 2026", "cod_sap": "15199421", "serie": "CTSS1204"}, {"solicitante": "Robson De Souza Mendonca", "apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "descricao": "CINTO PARAQUEDISTA;PARAQUEDISTA", "data_emp": "18/06/2026 08:24:59", "data_dev": "23/06/2026 08:24:59", "dias_atraso": 20, "protocolo": "249 - 2026", "cod_sap": "15042959", "serie": "HL005KV"}, {"solicitante": "Robson De Souza Mendonca", "apelido": "PARAFUSADEIRA DCD996", "descricao": "PARAFUSADEIRA ELETRICA;TIP;6041DW MAKITA", "data_emp": "18/06/2026 08:25:37", "data_dev": "23/06/2026 08:25:37", "dias_atraso": 20, "protocolo": "250 - 2026", "cod_sap": "15416151", "serie": "CTSS00001"}, {"solicitante": "Robson De Souza Mendonca", "apelido": "PARAFUSADEIRA DCD996", "descricao": "PARAFUSADEIRA ELETRICA;TIP;6041DW MAKITA", "data_emp": "18/06/2026 08:25:37", "data_dev": "23/06/2026 08:25:37", "dias_atraso": 20, "protocolo": "250 - 2026", "cod_sap": "15416151", "serie": "CTSS00002"}, {"solicitante": "Sheila Magalhaes", "apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "descricao": "CINTO PARAQUEDISTA;PARAQUEDISTA", "data_emp": "03/07/2026 08:25:21", "data_dev": "08/07/2026 08:25:21", "dias_atraso": 5, "protocolo": "258 - 2026", "cod_sap": "15042959", "serie": "KL012CAHVR2"}];
    const DADOS_INVENTARIO = [{"apelido": "TORQUIMETRO 10 A 50 GEDORE", "cod_individual": "CTSS0191", "cod_sap": "13246811", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO 45 A 220 LBF FT TRAMONTINA", "cod_individual": "TORQUIMETRO-01", "cod_sap": "13291526", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO 45 A 220 LBF FT TRAMONTINA", "cod_individual": "TORQUIMETRO-02", "cod_sap": "13291526", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1163", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1162", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1156", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1161", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2026", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1160", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1158", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1159", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1157", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1164", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1165", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1155", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1154", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1153", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1152", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1211", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/06/2026", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1210", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1212", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/06/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1213", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2027", "prazo": "12"}, {"apelido": "ESQUADRO", "cod_individual": "CTSS1214", "cod_sap": "15479626", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/06/2027", "prazo": "12"}, {"apelido": "TORQUIMETRO 50 A 270 LBF FT DREMOMETER", "cod_individual": "CTSS0055", "cod_sap": "15715393", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO 7 A 340 Nm NOVOTEST", "cod_individual": "TORQUIMETRO-01", "cod_sap": "13291350", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 300MM", "cod_individual": "CTSS0118", "cod_sap": "15734499", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/03/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 300MM", "cod_individual": "CTSS0119", "cod_sap": "15734499", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO ANALÓGICO 300MM", "cod_individual": "CTSS0094", "cod_sap": "13252398", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO ANALÓGICO 300MM", "cod_individual": "CTSS0095", "cod_sap": "13252398", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO ANALÓGICO 300MM", "cod_individual": "CTSS0178", "cod_sap": "13252398", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO ANALÓGICO 300MM", "cod_individual": "CTSS0179", "cod_sap": "13252398", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/07/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO ANALÓGICO 300MM", "cod_individual": "CTSS0180", "cod_sap": "13252398", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO ANALÓGICO 300MM", "cod_individual": "CTSS0181", "cod_sap": "13252398", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO ANALÓGICO 300MM", "cod_individual": "CTSS0011", "cod_sap": "13252398", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 300MM", "cod_individual": "CTSS0121", "cod_sap": "15733239", "status": "Ativo", "situacao": "Disponível", "dt_prox": "31/03/2027", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 300MM", "cod_individual": "CTSS0159", "cod_sap": "15733239", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/03/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 300MM", "cod_individual": "CTSS0160", "cod_sap": "15733239", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2027", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 300MM", "cod_individual": "CTSS0161", "cod_sap": "15733239", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/03/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 300MM", "cod_individual": "CTSS0163", "cod_sap": "15733239", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 300MM", "cod_individual": "CTSS0164", "cod_sap": "15733239", "status": "Ativo", "situacao": "Disponível", "dt_prox": "15/06/2027", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 300MM", "cod_individual": "CTSS 0013", "cod_sap": "15733239", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 200MM", "cod_individual": "CTSS0112", "cod_sap": "13292600", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 200MM", "cod_individual": "CTSS0113", "cod_sap": "13292600", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 200MM", "cod_individual": "CTSS0114", "cod_sap": "13292600", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 200MM", "cod_individual": "CTSS0003", "cod_sap": "13292600", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 200MM", "cod_individual": "CTSS0029", "cod_sap": "13292600", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 200MM", "cod_individual": "CTSS0115", "cod_sap": "13292600", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 200MM", "cod_individual": "CTSS0111", "cod_sap": "13292600", "status": "Ativo", "situacao": "Disponível", "dt_prox": "25/06/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO ", "cod_individual": "CTSS0187", "cod_sap": "15394751", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO ", "cod_individual": "CTSS0188", "cod_sap": "15394751", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO ", "cod_individual": "CTSS0189", "cod_sap": "15394751", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO ", "cod_individual": "CTSS0055", "cod_sap": "15394751", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO ", "cod_individual": "CTSS0184", "cod_sap": "15394751", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO ", "cod_individual": "CTSS0077", "cod_sap": "15394751", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/07/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO ", "cod_individual": "CTSS1001", "cod_sap": "15394751", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO ", "cod_individual": "CTSS 0185", "cod_sap": "15394751", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "TORQUIMETRO ", "cod_individual": "CTSS 0186", "cod_sap": "15394751", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO EXTERNO", "cod_individual": "CTSS0128", "cod_sap": "16910496", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO EXTERNO", "cod_individual": "CTSS0129", "cod_sap": "16910496", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO EXTERNO", "cod_individual": "CTSS0096", "cod_sap": "16910496", "status": "Ativo", "situacao": "Disponível", "dt_prox": "18/11/2026", "prazo": "12"}, {"apelido": "MICROMETRO EXTERNO", "cod_individual": "CTSS0034", "cod_sap": "16910496", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2027", "prazo": "12"}, {"apelido": "MICROMETRO EXTERNO", "cod_individual": "CTSS0038", "cod_sap": "16910496", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO EXTERNO", "cod_individual": "CTSS0130", "cod_sap": "16910496", "status": "Ativo", "situacao": "Disponível", "dt_prox": "05/11/2026", "prazo": "12"}, {"apelido": "MICROMETRO EXTERNO", "cod_individual": "CTSS0250", "cod_sap": "16910496", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO EXTERNO", "cod_individual": "CTSS0037", "cod_sap": "16910496", "status": "Ativo", "situacao": "Disponível", "dt_prox": "31/03/2027", "prazo": "12"}, {"apelido": "MICROMETRO EXTERNO", "cod_individual": "CTSS0127", "cod_sap": "16910496", "status": "Ativo", "situacao": "Disponível", "dt_prox": "31/03/2027", "prazo": "12"}, {"apelido": "MICROMETRO INTERNO", "cod_individual": "CTSS0057", "cod_sap": "15724702", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO INTERNO", "cod_individual": "CTSS0092", "cod_sap": "15724702", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO INTERNO", "cod_individual": "CTSS0033", "cod_sap": "15724702", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/12/2026", "prazo": "12"}, {"apelido": "MICROMETRO INTERNO", "cod_individual": "CTSS0097", "cod_sap": "15724702", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2027", "prazo": "12"}, {"apelido": "MICROMETRO INTERNO", "cod_individual": "CTSS0039", "cod_sap": "15724702", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO INTERNO", "cod_individual": "CTSS0250", "cod_sap": "15724702", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO INTERNO", "cod_individual": "CTSS0130", "cod_sap": "15724702", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "MICROMETRO INTERNO", "cod_individual": "CTSS0093", "cod_sap": "15724702", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0093", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "09/03/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0102", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "18/03/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0158", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0051", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0107", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "23/05/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0108", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0060", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0081", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0082", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/03/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "702/13A", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0078", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0079", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "21/06/2026", "prazo": "12"}, {"apelido": "CALIBRADOR ", "cod_individual": "CTSS0080", "cod_sap": "13144161", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL FIBRA DE CARBONO 450MM", "cod_individual": "CTSS0125", "cod_sap": "13150994", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL FIBRA DE CARBONO 450MM", "cod_individual": "CTSS0153", "cod_sap": "13150994", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL FIBRA DE CARBONO 450MM", "cod_individual": "CTSS0154", "cod_sap": "13150994", "status": "Ativo", "situacao": "Disponível", "dt_prox": "28/06/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL FIBRA DE CARBONO 450MM", "cod_individual": "CTSS0155", "cod_sap": "13150994", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "COMPARADOR DE DIÂMETRO INTERNO 18/35MM", "cod_individual": "CTSS0135", "cod_sap": "15749516", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "COMPARADOR DE DIÂMETRO INTERNO 18/35MM", "cod_individual": "CTSS0134", "cod_sap": "15749516", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "ESCALA GRADUADA DE AÇO INOX 1000MM", "cod_individual": "CTSS0043", "cod_sap": "15383194", "status": "Ativo", "situacao": "Disponível", "dt_prox": "08/03/2026", "prazo": "12"}, {"apelido": "GONIOMETRO COMBINADO COM RÉGUA GRADUADA", "cod_individual": "CTSS0141", "cod_sap": "15425909", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/04/2027", "prazo": "12"}, {"apelido": "ALINHADOR DE EIXO A LASER ", "cod_individual": "R52NBOENVYB", "cod_sap": "13027549", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "13/03/2027", "prazo": "12"}, {"apelido": "DETECTOR DE TENSAO POR APROXIMAÇÃO ", "cod_individual": "CTSS1204", "cod_sap": "15199421", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "01/06/2027", "prazo": "12"}, {"apelido": "PAQUIMETRO ANALOGICO 600MM", "cod_individual": "CTSS0171", "cod_sap": "13239313", "status": "Ativo", "situacao": "Disponível", "dt_prox": "03/05/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 0-150MM", "cod_individual": "CTSS0125", "cod_sap": "15734500", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/03/2026", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL 0-150MM", "cod_individual": "CTSS0204", "cod_sap": "15734500", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/07/2026", "prazo": "12"}, {"apelido": "ANEMOMETRO DIGITAL MDA-20", "cod_individual": "CTSS1059", "cod_sap": "13265286", "status": "Ativo", "situacao": "Disponível", "dt_prox": "05/02/2027", "prazo": "12"}, {"apelido": "CALIBRADO DE FOLGA", "cod_individual": "CTSS0061", "cod_sap": "15234673", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/03/2026", "prazo": "12"}, {"apelido": "CALIBRADO DE FOLGA", "cod_individual": "CTSS0109", "cod_sap": "15234673", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2026", "prazo": "12"}, {"apelido": "CALIBRADO DE FOLGA", "cod_individual": "CTSS0203", "cod_sap": "15234673", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/03/2026", "prazo": "12"}, {"apelido": "SUTA DIGITAL", "cod_individual": "CTSSOO54", "cod_sap": "15426383", "status": "Ativo", "situacao": "Disponível", "dt_prox": "20/04/2027", "prazo": "12"}, {"apelido": "PAQUIMETRO ANALOGICO 200", "cod_individual": "CTSS0007", "cod_sap": "13171473", "status": "Ativo", "situacao": "Disponível", "dt_prox": "31/03/2027", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL DE FIBRA DE CARBONO 150MM", "cod_individual": "CTSS0204", "cod_sap": "13293628", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "01/03/2027", "prazo": "12"}, {"apelido": "PAQUIMETRO DIGITAL DE FIBRA DE CARBONO 150MM", "cod_individual": "CTSS1209", "cod_sap": "13293628", "status": "Ativo", "situacao": "Disponível", "dt_prox": "22/05/2027", "prazo": "12"}, {"apelido": "TRANSFERIDOR DE GRAU", "cod_individual": "CTSS0123", "cod_sap": "13037587", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "10/04/2027", "prazo": "12"}, {"apelido": "FASIMETRO DIGITAL ", "cod_individual": "CTSS1049", "cod_sap": "13461990", "status": "Ativo", "situacao": "Disponível", "dt_prox": "27/01/2027", "prazo": "12"}, {"apelido": "MEGOMETRO DIGITAL", "cod_individual": "CTSS1051", "cod_sap": "16910523", "status": "Ativo", "situacao": "Disponível", "dt_prox": "27/01/2027", "prazo": "12"}, {"apelido": "MANOMETRO NITROGENIO", "cod_individual": "CTSS1099", "cod_sap": "15453499", "status": "Ativo", "situacao": "Disponível", "dt_prox": "01/03/2027", "prazo": "12"}, {"apelido": "MANOMETRO NITROGENIO", "cod_individual": "CTSS1092", "cod_sap": "15453499", "status": "Ativo", "situacao": "Disponível", "dt_prox": "20/03/2027", "prazo": "12"}, {"apelido": "MANOMETRO NITROGENIO", "cod_individual": "CTSS1096", "cod_sap": "15453499", "status": "Ativo", "situacao": "Disponível", "dt_prox": "20/03/2027", "prazo": "12"}, {"apelido": "ANEMOMETRO DIGITAL", "cod_individual": "CTSS0018", "cod_sap": "13457334", "status": "Ativo", "situacao": "Disponível", "dt_prox": "19/01/2027", "prazo": "12"}, {"apelido": "PIROMETRO DIGITAL", "cod_individual": "CTSS1192", "cod_sap": "15338145", "status": "Ativo", "situacao": "Disponível", "dt_prox": "25/05/2027", "prazo": "12"}, {"apelido": "OSCILIOSCOPIO DIGITAL", "cod_individual": "CTSS1044", "cod_sap": "15175040", "status": "Ativo", "situacao": "Disponível", "dt_prox": "29/01/2027", "prazo": "12"}, {"apelido": "DECIBELIMETRO DIGITAL", "cod_individual": "CTSS0087", "cod_sap": "13171565", "status": "Ativo", "situacao": "Disponível", "dt_prox": "07/01/2027", "prazo": "12"}, {"apelido": "MULTI TALABARTE P/ FERRAMENTAS - HÉRCULES ", "cod_individual": "0000364", "cod_sap": "13324274", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "MULTI TALABARTE P/ FERRAMENTAS - HÉRCULES ", "cod_individual": "0000370", "cod_sap": "13324274", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "LINHA DE  VIDA HLV01", "cod_individual": "CTSS000256", "cod_sap": "13122877", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/12/2026", "prazo": "12"}, {"apelido": "LINHA DE  VIDA HLV01", "cod_individual": "CTSS000257", "cod_sap": "13122877", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/12/2026", "prazo": "12"}, {"apelido": "LINHA DE  VIDA HLV01", "cod_individual": "CTSS000235", "cod_sap": "13122877", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/12/2026", "prazo": "12"}, {"apelido": "LINHA DE  VIDA HLV01", "cod_individual": "CTSS000212", "cod_sap": "13122877", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/12/2026", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES ", "cod_individual": "0000333", "cod_sap": "15368399", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES ", "cod_individual": "0000314", "cod_sap": "15368399", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000338", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000301", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000354", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000373", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000317", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000357", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000349", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000312", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000352", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000327", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000355", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000391", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000369", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000366", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "TALABARTE - HÉRCULES", "cod_individual": "0000340", "cod_sap": "16905195", "status": "Ativo", "situacao": "Disponível", "dt_prox": "10/06/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000303", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000363", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000310", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000339", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000348", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000320", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000144", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000346", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000377", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000332", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000311", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000341", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000229", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000122", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000331", "cod_sap": "15042959", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "09/04/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000163", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "08/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000324", "cod_sap": "15042959", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "09/04/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000313", "cod_sap": "15042959", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "09/04/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000362", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "05/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000130", "cod_sap": "15042959", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000380", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000304", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000241", "cod_sap": "15042959", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "24/04/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000358", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000307", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000365", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000389", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000351", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000118", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000319", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000336", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000372", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "04/05/2027", "prazo": "12"}, {"apelido": "CINTO PARAQUEDISTA - HÉRCULES ", "cod_individual": "0000364", "cod_sap": "15042959", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000356", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000325", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000382", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000343", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000326", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000147", "cod_sap": "13027488", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "01/07/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000170", "cod_sap": "13027488", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "01/07/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000230", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "01/05/2027", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000106", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000252", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000361", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000344", "cod_sap": "13027488", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000396", "cod_sap": "13027488", "status": "Ativo", "situacao": "Emprestada", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "TRAVA QUEDA RETRÁTIL", "cod_individual": "0000209", "cod_sap": "13027488", "status": "Ativo", "situacao": "Disponível", "dt_prox": "17/03/2026", "prazo": "12"}, {"apelido": "ADAPTADOR ANCORAGEM AC/AI 6KN", "cod_individual": "CTSS210", "cod_sap": "13174371", "status": "Ativo", "situacao": "Disponível", "dt_prox": "14/11/2026", "prazo": "12"}, {"apelido": "MEDIDOR DE ESPESSURA ", "cod_individual": "CTSS0131", "cod_sap": "13030896", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "MEDIDOR DE ESPESSURA ", "cod_individual": "CTSS0139", "cod_sap": "13030896", "status": "Ativo", "situacao": "Disponível", "dt_prox": "11/05/2026", "prazo": "12"}, {"apelido": "PAQUIMITRO ANALOGICO 200MM", "cod_individual": "CTSS 007", "cod_sap": "13316322", "status": "Ativo", "situacao": "Disponível", "dt_prox": "31/03/2027", "prazo": "12"}];

    const MESES_NOMES = [
      'janeiro', 'fevereiro', 'março', 'abril', 'maio', 'junho',
      'julho', 'agosto', 'setembro', 'outubro', 'novembro', 'dezembro'
    ];

    // Helper para converter "DD/MM/AAAA"
    function parseDataPT(dataStr) {
      if (!dataStr) return null;
      const partes = dataStr.split(' ')[0].split('/');
      if (partes.length === 3) {
        return new Date(parseInt(partes[2]), parseInt(partes[1]) - 1, parseInt(partes[0]));
      }
      return null;
    }

    // Processamento Inicial dos Dados de Devedores
    DADOS_DEVEDORES.forEach(item => {
      const dtDev = parseDataPT(item.data_dev);
      if (dtDev) {
        item.ano = dtDev.getFullYear();
        item.mesNum = dtDev.getMonth() + 1;
        item.mesNome = MESES_NOMES[dtDev.getMonth()];
      } else {
        item.ano = 'N/D';
        item.mesNum = 0;
        item.mesNome = 'N/D';
      }
    });

    // Processamento Inicial dos Dados de Inventário
    DADOS_INVENTARIO.forEach(item => {
      const dtProx = parseDataPT(item.dt_prox);
      if (dtProx) {
        item.ano = dtProx.getFullYear();
        item.mesNum = dtProx.getMonth() + 1;
        item.mesNome = MESES_NOMES[dtProx.getMonth()];
      } else {
        item.ano = 'N/D';
        item.mesNum = 0;
        item.mesNome = 'N/D';
      }
    });

    // Inicialização de Filtros
    function popularFiltros() {
      // Filtros Devedores
      const apelidosDev = [...new Set(DADOS_DEVEDORES.map(d => d.apelido).filter(Boolean))].sort();
      const anosDev = [...new Set(DADOS_DEVEDORES.map(d => d.ano).filter(a => a !== 'N/D'))].sort();
      const mesesDev = [...new Set(DADOS_DEVEDORES.map(d => d.mesNome).filter(m => m !== 'N/D'))];
      mesesDev.sort((a, b) => MESES_NOMES.indexOf(a) - MESES_NOMES.indexOf(b));

      const selApelidoDev = document.getElementById('filtro-dev-apelido');
      apelidosDev.forEach(ap => {
        selApelidoDev.innerHTML += `<option value="${ap}">${ap}</option>`;
      });

      const selAnoDev = document.getElementById('filtro-dev-ano');
      anosDev.forEach(an => {
        selAnoDev.innerHTML += `<option value="${an}">${an}</option>`;
      });

      const selMesDev = document.getElementById('filtro-dev-mes');
      mesesDev.forEach(m => {
        selMesDev.innerHTML += `<option value="${m}">${m}</option>`;
      });

      // Filtros Inventário
      const anosInv = [...new Set(DADOS_INVENTARIO.map(d => d.ano).filter(a => a !== 'N/D'))].sort();
      const mesesInv = [...new Set(DADOS_INVENTARIO.map(d => d.mesNome).filter(m => m !== 'N/D'))];
      mesesInv.sort((a, b) => MESES_NOMES.indexOf(a) - MESES_NOMES.indexOf(b));

      const selAnoInv = document.getElementById('filtro-inv-ano');
      anosInv.forEach(an => {
        selAnoInv.innerHTML += `<option value="${an}">${an}</option>`;
      });

      const selMesInv = document.getElementById('filtro-inv-mes');
      mesesInv.forEach(m => {
        selMesInv.innerHTML += `<option value="${m}">${m}</option>`;
      });
    }

    // Controle de expansão de linhas na tabela
    const rowsExpandidas = new Set();
    function toggleExpand(user) {
      if (rowsExpandidas.has(user)) {
        rowsExpandidas.delete(user);
      } else {
        rowsExpandidas.add(user);
      }
      renderDevedores();
    }

    // Renderizar Seção Devedores
    function renderDevedores() {
      const fApelido = document.getElementById('filtro-dev-apelido').value;
      const fAno = document.getElementById('filtro-dev-ano').value;
      const fMes = document.getElementById('filtro-dev-mes').value;

      const filtrados = DADOS_DEVEDORES.filter(item => {
        if (fApelido !== 'Todos' && item.apelido !== fApelido) return false;
        if (fAno !== 'Todos' && item.ano != fAno) return false;
        if (fMes !== 'Todos' && item.mesNome !== fMes) return false;
        return true;
      });

      document.getElementById('kpi-devedores-total').textContent = filtrados.length;

      // Obter meses presentes nos dados filtrados
      const mesesPresentes = [...new Set(filtrados.map(d => d.mesNome).filter(m => m !== 'N/D'))];
      mesesPresentes.sort((a, b) => MESES_NOMES.indexOf(a) - MESES_NOMES.indexOf(b));

      // Cabeçalho dinâmico da matriz
      const trHead = document.getElementById('header-matriz-devedores');
      trHead.innerHTML = `<th>Nome Usuário Solicitante</th>`;
      mesesPresentes.forEach(m => {
        trHead.innerHTML += `<th class="col-num">${m}</th>`;
      });
      trHead.innerHTML += `<th class="col-num">Total</th>`;

      // Agrupar por Usuário
      const porUsuario = {};
      filtrados.forEach(item => {
        if (!porUsuario[item.solicitante]) {
          porUsuario[item.solicitante] = { total: 0, meses: {}, itens: [] };
        }
        porUsuario[item.solicitante].total += 1;
        porUsuario[item.solicitante].meses[item.mesNome] = (porUsuario[item.solicitante].meses[item.mesNome] || 0) + 1;
        porUsuario[item.solicitante].itens.push(item);
      });

      // Ordenar por Nome Solicitante alfabético (como no Power BI)
      const usuariosOrdenados = Object.keys(porUsuario).sort();

      const tbody = document.getElementById('body-matriz-devedores');
      tbody.innerHTML = '';

      const totalPorMes = {};
      mesesPresentes.forEach(m => totalPorMes[m] = 0);
      let grandTotal = 0;

      usuariosOrdenados.forEach(user => {
        const uData = porUsuario[user];
        grandTotal += uData.total;
        const isExp = rowsExpandidas.has(user);

        let rowHtml = `<tr>
          <td>
            <span class="expander-btn" onclick="toggleExpand('${user.replace(/'/g, "\'")}')">${isExp ? '−' : '+'}</span>
            <strong>${user}</strong>
          </td>`;

        mesesPresentes.forEach(m => {
          const qtd = uData.meses[m] || '';
          if (qtd) totalPorMes[m] += qtd;
          rowHtml += `<td class="col-num">${qtd}</td>`;
        });

        rowHtml += `<td class="col-num"><strong>${uData.total}</strong></td></tr>`;

        // Detalhes se expandido
        if (isExp) {
          rowHtml += `<tr><td colspan="${mesesPresentes.length + 2}" style="padding: 4px 10px 10px 32px; background: #fafafa;">`;
          uData.itens.forEach(it => {
            rowHtml += `<div class="subrow-content">
              <strong>${it.apelido || it.descricao}</strong> | Protocolo: ${it.protocolo} | Empréstimo: ${it.data_emp} | Devolução prevista: ${it.data_dev} | <span class="badge-status badge-atrasado">${it.dias_atraso} dias de atraso</span>
            </div>`;
          });
          rowHtml += `</td></tr>`;
        }

        tbody.innerHTML += rowHtml;
      });

      // Linha de Total Geral
      let totalRow = `<tr class="row-total">
        <td>Total</td>`;
      mesesPresentes.forEach(m => {
        totalRow += `<td class="col-num">${totalPorMes[m]}</td>`;
      });
      totalRow += `<td class="col-num">${grandTotal}</td></tr>`;
      tbody.innerHTML += totalRow;

      // Ranking Direita (Ordenado por Total decrescente)
      const barList = document.getElementById('bar-list-devedores');
      barList.innerHTML = '';
      const ranking = Object.keys(porUsuario).map(u => ({ nome: u, qtd: porUsuario[u].total }));
      ranking.sort((a, b) => b.qtd - a.qtd);

      const maxQtd = ranking.length > 0 ? Math.max(...ranking.map(r => r.qtd)) : 1;

      ranking.forEach(r => {
        const pct = (r.qtd / maxQtd) * 100;
        barList.innerHTML += `
          <div class="bar-item">
            <div class="bar-label-row">
              <span title="${r.nome}">${r.nome}</span>
            </div>
            <div class="bar-track">
              <div class="bar-fill" style="width: ${pct}%">${r.qtd}</div>
            </div>
          </div>
        `;
      });
    }

    // Renderizar Seção Manutenção
    function renderManutencao() {
      const fAno = document.getElementById('filtro-inv-ano').value;
      const fMes = document.getElementById('filtro-inv-mes').value;
      const fBusca = document.getElementById('filtro-inv-busca').value.toLowerCase().trim();

      const filtrados = DADOS_INVENTARIO.filter(item => {
        if (fAno !== 'Todos' && item.ano != fAno) return false;
        if (fMes !== 'Todos' && item.mesNome !== fMes) return false;
        if (fBusca && !item.apelido.toLowerCase().includes(fBusca)) return false;
        return true;
      });

      document.getElementById('kpi-manutencao-total').textContent = filtrados.length;

      // Agrupar por Apelido, Ano, Mês
      const agrupado = {};
      filtrados.forEach(item => {
        const chave = `${item.apelido}___${item.ano}___${item.mesNome}`;
        if (!agrupado[chave]) {
          agrupado[chave] = {
            apelido: item.apelido,
            ano: item.ano,
            mes: item.mesNome,
            qtd: 0,
            status: item.status || 'Ativo'
          };
        }
        agrupado[chave].qtd += 1;
      });

      // Ordenar alfabeticamente pelo Apelido
      const lista = Object.values(agrupado).sort((a, b) => a.apelido.localeCompare(b.apelido));

      const tbody = document.getElementById('body-manutencao');
      tbody.innerHTML = '';

      let totalGeral = 0;
      lista.forEach(linha => {
        totalGeral += linha.qtd;
        tbody.innerHTML += `<tr>
          <td><strong>${linha.apelido}</strong></td>
          <td class="col-num">${linha.qtd}</td>
          <td>${linha.ano}</td>
          <td>${linha.mes}</td>
          <td><span class="badge-status badge-warn">${linha.status}</span></td>
        </tr>`;
      });

      // Linha de Total
      tbody.innerHTML += `<tr class="row-total">
        <td>Total</td>
        <td class="col-num">${totalGeral}</td>
        <td>-</td>
        <td>-</td>
        <td>-</td>
      </tr>`;
    }

    function resetFiltrosDevedores() {
      document.getElementById('filtro-dev-apelido').value = 'Todos';
      document.getElementById('filtro-dev-ano').value = 'Todos';
      document.getElementById('filtro-dev-mes').value = 'Todos';
      renderDevedores();
    }

    function resetFiltrosManutencao() {
      document.getElementById('filtro-inv-ano').value = 'Todos';
      document.getElementById('filtro-inv-mes').value = 'Todos';
      document.getElementById('filtro-inv-busca').value = '';
      renderManutencao();
    }

    // Inicialização
    popularFiltros();
    // Definir padrões de exibição semelhantes à captura
    const selAnoInv = document.getElementById('filtro-inv-ano');
    if ([...selAnoInv.options].some(o => o.value == '2026')) {
      selAnoInv.value = '2026';
    }
    renderDevedores();
    renderManutencao();
  </script>
</body>
</html>
