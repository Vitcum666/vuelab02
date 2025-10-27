<script setup lang="ts">
import { toRefs, defineProps } from 'vue'
import { type Event } from '@/types'
import { useMessageStore } from '@/stores/message'
import { storeToRefs } from 'pinia'

const props = defineProps<{
  event: Event
  id: String
}>()
const { event } = toRefs(props)
const store = useMessageStore()
const { message } = storeToRefs(store)
</script>

<template>
  <div v-if="message" id="detailFlashMessage">
    <h4>{{ message }}</h4>
  </div>
  <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
  <p>{{ event.description }}</p>
</template>