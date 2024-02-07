<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: String,
});

const emit = defineEmits(['onClickRemove']);
</script>

<template>
  <article class="cart-item">
    <button @click="() => emit('onClickRemove')" class="btn-reset cart-item__close">
      <svg width="15" height="15" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
        <g id="icons">
          <path id="Vector 4 (Stroke)" fill-rule="evenodd" clip-rule="evenodd"
            d="M2.92899 17.3538C2.73373 17.1585 2.73373 16.842 2.92899 16.6467L16.364 3.21166C16.5593 3.0164 16.8759 3.0164 17.0711 3.21166C17.2664 3.40692 17.2664 3.72351 17.0711 3.91877L3.6361 17.3538C3.44084 17.5491 3.12425 17.5491 2.92899 17.3538Z"
            fill="#05070B" />
          <path id="Vector 5 (Stroke)" fill-rule="evenodd" clip-rule="evenodd"
            d="M2.92892 3.3538C3.12418 3.15854 3.44076 3.15854 3.63602 3.3538L17.0711 16.7888C17.2663 16.9841 17.2663 17.3007 17.0711 17.4959C16.8758 17.6912 16.5592 17.6912 16.3639 17.4959L2.92892 4.0609C2.73366 3.86564 2.73366 3.54906 2.92892 3.3538Z"
            fill="#05070B" />
        </g>
      </svg>
    </button>
    <div class="cart-item__lside">
      <div class="cart-item__image">
        <img :src="imageUrl" :alt="title" />
      </div>
    </div>

    <div class="cart-item__rside">
      <h3 class="cart-item__title">{{ title }}</h3>

      <span class="cart-item__delivery-info">24 hours pre-order</span>

      <span class="cart-item__price">{{ price }} Dh</span>
    </div>
  </article>
</template>

<style scoped>
.cart-item {
  position: relative;
  display: flex;
  gap: 10px;
  border-bottom: 1px solid #f3f3f3;
  padding-bottom: 5px;
}

.cart-item:not(:last-child) {
  margin-bottom: 20px;
}

.cart-item__image {
  width: 100px;
  height: 100%;
}

.cart-item__rside {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.cart-item__title {
  font-size: 18px;
}

.cart-item__delivery-info {
  color: #9DA0A6;
}

.cart-item__price {
  font-size: 16px;
}

.cart-item__close {
  position: absolute;
  top: 0px;
  right: 10px;
}
</style>