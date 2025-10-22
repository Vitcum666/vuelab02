<script setup lang="ts">
import StudentCard from '@/components/StudentCard.vue'
import type { Student } from '@/types'
import { ref, onMounted } from 'vue'
import StudentService from '@/services/StudentService'

const students = ref<Student[]>([])
const loading = ref(true)
const error = ref<string | null>(null)

onMounted(() => {
  StudentService.getStudents()
    .then((response) => {
      students.value = response.data
      loading.value = false
    })
    .catch((err) => {
      console.error('Error fetching students:', err)
      error.value = 'Failed to load students'
      loading.value = false
    })
})
</script>

<template>
  <div class="student-list-view">
    <h1>Student Information</h1>
    
    <div v-if="loading" class="loading">
      Loading students...
    </div>
    
    <div v-else-if="error" class="error">
      {{ error }}
    </div>
    
    <div v-else class="students-grid">
      <StudentCard 
        v-for="student in students" 
        :key="student.id" 
        :student="student" 
      />
    </div>
  </div>
</template>

<style scoped>
.student-list-view {
  padding: 20px;
}

.student-list-view h1 {
  text-align: center;
  margin-bottom: 30px;
  color: #2c3e50;
}

.students-grid {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.loading {
  text-align: center;
  font-size: 18px;
  color: #7f8c8d;
  margin: 50px 0;
}

.error {
  text-align: center;
  font-size: 18px;
  color: #e74c3c;
  margin: 50px 0;
}
</style>