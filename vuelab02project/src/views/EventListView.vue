<script setup lang="ts">
import { ref, computed, onMounted, watchEffect } from 'vue'
import type { Event as EventModel } from '@/types'
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService'
import { useRouter } from 'vue-router'

const router = useRouter()
const events = ref<EventModel[] | null>(null)
const totalEvents = ref(0)
const hasNextPage = computed(() => {
  const totalPages = Math.ceil(totalEvents.value / 3)
  return page.value < totalPages
})

const props = defineProps({
  page: {
    type: Number,
    required: true
  },
  perPage: {
    type: Number,
    default: 2
  }
})

const changePageSize = (event: Event) => {
  const newPerPage = parseInt((event.target as HTMLSelectElement).value)
  router.push({ 
    name: 'event-list-view', 
    query: { page: 1, perPage: newPerPage } 
  })
}
const page = computed(() => props.page)

onMounted(() => {
  watchEffect(() => {
    EventService.getEvents(3, page.value)
      .then((response) => {
        events.value = response.data
        totalEvents.value = response.headers['x-total-count']
      })
      .catch((error) => {
        console.log(error)
      })
  })
})
</script>

<template>
  <h1>Events For Good</h1>

  <!-- Events display -->
  <div class="flex flex-col items-center">
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <div class="page-size-control">
      <label for="page-size">Events per page:</label>
      <select id="page-size" :value="props.perPage" @change="changePageSize">
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="5">5</option>
        <option value="10">10</option>
      </select>
    </div>
    <div class="pagination">
      <RouterLink
        id="page-prev"
        :to="{ name: 'event-list-view', query: { page: page - 1, perPage: props.perPage } }"
        rel="prev"
        v-if="page != 1"
        >&#60; Prev Page</RouterLink
      >

      <RouterLink
        id="page-next"
        :to="{ name: 'event-list-view', query: { page: page + 1, perPage: props.perPage } }"
        rel="next"
        v-if="hasNextPage"
        >Next Page &#62;</RouterLink
      >
    </div>
  </div>
</template>

<style scoped>
.page-size-control {
  margin: 20px 0;
  text-align: center;
}

.page-size-control label {
  margin-right: 10px;
  font-weight: bold;
}

.page-size-control select {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.pagination {
  display: flex;
  width: 290px;
}
.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}
</style>
