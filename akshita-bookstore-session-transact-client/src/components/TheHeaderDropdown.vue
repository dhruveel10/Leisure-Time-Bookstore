<script setup lang="ts">
  import { apiUrl } from '@/api';

  import { library } from '@fortawesome/fontawesome-svg-core';
  import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
  import { faAngleDown } from '@fortawesome/free-solid-svg-icons'
  library.add(faAngleDown);

  import { useCategoryStore } from '@/stores/category';
  const categoryStore = useCategoryStore();
</script>


<style scoped>
.header-dropdown {
  position: relative;
}

.categories-button {
  margin-top: 0.25em;
  background-color: #D9D9D9;
  color: #111111;
  border-radius: 10px;
  border-style: none;
  transition: background-color 1s; /* Add transition for button background color */
}

.header-dropdown ul {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.3s; /* Add transition for max-height */
  background-color: white;
  color: black;
  position: absolute; /* Keep the ul absolutely positioned */
  top: 100%; /* Position it just below the button */
  left: 0;
  right: 0;
  z-index: 1;
  display: block;
}

.header-dropdown li {
  padding: 0.25em 0.5em;
}

.header-dropdown a {
  color: black;
  text-decoration: none;
}

.header-dropdown li:hover {
  background: lightgray;
}

.header-dropdown:hover ul {
  max-height: 200px; /* Adjust the max-height as per your content */
  cursor: pointer;
}
</style>



<template>
  <div class="header-dropdown">
    <button class="button categories-button">
      Categories
      <font-awesome-icon :icon="['fas', 'angle-down']" />
    </button>
    <ul>
      <li v-for="category in categoryStore.categoryList" :key="category.categoryId">
        <router-link :to="'/category/' + category.name">
          {{ category.name }}
        </router-link>
      </li>
    </ul>
  </div>
</template>
