# atividade-igor
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Carrinho React</title>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css"
    integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">

</head>

<body>
  <div class="container">
    <h1>Carrinho React</h1>
    <hr>
    <div id="app" class="row">
      <div class="col-sm-8">
        <div class="row loja">
          <!-- <div class="col-sm-4 mb-3">
            <div class="card">
              <div class="card loja__item">
                <img class="card-img-top" src="https://lorempixel.com/500/300" alt="">
                <div class="card-body">
                  <h5 class="card-title">JSRaiz para FW</h5>
                  <small>R$300,00</small>
                  <p class="card-text">O melhor curso do mundo</p>
                  <button data-value="300" class="btn btn-primary">Adicionar</button>
                </div>
              </div>
            </div>
          </div>col-sm-4 -->
        </div>
      </div>
      <div class="col-sm-4">
        <div class="carrinho">
          <div class="carrinho__itens">
            <!-- <div class="card carrinho__item">
              <div class="card-body">
                <h5 class="card-title">JSRaiz para FW</h5>
                <p class="card-text">Preço unidade: R$300,00 | Quantidade: 2</p>
                <p class="card-text">Valor: R$600</p>
                <button class="btn btn-danger btn-sm">Remover</button>
              </div>
            </div> -->
          </div>
          <div class="carrinho__total mt-2 p-3">
            <!-- <h6>Total: <strong>R$600,00</strong></h6> -->
          </div>
        </div>
      </div>
    </div>
  </div>
  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
  
  <script type="text/babel">
  const produtosLista = [
    {
      id: 'abc123',
      nome: 'JSRaiz para FW',
      preco: 300,
      descricao: 'O melhor curso do mundo',
      imagem: 'https://lorempixel.com/500/300'
    },
    {
      id: 'bbc123',
      nome: 'JSRaiz para Node',
      preco: 1200,
      descricao: 'O melhor curso e todos',
      imagem: 'https://lorempixel.com/500/300'
    },
    {
      id: 'cbc123',
      nome: 'Programação funcional com JS',
      preco: 500,
      descricao: 'O melhor funcional de todos',
      imagem: 'https://lorempixel.com/500/300'
    }
  ];

  function ProdutoComponent(props) {
    return (
      <div className="col-sm-4 mb-3">
        <div className="card loja__item">
          <img className="card-img-top" src={props.item.imagem} />
          <div className="card-body">
            <h5 className="card-title">{props.item.nome}</h5>
            <small>R${props.item.preco}</small>
            <p className="card-text">{props.item.descricao}</p>
            <button className="btn btn-primary" onClick={props.onAddCarrinho.bind(null, props.item)}>Adicionar</button>
          </div>
        </div>
      </div>
    )
  }

  function ListaProdutosComponent(props) { 
    return (
      <div className="row loja">
        {props.children}  
      </div>
    )
  }

  function valorTotal(carrinhoItens) {
    return Object.keys(carrinhoItens).reduce(function(acc, produtoId) {
      return acc + (carrinhoItens[produtoId].preco * carrinhoItens[produtoId].quantidade)
    }, 0);
  }

  function CarrinhoComponent(props) {
    return (
      <div className="carrinho">
        <div className="carrinho__itens">
          {Object.keys(props.itens).map(function(produtoId, index) {
            return (
              <div className="card carrinho__item" key={`item-carrinho-${index}`}>
                <div className="card-body">
                  <h5 className="card=title">{props.itens[produtoId].nome}</h5>
                  <p className="card-text">Preço unidade: R${props.itens[produtoId].preco} | Quantidade: {props.itens[produtoId].quantidade}</p>
                  <p className="card-text">Valor: R${props.itens[produtoId].preco * props.itens[produtoId].quantidade}</p>
                  <button onClick={props.onRemoveItemCarrinho.bind(null, produtoId)} className="btn btn-danger btn-sm">Remover</button>
                </div>
              </div>
            )
          })}
        </div>
        <div className="carrinho__total mt-2 p-3">
          <h6>Total: <strong>R${valorTotal(props.itens)}</strong></h6>
        </div>
      </div> 
    )
  }

  function AppComponente() {
    const [carrinhoItens, addItemCarrinho] = React.useState({});

    function addCarrinho(produto) {
      if (!carrinhoItens[produto.id]) {
        addItemCarrinho({
          ...carrinhoItens,
          [produto.id]: {
            ...produto,
            quantidade: 1
          }
        })
      } else {
        addItemCarrinho({
          ...carrinhoItens,
          [produto.id]: {
            ...produto,
            quantidade: ++carrinhoItens[produto.id].quantidade
          }
        })
      }
    }

    function removeItemCarrinho(produtoId) {
      if (carrinhoItens[produtoId].quantidade <= 1) {
        delete carrinhoItens[produtoId];
        addItemCarrinho({ ...carrinhoItens });
      } else {
        addItemCarrinho({
          ...carrinhoItens,
          [produtoId]: {
            ...carrinhoItens[produtoId],
            quantidade: --carrinhoItens[produtoId].quantidade
          }
        })
      }
    }

    return (
      <React.Fragment>
        <div className="col-sm-8">
          <ListaProdutosComponent>
            {produtosLista.map((produto, index) => (
              <ProdutoComponent
                item={produto}
                onAddCarrinho={addCarrinho}
                key={`produto-${index}`}
              />
            ))}
          </ListaProdutosComponent>
        </div>
        <div className="col-sm-4">
          <CarrinhoComponent itens={carrinhoItens} onRemoveItemCarrinho={removeItemCarrinho} />
        </div>
      </React.Fragment>
    )
  }

  ReactDOM.render(
    React.createElement(AppComponente),
    document.getElementById('app')
  )
  </script>
</body>

</html>
