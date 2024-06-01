<script setup lang="ts">
import PostComponent from '../components/PostComponent.vue'
</script>

<template>
  <main
    class="flex justify-center space-y-2 flex-col h-1 justify-items-center items-center w-full p-4 overflow-y-scroll"
  >
    <PostComponent
      v-for="item in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]"
      :key="item"
    ></PostComponent>
  </main>
</template>
