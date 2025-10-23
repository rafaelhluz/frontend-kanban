<script setup>
import Card from "./Card.vue";
import draggable from 'vuedraggable';
import { inject, ref } from 'vue';
import AddCardModal from './Modals/AddCardModal.vue';
import AddColumnModal from './Modals/AddColumnModal.vue';
import api from '../services/api';

const list_todo = inject('list_todo');
const list_columns = inject('list_columns');

const showTaskModal = ref(false)
const selectedColumn = ref(null)
const editTask = ref(null)
const editMode = ref(false)

const showColumnModal = ref(false)

function openAdd(columnId){
  showTaskModal.value = true
  selectedColumn.value = columnId
  editTask.value = null
  editMode.value = false
}

function openEditTask(taskId){
  const task = list_todo.find(t => t.id === taskId)
  if (task) {
    editTask.value = task
    editMode.value = true
    showTaskModal.value = true
    selectedColumn.value = list_columns.find(c => c.taskC.includes(taskId))?.id || null
  }
}

function closeTaskModal(){
  showTaskModal.value = false
  selectedColumn.value = null
  editTask.value = null
  editMode.value = false
}

function openAddColumn(){
  showColumnModal.value = true
}

function closeColumnModal(){
  showColumnModal.value = false
}

async function addColumn(newColumn){
  try {
    const response = await api.post('/positions', newColumn)
    list_columns.push(response.data)
    closeColumnModal()
  } catch (err) {
    console.error('Erro ao criar coluna:', err)
  }
}

async function updateTaskOrder(column){
  try {
    await api.put(`/positions/${column.id}/tasks`, { taskC: column.taskC })
  } catch (err) {
    console.error('Erro ao atualizar ordem de tarefas:', err)
  }
}

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
fetchData()

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
        @end="() => updateTaskOrder(column)"
      >
        <template #item="{ element: taskId }">
          <div @click="openEditTask(taskId)">
            <Card :task="list_todo.find(t => t.id === taskId)" />
          </div>
        </template>
      </draggable>

      <button class="add-btn" @click="openAdd(column.id)">+ Adicionar tarefa</button>
    </div>

    <button class="button-coll" @click="openAddColumn()">+ Nova coluna</button>
  </div>

  <AddCardModal
    v-if="showTaskModal"
    :show="showTaskModal"
    :columnId="selectedColumn"
    :editTask="editTask"
    :editMode="editMode"
    @close="closeTaskModal"
  />

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
  margin-top: 8px;
}

.add-btn:hover {
  background-color: #0C1E3EE5;
  transition: 0.3s;
  color: white;
}
</style>
