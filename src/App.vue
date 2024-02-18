<script setup>
import { ref, watch, provide, computed } from 'vue';

import Header from './components/Header.vue';
import Cart from './components/Cart.vue';

/* Корзина (START) */
const cartItems = ref([]);

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
  <Cart v-if="cartState" :cartState="cartState" />
  <main class="main">
    <router-view></router-view>
  </main>
</template>
