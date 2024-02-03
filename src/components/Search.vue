<template>
  <div class="search">
    <label class="search__label">
      <input class="search__input" type="search" placeholder="Search...">
    </label>
  </div>
</template>

<style scoped>
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