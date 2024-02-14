<script setup>
import { ref, watch, provide, computed } from 'vue';
import axios from 'axios';

import Header from './components/Header.vue';
import Cart from './components/Cart.vue';

/* Корзина (START) */
const cartItems = ref([]);
const isCreatingOrder = ref(false);

const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + parseFloat(item.price), 0));
const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value);
const cartIsEmpty = computed(() => cartItems.value.length === 0)

const cartState = ref(false);

const cartItemsCount = computed(() => cartItems.value.length);

const openCart = () => {
  cartState.value = true;
}

const closeCart = () => {
  cartState.value = false;
}

const addToCart = (item) => {
  item.isAddedToCart = true;
  cartItems.value.push(item);
}
const removeFromCart = (item) => {
  cartItems.value.splice(cartItems.value.indexOf(item), 1);
  item.isAddedToCart = false;
  console.log(cartItems.value);
}

const createOrder = async () => {
  try {
    isCreatingOrder.value = true;
    const { data } = await axios.post('https://c830cc050abe55c8.mokky.dev/orders', {
      items: cartItems.value,
      totalPrice: totalPrice.value,
    });

    cartItems.value = [];
    cartState.value = false;

    return data;
  } catch (error) {
    console.log(error);
  } finally {
    isCreatingOrder.value = false;
  }
}

watch(cartItems, () => {
  localStorage.setItem('cart', JSON.stringify(cartItems.value));
},
{ deep: true });

provide('cart', {
  cartItems,
  openCart,
  closeCart,
  addToCart,
  removeFromCart
});
/* Корзина (END) */
</script>

<template>
  <Header @openCart="openCart" :cartItemsCount="cartItemsCount" />
  <Cart v-if="cartState" @closeCart="closeCart" :totalPrice="totalPrice" @createOrder="createOrder"
    :cartButtonDisabled="cartButtonDisabled" />
  <main class="main">
    <router-view></router-view>
  </main>
</template>
