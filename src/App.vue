<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import NavBar from './components/NavBar.vue'
</script>

<template>
  <div class="w-full flex flex-col">
    <NavBar />
    <RouterView class="mt-12 mb-4" />
  </div>
</template>
