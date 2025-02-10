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
    ul{
      display: flex;
      gap: 3rem;
      li{
        cursor: pointer;
        &:hover{
          font-weight: bolder;
          color: blue;
        }
      }
    }
  }
</style>