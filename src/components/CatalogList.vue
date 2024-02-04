<script setup>
import ProductCard from './ProductCard.vue';

defineProps({
  items: Array
});

const emit = defineEmits(['addToFavorite']);
</script>

<template>
  <div class="catalog__grid">
    <ProductCard 
      v-for="item in items" 
      :key="item.id" 
      :id="item.id"  
      :title="item.title" 
      :imageUrl="item.image" 
      :price="item.price"
      :onClickFavorite="() => emit('addToFavorite', item)"
      :isFavorite="item.isFavorite"
    />
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