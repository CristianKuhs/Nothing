<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Calculadora de Horas Extras (CLT)</title>
  <style>
    *{box-sizing:border-box;margin:0;padding:0}
    :root{
      --bg:#0b0f14;
      --card:#141b24;
      --muted:#9aa4b2;
      --accent:#7c5cff;
      --accent-2:#00d4ff;
      --success:#27c38a;
    }
    body{
      font-family:Inter, Arial, sans-serif;
      background:var(--bg);
      color:#e6eef6;
      min-height:100vh;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:20px;
    }
    .container{
      width:100%;
      max-width:900px;
      display:flex;
      flex-direction:column;
      gap:20px;
    }
    .card{
      background:var(--card);
      border-radius:12px;
      padding:20px;
      box-shadow:0 6px 16px rgba(0,0,0,0.4);
    }
    h1{font-size:1.4rem;margin-bottom:6px}
    p{color:var(--muted);font-size:0.9rem;margin-bottom:12px}
    .form-row{display:flex;gap:16px;margin-bottom:16px;flex-wrap:wrap}
    .field{flex:1;display:flex;flex-direction:column;min-width:180px}
    label{font-size:0.8rem;color:var(--muted);margin-bottom:4px}
    input[type="number"]{
      background:#1c252f;
      border:1px solid #2a3542;
      padding:10px;
      border-radius:8px;
      color:#fff;
      font-size:1rem;
    }
    input[type="number"]:focus{outline:1px solid var(--accent)}
    .minutes{display:flex;gap:8px}
    .actions{display:flex;gap:10px;margin-top:10px;flex-wrap:wrap}
    .btn{padding:10px 14px;border-radius:8px;border:0;cursor:pointer;font-weight:600}
    .btn-primary{background:linear-gradient(90deg,var(--accent),var(--accent-2));color:#081322}
    .btn-ghost{background:#1c252f;border:1px solid #2a3542;color:var(--muted)}
    .result{margin-top:16px;display:flex;flex-direction:column;gap:8px}
    .row{display:flex;justify-content:space-between;align-items:center;padding:6px 0;border-bottom:1px solid #2a3542}
    .row:last-child{border-bottom:0}
    .big{font-size:1.1rem;font-weight:700}
    footer{margin-top:12px;color:var(--muted);font-size:0.8rem}
  </style>
</head>
<body>
  <main class="container">
    <section class="card">
      <h1>Calculadora de Horas Extras (CLT)</h1>
      <p>Calcule horas extras com base na CLT — jornada padrão 44h/sem (220h/mês).</p>
      <form id="calcForm" onsubmit="return false">
        <div class="form-row">
          <div class="field">
            <label for="salary">Salário mensal (R$)</label>
            <input id="salary" type="number" min="0" step="0.01" value="">
          </div>
          <div class="field">
            <label for="monthlyHours">Horas mensais</label>
            <input id="monthlyHours" type="number" min="1" step="1" value="220">
          </div>
        </div>
        <div class="form-row">
          <div class="field">
            <label>50% (horas e minutos)</label>
            <div class="minutes">
              <input id="h50" type="number" placeholder="Horas" min="0" step="1" value="">
              <input id="m50" type="number" placeholder="Minutos" min="0" max="59" step="1" value="">
            </div>
          </div>
          <div class="field">
            <label>100% (horas e minutos)</label>
            <div class="minutes">
              <input id="h100" type="number" placeholder="Horas" min="0" step="1" value="">
              <input id="m100" type="number" placeholder="Minutos" min="0" max="59" step="1" value="">
            </div>
          </div>
        </div>
        <div class="actions">
          <button id="calcBtn" class="btn btn-primary" type="button">Calcular</button>
          <button id="clearBtn" class="btn btn-ghost" type="button">Limpar</button>
        </div>
        <div class="result" id="resultBox">
          <div class="row"><div>Valor da hora</div><div id="hourValue" class="big">R$ 0,00</div></div>
          <div class="row"><div>Hora extra 50%</div><div id="hour50">R$ 0,00</div></div>
          <div class="row"><div>Hora extra 100%</div><div id="hour100">R$ 0,00</div></div>
          <div class="row"><div>Total 50%</div><div id="total50">R$ 0,00</div></div>
          <div class="row"><div>Total 100%</div><div id="total100">R$ 0,00</div></div>
          <div class="row"><div>Total Extras</div><div id="totalExtras" class="big">R$ 0,00</div></div>
          <div class="row"><div>Total Geral</div><div id="grossTotal" class="big">R$ 0,00</div></div>
        </div>
        <footer>
          Esta calculadora aplica regras básicas da CLT (adicionais de 50% e 100% sobre a hora normal). Para casos com adicional noturno, banco de horas, insalubridade ou descontos, consulte o DP.
        </footer>
      </form>
    </section>
  </main>
  <script>
    const el = id => document.getElementById(id);
    const fmt = num => num.toLocaleString('pt-BR',{style:'currency',currency:'BRL'});
    function minutesToDecimal(h,m){return (Number(h)||0)+(Number(m)||0)/60;}
    function calculate(){
      const salary=Number(el('salary').value)||0;
      const monthly=Number(el('monthlyHours').value)||220;
      const hourVal= monthly? salary/monthly:0;
      const dec50=minutesToDecimal(el('h50').value,el('m50').value);
      const dec100=minutesToDecimal(el('h100').value,el('m100').value);
      const hour50=hourVal*1.5;
      const hour100=hourVal*2;
      const total50=dec50*hour50;
      const total100=dec100*hour100;
      const extras=total50+total100;
      const gross=salary+extras;
      el('hourValue').textContent=fmt(hourVal);
      el('hour50').textContent=fmt(hour50);
      el('hour100').textContent=fmt(hour100);
      el('total50').textContent=fmt(total50);
      el('total100').textContent=fmt(total100);
      el('totalExtras').textContent=fmt(extras);
      el('grossTotal').textContent=fmt(gross);
    }
    document.getElementById('calcBtn').addEventListener('click',calculate);
    document.getElementById('clearBtn').addEventListener('click',()=>{
      ['salary','h50','m50','h100','m100'].forEach(id=>el(id).value='');
      el('monthlyHours').value=220;
      calculate();
    });
  </script>
</body>
</html>
