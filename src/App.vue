<script setup>
import { reactive, provide, onMounted } from 'vue'
import Header from './layout/Header.vue'
import Board from './components/Board.vue'
import Footer from './layout/Footer.vue'
import api from './services/api'

const list_columns = reactive([])
const list_todo = reactive([])

provide('list_columns', list_columns)
provide('list_todo', list_todo)

async function fetchData() {
  try {
    const [columnsRes, tasksRes] = await Promise.all([
      api.get('/positions'),
      api.get('/tasks')
    ])
    
    list_columns.push(...columnsRes.data)
    list_todo.push(...tasksRes.data)
  } catch (err) {
    console.error('Erro ao buscar dados do backend:', err)
  }
}

onMounted(fetchData)
</script>

<template>
  <Header />
  <main>
    <Board />
  </main>
  <Footer/>
</template>

<style>
main {
  padding: 20px;
  background-color: #f4f5f7;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
</style>
