<script setup>
import Header from './components/Header.vue';
import Search from './components/Search.vue';
import CatalogList from './components/CatalogList.vue';
import Cart from './components/Cart.vue';
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
            <select class="sort__select" name="sort" id="">
              <option value="">По названию</option>
              <option value="">По цене (дешевле)</option>
              <option value="">По цене (дорогие)</option>
            </select>
          </div>

          <Search />
        </div>

        <CatalogList />
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
</style>
