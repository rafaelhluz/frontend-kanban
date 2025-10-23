<script setup>
import Card from "./Card.vue";
import draggable from 'vuedraggable';
import { inject, ref } from 'vue';
import AddCardModal from './Modals/AddCardModal.vue';

const list_todo     = inject('list_todo');
const list_columns  = inject('list_columns');

const showModal     = ref(false)
const selectedColumn = ref(null)

function openAdd(columnId){
  showModal.value = true
  selectedColumn.value = columnId
}

function closeModal(){
  showModal.value = false
  selectedColumn.value = null
}
</script>

<template>
    
  <div class="board-container">
    <div
      v-for="column in list_columns"
      :key="column.id"
      class="column"
    >
      <div class="column-header">
        <h2>{{ column.titleC }}</h2>
        <span>{{ column.taskC.length }}</span>
      </div>

      <draggable
        v-model="column.taskC"
        :group="{ name: 'tasks', pull: true, put: true }"
        item-key="id"
        animation="200"
        ghost-class="ghost"
      >
        <template #item="{ element: task }">
          <Card :task="list_todo[task]" />
        </template>
      </draggable>

      <button class="add-btn" @click="openAdd(column.id)">+ Adicionar tarefa</button>
    </div>
    
    <button class="buttonCol">+</button>
  
  </div>


  <AddCardModal
    v-if="showModal"
    :show="showModal"
    :columnId="selectedColumn"
    @close="closeModal"
  />
</template>

<style scoped>
.board-container {
  display: flex;
  flex-direction: row;
  gap: 16px;
  flex-grow: 1;
}

.column {
  background-color: #ebecf0;
  border-radius: 8px;
  padding: 12px;
  width: 300px;
  display: flex;
  flex-direction: column;
}

.buttonCol{
  /*border-radius: 8px;
  padding: 12px;
  width: 300px;
  display: flex;*/
  border: none;
  cursor: pointer;
  justify-content: center;
  align-items: center;
}

.buttonCol button:hover {
  background-color: gray;
  transition: 0.5s;
}
</style>