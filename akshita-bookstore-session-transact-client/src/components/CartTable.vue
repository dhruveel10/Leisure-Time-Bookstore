<script setup lang="ts">
import type { BookItem } from "@/types";
import { useCartStore } from "@/stores/cart";
import { useRouter } from "vue-router";
import {asDollarsAndCents} from "@/utils"
import { library } from '@fortawesome/fontawesome-svg-core';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faPlusCircle, faMinusCircle } from '@fortawesome/free-solid-svg-icons';

library.add(faPlusCircle, faMinusCircle);
const cartStore = useCartStore();
const router = useRouter();

const bookImageFileName = function (book: BookItem): string {
  let name = book.title.toLowerCase();
  name = name.replace(/ /g, "-");
  name = name.replace(/'/g, "");
  return `${name}.gif`;
};

const bookImagePrefix = `${import.meta.env.BASE_URL}/book-images/`;

const updateCart = function (book: BookItem, quantity: number) {
  cartStore.updateBookQuantity(book, quantity);
};

const clearCart = function(){
  cartStore.clearCart();
};

const calculateSubtotal = function() {
  let subtotal = 0;
  for (const item of cartStore.cart.items) {
    subtotal += item.book.price * item.quantity;
  }
  return asDollarsAndCents(subtotal);
};

const calculateTotalQuantity = function() {
  let totalQuantity = 0;
  for (const item of cartStore.cart.items) {
    totalQuantity += item.quantity;
  }
  return totalQuantity;
};

</script>

<style scoped>

.main-div{
   background-color: #dcd1c3;
}

.cart-table {
  display: grid;
  grid-template-columns: max-content minmax(10em, 20em) repeat(3, max-content);
  row-gap: 1em;
  width: fit-content;
  margin: 0 auto;
  background-color: #e0dad3;
}

ul {
  display: contents;
}

ul > li {
  display: contents;
}

.table-heading {
  background-color: #333;
  color: white;
}

.table-heading > * {
  background-color: #333;
  padding: 0.5em;
}

.heading-book {
  grid-column: 1 / 3;
}

.heading-price {
  grid-column: 3 / 5;
  text-align: right;
}

.heading-quantity{
  text-align: right;
}

.heading-subtotal {
  text-align: right;
  grid-column: -2 / -1;
}

.cart-book-image {
  padding: 0 1em;
}

.cart-book-image > * {
  margin-left: auto;
  margin-right: 0;
}

img {
  display: block;
  width: 80px;
  height: auto;
}

.rect {
  background-color: var(--primary-color-dark);
}

.narrow-rect {
  width: 75px;
  height: 100px;
}

.square {
  width: 100px;
  height: 100px;
}

.wide-rect {
  width: 125px;
  height: 100px;
}

.cart-book-price {
  padding-left: 1em;
  text-align: right;
  margin-left: 1em;
}

.cart-book-quantity {
  padding-left: 1em;
  padding-right: 1em;
  text-align: right;
}

.cart-book-subtotal {
  text-align: right;
  padding-left: 1em;
  padding-right: 1em;
}

/* Row separators in the table */

.line-sep {
  display: block;
  height: 2px;
  background-color: gray;
  grid-column: 1 / -1;
}

/* Increment/decrement buttons */

.icon-button {
  border: none;
  cursor: pointer;
  color: none;
  background-color: none;
}

.inc-button {
  font-size: 1.125rem;
  margin-right: 0.25em;
  color: none;
  background-color: none;
}

.inc-button:hover {
  color: none;
}

.dec-button {
  font-size: 1.125rem;
  color: none;
}

.dec-button:hover {
  color: none;
}

input[type="number"] {
  width: 4em;
}

select {
  background-color: var(--primary-color);
  color: white;
  border: 2px solid var(--primary-color-dark);
  border-radius: 3px;
}

.clear-cart-button {
  background-color: red;
  color: white;
  border: none;
  padding: 0.5em 1em;
  position: relative;
  margin-bottom: 2%;
  margin-left: 80%;
  font-size: 0.85rem;
}

.cart-summary {
  padding: 10px; /* Adjust the padding to control the spacing inside the div */
  text-align: right; /* Align text to the right within the div */
  margin-top: 10px; /* Add some margin at the top for spacing */
}

/* Style for the individual paragraphs (optional) */
.cart-summary p {
  margin: 0; /* Remove default margin from the paragraphs */
}

.continue-shopping-button {
  background-color: white; /* Use your color variable here */
  color: #B45B5B;
  border: none;
  padding: 0.5em 0.75em;
  font-size: 1.2rem;
  margin: 1em; 
  cursor: pointer;
}

/* Proceed to Checkout Button */
.proceed-to-checkout-button {
  background-color: #B45B5B; /* Use your color variable here */
  color: white;
  border: none;
  padding: 0.5em 0.75em;
  font-size: 1.5rem;
  cursor: pointer;
  margin-left: 2em;
}

.summary-and-checkout-container{
  display: flex;
  flex-direction: column;
  margin-right: 20px;
  margin-top: -90px;
}

.cart-summary-above {
  text-align: right;
  margin-bottom: 20px;
}

.proceed-to-checkout-button-container {
  align-self: flex-end;
  text-align: right;
  margin-bottom: 10px;
}


</style>

<template>
  <div class="main-div">
    <p style="text-align: center; font-size: 48px;">Cart Page</p>

    <button @click="clearCart" class="clear-cart-button">Clear Cart</button>

    <div v-if="cartStore.cart.items.length > 0">
      <div class="cart-table">
        <ul>
          <li class="table-heading">
            <div class="heading-book">Book</div>
            <div class="heading-price">Price/Quantity</div>
            <div class="heading-subtotal">Amount</div>
          </li>

          <template v-for="item in cartStore.cart.items" :key="item.book.bookId">
            <li>
              <div class="cart-book-image">
                <img
                  :src="`${bookImagePrefix}/${bookImageFileName(item.book)}`"
                  :alt="item.book.title"
                  width="80px"
                  height="auto"
                />
              </div>
              <div class="cart-book-title">{{ item.book.title }}</div>
              <div class="cart-book-price">
                ${{ (item.book.price / 100).toFixed(2) }}
              </div>
              <div class="cart-book-quantity">

                <button style="background-color: transparent;"
                  class="icon-button inc-button"
                  @click="updateCart(item.book, item.quantity + 1)"
                >
                <font-awesome-icon :icon="faPlusCircle" :style="{ color: 'black', fontSize:'20px'}"/>
                </button>

                <span class="quantity">{{ item.quantity }}</span
                >&nbsp;

                <button style="background-color: transparent;"
                  class="icon-button dec-button"
                  @click="updateCart(item.book, item.quantity - 1)"
                >
                <font-awesome-icon :icon="faMinusCircle" :style="{ color: 'black', fontSize:'20px'}"/>
                </button>
              </div>
              <div class="cart-book-subtotal">{{ asDollarsAndCents(item.book.price * item.quantity)}}</div>
            </li>
            <li class="line-sep"></li>
          </template>
        </ul>
      </div>
      
          <router-link :to="'../category/Classics'">
            <button class="continue-shopping-button">Continue Shopping</button>
          </router-link>
          
          <div class="summary-and-checkout-container">
            <div class="cart-summary">
              <p>Total Items in Cart: {{ calculateTotalQuantity() }}</p>
              <p>Subtotal: {{ calculateSubtotal() }}</p>
            </div>

            <div class="proceed-to-checkout-button-container">
              <router-link to="/checkout">
                <button class="proceed-to-checkout-button">Proceed to Checkout</button>
              </router-link>
            </div>
          </div>
    </div>

    <div v-else>
      <p>Your cart is empty.</p>
      <router-link :to="'../category/Classics'">
          <button class="continue-shopping-button">Continue Shopping</button>
        </router-link>
    </div>
  </div>

  

</template>
