<script setup>
defineProps(['task'])
</script>

<template>
  <div class="card" @click="$emit('edit-task', task.id)">
    <p class="title">{{ task.title }}</p>
    <small class="date">{{ task.date }}</small>
  </div>
</template>

<style scoped>
.card {
  background-color: #fff;
  border-radius: 8px;
  padding: 10px 12px;
  margin-bottom: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  cursor: pointer;
  transition: transform 0.2s;
}
.card:hover {
  transform: scale(1.02);
}
.title {
  font-size: 14px;
  font-weight: 600;
  color: #333;
}
.date {
  font-size: 12px;
  color: #777;
}
</style>
