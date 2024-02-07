<script setup>
import CartList from './CartList.vue';

const emit = defineEmits(['closeCart']);
</script>

<template>
  <div @click="() => emit('closeCart')" class="overlay"></div>
  <aside class="cart">
    <h2 class="cart__title">BAG</h2>

    <div class="cart__content">
      <CartList />

      <div class="cart__details">
        <div class="cart__total">
          Total amount:
          <span>10 000</span> Dh
        </div>
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
}
.cart__details {
  margin-top: auto;
}
</style>