<script setup>
import { ref, computed } from 'vue';



const produtos = ref([
  {
    id: 1,
    nome: 'Camiseta',
    preco: 49.90,
    img: 'https://photos.enjoei.com.br/camiseta-quando-der-tudo-errado-faz-o-l-que-passa-79058276/1200xN/czM6Ly9waG90b3MuZW5qb2VpLmNvbS5ici9wcm9kdWN0cy81MzQwMjk5L2Q4N2U0MmM2YjJjODFhMTI2MDMyYmRkYTY4ODdjZjVlLmpwZw'
  },
  {
    id: 2,
    nome: 'Calça',
    preco: 99.90
  },
  {
    id: 3,
    nome: 'Meia',
    preco: 9.90
  },
  {
    id: 4,
    nome: 'Sapato',
    preco: 199.90
  },
  {
    id: 5,
    nome: 'Boné',
    preco: 29.90
  },
  {
    id: 6,
    nome: 'Óculos',
    preco: 99.90
  },
  {
    id: 7,
    nome: 'Relógio',
    preco: 299.90
  },
  {
    id: 8,
    nome: 'Bermuda',
    preco: 79.90
  },
  {
    id: 9,
    nome: 'Cueca',
    preco: 19.90
  },
  {
    id: 10,
    nome: 'Meia',
    preco: 9.90
  }
])
const carrinhos = ref({
  items: [
    {
      id: 1,
      nome: 'Camiseta',
      preco: 49.90,
      quantidade: 1,
      valorTotal: 49.90
    },
    {
      id: 2,
      nome: 'Calça',
      preco: 99.90,
      quantidade: 2,
      valorTotal: 199.80
    },
    {
      id: 3,
      nome: 'Meia',
      preco: 9.90,
      quantidade: 4,
      valorTotal: 39.60
    },
  ],
})

const valorTotal = computed(() => {
  if (carrinhos.value.items.length > 0) {
    return carrinhos.value.items.map(item => item.valorTotal).reduce((v1, v2) => v1 + v2)
  } else {
    return 0
  }
})

function addCarrinho(produto) {

  const index = carrinhos.value.items.findIndex(x => x.id === produto.id)
  if (index === -1) {
    carrinhos.value.items.push({
      id: produto.id,
      nome: produto.nome,
      preco: produto.preco,
      quantidade: 1,
      valorTotal: produto.preco
    })
  }
  else {
    carrinhos.value.items[index].quantidade++
    carrinhos.value.items[index].preco = produto.preco + produto.preco
  }
}
function removeUnidadeItem(index) {
  if (carrinhos.value.items[index].quantidade == 1) {
    carrinhos.value.items.splice(index, 1)
  }
  else {
    carrinhos.value.items[index].quantidade--
  }
}
function removeItem(index) {
  carrinhos.value.items.splice(index, 1)
}
</script>


<template>
  <div>
    <h1>Montashop</h1>
    <ul class="listaProdutos">
      <li class="produto" v-for="produto in produtos" :key="produto.id">
        <p>{{ produto.nome }} </p>
        <img class="img" :src="produto.img"  >
        <div class="carrinhoPreco"><p>valor: R${{ produto.preco }}</p>
        <button class="addCarrinho" @click="addCarrinho(produto)"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 576 512"><!--! Font Awesome Pro 6.4.0 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. --><path d="M0 24C0 10.7 10.7 0 24 0H69.5c22 0 41.5 12.8 50.6 32h411c26.3 0 45.5 25 38.6 50.4l-41 152.3c-8.5 31.4-37 53.3-69.5 53.3H170.7l5.4 28.5c2.2 11.3 12.1 19.5 23.6 19.5H488c13.3 0 24 10.7 24 24s-10.7 24-24 24H199.7c-34.6 0-64.3-24.6-70.7-58.5L77.4 54.5c-.7-3.8-4-6.5-7.9-6.5H24C10.7 48 0 37.3 0 24zM128 464a48 48 0 1 1 96 0 48 48 0 1 1 -96 0zm336-48a48 48 0 1 1 0 96 48 48 0 1 1 0-96z"/></svg> </button> </div>
      </li>
    </ul>
  </div>
  <div>
    <h1>carrinho de compras </h1>
    <ul>
      <li v-for="(produtoCarrinho, index) in carrinhos.items" :key="produtoCarrinho.id">
        <p>{{ produtoCarrinho.nome }}</p>
        <p>{{ produtoCarrinho.valorTotal }}</p>
        <p>{{ produtoCarrinho.quantidade }}</p>
        <button @click="removeItem(index)">remover item </button>
        <button @click="removeUnidadeItem(index)">-</button>
      </li>
    </ul>
    <p>Total: {{ valorTotal }}</p>
  </div>
</template>

<style scoped>
.listaProdutos {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr  ;
  column-gap: 20px;
  row-gap: 50px ;
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
}
.img{
  width: 100px;
}
.carrinhoPreco{
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  justify-items: center;
}
.addCarrinho{
  height: 30px;
  width: 30px;
}
</style>
