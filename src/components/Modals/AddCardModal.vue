<template>
  <div v-if="show" class="modal-overlay" @click.self="close">
    <div class="modal">
      <h2>{{ editMode ? 'Editar Tarefa' : 'Adicionar Tarefa' }}</h2>

      <label for="title">Título</label>
      <input
        id="title"
        v-model="title"
        type="text"
        placeholder="Digite o título da tarefa"
      />

      <label for="date">Data</label>
      <input
        id="date"
        v-model="date"
        type="date"
      />

      <div class="actions">
        <button @click="save">{{ editMode ? 'Salvar' : 'Adicionar' }}</button>
        <button class="cancel" @click="close">Cancelar</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { inject, ref, watch } from 'vue'

const props = defineProps({
  show: Boolean,
  columnId: Number,
  editMode: Boolean,
  editTask: Object
})
const emit = defineEmits(['close'])

const list_todo = inject('list_todo')
const list_columns = inject('list_columns')

const title = ref('')
const date = ref('')

watch(() => props.editTask, (task) => {
  if (task) {
    title.value = task.title
    date.value = task.date
  }
}, { immediate: true })

function close() {
  emit('close')
  title.value = ''
  date.value = ''
}

function save() {
  if (!title.value) return alert('Informe o título da tarefa.')

  if (props.editMode && props.editTask) {
    props.editTask.title = title.value
    props.editTask.date = date.value
  } else {
    const newTask = {
      id: Date.now(),
      title: title.value,
      date: date.value || new Date().toLocaleDateString()
    }

    list_todo.push(newTask)

    const column = list_columns.find(c => c.id === props.columnId)
    if (column) column.taskC.push(list_todo.length - 1)
  }

  close()
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.4);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
}

.modal {
  background: white;
  padding: 20px;
  border-radius: 10px;
  width: 320px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
}

label {
  display: block;
  font-size: 14px;
  margin-top: 10px;
  color: #333;
}

input {
  width: 100%;
  padding: 8px;
  border-radius: 6px;
  border: 1px solid #ccc;
  margin-top: 5px;
}

.actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 20px;
}

button {
  border: none;
  padding: 8px 14px;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  opacity: 0.9;
}

.cancel {
  background: #ccc;
  color: #333;
}

.actions button:first-child {
  background: #007bff;
  color: white;
}
</style>
