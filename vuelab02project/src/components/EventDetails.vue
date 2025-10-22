<script setup lang="ts">
import type { Event } from '@/types'

defineProps<{
  event: Event
}>()
</script>

<template>
  <div class="event-details">
    <div class="category">{{ event.category }}</div>
    <div class="organizer">{{ event.organizer }}</div>
  </div>
</template>

<style scoped>
.event-details {
  text-align: right;
}

.category,
.organizer {
  font-size: 16px;
}
</style>