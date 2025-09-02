<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Calculadora CLT - Horas Extras e Salário</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: radial-gradient(circle at top left, #1a1a1a, #0d0d0d);
      color: #f5f5f5;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }
    .container {
      width: 100%;
      max-width: 550px;
      background: rgba(255, 255, 255, 0.06);
      backdrop-filter: blur(14px);
      padding: 30px;
      border-radius: 20px;
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.5);
      animation: fadeIn 1s ease;
    }
    h1 {
      text-align: center;
      margin-bottom: 25px;
      font-size: 1.8rem;
      color: #00ffcc;
      letter-spacing: 1px;
    }
    .field {
      margin-bottom: 18px;
    }
    label {
      display: block;
      font-weight: 600;
      margin-bottom: 6px;
      color: #ddd;
    }
    input {
      width: 100%;
      padding: 10px;
      border: none;
      border-radius: 10px;
      font-size: 1rem;
      outline: none;
      background: rgba(255, 255, 255, 0.12);
      color: #fff;
      transition: all 0.2s ease;
    }
    input:focus {
      background: rgba(255, 255, 255, 0.18);
      box-shadow: 0 0 0 2px #00ffcc;
    }
    button {
      width: 100%;
      padding: 14px;
      font-size: 1rem;
      font-weight: 600;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      background: linear-gradient(135deg, #00ffcc, #0077ff);
      color: #0d0d0d;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
      margin-top: 10px;
    }
    button:hover {
      transform: scale(1.03);
      box-shadow: 0 4px 14px rgba(0, 255, 200, 0.5);
    }
    .result {
      margin-top: 22px;
      padding: 18px;
      border-radius: 15px;
      background: rgba(255, 255, 255, 0.08);
      line-height: 1.6;
      font-size: 1rem;
    }
    .highlight {
      color: #00ffcc;
      font-weight: bold;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Calculadora CLT</h1>
    <div class="field">
      <label for="salario">Salário Base (R$):</label>
      <input type="number" id="salario" placeholder="Ex: 2300" min="0">
    </div>
    <div class="field">
      <label for="antecipacao">Antecipação (R$):</label>
      <input type="number" id="antecipacao" placeholder="Ex: 1000" min="0">
    </div>
    <div class="field">
      <label for="horas50">Horas Extras 50% (hh:mm):</label>
      <input type="text" id="horas50" placeholder="Ex: 08:41">
    </div>
    <div class="field">
      <label for="horas100">Horas Extras 100% (hh:mm):</label>
      <input type="text" id="horas100" placeholder="Ex: 04:22">
    </div>
    <button onclick="calcular()">Calcular</button>
    <div class="result" id="resultado"></div>
  </div>

  <script>
    function parseHorasMinutos(hm) {
      if (!hm || !hm.includes(":")) return 0;
      let [h, m] = hm.split(":").map(Number);
      return h + (m / 60);
    }

    function calcular() {
      const salario = parseFloat(document.getElementById('salario').value) || 0;
      const antecipacao = parseFloat(document.getElementById('antecipacao').value) || 0;
      const horas50 = parseHorasMinutos(document.getElementById('horas50').value);
      const horas100 = parseHorasMinutos(document.getElementById('horas100').value);

      if (salario <= 0) {
        document.getElementById('resultado').innerHTML = "<span class='highlight'>Informe um salário válido.</span>";
        return;
      }

      // valor hora pela CLT (220h)
      const valorHora = salario / 220;

      // valores horas extras
      const valorHora50 = valorHora * 1.5;
      const valorHora100 = valorHora * 2;

      const total50 = horas50 * valorHora50;
      const total100 = horas100 * valorHora100;

      const totalExtras = total50 + total100;
      const liquido = (salario - antecipacao) + totalExtras;

      document.getElementById('resultado').innerHTML = `
        Valor hora normal: <span class="highlight">R$ ${valorHora.toFixed(2)}</span><br>
        Hora extra 50%: <span class="highlight">R$ ${valorHora50.toFixed(2)}</span><br>
        Hora extra 100%: <span class="highlight">R$ ${valorHora100.toFixed(2)}</span><br><br>
        Total horas 50%: <span class="highlight">R$ ${total50.toFixed(2)}</span><br>
        Total horas 100%: <span class="highlight">R$ ${total100.toFixed(2)}</span><br>
        <hr>
        Total horas extras: <span class="highlight">R$ ${totalExtras.toFixed(2)}</span><br>
        Salário - Antecipação: <span class="highlight">R$ ${(salario - antecipacao).toFixed(2)}</span><br>
        <strong>Valor líquido no 5º dia útil: <span class="highlight">R$ ${liquido.toFixed(2)}</span></strong>
      `;
    }
  </script>
</body>
</html>
