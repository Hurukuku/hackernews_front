<script setup lang="ts">
import { useRoute } from 'vue-router'
import { ref } from 'vue'
import NotFound from '@/components/NotFound.vue'
interface item {
  by: string
  descendants: number
  id: number
  required: true
  kids: number[]
  score: number
  time: number
  title: string
  type: string
  url: string
}

const route = useRoute()
const errorMessage = ref<string>('')
const post = ref<item>()

fetch('https://hacker-news.firebaseio.com/v0/item/' + route.params.id + '.json')
  .then(async (response) => {
    const isJson = response.headers.get('content-type')?.includes('application/json')
    const data = isJson && (await response.json())

    // check for error response
    if (!response.ok) {
      // get error message from body or default to response status
      const error = (data && data.message) || response.status
      return Promise.reject(error)
    }

    post.value = data
  })
  .catch((error) => {
    errorMessage.value = error
    console.error('There was an error!', error)
  })
console.log(post)
</script>
<template>
  <div class="text-2xl font-extrabold" v-if="post">
    <div></div>
    {{ post.title }}
  </div>
  <div v-else><NotFound></NotFound></div>
</template>
