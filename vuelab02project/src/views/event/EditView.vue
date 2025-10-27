<script setup lang="ts">
import { toRefs } from 'vue'
import { type Event } from '@/types'
import { useRouter } from 'vue-router'
import { useMessageStore } from '@/stores/message'

const props = defineProps<{
  event: Event
  id: String
}>()
const { event } = toRefs(props)
const router = useRouter()
const store = useMessageStore()

const edit = () => {
  // Simulate successful update, then set message and navigate
  store.updateMessage('The data has been updated')
  setTimeout(() => {
    store.resetMessage()
  }, 3000)
  router.push({ name: 'event-detail-view', params: { id: props.event.id } })
}
</script>
<template>
  <p>Edit event here</p>
  <button @click="edit">Save Changes</button>
</template>