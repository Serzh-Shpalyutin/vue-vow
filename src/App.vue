<script setup>
import { onMounted, ref, watch, reactive, provide } from 'vue';
import axios from 'axios';

import Header from './components/Header.vue';
import CatalogList from './components/CatalogList.vue';
import Cart from './components/Cart.vue';

const items = ref([]);

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
});

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }

      item.isFavorite = true;
      const { data } = await axios.post('https://c830cc050abe55c8.mokky.dev/favorites', obj);
      item.favoriteId = data.id;
      
    } else {
      item.isFavorite = false;
      await axios.delete(`https://c830cc050abe55c8.mokky.dev/favorites/${item.favoriteId}`);
      item.favoriteId = null;
    }

  } catch (error) {
    console.log(error);
  }
};

const fetchFavorites = async () => {
  try {

    const { data: favorites } = await axios.get('https://c830cc050abe55c8.mokky.dev/favorites');

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id);

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })

  } catch (error) {
    console.log(error);
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(`https://c830cc050abe55c8.mokky.dev/items`, {
      params
    });

    items.value = data.map((obj) => ({
      ...obj,
      favoriteId: null,
      isFavorite: false,
      isAddedToCart: false
    }))

  } catch (error) {
    console.log(error);
  }
}

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});
watch(filters, fetchItems);
</script>

<template>
  <Header />
  <!-- <Cart /> -->
  <main class="main">
    <section class="catalog">
      <div class="container">
        <div class="catalog__head">
          <h1 class="title">Products</h1>

          <div class="sort">
            <select @change="onChangeSelect" class="sort__select" name="sort" id="">
              <option value="name">by name</option>
              <option value="price">By price (cheaper)</option>
              <option value="-price">By price (expensive)</option>
            </select>
          </div>


          <div class="search">
            <label class="search__label">
              <input @input="onChangeSearchInput" class="search__input" type="search" placeholder="Search...">
            </label>
          </div>
        </div>

        <CatalogList :items="items" @addToFavorite="addToFavorite" />
      </div>
    </section>
  </main>
</template>

<style scoped>
.title {
  font-size: 56px;
  line-height: 60px;
}

.catalog {
  padding: 120px 0px;
}

.catalog__head {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  margin-bottom: 40px;
}

.sort__select {
  padding: 10px;
  border: none;
  cursor: pointer;
}

.search__label {
  display: flex;
  align-items: center;
}

.search span {
  font-size: 18px;
  margin-right: 15px;
}

.search__input {
  width: 250px;
  border: none;
  border-bottom: 1px solid #000;
  padding: 10px;
}
</style>
