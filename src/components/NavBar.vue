<template>
  <nav>
    <h1>맛있게먹자</h1>
    <div class="router-link">
      <router-link to="/">상품목록</router-link>
      <router-link to="/cart">장바구니</router-link>
    </div>
    <ul>
      <li v-for="(list,idx) in categories" :key="idx" @click="selectedClick(list)">
        {{ list }}
      </li>
    </ul>
  </nav>
</template>

<script setup>
import { useRouter } from 'vue-router';

  defineProps({
    categories: Array
  });
  const router = useRouter();
  const emit = defineEmits(['selected']);
  const selectedClick = (list) => {
    emit('selected', list);
    router.push('/');
  }
</script>

<style lang="scss" scoped>
  nav{
    display: flex;
    justify-content: space-between;
    padding: 2rem 5rem;
    .router-link{
      display: flex;
      // gap: 1rem;
      font-size: 1.2rem;
      font-weight: bolder;
      a{
        text-decoration: none;
        color: #222;
        padding: 5px 10px;
        &:hover{
          background-color: red;
          color: #fff;
        }
      }
    }
    ul{
      display: flex;
      gap: 3rem;
      li{
        cursor: pointer;
        &:hover{
          font-weight: bolder;
          color: red;
        }
      }
    }
  }
  @media screen and (max-width:900px){
    nav{
      // width: 100%;
      flex-direction: column;
      gap: 1rem;
      margin: 0 auto;
      padding: 20px;
      .router-link{
        a{
          // padding: 0;
          text-align: center;
          margin: 0 auto;
        }
      }
      ul{
        justify-content: center;
        align-items: center;
      }
    }
  }
</style>