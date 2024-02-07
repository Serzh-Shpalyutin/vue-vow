<script setup>
import { inject } from 'vue';
import CartItem from './CartItem.vue';

const { cartItems, removeFromCart } = inject('cart');
</script> 

<template>
  <div v-if="cartItems.length" class="cart__list">
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

  <div v-else class="">
    <p>Cart is empty</p>
  </div>
</template>

<style scoped></style>