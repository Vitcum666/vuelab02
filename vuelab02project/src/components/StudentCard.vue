<script setup lang="ts">
import type { Student } from '@/types'

defineProps<{
  student: Student
}>()
</script>

<template>
  <div class="student-card">
    <h3>{{ student.name }} {{ student.surname }}</h3>
    <p class="gpa">GPA: {{ student.gpa }}</p>
    <p class="student-id">ID: {{ student.id }}</p>
  </div>
</template>

<style scoped>
.student-card {
  padding: 20px;
  width: 280px;
  cursor: pointer;
  border: 1px solid #39495c;
  margin: 10px;
  border-radius: 8px;
  background-color: #f9f9f9;
  transition: transform 0.2s, box-shadow 0.2s;
}

.student-card:hover {
  transform: scale(1.02);
  box-shadow: 0 4px 12px 0 rgba(0, 0, 0, 0.15);
}

.student-card h3 {
  margin-bottom: 10px;
  color: #2c3e50;
  font-size: 18px;
}

.gpa {
  font-size: 16px;
  font-weight: bold;
  color: #27ae60;
  margin-bottom: 5px;
}

.student-id {
  font-size: 14px;
  color: #7f8c8d;
}
</style>