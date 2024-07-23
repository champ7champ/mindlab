<script setup lang="ts">
import type { PropType } from 'vue'
import Post from '@/components/Post/Post.vue'

const emits = defineEmits(['changeTitle', 'deletePost'])

const props = defineProps({
  posts: {
    type: Array as () => Array<{ id: number; title: string; body: string }>,
    default: () => [],
  },

  error: {
    type: [String, Number, null] as PropType<string | number | null>,
    default: null,
  },

  isLoading: {
    type: Boolean,
    default: false,
  },
})

function handleChangeTitle(updatedPost: { id: number; title: string; body: string }, postId: number) {
  emits('changeTitle', updatedPost, postId)
}

function deletePost(id: number) {
  emits('deletePost', id)
}
</script>

<template>
  <template v-if="!props.isLoading">
    <div v-if="!error" class="space-y-4">
      <Post
        v-for="post in props.posts"
        :key="post.id"
        :post="post"
        @change-title="handleChangeTitle"
        @delete-post="deletePost"
      />
    </div>

    <div v-else class="text-2xl text-red-400">
      <span>
        Кажется у нас ошибка в запросе {{ error }}
      </span>
    </div>
  </template>
  
  <template v-else>
    <div class="w-full h-24 flex items-center justify-center">
      ЗАГРУЗКА
    </div>
  </template>
</template>

<style scoped>

</style>