<script setup>
import { ref, computed } from 'vue'

const produtos = ref([
  {
    id: 1,
    nome: 'Camiseta',
    preco: 49.9,
    img: 'https://photos.enjoei.com.br/camiseta-quando-der-tudo-errado-faz-o-l-que-passa-79058276/1200xN/czM6Ly9waG90b3MuZW5qb2VpLmNvbS5ici9wcm9kdWN0cy81MzQwMjk5L2Q4N2U0MmM2YjJjODFhMTI2MDMyYmRkYTY4ODdjZjVlLmpwZw'
  },
  {
    id: 2,
    nome: 'Calça',
    preco: 99.9,
    img: 'https://cf.shopee.com.br/file/72d5e2208fccd35a1ae505e4c647a9b7'
  },
  {
    id: 3,
    nome: 'Meia',
    preco: 9.9,
    img: 'https://cdn.awsli.com.br/2500x2500/1605/1605396/produto/171141184/3d847cc98e.jpg'
  },
  {
    id: 4,
    nome: 'Tenis',
    preco: 199.9,
    img: 'https://i.pinimg.com/736x/7a/6b/11/7a6b110dd0f923c8041290cbcf8dbc05--fake-shoes-jordan-.jpg'
  },
  {
    id: 5,
    nome: 'Boné',
    preco: 29.9,
    img: 'https://static.wixstatic.com/media/a6af46_0cb9589adc1f4268bf718466c7f06bfe.jpg/v1/fill/w_420,h_420,al_c,lg_1,q_80,enc_auto/a6af46_0cb9589adc1f4268bf718466c7f06bfe.jpg'
  },
  {
    id: 6,
    nome: 'Relógio',
    preco: 299.9,
    img: 'https://i.pinimg.com/originals/4d/4a/f1/4d4af145e7a415a04306c3f806790323.png'
  },
  {
    id: 7,
    nome: 'Bermuda',
    preco: 79.9,
    img: 'https://static.ricmais.com.br/uploads/2021/10/shorts-folha-maconha.jpeg'
  },
  {
    id: 8,
    nome: 'Cueca',
    preco: 19.9,
    img: 'https://images.tcdn.com.br/img/img_prod/1148263/cueca_bob_esponja_10023_1_62c1c2ab72f967293b73a1a53049d234.jpg'
  },
  {
    id: 9,
    nome: 'Moletom',
    preco: 9.9,
    img: 'https://http2.mlstatic.com/D_NQ_NP_671212-MLB47138408262_082021-W.jpg'
  }
])
const carrinhos = ref({
  items: [
    {
      id: 1,
      nome: 'Camiseta',
      preco: 49.9,
      quantidade: 1,
      valorTotal: 49.9
    },
    {
      id: 2,
      nome: 'Calça',
      preco: 99.9,
      quantidade: 2,
      valorTotal: 199.8
    },
    {
      id: 3,
      nome: 'Meia',
      preco: 9.9,
      quantidade: 4,
      valorTotal: 39.6
    }
  ]
})

const valorTotal = computed(() => {
  if (carrinhos.value.items.length > 0) {
    return carrinhos.value.items.map((item) => item.valorTotal).reduce((v1, v2) => v1 + v2)
  } else {
    return 0
  }
})

function addCarrinho(produto) {
  const index = carrinhos.value.items.findIndex((x) => x.id === produto.id)
  if (index === -1) {
    carrinhos.value.items.push({
      id: produto.id,
      nome: produto.nome,
      preco: produto.preco,
      quantidade: 1,
      valorTotal: produto.preco
    })
  } else {
    carrinhos.value.items[index].quantidade++
    //carrinhos.value.items[index].preco = produto.preco + produto.preco
    carrinhos.value.items[index].valorTotal =
      carrinhos.value.items[index].valorTotal + produto.preco // -chat gpt ajudou// estudar depois
  }
}
function removeUnidadeItem(index) {
  if (carrinhos.value.items[index].quantidade == 1) {
    carrinhos.value.items.splice(index, 1)
  } else {
    carrinhos.value.items[index].quantidade--
    // carrinhos.value.items[index].valorTotal = carrinhos.value.items[index].valorTotal - produtos.value.items[index].preco
    //Para pegar o valor do produto, você precisa acessar a propriedade "preco" do objeto do produto no array "produtos" usando o índice específico do item no array "carrinhos". Você pode fazer isso usando a propriedade "id" presente nos dois objetos para encontrar o índice correto no array "produtos". Por exemplo:
    const produtoIndex = produtos.value.findIndex((p) => p.id === carrinhos.value.items[index].id)
    carrinhos.value.items[index].valorTotal =
      carrinhos.value.items[index].valorTotal - produtos.value[produtoIndex].preco
    //No código acima, a constante "produtoIndex" encontra o índice correto do produto no array "produtos" usando a propriedade "id" do produto no carrinho. Em seguida, o preço do produto é acessado usando esse índice e subtraído do valor total do item no carrinho.

  }
}
function removeItem(index) {
  carrinhos.value.items.splice(index, 1)
}
const mostrarMenu = ref(false)
function mostrarCarrinhoCompras() {
  mostrarMenu.value = !mostrarMenu.value
}
</script>

<template>
  <transition name="carrinho-transition">
    <div v-if="mostrarMenu" :class="[true ? 'carrinhoComprasSideBarActive' : 'carrinhoComprasSideBarDesactive']"
      class="carrinhoComprasSideBar">
      <h2>CARRINHO DE COMPRAS</h2>
      <ul class="itensCarrinho">
        <li class="itemCarrinho" v-for="(produtoCarrinho, index) in carrinhos.items" :key="produtoCarrinho.id">
          <H3 style="">{{ produtoCarrinho.nome }}</H3>
          <p style="margin-top: -10px;">Preco: {{ produtoCarrinho.valorTotal.toFixed(2) }}</p>
          <p style="margin-top: -10px;">Quantidade: {{ produtoCarrinho.quantidade }}</p>
          <div style="margin-top: -10px;" class="removeDeleteCart"> 
            <fa icon="xmark" class="removeItem" @click="removeItem(index)"></fa>
          <fa  icon="minus" class="removeItem" @click="removeUnidadeItem(index)"></fa>
        </div>
          
        </li>
      </ul>
      <div>
        <p>Total: {{ valorTotal.toFixed(2) }}</p>
        <button class="fecharbutton" @click="mostrarCarrinhoCompras()">FECHAR</button>
      </div>
    </div>
  </transition>

  <header>
    <nav class="header">
      <a class="youtube" href="https://www.youtube.com/watch?v=7poCOMwmEwk&ab_channel=ostentafunksp1&t=040" target="_blank">
      <div><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512"><!--! Font Awesome Pro 6.4.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M549.655 124.083c-6.281-23.65-24.787-42.276-48.284-48.597C458.781 64 288 64 288 64S117.22 64 74.629 75.486c-23.497 6.322-42.003 24.947-48.284 48.597-11.412 42.867-11.412 132.305-11.412 132.305s0 89.438 11.412 132.305c6.281 23.65 24.787 41.5 48.284 47.821C117.22 448 288 448 288 448s170.78 0 213.371-11.486c23.497-6.321 42.003-24.171 48.284-47.821 11.412-42.867 11.412-132.305 11.412-132.305s0-89.438-11.412-132.305zm-317.51 213.508V175.185l142.739 81.205-142.739 81.201z"/></svg> </div></a>
      <h1 class="header-tittle">MontaShop</h1>
      <button @click="mostrarCarrinhoCompras()" class="carrinhovazio">
        <fa icon="cart-shopping"></fa>
      </button>
    </nav>
  </header>
  <div class="container">
    <ul class="listaProdutos">
      <li class="produto" v-for="produto in produtos" :key="produto.id">
        <h2>{{ produto.nome }}</h2>
        <img class="img" :src="produto.img" />
        <div class="carrinhoPreco">
          <p>R${{ produto.preco }}</p>
          <button class="addCarrinho" @click="addCarrinho(produto)">
            <fa icon="cart-plus" style="color: black"></fa>
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
h3,
h2 {
  text-shadow: 2px 2px rgb(243, 225, 255);

}

.listaProdutos {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  column-gap: 200px;
  row-gap: 50px;
  justify-items: center;
  align-items: center;
}

.produto {
  width: 240px;
  height: 300px;
  border: 2px solid rgb(243, 225, 255);
  display: grid;
  justify-items: center;
  border-radius: 20px;
  background-color: white;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.486);
  transition: 0.1s;
  align-content: center;
}

.produto:hover {
  width: 220px;
  height: 275px;
  border: 2px solid rgb(243, 225, 255);
  display: grid;
  justify-items: center;
  border-radius: 20px;
  background-color: white;
  box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.486);
}

.img {
  width: 130px;
  height: 150px;
}

.carrinhoPreco {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  justify-items: center;
}

.addCarrinho {
  display: grid;
  height: 30px;
  width: 30px;
  background-color: white;
  outline: inherit;
  border: 2px solid black;
  border-radius: 5px;
  transition: ease-out 0.1s;
  align-content: center;
  justify-content: center;
}

.addCarrinho:active {
  
  height: 28px;
  width: 28px;
  background-color: white;
  outline: inherit;
  border: 2px solid black;
  border-radius: 8px;
}

.addCarrinho:hover {
  cursor: pointer;

}

.header {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  background-color: white;
  height: 150px;
  border-radius: 40px;
  justify-items: center;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.377);
  align-items: center;
}

.header-tittle {
  font-size: 50px;
  font-weight: 900;
  text-shadow: 2px 2px rgb(243, 225, 255);
}

.carrinhoComprasSideBar {
  position: absolute;
  right: -500px;
  display: grid;
  justify-content: center;
  width: 400px;
  height: 140vh;
  background-color: white;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.623);

}

.carrinhoComprasSideBarActive {
  position: absolute;
  right: 0px;
}

.carrinhoComprasSideBarDesactive {
  position: absolute;
  right: -500px;
}

.carrinho-transition-enter-active,
.carrinho-transition-leave-active {
  transition: all 0.7s ease-out;
}

.carrinho-transition-enter-from,
.carrinho-transition-leave-to {
  transform: translateX(100%);
}

.itensCarrinho {
  display: grid;
  grid-template-columns: 1fr 1fr;
  column-gap: 20px;
  margin: 0;
  padding: 0;
  margin-top: 10px;
  row-gap: 8px;
}
.itemCarrinho{
  display: grid;
  height: 190px;
  width: 135px;
  justify-items: start;
  align-items: center;
  
}
ul li {
  text-decoration-line: none;
  list-style: none;
  margin: 0;
  padding: 0;
}

.carrinhovazio {
  margin-left: 250px;
  width: 40px;
  height: 40px;
  border-radius: 5px;
  background-color: white;
  transition: ease-out 0.2s;
}

.carrinhovazio:hover {
  cursor: pointer;
  border-radius: 15px;
}

.carrinhovazio:active {
  cursor: pointer;
  width: 35px;
  height: 35px;
}

.removeItem {
  width: 20px;
  height: 20px;
  background-color: white;
  border-radius: 4px;
  border: inherit;
  outline: 2px solid black;
  transition: ease-out 0.2s;

}
.removeItem:hover {
  cursor: pointer;
}

.removeItem:active {
  cursor: pointer;
  width: 20px;
  height: 20px;
  border-radius: 6px;
}
.removeDeleteCart{
display: grid;
grid-template-columns:  1fr 1fr;
align-items: center;
justify-items: center;
column-gap: 30px;
}
.fecharbutton{
  background-color: white;
  border-radius: 4px;
  border: inherit;
  outline: 2px solid black;
  cursor: pointer;
  transition: ease-out .2s;
}
.fecharbutton:active{

  border: inherit;
  border-radius: 6px;

}
img{
  border-radius: 5%;
}
.youtube{
  margin-left: -250px;
  width: 45px;
  cursor: pointer;
}
</style>
