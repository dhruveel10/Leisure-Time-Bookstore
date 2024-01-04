<script setup lang="ts">
import { inject } from "vue";
import type { CategoryItem } from "@/types";


import { useCategoryStore } from '@/stores/category';
const categoryStore = useCategoryStore();
const categoryImageFileName = function (category: CategoryItem): string {
  let name = category.name.toLowerCase();
  name = name.replace(/ /g, "-");
  name = name.replace(/'/g, "");
  return `${name}.gif`;
};

const categoryImagePrefix = `${import.meta.env.BASE_URL}/category-images/`
</script>

<style scoped>
ul {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;

  gap: 1em;
}

li {
  text-align: center;
  cursor: pointer;
}

li div {
  margin-bottom: -2em;
  padding: 0.5em 0;
  background: rgba(0, 0, 0, 0.5); /* last # is percent opacity */
  color: white;
  transform: translateY(-2.25em);
}

.category-image {
  width: 140px; /* Adjust the width as needed */
  height: 240px; /* Maintain aspect ratio */
}

</style>

<template>
  <ul>
    <li v-for="category in categoryStore.categoryList" :key="category.categoryId">
      <router-link :to="'../category/' + category.name">
        <img
          :src="categoryImagePrefix + categoryImageFileName(category)"
          :alt="category.name + ' category'"
          class="category-image"
        />
        <div>{{ category.name }}</div>
      </router-link>
    </li>
  </ul>
</template>
