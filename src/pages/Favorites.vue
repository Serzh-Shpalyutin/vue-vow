<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';
import CatalogList from '../components/CatalogList.vue';

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get('https://c830cc050abe55c8.mokky.dev/favorites?_relations=items');

    favorites.value = data.map(obj => obj.item);
  } catch {
    console.log(error);
  }
});
</script>

<template>
  <section class="catalog">
    <div class="container">
      <div class="catalog__head">
        <h1 class="title">Favorites</h1>
      </div>

      <CatalogList v-if="favorites.length" :items="favorites" isFavorites />

      <p class="catalog__empty" v-else>No items in favorites</p>
    </div>
  </section>
</template>

<style scoped>
  .catalog__empty {
    font-size: 16px;
    line-height: 110%;
  }
</style>