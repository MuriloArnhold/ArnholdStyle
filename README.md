
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Arnhold Style</title>
  <style>
    body { margin: 0; font-family: Arial, sans-serif; background-color: #fff; }
    header { background-color: #d32f2f; color: white; padding: 15px; display: flex; align-items: center; justify-content: space-between; }
    header img { height: 40px; }
    nav a { color: white; margin: 0 10px; text-decoration: none; font-weight: bold; }
    nav a:hover { text-decoration: underline; }
    .produtos { display: flex; flex-wrap: wrap; justify-content: center; margin: 20px; }
    .card { border: 1px solid #ccc; border-radius: 8px; margin: 10px; padding: 15px; width: 220px; text-align: center; box-shadow: 2px 2px 6px rgba(0,0,0,0.1); }
    .card img { max-width: 100%; border-radius: 6px; }
    .preco { color: #4CAF50; font-weight: bold; margin: 8px 0; }
    button { background-color: #d32f2f; color: white; border: none; padding: 8px 12px; border-radius: 4px; cursor: pointer; }
    button:hover { background-color: #b71c1c; }
    #carrinho, #checkout { margin: 20px; }
    #carrinho ul { list-style: none; padding: 0; }
    #carrinho li { margin: 5px 0; }
    .quantidade { margin: 0 5px; font-weight: bold; }
    input[type="text"] { width: 100%; padding: 6px; margin: 8px 0; border: 1px solid #ccc; border-radius: 4px; }
    .aba { display: none; }
    .aba.ativa { display: block; }
  </style>
</head>
<body>
  <header>
    <img src=E:\Downloads\Progeto 01\img\HTML\Barato.jpg"" alt="Logo da loja">
    <h1>Arnhold Style</h1>
    <nav>
      <a href="#" onclick="mostrarAba('loja')">Loja</a>
      <a href="#" onclick="mostrarAba('carrinho')">Carrinho</a>
      <a href="#" onclick="mostrarAba('checkout')">Checkout</a>
    </nav>
  </header>

  <!-- Aba Loja -->
  <section id="loja" class="aba ativa">
    <div class="produtos">
      <div class="card">
        <img src="E:\Downloads\Progeto 01\img\HTML\8128yLLVZlL._AC_SX522_ (1).jpg" alt="Camiseta">
        <h3>Camiseta Polo Lacoste</h3>
        <p class="preco">R$ 179,90</p>
        <button onclick="adicionarCarrinho('Camiseta Polo Lacoste', 179.90, '5e9204e3-a89a-4edc-9b81-ab1bb62b9b5c')">Adicionar ao Carrinho</button>
      </div>

      <div class="card">
        <img src="E:\Downloads\Progeto 01\img\HTML\OIP.webp" alt="Tênis">
        <h3>Tênis Olympikus</h3>
        <p class="preco">R$ 299,90</p>
        <button onclick="adicionarCarrinho('Tênis Olympikus', 299.90, '5e9204e3-a89a-4edc-9b81-ab1bb62b9b5c')">Adicionar ao Carrinho</button>
      </div>

      <div class="card">
        <img src="E:\Downloads\Progeto 01\img\HTML\51SbDFGmdYL._AC_SY500_.jpg" alt="Tênis">
        <h3>Tênis OUS</h3>
        <p class="preco">R$ 359,90</p>
        <button onclick="adicionarCarrinho('Tênis OUS', 359.90, '5e9204e3-a89a-4edc-9b81-ab1bb62b9b5c')">Adicionar ao Carrinho</button>
      </div>

      <div class="card">
        <img src="E:\Downloads\Progeto 01\img\HTML\Nike.webp" alt="Tênis">
        <h3>Tênis Nike Air Max</h3>
        <p class="preco">R$ 499,90</p>
        <button onclick="adicionarCarrinho('Tênis Nike Air Max', 499.90, '5e9204e3-a89a-4edc-9b81-ab1bb62b9b5c')">Adicionar ao Carrinho</button>
      </div>

      <div class="card">
        <img src="E:\Downloads\Progeto 01\img\HTML\OIP (1).webp" alt="Camisa">
        <h3>Camisa Basica</h3>
        <p class="preco">R$ 79,90</p>
        <button onclick="adicionarCarrinho('Camisa Basica', 79.90, '5e9204e3-a89a-4edc-9b81-ab1bb62b9b5c')">Adicionar ao Carrinho</button>
      </div>
    </div>
  </section>

  <!-- Aba Carrinho -->
  <section id="carrinho" class="aba">
    <h2>Seu Carrinho</h2>
    <ul id="listaCarrinho"></ul>
    <p id="subtotal">Subtotal: R$ 0,00</p>
    <input type="text" id="cep" placeholder="Digite seu CEP" oninput="atualizarCarrinho()">
    <p id="frete">Frete: R$ 0,00</p>
    <p id="total">Total: R$ 0,00</p>
    <button onclick="abrirCheckout()">Finalizar Compra</button>
  </section>

  <!-- Aba Checkout -->
  <section id="checkout" class="aba">
    <h2>Finalização da Compra</h2>
    <p>Confira os itens do seu carrinho e copie a chave PIX para pagamento:</p>
    <div id="pix-box"></div>
    <button onclick="mostrarAba('carrinho')">Voltar ao Carrinho</button>
  </section>

  <script>
    let carrinho = [];

    function mostrarAba(nome) {
      document.querySelectorAll('.aba').forEach(aba => aba.classList.remove('ativa'));
      document.getElementById(nome).classList.add('ativa');
    }

    function adicionarCarrinho(nome, preco, chavePix) {
      let item = carrinho.find(p => p.nome === nome);
      if (item) {
        item.quantidade++;
      } else {
        carrinho.push({nome, preco, chavePix, quantidade: 1});
      }
      atualizarCarrinho();
      mostrarAba('carrinho');
    }

    function removerItem(nome) {
      let index = carrinho.findIndex(p => p.nome === nome);
      if (index !== -1) {
        carrinho[index].quantidade--;
        if (carrinho[index].quantidade <= 0) {
          carrinho.splice(index, 1);
        }
      }
      atualizarCarrinho();
    }

    async function calcularFreteAPI(subtotal, cep) {
      if (subtotal >= 15) return 0;
      if (!cep) return 0;
      let primeiroDigito = cep.charAt(0);
      if (["9"].includes(primeiroDigito)) return 10;
      if (["7","8"].includes(primeiroDigito)) return 20;
      return 30;
    }

    async function atualizarCarrinho() {
      const lista = document.getElementById('listaCarrinho');
      lista.innerHTML = '';
      let subtotal = 0;
      carrinho.forEach(item => {
        lista.innerHTML += `
          <li>
            ${item.nome} - R$ ${item.preco.toFixed(2)} 
            <span class="quantidade">x${item.quantidade}</span>
            <button onclick="removerItem('${item.nome}')">-</button>
          </li>`;
        subtotal += item.preco * item.quantidade;
      });

      let cep = document.getElementById('cep').value;
      let frete = await calcularFreteAPI(subtotal, cep);
      let total = subtotal + frete;

      document.getElementById('subtotal').innerText = "Subtotal: R$ " + subtotal.toFixed(2);
      document.getElementById('frete').innerText = "Frete: R$ " + frete.toFixed(2);
      document.getElementById('total').innerText = "Total: R$ " + total.toFixed(2);
    }

    function abrirCheckout() {
      // Gera chave PIX simulada
      let chavePix = "pix-" + Math.random().toString(36).substring(2, 15);
      let mensagem = "<p><strong>Chave PIX:</strong></p><p style='color:#d32f2f'>" + chavePix + "</p>";
      mensagem += "<button onclick=\"navigator.clipboard.writeText('" + chavePix + "')\">Copiar Chave</button>";
      document.getElementById('pix-box').innerHTML = mensagem;
      mostrarAba('checkout');
    }
  </script>
</body>
</html>>
