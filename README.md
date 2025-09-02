<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Calculadora CLT - Horas Extras</title>
  <style>
    *{box-sizing:border-box;margin:0;padding:0;font-family:'Inter',Arial,sans-serif}
    :root{
      --bg:#0e141b;
      --card:#1a222d;
      --muted:#9aa4b2;
      --accent:#6c5ce7;
      --accent-2:#00cec9;
      --success:#27c38a;
      --danger:#ff7675;
    }
    body{
      background:linear-gradient(135deg, #0e141b 0%, #131a24 100%);
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
      animation:fadeIn 0.8s ease;
    }
    @keyframes fadeIn{from{opacity:0;transform:translateY(20px)}to{opacity:1;transform:translateY(0)}}
    .card{
      background:var(--card);
      border-radius:16px;
      padding:24px;
      box-shadow:0 8px 24px rgba(0,0,0,0.45);
      transition:transform .2s ease;
    }
    .card:hover{transform:translateY(-2px)}
    h1{font-size:1.6rem;margin-bottom:8px;color:#fff}
    p{color:var(--muted);font-size:0.95rem;margin-bottom:20px}
    .form-row{display:flex;gap:20px;margin-bottom:16px;flex-wrap:wrap}
    .field{flex:1;display:flex;flex-direction:column;min-width:200px}
    label{font-size:0.85rem;color:var(--muted);margin-bottom:6px}
    input[type="number"]{
      width:100%;
      padding:12px;
      border:none;
      border-radius:10px;
      font-size:1rem;
      outline:none;
      background: rgba(255,255,255,0.12);
      color:#fff;
      transition:all .2s ease;
    }
    input[type="number"]:focus{
      background: rgba(255,255,255,0.18);
      box-shadow:0 0 0 2px var(--accent);
    }
    .minutes{display:flex;gap:10px}
    .actions{display:flex;gap:12px;margin-top:12px;flex-wrap:wrap}
    .btn{padding:12px 18px;border-radius:10px;border:0;cursor:pointer;font-weight:600;transition:opacity .2s}
    .btn:hover{opacity:0.9}
    .btn-primary{background:linear-gradient(90deg,var(--accent),var(--accent-2));color:#fff}
    .btn-ghost{background:#222c38;border:1px solid #2f3b49;color:var(--muted)}
    .result{margin-top:20px;display:flex;flex-direction:column;gap:10px}
    .row{display:flex;justify-content:space-between;align-items:center;padding:10px 0;border-bottom:1px solid #2a3542}
    .row:last-child{border-bottom:0}
    .big{font-size:1.2rem;font-weight:700;color:#fff}
    footer{margin-top:16px;color:var(--muted);font-size:0.85rem;line-height:1.4}
    .highlight{color:var(--accent-2);font-weight:600}
    @media(max-width:600px){
      h1{font-size:1.3rem}
      .field{min-width:100%}
    }
  </style>
</head>
<body>
  <main class="container">
    <section class="card">
      <h1>💼 Calculadora de Horas Extras (CLT)</h1>
      <p>Insira o salário bruto, a carga horária mensal e as horas extras realizadas. A calculadora aplica os adicionais de <span class="highlight">50%</span> e <span class="highlight">100%</span> conforme previsto na CLT.</p>
      <form id="calcForm" onsubmit="return false">
        <div class="form-row">
          <div class="field">
            <label for="salary">Salário mensal (R$)</label>
            <input id="salary" type="number" min="0" step="0.01" placeholder="" value="">
          </div>
          <div class="field">
            <label for="monthlyHours">Horas mensais</label>
            <input id="monthlyHours" type="number" min="1" step="1" value="220">
          </div>
        </div>
        <div class="form-row">
          <div class="field">
            <label>Horas Extras 50%</label>
            <div class="minutes">
              <input id="h50" type="number" placeholder="Horas" min="0" value="">
              <input id="m50" type="number" placeholder="Minutos" min="0" max="59" value="">
            </div>
          </div>
          <div class="field">
            <label>Horas Extras 100%</label>
            <div class="minutes">
              <input id="h100" type="number" placeholder="Horas" min="0" value="">
              <input id="m100" type="number" placeholder="Minutos" min="0" max="59" value="">
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
          ⚖️ <strong>Atenção:</strong> Esta calculadora segue a CLT para cálculo básico de horas extras. Não substitui cálculos de <em>DP/Contabilidade</em> em casos de adicional noturno, banco de horas, insalubridade ou descontos legais.
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
