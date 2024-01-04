<script setup lang="ts">
import TheHeaderDropdown from "@/components/TheHeaderDropdown.vue";
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faShoppingCart } from '@fortawesome/free-solid-svg-icons';
import { faUser } from '@fortawesome/free-solid-svg-icons';
import { faSearch } from "@fortawesome/free-solid-svg-icons";

import { useCartStore } from "@/stores/cart";
const cartStore = useCartStore();

const categoryImagePrefix = `${import.meta.env.BASE_URL}`;
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1em;
  background: #B0909C;
}

.text-logo {
  font-family: "Times New Roman", Times, serif;
  text-decoration-line: none;
  color: #111111;
}

.search-bar {
  padding: 0.5em;
}

.logo-and-title {
  display: flex;
  align-items: center;
  gap: 1em;
}

.search-container {
  display: flex;
  align-items: center;
}

.search-bar {
  flex-grow: 1;
  border: none;
  border-bottom: 1px solid #ccc;
  padding-right: 30px;
  border-radius: 25px;
}

.search-button {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  outline: none;
}

.inline-buttons {
  display: flex;
  align-items: center;
}

.button {
  margin-right: 10px;
  font-family: "Times New Roman", Times, serif;
}

.badge {
  background: none;
  color: white;
  border-radius: 50%;
  padding: 2px 5px;
  font-size: 12px;
  position: relative;
  top: -7px;
  left: -21px;
}

span {
  display: inline-flex;
  align-items: center;
}

@media (max-width: 600px) {
  header {
    flex-direction: column;
  }
  .header-dropdown-and-cart {
    margin-top: 1em;
  }
}
</style>

<template>
  <header class="container">
    <section class="logo-and-title">
      <router-link to="/">
        <img
          src="/site-images/bookstore-logo.png"
          alt="Another Bookstore Logo"
          width="100"
          height="auto"
        />
      </router-link>
      <p style="font-size: 28px; padding: -10px">
        <router-link to="/" class="text-logo">Leisure Time<br>BookStore</router-link>
      </p>
    </section>

    <section class="title-and-search-bar">
      <form action="#">
        <div class="search-container">
          <input type="text" class="search-bar" placeholder="Search..." style="background-image: url('/site-images/search-icon.png'); background-size: 20px 20px ; background-position: 5px center; background-repeat: no-repeat; padding-left: 30px;">
          <button type="submit" class="search-button">
          </button>
        </div>
      </form>
    </section>

    <TheHeaderDropdown></TheHeaderDropdown>

    <section class="header-dropdown-and-cart">
      <div class="inline-buttons">
        <router-link to="/cart">
          <button class="button" style="border: none; background: none">
            <span>
              <font-awesome-icon :icon="faShoppingCart" :style="{ color: 'black', fontSize:'32px'}"/>
              <span class="badge">{{cartStore.count}}</span>
            </span>
          </button>
        </router-link>
        <p>Hello, User</p>
        <button class="button" style="border: none; background: none">
          <font-awesome-icon :icon="faUser" :style="{ color: 'black', fontSize:'32px' }"/>
        </button>
      </div>
    </section>
  </header>
</template>