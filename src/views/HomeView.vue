<script setup lang="ts">
import PostComponent from '../components/PostComponent.vue'
import { ref } from 'vue'

const topPosts = ref<number[]>([])
const errorMessage = ref(null)

fetch('https://hacker-news.firebaseio.com/v0/topstories.json?count=20')
  .then(async (response) => {
    const isJson = response.headers.get('content-type')?.includes('application/json')
    const data = isJson && (await response.json())

    // check for error response
    if (!response.ok) {
      // get error message from body or default to response status
      const error = (data && data.message) || response.status
      return Promise.reject(error)
    }

    topPosts.value = data
  })
  .catch((error) => {
    errorMessage.value = error
    console.error('There was an error!', error)
  })
console.log(topPosts)
</script>

<template>
  <main class="mt-16 flex flex-col h-full items-center space-y-2">
    <PostComponent v-for="item in topPosts.slice(0, 20)" :key="item" :postId="item"></PostComponent>
  </main>
</template>
-
