<script setup>
import CartList from './CartList.vue';

const emit = defineEmits(['closeCart', 'createOrder']);

const props = defineProps({
  totalPrice: Number,
  cartButtonDisabled: Boolean
})
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
          <span>{{ totalPrice }}</span> Dh
        </div>

        <button @click="() => emit('createOrder')" :disabled="cartButtonDisabled" class="btn btn-reset btn-checkout">go to checkout</button>
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
</style>