<script setup lang="ts">
import type { HTMLAttributes } from 'vue'
import { cn } from '@/lib/utils'
import { useRouter } from 'vue-router'
import { defineProps, defineEmits } from 'vue'

const router = useRouter()
const emit = defineEmits(['row-click'])

const props = defineProps<{
  class?: HTMLAttributes['class']
  href?: string | null
}>()

function handleRowClick() {
  if (props.href) {
    router.push(props.href)
  } else {
    emit('row-click')
  }
}
</script>

<template>
  <tr
    :class="cn('border-b transition-colors hover:bg-muted/50 cursor-pointer', props.class)"
    @click="handleRowClick"
  >
    <slot />
  </tr>
</template>
