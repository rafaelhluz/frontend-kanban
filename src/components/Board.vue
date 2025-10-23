<script setup>
import Card from "./Card.vue";
import draggable from 'vuedraggable';
import { inject, ref } from 'vue';
import AddCardModal from './Modals/AddCardModal.vue';
import AddColumnModal from './Modals/AddColumnModal.vue'; // modal de coluna

const list_todo     = inject('list_todo');
const list_columns  = inject('list_columns');

// Modal de tarefas
const showTaskModal     = ref(false)
const selectedColumn    = ref(null)
const editTask          = ref(null)
const editMode          = ref(false)

// Modal de colunas
const showColumnModal   = ref(false)

function openAdd(columnId){
  showTaskModal.value = true
  selectedColumn.value = columnId
  editTask.value = null
  editMode.value = false
}

function openEditTask(taskIndex){
  showTaskModal.value = true
  editTask.value = list_todo[taskIndex]
  editMode.value = true
}

function closeTaskModal(){
  showTaskModal.value = false
  selectedColumn.value = null
  editTask.value = null
  editMode.value = false
}

// Abrir modal de nova coluna
function openAddColumn(){
  showColumnModal.value = true
}

// Fechar modal de nova coluna
function closeColumnModal(){
  showColumnModal.value = false
}

// Recebe a nova coluna emitida do modal e adiciona à lista
function addColumn(newColumn){
  const id = Date.now() // gera um ID único simples
  list_columns.push({ id, ...newColumn })
}
</script>

<template>
  <div class="board-container">
    <!-- Loop das colunas -->
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
          <div @click="openEditTask(task)">
            <Card :task="list_todo[task]" />
          </div>
        </template>
      </draggable>

      <button class="add-btn" @click="openAdd(column.id)">+ Adicionar tarefa</button>
    </div>

    <!-- Botão de adicionar coluna -->
    <button class="button-coll" @click="openAddColumn()">+ Nova coluna</button>
  </div>

  <!-- Modal de adicionar/editar tarefa -->
  <AddCardModal
    v-if="showTaskModal"
    :show="showTaskModal"
    :columnId="selectedColumn"
    :editTask="editTask"
    :editMode="editMode"
    @close="closeTaskModal"
  />

  <!-- Modal de adicionar coluna -->
  <AddColumnModal
    v-if="showColumnModal"
    :show="showColumnModal"
    @close="closeColumnModal"
    @add-column="addColumn"
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

.button-coll{
  border-radius: 8px;
  padding: 12px;
  width: 300px;
  height: 200px;
  display: flex;
  border: none;
  cursor: pointer;
  justify-content: center;
  align-items: center;
  background-color: #ebecf0;
  font-size: large;
}

.button-coll:hover {
  background-color: #0C1E3EE5;
  transition: 0.5s;
  color: white;
}

.add-btn {
  cursor: pointer;
  border-radius: 5px;
}

.add-btn:hover {
  background-color: #0C1E3EE5;
  transition: 0.3s;
  color: white;
}
</style>
