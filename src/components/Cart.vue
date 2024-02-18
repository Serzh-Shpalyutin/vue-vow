<script setup>
import { ref, inject, computed } from 'vue';
import axios from 'axios';
import CartList from './CartList.vue';

const {
  cartItems,
  closeCart,
} = inject('cart');

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + parseFloat(item.price), 0));
const isCreatingOrder = ref(false);
const orderId = ref(null);

const createOrder = async () => {
  try {
    isCreatingOrder.value = true;
    const { data } = await axios.post('https://c830cc050abe55c8.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: totalPrice.value,
    });

    cartItems.value = [];

    orderId.value = data.id;
  } catch (error) {
    console.log(error);
  } finally {
    isCreatingOrder.value = false;
  }
}


const buttonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value);
const cartIsEmpty = computed(() => cartItems.value.length === 0)
</script>

<template>
  <div @click="closeCart" class="overlay"></div>
  <aside class="cart">
    <h2 class="cart__title">BAG</h2>

    <div class="cart__content">
      <div class="cart__inner">
        <CartList />

        <div class="cart__empty" v-if="cartIsEmpty || orderId">
          <p v-if="cartIsEmpty && !orderId">Cart is empty</p>
          <p class="cart__order" v-else-if="orderId">Ваш заказ <span>#{{ orderId }}</span> успешно оформлен и в ближайшее время будет передан в службу доставки</p>
        </div>
      </div>

      <div class="cart__details">
        <div class="cart__total">
          Total amount:
          <span>{{ totalPrice }}</span> Dh
        </div>

        <button @click="createOrder" :disabled="buttonDisabled" class="btn btn-reset btn-checkout">go to checkout</button>
      </div>
    </div>
  </aside>
</template>

<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.3);
  z-index: 1;
  cursor: pointer;
}

.cart {
  overflow: auto;
  position: fixed;
  top: 0;
  right: 0;
  width: 420px;
  height: 100%;
  background-color: #fff;
  z-index: 2;
  padding: 30px;
}

.cart__title {
  font-size: 32px;
  line-height: 115%;
  margin-bottom: 40px;
}

.cart__content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 80vh;
}

.cart__details {
  margin-top: 20px;
}

.cart__total {
  font-size: 18px;
  line-height: 108%;
  margin-bottom: 20px;
}

.btn-checkout[disabled] {
  opacity: 0.3;
}

.cart__order {
  font-size: 18px;
  line-height: 110%;
  text-align: center;
}
.cart__order span {
  font-weight: 700;
}
</style>