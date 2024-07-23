<script setup lang="ts">
import Posts from '@/components/Posts/Posts.vue'
import { onMounted, reactive, ref } from 'vue'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import { Dialog, DialogContent, DialogHeader, DialogTitle, DialogTrigger } from '@/components/ui/dialog'
import { Label } from '@/components/ui/label'
import { Textarea } from '@/components/ui/textarea'

const posts = ref<Array<{ id: number; title: string; body: string }>>()
const error = ref<string | number | null>()
const isLoading = ref(false)

const formData = reactive({
  title: '',
  body: '',
})

const isOpenModal = ref(false)

function createPost() {
  posts.value?.unshift({
    id: posts.value.length + 1,
    ...formData,
  })

  isOpenModal.value = false

  formData.title = ''
  formData.body = ''
}

function updatePost(updatedPost: { id: number; title: string; body: string }, postId: number) {
  if (!posts.value) return

  const postIndex = posts.value.findIndex((post) => post.id === postId)

  if (postIndex !== -1) {
    posts.value[postIndex] = updatedPost
  }
}

onMounted(async () => {
  isLoading.value = true

  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts')

    if (!response.ok) {
      error.value = response.status

      return
    }

    posts.value = await response.json()

    if (posts.value?.length) {
      posts.value.length = 10
    }
  } catch (err) {
    console.log(err)
  } finally {
    isLoading.value = false
  }
})
</script>

<template>
  <main>
    <Dialog :open="isOpenModal" @update:open="isOpenModal = $event">
      <DialogTrigger as-child>
        <Button class="mb-4" variant="secondary">
          Добавить новый пост
        </Button>
      </DialogTrigger>

      <DialogContent class="sm:max-w-[625px]">
        <DialogHeader>
          <DialogTitle>
            Добавление поста
          </DialogTitle>
        </DialogHeader>

        <form @submit.prevent="createPost">
          <div class="grid gap-4 py-4">
            <div class="grid items-center gap-4">
              <Label for="title">
                Название
              </Label>

              <Input id="title" v-model="formData.title" />
            </div>

            <div class="grid items-center gap-4">
              <Label for="body">
                Содержание
              </Label>

              <Textarea id="body" v-model="formData.body" />
            </div>
          </div>

          <div class="flex justify-end">
            <Button type="submit">
              Создать пост
            </Button>
          </div>
        </form>
      </DialogContent>
    </Dialog>
    
    <Posts
      :error="error"
      :is-loading="isLoading"
      :posts="posts"
      @change-title="updatePost"
    />
  </main>
</template>
