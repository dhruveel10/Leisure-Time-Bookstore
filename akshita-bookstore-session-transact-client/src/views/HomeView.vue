<script setup lang="ts">
import TheHomeCategoryList from "@/components/TheHomeCategoryList.vue";
const categoryClassic = "Classics"
</script>

<style scoped>
 .home-page {
   display: flex;
   flex-direction: column;
   background: url('/site-images/background.jpg') no-repeat center center fixed;
   background-size: cover;
   background-color: #f0f0f0;
   margin: 0;
   padding: 0;
   align-items: center;
   justify-content: center;
 }

.welcome-text {
  text-align: center;
  margin: 20px;
  display: flex; /* Add this to make children display in a row */
  flex-direction: column; /* Stack children vertically */
  align-items: center;
  font-family: "Times New Roman", Times, serif;
}

.text-container {
  text-align: center;
}

.start-shopping-button {
  margin-top: 10px;
  padding: 10px;
  font-size: 48px;
  background: #B45B5B;
  color: black;
  cursor: pointer; /* Set the cursor style to pointer */
}

 body {
   font-family: "Times New Roman", Times, serif !important;
 }

</style>

<template>
  <div class="home-page">
    <section class="welcome-text">
      <div class="text-container">
        <p style="font-size: 36px">LIMITED TIME ONLY</p>
        <p style="font-size: 48px">20% OFF BOOKS</p>
          <router-link :to="'../category/Classics'">
            <button class="start-shopping-button">START SHOPPING!</button>
          </router-link>
      </div>
    </section>
    <section class="category-images container">
      <the-home-category-list></the-home-category-list>
    </section>
  </div>
</template>
