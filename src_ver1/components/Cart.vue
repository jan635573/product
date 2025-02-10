<template>
  <div class="cart">
    <OpenModal :isOpen="isOpenModal" @closeModal="closeModal"/>
    <div class="cart-list">
      <h2>장바구니</h2>
      <h3 v-if="cart.length === 0">장바구니가 비었습니다.</h3>
      <ul v-else>
        <li v-for="list in cart" :key="list.id">
          <img :src="list.image" :alt="list.name"/>
          <p>{{ list.name }}</p>
          <p>가격 : {{ list.price }}원</p>
          <p>수량 : {{ list.count }}</p>
          <button @click="removeCart(list.id)">삭제</button>
        </li>
      </ul>
    </div>
    <div class="cart-add">
      <h2>주문내역</h2>
      <div class="total">
        <p>상품금액 : {{ totalPrice }}원</p>
        <p>배송비 : {{ shippingFee === 0 ? "무료" : `${shippingFee}원` }}</p>
        <p>(배송비 3,000원 5만원 이상 구매시 무료배송)</p>
        <p>총 결제금액 : {{ payment }}원</p>
        <button @click="handleOpen">주문결제</button>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { computed, ref } from 'vue';
  import OpenModal from './OpenModal.vue';
  const isOpenModal = ref(false);
  const handleOpen = () => {
    isOpenModal.value = true;
  }
  const closeModal = (value) => {
    isOpenModal.value = value;
    emit('clear-cart',true);
  }

  const props = defineProps({
    cart: Array
  });
  const emit = defineEmits(['remove-cart-id', 'clear-cart']);
  const removeCart = (cartID) => {
    emit('remove-cart-id', cartID);
  }
  const totalPrice = computed(()=>{
    // let sum = 0;
    // props.cart.forEach((item)=>{
    //   //가격 * 수량
    //   sum += item.price * item.count;
    // });
    // return sum;

    //배열 객체 사용
    let result = props.cart.reduce((sum,item)=>{
      return sum += item.price * item.count
    },0);
    // console.log(result);
    return result;
  });
  const shippingFee = computed(()=>{
    return totalPrice.value >= 50000 ? 0 : 3000;
  });
  const payment = computed(()=>{
    return totalPrice.value + shippingFee.value;
  });
</script>

<style lang="scss" scoped>
  .cart{
    display: flex;
    padding: 2rem 5rem;
    gap: 1rem;
    div{
      flex: 1;
      border: 1px solid #222;
      border-radius: 5px;
    }
    .cart-list{
      ul{
        li{
          display: flex;
          gap: 1rem;
          align-items: center;
          padding: 1rem;
        }
      }
    }
    .cart-add{
      .total{
        border: none;
        padding: 1rem;
      }
    }
  }
</style>