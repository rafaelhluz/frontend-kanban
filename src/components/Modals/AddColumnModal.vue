<script setup>
import { ref } from 'vue'
import api from '../../services/api'

const props = defineProps({
  show: Boolean,
})

const emit = defineEmits(['close', 'add-column'])

const title = ref('')

async function handleAdd() {
  if (!title.value.trim()) return

  try {
    const response = await api.post('/positions', { titleC: title.value, taskC: [] })
    emit('add-column', response.data)
    title.value = ''
    emit('close')
  } catch (err) {
    console.error('Erro ao criar coluna:', err)
  }
}
</script>

<template>
  <div class="modal-backdrop" v-if="show" @click.self="emit('close')">
    <div class="modal">
      <h3>Nova Coluna</h3>
      <input
        v-model="title"
        type="text"
        placeholder="Digite o nome da coluna"
      />
      <div class="actions">
        <button @click="emit('close')">Cancelar</button>
        <button @click="handleAdd">Adicionar</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
}

.modal {
  background: white;
  padding: 24px;
  border-radius: 8px;
  min-width: 320px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.modal h3 {
  margin: 0;
  font-size: 18px;
}

.modal input {
  padding: 8px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

.actions {
  display: flex;
  justify-content: flex-end;
  gap: 8px;
}

.actions button {
  padding: 8px 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.actions button:hover {
  opacity: 0.9;
}

.actions button:last-child {
  background-color: #0c1e3e;
  color: white;
}
</style>
