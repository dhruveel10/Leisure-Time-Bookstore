<script setup lang="ts">
  import type { BookItem } from "@/types";
  import TheCategoryNav from "@/components/TheCategoryNav.vue";
  import TheCategoryBookList from "@/components/TheCategoryBookList.vue";
  import { useRoute } from "vue-router";
  import { useBookStore } from "@/stores/book";
import { watch } from "vue";

  const bookStore = useBookStore();
  const route = useRoute();

  watch(
    () => route.params.name, 
    (newCategoryName) => {
      bookStore.fetchBooks(newCategoryName as string)
    },
    {immediate: true}
  );

</script>


<style scoped>
  .category-page{
    background: url('/site-images/background.jpg') no-repeat center center fixed;
    background-size: cover;
    background-color: #f0f0f0;
  }

</style>



<template>
  <div class="category-page">
    <the-category-nav></the-category-nav>
    <the-category-book-list></the-category-book-list>
  </div>
</template>

