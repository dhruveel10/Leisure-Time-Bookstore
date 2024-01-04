<script setup lang="ts">
import { RouterView, useRoute } from "vue-router";
import { useCategoryStore } from "./stores/category";
import TheHeader from "@/components/TheHeader.vue";
import TheFooter from "@/components/TheFooter.vue";

const categoryStore = useCategoryStore();
categoryStore.fetchCategories();

const route = useRoute();
</script>

<template>
  <suspense>
    <div id="app">
      <the-header></the-header>
      <router-view :key="route.fullPath"></router-view>
      <the-footer></the-footer>
    </div>
  </suspense>
</template>
