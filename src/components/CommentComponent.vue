<script setup lang="ts">
import { ref } from 'vue'
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
interface Props {
  comment: item
}
const showComments = ref<boolean>(false)
const props = defineProps<Props>()
</script>
<template>
  <div class="p-2 pr-8 border-l border-[#3a3a3a] commenttext">
    <div class="font-bold text-lg">
      {{ comment.author }}
    </div>
    <div v-html="comment.text"></div>
    <div v-if="showComments">
      <div class="text-sm text-[#9e4001] cursor-pointer" @click="showComments = false">
        Hide Comments
      </div>
      <div v-for="(item, index) in comment.children" :key="index">
        <CommentComponent :comment="item"></CommentComponent>
      </div>
    </div>
    <div v-else-if="comment.children.length > 0">
      <div class="text-sm text-[#9e4001] cursor-pointer" @click="showComments = true">
        Show comments
      </div>
    </div>
  </div>
</template>
