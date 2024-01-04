<script setup lang="ts">
import { defineProps } from "vue";
import type { BookItem } from "@/types";
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import {faBookOpenReader} from "@fortawesome/free-solid-svg-icons";
import { useCartStore } from "@/stores/cart";

const cartStore = useCartStore();

const props = defineProps<{
  book: BookItem;
}>();
const bookImageFileName = function (book: BookItem): string {
  let name = book.title.toLowerCase();
  name = name.replace(/ /g, "-");
  name = name.replace(/'/g, "");
  return `${name}.gif`;
};

const bookImagePrefix = `${import.meta.env.BASE_URL}/book-images/`;


</script>

<style scoped>
.book-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  margin: -10 auto;
  margin-left: 75px;
}

.book-box {
  background-color: #CBBDBD;
  gap: 0.25em;
  box-sizing: border-box;
  width: 240px;
  margin-right: 1px;
  margin-bottom: 1em;
  justify-content: center;
  flex: 1 1 200px; /* allow items to grow & shrink as needed */
  max-width: 240px;
  text-align: center;
  padding-top: 10px;
}

.readme-button {
  position: absolute;
  top: 140px; /* Adjust the top position as needed */
  left: 65px; /* Adjust the left position as needed */
  z-index: 1; /* Ensure the button appears above the image */
  background: lightgray;
  padding: 5px 5px;
  border: none;
  cursor: pointer;
  font-size: 14px;
}

.book-image {
  display: flex; /* Add relative positioning to the container */
  align-items: center;
  justify-content: center;
}

.image-container {
  position: relative;
}


/* Clear the margin on every third book to start a new row */
.book-box:nth-child(3n) {
  margin-right: 0;
}

.book-title {
  font-weight: bold;
}

.book-author {
  font-style: italic;
}

button{
  background: #B45B5B;
  border-style: none;
  margin-bottom: 10px;
}

</style>

<template>
  <div class="book-container">
    <li class="book-box">
      <div class="book-image">
        <div class="image-container">
          <button class="readme-button">
            <span>
              <font-awesome-icon :icon="faBookOpenReader" :style="{ color: 'black', fontSize:'32px'}"/>
            </span>
          </button>
          <img
              :src=" bookImagePrefix + bookImageFileName(props.book)"
              :alt="book.title"
          />
        </div>
      </div>
      <div class="book-title" style="font-size: 15px">{{ book.title }}</div>
      <div class="book-author" style="font-size: 15px">{{ book.author }}</div>
      <div class="book-price" style="font-size: 15px">${{ (book.price / 100).toFixed(2) }}</div>
      <button class="button" style="font-size: 15px" @click="cartStore.addToCart(book)">Add to Cart</button>
    </li>
  </div>
</template>
