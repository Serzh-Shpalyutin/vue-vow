<script setup>
import ProductCard from './ProductCard.vue';

const onClickAddToCart = () => {
  
}
</script>

<template>
  <div class="catalog__grid">
    <ProductCard imageUrl="../../public/product1.jpg" title="Golden Sunset" :price="590" onClickAddToCart="onClickAddToCart"/>
  </div>
</template>

<style scoped>
.catalog__grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

@media (max-width: 1280px) {
  .catalog__grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 768px) {
  .catalog__grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 560px) {
  .catalog__grid {
    grid-template-columns: repeat(1, 1fr);
  }
}
</style>