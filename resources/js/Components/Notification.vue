<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  message: String,
  type: {
    type: String,
    default: 'success'
  }
})

const localMessage = ref('')
const localType = ref('success')
const visible = ref(false)

watch(
  () => props.message,
  (newMessage) => {
    if (newMessage) {
      localMessage.value = newMessage
      localType.value = props.type
      visible.value = true

      setTimeout(() => {
        visible.value = false
        localMessage.value = ''
      }, 3000)
    }
  },
  { immediate: true }
)
</script>

<template>
  <transition name="fade">
    <div
      v-if="visible"
      :class="[
        'fixed top-5 right-5 max-w-xs p-4 rounded-lg shadow-lg text-white z-50',
        type === 'success' ? 'bg-green-500' : 'bg-red-500'
      ]"
    >
      {{ message }}
    </div>
  </transition>
</template>

<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
