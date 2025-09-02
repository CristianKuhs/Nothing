<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Calculadora de Horas Extras (CLT) — Dark</title>
  <style>
    *{box-sizing:border-box;margin:0;padding:0}
    :root{
      --bg:#0b0f14; /* very dark */
      --card:#0f1720; /* card background */
      --muted:#9aa4b2;
      --accent:#7c5cff;
      --accent-2:#00d4ff;
      --glass:rgba(255,255,255,0.04);
      --glass-2:rgba(124,92,255,0.06);
      --success:#27c38a;
      --danger:#ff5c7c;
      --glass-border:rgba(255,255,255,0.04);
    }
    body{
      font-family:Inter, ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
      background: radial-gradient(1200px 600px at 10% 10%, rgba(124,92,255,0.06), transparent 8%),
                  radial-gradient(800px 400px at 90% 90%, rgba(0,212,255,0.03), transparent 6%),
                  var(--bg);
      color:#e6eef6;
      min-height:100vh;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:28px;
    }
    .container{
      width:100%;
      max-width:980px;
      display:grid;
      grid-template-columns:1fr 360px;
      gap:24px;
      align-items:start;
    }
    .card{
      background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:14px;
      padding:22px;
      box-shadow: 0 6px 20px rgba(2,6,23,0.6), inset 0 1px 0 rgba(255,255,255,0.02);
      border:1px solid var(--glass-border);
    }
    header h1{font-size:1.15rem;margin-bottom:6px}
    header p{color:var(--muted);font-size:0.88rem}
    .form-row{display:flex;gap:12px;margin-top:14px}
    .field{flex:1;display:flex;flex-direction:column}
    label{font-size:0.78rem;color:var(--muted);margin-bottom:6px}
    input[type="number"], input[type="text"], select{
      background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.005));
      border:1px solid rgba(255,255,255,0.03);
      padding:10px 12px;border-radius:10px;color:inherit;font-size:0.98rem;
      outline:none;transition:box-shadow .18s, border-color .12s;
    }
    input[type="number"]:focus, input[type="text"]:focus, select:focus{box-shadow:0 6px 20px rgba(124,92,255,0.08);border-color:var(--accent)}
    .small{font-size:0.82rem;color:var(--muted)}
    .minutes{
      display:flex;gap:8px;align-items:center
    }
    .minutes input{width:100px}
    .actions{display:flex;gap:10px;margin-top:16px}
    .btn{padding:10px 14px;border-radius:10px;border:0;cursor:pointer;font-weight:600}
    .btn-primary{background:linear-gradient(90deg,var(--accent),var(--accent-2));color:#081322}
    .btn-ghost{background:transparent;border:1px solid rgba(255,255,255,0.04);color:var(--muted)}
    .result{
      display:flex;flex-direction:column;gap:10px;margin-top:12px;padding:14px;border-radius:10px;background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent);
      border:1px solid rgba(255,255,255,0.02)
    }
    .row{display:flex;justify-content:space-between;align-items:center}
    .big{font-size:1.06rem;font-weight:700}
    .muted{color:var(--muted)}
    .summary{position:sticky;top:28px}
    .kpi{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
    .kpi .item{padding:10px;border-radius:8px;background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent);border:1px solid rgba(255,255,255,0.02)}
    .item .label{font-size:0.78rem;color:var(--muted)}
    .item .value{font-weight:700;margin-top:6px}
    .help{font-size:0.8rem;color:var(--muted);margin-top:10px}
    footer{margin-top:14px;color:var(--muted);font-size:0.82rem}
    .footer-actions{display:flex;gap:10px;justify-content:flex-end;margin-top:12px}
    @media (max-width:980px){
      .container{grid-template-columns:1fr}
      .summary{position:static}
    }
    .divider{height:1px;background:linear-gradient(90deg,transparent, rgba(255,255,255,0.03), transparent);margin:10px 0;border-radius:2px}
    .copy-ok{display:inline-block;padding:6px 8px;border-radius:999px;background:rgba(39,195,138,0.12);color:var(--success);font-weight:700;margin-left:8px}
  </style>
</head>
<body>
  <main class="container" role="main">
    <section class="card">
      <header>
        <h1>Calculadora de Horas Extras (CLT)</h1>
        <p>Calcule horas extras com base na CLT — jornada padrão 44h/sem (220h/mês). Insira horas e minutos separadamente.</p>
      </header>
      <form id="calcForm" onsubmit="return false" aria-label="Formulário de cálculo de horas extras">
        <div class="form-row">
          <div class="field">
            <label for="salary">Salário mensal (R$)</label>
            <input id="salary" type="number" min="0" step="0.01" value="2300" aria-describedby="salaryHelp">
            <div id="salaryHelp" class="small">Digite o salário bruto. Padrão 220h/mês para jornada de 44h.</div>
          </div>
          <div class="field">
            <label for="monthlyHours">Horas mensais (padrão 220)</label>
            <input id="monthlyHours" type="number" min="1" step="1" value="220">
          </div>
        </div>
        <div style="margin-top:12px;font-weight:700">Horas Extras</div>
        <div class="form-row" style="margin-top:8px">
          <div class="field">
            <label>50% (adicional 50%)</label>
            <div class="minutes">
              <input id="h50" type="number" placeholder="Horas" min="0" step="1" value="8">
              <input id="m50" type="number" placeholder="Minutos" min="0" max="59" step="1" value="41">
            </div>
          </div>
          <div class="field">
            <label>100% (adicional 100%)</label>
            <div class="minutes">
              <input id="h100" type="number" placeholder="Horas" min="0" step="1" value="4">
              <input id="m100" type="number" placeholder="Minutos" min="0" max="59" step="1" value="22">
            </div>
          </div>
        </div>
        <div class="actions">
          <button id="calcBtn" class="btn btn-primary" type="button">Calcular</button>
          <button id="clearBtn" class="btn btn-ghost" type="button">Limpar</button>
          <button id="exportBtn" class="btn btn-ghost" type="button">Exportar CSV</button>
        </div>
        <div class="result" id="resultBox" aria-live="polite">
          <div class="row"><div class="muted">Valor da hora</div><div id="hourValue" class="big">R$ 0,00</div></div>
          <div class="row"><div class="muted">Hora extra 50%</div><div id="hour50">R$ 0,00</div></div>
          <div class="row"><div class="muted">Hora extra 100%</div><div id="hour100">R$ 0,00</div></div>
          <div class="divider"></div>
          <div class="row"><div class="muted">Total 50%</div><div id="total50">R$ 0,00</div></div>
          <div class="row"><div class="muted">Total 100%</div><div id="total100">R$ 0,00</div></div>
          <div class="divider"></div>
          <div class="row"><div class="muted">Total Horas Extras</div><div id="totalExtras" class="big">R$ 0,00</div></div>
          <div class="row"><div class="muted">Total (Salário + Extras)</div><div id="grossTotal" class="big">R$ 0,00</div></div>
        </div>
        <footer>
          <div class="small">Observação: esta calculadora aplica as regras básicas da CLT (adicionais de 50% e 100% sobre a hora normal). Não substitui cálculo oficial do Departamento Pessoal/Contábil — para casos com adicional noturno, banco de horas, taxa de insalubridade/periculosidade ou descontos, consulte um profissional.</div>
        </footer>
      </form>
    </section>
    <aside class="card summary" aria-label="Resumo">
      <h3 style="margin-bottom:8px">Resumo rápido</h3>
      <div class="kpi">
        <div class="item"><div class="label">Horas 50%</div><div id="kpi50" class="value">0h 0m</div></div>
        <div class="item"><div class="label">Horas 100%</div><div id="kpi100" class="value">0h 0m</div></div>
        <div class="item"><div class="label">Valor/Hora</div><div id="kpiHour" class="value">R$ 0,00</div></div>
        <div class="item"><div class="label">Extra Mensal</div><div id="kpiTotal" class="value">R$ 0,00</div></div>
      </div>
      <div class="help">Use o botão <strong>Exportar CSV</strong> para salvar um resumo. Suporta entrada por teclado (tab / enter).</div>
      <div class="footer-actions">
        <button id="copyBtn" class="btn btn-ghost" type="button">Copiar resumo</button>
      </div>
    </aside>
  </main>
  <script>
    const el = id => document.getElementById(id);
    const fmt = num => num.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' });
    function minutesToDecimal(h, m){
      h = Number(h) || 0; m = Number(m) || 0; return h + (m/60);
    }
    function decimalToHrs(minDec){
      const h = Math.floor(minDec);
      const m = Math.round((minDec - h) * 60);
      return {h,m};
    }
    function calculate(){
      const salary = Number(el('salary').value) || 0;
      const monthlyHours = Number(el('monthlyHours').value) || 220;
      const hourVal = monthlyHours > 0 ? (salary / monthlyHours) : 0;
      const h50 = Number(el('h50').value) || 0; const m50 = Number(el('m50').value) || 0;
      const h100 = Number(el('h100').value) || 0; const m100 = Number(el('m100').value) || 0;
      const dec50 = minutesToDecimal(h50, m50);
      const dec100 = minutesToDecimal(h100, m100);
      const hour50 = hourVal * 1.5;
      const hour100 = hourVal * 2.0;
      const total50 = dec50 * hour50;
      const total100 = dec100 * hour100;
      const totalExtras = total50 + total100;
      const grossTotal = salary + totalExtras;
      el('hourValue').textContent = fmt(hourVal);
      el('hour50').textContent = fmt(hour50);
      el('hour100').textContent = fmt(hour100);
      el('total50').textContent = fmt(total50);
      el('total100').textContent = fmt(total100);
      el('totalExtras').textContent = fmt(totalExtras);
      el('grossTotal').textContent = fmt(grossTotal);
      const kpi50El = el('kpi50');
      const kpi100El = el('kpi100');
      const kpiHourEl = el('kpiHour');
      const kpiTotalEl = el('kpiTotal');
      kpi50El.textContent = `${Math.floor(dec50)}h ${Math.round((dec50-Math.floor(dec50))*60)}m`;
      kpi100El.textContent = `${Math.floor(dec100)}h ${Math.round((dec100-Math.floor(dec100))*60)}m`;
      kpiHourEl.textContent = fmt(hourVal);
      kpiTotalEl.textContent = fmt(totalExtras);
      el('resultBox').setAttribute('data-updated', Date.now());
      return {
        salary, monthlyHours, hourVal, dec50, dec100, hour50, hour100, total50, total100, totalExtras, grossTotal
      };
    }
    document.addEventListener('DOMContentLoaded', ()=>{
      el('calcBtn').addEventListener('click', ()=>{
        calculate();
      });
      el('clearBtn').addEventListener('click', ()=>{
        el('salary').value = '';
        el('h50').value = 0; el('m50').value = 0;
        el('h100').value = 0; el('m100').value = 0;
        el('monthlyHours').value = 220;
        calculate();
      });
      el('exportBtn').addEventListener('click', ()=>{
        const res = calculate();
        const rows = [
          ['Salario','Horas/mês','Valor hora'],
          [res.salary, res.monthlyHours, res.hourVal],
          [],
          ['Horas 50% (h)','Horas 50% (dec)','Valor hora 50%','Total 50%'],
          [el('h50').value + ':' + el('m50').value, res.dec50, res.hour50, res.total50],
          [],
          ['Horas 100% (h)','Horas 100% (dec)','Valor hora 100%','Total 100%'],
          [el('h100').value + ':' + el('m100').value, res.dec100, res.hour100, res.total100],
          [],
          ['Total Extras','Total Geral'],
          [res.totalExtras, res.grossTotal]
        ];
        const csv = rows.map(r => r.map(c => '"'+(c===undefined? '': String(c))+'"').join(',')).join('\n');
        const blob = new Blob([csv], {type: 'text/csv;charset=utf-8;'});
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url; a.download = 'horas_extras_resumo.csv'; document.body.appendChild(a); a.click(); a.remove();
      });
      el('copyBtn').addEventListener('click', ()=>{
        const res = calculate();
        const text = `Salário: R$ ${res.salary}\nValor/Hora: R$ ${res.hourVal.toFixed(2)}\nTotal Extras: R$ ${res.totalExtras.toFixed(2)}\nTotal Geral: R$ ${res.grossTotal.toFixed(2)}`;
        navigator.clipboard.writeText(text).then(()=>{
          const elCopy = document.createElement('span'); elCopy.className='copy-ok'; elCopy.textContent='Copiado';
          document.querySelector('.footer-actions').appendChild(elCopy);
          setTimeout(()=>elCopy.remove(),1600);
        });
      });
      ['salary','monthlyHours','h50','m50','h100','m100'].forEach(id=>{
        el(id).addEventListener('keydown', (ev)=>{
          if(ev.key === 'Enter') { ev.preventDefault(); calculate(); }
        });
      });
      calculate();
    });
  </script>
</body>
</html>
