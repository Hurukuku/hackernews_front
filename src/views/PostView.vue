<script setup lang="ts">
import { useRoute } from 'vue-router'
import { ref } from 'vue'
import NotFound from '@/components/NotFound.vue'
import IconUpVote from '@/components/icons/IconUpVote.vue'
import CommentComponent from '@/components/CommentComponent.vue'
const route = useRoute()
const errorMessage = ref<string>('')

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

const post = ref<item>()
const statusCode = ref<number>(0)

fetch('http://hn.algolia.com/api/v1/items/' + route.params.id)
  .then(async (response) => {
    const isJson = response.headers.get('content-type')?.includes('application/json')
    const data = isJson && (await response.json())
    console.log(response)
    // check for error response
    if (!response.ok) {
      // get error message from body or default to response status
      const error = (data && data.message) || response.status
      return Promise.reject(error)
    }

    post.value = data
    console.log(response)
  })
  .catch((error) => {
    errorMessage.value = error
    console.error('There was an error!', error)
  })
console.log(post)
</script>
<template>
  <div v-if="post && post.parent_id == null">
    <div class="flex flex-col w-full items-center pt-12">
      <div class="p-4 w-[50%] border-[#3a3a3a] border rounded shadow-sm">
        <div class="flex h-full w-full flex-col">
          <div class="flex h-full flex-row justify-between">
            <div class="flex shrink-0 w-5/6 flex-col justify-between">
              <div class="flex flex-row align-middle items-center">
                <div class="text-2xl font-bold">
                  {{ post.title }}
                </div>
              </div>
              <div class="commenttext">
                <a :href="post.url">Link</a>
              </div>
              <div class="flex flex-row">
                <IconUpVote class="h-[10px] mt-[5px] mr-2"></IconUpVote> {{ post.points }} points
              </div>
            </div>
            <div class="text-right flex shrink-0 w-1/6 flex-col justify-between">
              <div>12 hours ago</div>
              <div>
                <RouterLink :to="'/user/' + post.author">{{ post.author }}</RouterLink>
              </div>
            </div>
          </div>
          <div v-for="(item, index) in post.children" :key="index">
            <CommentComponent :comment="item" />
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <div v-if="statusCode == 200">
      <NotFound></NotFound>
    </div>
    <div v-else>Loading...</div>
  </div>
</template>
