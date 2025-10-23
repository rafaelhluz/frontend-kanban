<script setup>
import { inject, ref, watch } from 'vue'
import api from '../../services/api'

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
const color = ref('#007bff')
const steps = ref([])
const comments = ref([])
const attachments = ref([])

watch(() => props.editTask, (task) => {
  if (task) {
    title.value = task.title
    date.value = task.date
    color.value = task.color || '#007bff'
    steps.value = task.steps || []
    comments.value = task.comments || []
    attachments.value = task.attachments || []
  }
}, { immediate: true })

function close() {
  emit('close')
  title.value = ''
  date.value = ''
  color.value = '#007bff'
  steps.value = []
  comments.value = []
  attachments.value = []
}

async function save() {
  if (!title.value) return alert('Informe o título da tarefa.')

  const taskData = {
    title,
    date,
    color,
    steps,
    comments,
    attachments
  }

  try {
    if (props.editMode && props.editTask) {
      const response = await api.put(`/tasks/${props.editTask.id}`, taskData)
      Object.assign(props.editTask, response.data)
    } else {
      const response = await api.post('/tasks', { ...taskData, columnId: props.columnId })
      list_todo.push(response.data)

      const column = list_columns.find(c => c.id === props.columnId)
      if (column) column.taskC.push(response.data.id)
    }
    close()
  } catch (err) {
    console.error('Erro ao salvar task:', err)
  }
}
</script>


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

      <label for="color">Cor do card</label>
      <input id="color" type="color" v-model="color" />

      <label>Steps / Passos</label>
      <div class="steps">
          <div v-for="step in steps" :key="step.id" class="step-item">
            <input type="checkbox" v-model="step.done" />
              <span :style="{ textDecoration: step.done ? 'line-through' : 'none' }">{{ step.text }}</span>
              <button @click="removeStep(step.id)">x</button>
          </div>
          <div class="step-add">
            <input v-model="newStep" placeholder="Novo passo..." />
            <button @click="addStep">+</button>
          </div>
      </div>

      <label>Comentários</label>
      <div class="comments">
        <div v-for="comment in comments" :key="comment.id" class="comment-item">
          <span>{{ comment.text }}</span>
        </div>
        <input v-model="newComment" placeholder="Novo comentário..." @keyup.enter="addComment" />
        <button @click="addComment">Adicionar</button>
      </div>

      <label>Anexos</label>
      <input type="file" multiple @change="handleFileUpload" />
        <ul>
          <li v-for="file in attachments" :key="file">{{ file }}</li>
        </ul>

      <div class="actions">
        <button @click="save">{{ editMode ? 'Salvar' : 'Adicionar' }}</button>
        <button class="cancel" @click="close">Cancelar</button>
      </div>
    </div>
  </div>
</template>

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

.steps, .comments {
  background: #f7f7f7;
  padding: 8px;
  border-radius: 6px;
  margin-top: 6px;
}
.step-item {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-bottom: 4px;
}
.step-item button {
  background: none;
  border: none;
  color: red;
  cursor: pointer;
}
.step-add {
  display: flex;
  gap: 6px;
  margin-top: 6px;
}
.comments input {
  width: 100%;
  margin-top: 5px;
  padding: 6px;
  border-radius: 4px;
  border: 1px solid #ccc;
}
.comments button {
  margin-top: 5px;
  background: #007bff;
  color: white;
}

</style>
