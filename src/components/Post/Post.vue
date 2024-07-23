<script setup lang="ts">
import Button from '@/components/ui/button/Button.vue'
import Input from '@/components/ui/input/Input.vue'
import { ref } from 'vue'

const emits = defineEmits(['changeTitle'])

const props = defineProps({
  post: {
    type: Object as () => { id: number; title: string; body: string },
    default: () => ({}),
  },
})

const isChangeable = ref(false)
const newTitleValue = ref('')

function changeTitle() {
  const sendData = {
    ...props.post,
    title: newTitleValue.value,
  }

  emits('changeTitle', sendData, props.post.id)
}

function toggleChangeable() {
  isChangeable.value = !isChangeable.value

  if (!isChangeable.value) {
    changeTitle()
  } else {
    newTitleValue.value = props.post.title
  }
}
</script>

<template>
  <div class=" ring-1 ring-[#000]/5 rounded-xl px-4 py-4 flex flex-col gap-4">
    <div v-if="!isChangeable" class="text-2xl uppercase">
      {{ props.post.title }}
    </div>

    <div v-else class=" rounded-full">
      <Input v-model="newTitleValue" class="ring-1 ring-red-300" size="xl" />
    </div>
    
    <div class="text-muted-foreground">
      {{ props.post.body }}
    </div>

    <div class="flex self-end space-x-4">
      <Button class="max-w-96 self-end" @click="toggleChangeable">
        {{ isChangeable ? 'Принять изменения' : 'Изменить текст' }}
      </Button>

      <Button
        v-if="isChangeable"
        class="max-w-96 self-end"
        variant="destructive"
        @click="isChangeable = !isChangeable"
      >
        Отменить изменения
      </Button>
    </div>
  </div>
</template>

<style scoped>

</style>