<script setup lang="ts">
import { useCategoryStore } from '@/stores/category';
const categoryStore = useCategoryStore();
</script>

<style scoped>
.category-buttons {
  display: flex;
  flex-direction: row;
  background-color: lightgrey;
  text-align: center;
  justify-content: center;
}

.button.category-button.router-link-active {
  background-color: darkgrey;
}

.button.category-button {
  background-color: lightgrey;
}

.button.category-button:hover,
.button.category-button:active {
  background-color: darkgrey;
}
</style>

<template>
  <nav class="category-nav" style="font-size: 15px; justify-content: center;">
    <ul class="category-buttons">
      <li v-for="category in categoryStore.categoryList" :key="category.categoryId">
        <router-link
          :to="'/category/' + category.name"
          class="button category-button"
          style="color: black;"
        >
          {{ category.name }}
        </router-link>
      </li>
    </ul>
  </nav>
</template>
