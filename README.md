<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Quiz naPorta</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      padding: 24px 16px;
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      color: #333;
    }
    .quiz {
      max-width: 600px;
      margin: 30px auto;
      padding: 28px 20px;
      background: white;
      border: 1px solid #ddd;
      border-radius: 12px;
      text-align: center;
    }
    h1 { font-size: 24px; line-height: 1.4; }
    .option {
      display: block;
      width: 100%;
      padding: 16px;
      margin: 12px 0;
      border: 1px solid #ddd;
      border-radius: 7px;
      background: #f4f4f5;
      color: #222;
      font-size: 16px;
      text-align: left;
      cursor: pointer;
    }
    .option:hover { background: #e9e9eb; }
    .option:focus-visible, .action:focus-visible {
      outline: 3px solid #2563eb;
      outline-offset: 3px;
    }
    .action {
      width: 100%;
      padding: 15px;
      margin-top: 12px;
      border: 0;
      border-radius: 7px;
      background: #222;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }
    #resultado {
      margin-top: 22px;
      padding: 18px;
      border-radius: 8px;
      line-height: 1.5;
    }
    .certo { background: #e8f6ec; color: #216b37; }
    .errado { background: #fff0e8; color: #873d19; }
    [hidden] { display: none !important; }
  </style>
</head>
<body>
  <main class="quiz">
    <p><strong>QUIZ NAPORTA</strong></p>
    <h1>Qual foi o maior pico em volume/dia da naPorta?</h1>
    <p>Escolha uma alternativa:</p>

    <div id="opcoes">
      <button class="option" data-resposta="a">a) 45 mil pacotes</button>
      <button class="option" data-resposta="b">b) 50 mil pacotes</button>
      <button class="option" data-resposta="c">c) 60 mil pacotes</button>
    </div>

    <button id="conferir" class="action">Conferir resposta</button>

    <section id="resultado" aria-live="polite" hidden>
      <h2 id="titulo"></h2>
      <p>A resposta correta é <strong>50 mil pacotes por dia!</strong></p>
      <button id="tentar" class="action">Tentar novamente</button>
    </section>
  </main>

  <script>
    const params = new URLSearchParams(window.location.search);
    let escolha = params.get("resposta");

    const opcoes = document.querySelectorAll(".option");
    const resultado = document.getElementById("resultado");
    const titulo = document.getElementById("titulo");

    function selecionar(valor) {
      escolha = valor;
      opcoes.forEach(botao => {
        botao.setAttribute(
          "aria-pressed",
          String(botao.dataset.resposta === escolha)
        );
        botao.style.borderColor =
          botao.dataset.resposta === escolha ? "#2563eb" : "#ddd";
      });
    }

    opcoes.forEach(botao => {
      botao.addEventListener("click", () => {
        selecionar(botao.dataset.resposta);
      });
    });

    if (["a", "b", "c"].includes(escolha)) {
      selecionar(escolha);
    } else {
      escolha = null;
    }

    document.getElementById("conferir").addEventListener("click", () => {
      if (!escolha) {
        alert("Escolha uma alternativa antes de conferir.");
        return;
      }

      const acertou = escolha === "b";
      titulo.textContent = acertou
        ? "Parabéns! Você acertou!"
        : "Não foi dessa vez!";

      resultado.className = acertou ? "certo" : "errado";
      resultado.hidden = false;
      resultado.scrollIntoView({ behavior: "smooth", block: "nearest" });
    });

    document.getElementById("tentar").addEventListener("click", () => {
      escolha = null;
      selecionar("");
      resultado.hidden = true;
      opcoes.forEach(botao => {
        botao.style.borderColor = "#ddd";
        botao.setAttribute("aria-pressed", "false");
      });
    });
  </script>
</body>
</html>
