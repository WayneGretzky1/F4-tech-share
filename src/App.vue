<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <!-- Header -->
  <div class="header">
    <div class="home-menu pure-menu pure-menu-horizontal pure-menu-fixed">
      <ul class="pure-menu-list">
        <li class="pure-menu-item pure-menu-selected">
          <RouterLink to="/" class="pure-menu-link">Home</RouterLink>
        </li>
      </ul>
    </div>
  </div>
  <RouterView />
</template>

<style scoped>
.home-menu .pure-menu-link,
.home-menu .pure-menu-heading {
  color: #ffffff;
}

</style>
