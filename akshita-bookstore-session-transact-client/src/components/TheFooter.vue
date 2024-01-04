<script setup lang="ts">
  import { library } from "@fortawesome/fontawesome-svg-core";
  import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
  import {faFacebook} from "@fortawesome/free-brands-svg-icons";
  import {faTwitter} from "@fortawesome/free-brands-svg-icons/faTwitter";


</script>

<style scoped>
footer {
  background: #B0909C;
  display: flex;
  justify-content: space-between; /* Distribute items along the main axis */
  align-items: center; /* Center items along the cross axis */
  padding: 1em;
}

.links,
.social-media-icons {
  display: flex;
  gap: 1em;
}

.social-media-icons {
  text-align: center;
  gap: 0.25em;
}

.router-link {
  text-decoration: none;
  color: #111111; /* Set the color for link text */
}

.router-link:hover {
  text-decoration: underline; /* Add underline on hover */
}
</style>

<template>
  <footer class="container">
    <section>
      <p>&copy; 2023 Leisure Time BookStore</p>
    </section>
    <section class="links">
      <router-link to="/" class="router-link">about</router-link>
      <router-link to="/" class="router-link">contact</router-link>
      <router-link to="/" class="router-link">directions</router-link>
    </section>
    <section class="social-media-icons">
      <router-link to="/" class="router-link">
        <font-awesome-icon :icon="faFacebook"/>
      </router-link>
      <router-link to="/" class="router-link">
        <font-awesome-icon :icon="faTwitter"/>
      </router-link>
    </section>
  </footer>
</template>
