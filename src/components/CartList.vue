<script setup>
import { inject } from 'vue';
import CartItem from './CartItem.vue';

const { cartItems, removeFromCart } = inject('cart');
</script> 

<template>
  <div v-auto-animate class="cart__list">
    <CartItem 
    v-for="item in cartItems" 
    :key="item.id" :id="item.id" 
    :title="item.title" 
    :imageUrl="item.image"
    :price="item.price" 
    :isAddedToCart="item.isAddedToCart"
    @onClickRemove="() => removeFromCart(item)"
    />
  </div>
</template>

<style scoped></style>