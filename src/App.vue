<script setup>
import { reactive, provide } from 'vue'
import Header from './layout/Header.vue'
import Board from './components/Board.vue'
import Footer from './layout/Footer.vue'

// Estado global simples
const list_columns = reactive([
  { id: 1, titleC: 'A Fazer', taskC: [0, 1] },
  { id: 2, titleC: 'Em Progresso', taskC: [] },
  { id: 3, titleC: 'Concluído', taskC: [] },
])

const list_todo = reactive([
  { id: 0, title: 'Implementar backend', date: '22/10/2025' },
  { id: 1, title: 'Criar UI base', date: '23/10/2025' },
])

// Prover o estado
provide('list_columns', list_columns)
provide('list_todo', list_todo)
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
