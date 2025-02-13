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
        <p><strong>상품금액</strong> : {{ totalPrice }}원</p>
        <p><strong>배송비</strong> : {{ shippingFee === 0 ? "무료" : `${shippingFee}원` }}</p>
        <p>(배송비 3,000원 5만원 이상 구매시 무료배송)</p>
        <p><strong>총 결제금액</strong> : {{ payment }}원</p>
        <button @click="handleOpen">주문결제</button>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { computed, ref } from 'vue';
  import OpenModal from './OpenModal.vue';
import { useRouter } from 'vue-router';
  const isOpenModal = ref(false);
  //라우터 인스턴스 생성
  const router = useRouter();
  const handleOpen = () => {
    isOpenModal.value = true;
  }
  const closeModal = (value) => {
    isOpenModal.value = value;
    emit('clear-cart',true);
    router.push('/') //상품 목록 페이지로 이동
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
    flex-direction: row;
    padding: 2rem 5rem;
    gap: 1rem;
    h2,h3{
      margin: 15px;
    }
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
          img{
            width: 150px;
            height: 150px;
            object-fit: cover;
            border: 5px;
            border-radius: 10px;
          }
          button{
            background-color: #ddd;
            outline: none;
            border: none;
            padding: 5px;
            cursor: pointer;
          }
        }
      }
    }
    .cart-add{
      .total{
        border: none;
        padding: 1rem;
        button{
          background-color: red;
          color: #fff;
          cursor: pointer;
          padding: 10px 15px;
          border: none;
          outline: none;
          margin-top: 20px;
        }
      }
    }
  }

  @media screen and (max-width:900px){
    .cart{
      flex-direction: column;
      padding: 1rem;
      .cart-list{
        ul{
          li{
            img{
            width: 80px;
            height: 80px;
            }
          }
        }
      }
    }
  }
</style>