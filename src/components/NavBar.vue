<script setup lang="ts">
import { ref } from 'vue'

import { RouterLink } from 'vue-router'
</script>

<template>
  <div
    class="fixed pl-8 pr-8 h-12 align-middle items-center space-x-8 flex flex-row w-full backdrop-blur-md border-b-2 border-b-[#3a3a3a]"
  >
    <div class="text-[24px] font-bold text-[#ff6600] p-2 mr-8">
      <RouterLink to="/"> HackerNews </RouterLink>
    </div>
    <RouterLink class="text-center p-2" to="/New"> New </RouterLink>
  </div>
</template>
