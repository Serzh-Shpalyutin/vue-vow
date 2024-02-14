<script setup>
import { reactive, watch, inject, ref, onMounted } from 'vue';
import axios from 'axios';
import CatalogList from '../components/CatalogList.vue';

const { addToCart, removeFromCart, cartItems } = inject('cart');

const items = ref([]);

const onClickAddToCart = (item) => {
  if (!item.isAddedToCart) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

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
        item_id: item.id
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
      const favorite = favorites.find((favorite) => favorite.item_id === item.id);

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
  const localCart = localStorage.getItem('cart');
  cartItems.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAddedToCart: cartItems.value.some((cartItem) => cartItem.id === item.id)
  }));
});

watch(cartItems, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAddedToCart: false
  }))
});

watch(filters, fetchItems);
</script>

<template>
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

      <CatalogList :items="items" @addToFavorite="addToFavorite" @addToCart="onClickAddToCart" />
    </div>
  </section>
</template>

<style scoped>
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