<template>
  <div class="product-list">
    <div class="card" v-for="list in products" :key="list.id">
      <img :src="list.image" :alt="list.name"/>
      <p>{{ list.name }}</p>
      <p>가격 : {{ list.price }}원</p>
      <button @click="addToList(list)">장바구니 추가</button>
    </div>
  </div>
</template>

<script setup>
  defineProps({
    products: Array
  });
  const emit = defineEmits(['add-to-cart']);
  const addToList = (list) => {
    emit('add-to-cart', list);
  }
</script>

<style lang="scss" scoped>
  .product-list{
    display: flex;
    flex-wrap: wrap;
    gap: 2rem;
    justify-content: center;
    margin-top: 80px;
    .card{
      width: 200px;
      text-align: center;
      img{
        width: 100%;
        height: 200px;
        object-fit: cover;
        border: 5px;
        border-radius: 10px;
      }
      p{
        &:first-of-type{
          font-weight: bolder;
        }
      }
      button{
        background-color: gold;
        outline: none;
        border: none;
        padding: 10px;
        // color: #fff;
        margin-top: 10px;
        cursor: pointer;
        &:hover{
          background-color: red;
          color: #fff;
        }
      }
    }
  }
  @media screen and (max-width:900px){
    .product-list{
      margin-top: 40px;
    }
  }
</style>