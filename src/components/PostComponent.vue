<script setup lang="ts">
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import IconUpVote from '../components/icons/IconUpVote.vue'
const props = defineProps({
  postId: { type: Number, required: true }
})

interface item {
  author: string
  children: item[]
  id: number
  title: string
  url: string
  text: string
  parent_id: number
  created_at: string
  points: number
}

const errorMessage = ref<string>('')
const post = ref<item>()
fetch('http://hn.algolia.com/api/v1/items/' + props.postId)
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
console.log(props.postId)
</script>
<template>
  <div
    class="text-sm h-24 flex w-1/3 p-2 flex-col border-[#3a3a3a] border rounded shadow-sm"
    v-if="post"
  >
    <div class="flex h-full flex-row justify-between">
      <div class="flex shrink-0 w-4/5 flex-col justify-between">
        <div class="flex flex-row align-middle items-center">
          <RouterLink class="text-lg font-bold" :to="'/post/' + props.postId">
            {{ post.title }}
          </RouterLink>
        </div>
        <div class="flex flex-row">
          <IconUpVote class="h-[10px] mt-[5px] mr-2"></IconUpVote> {{ post.points }} points
        </div>
      </div>
      <div class="text-right flex shrink-0 w-1/5 flex-col justify-between">
        <div>12 hours ago</div>
        <div>
          <RouterLink :to="'/user/' + post.author">{{ post.author }}</RouterLink>
        </div>
      </div>
    </div>

    <div class="flex flex-row"></div>
  </div>
</template>
