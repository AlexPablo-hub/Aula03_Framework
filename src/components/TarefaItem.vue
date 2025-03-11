<template>
  <li :class="{ concluida: tarefa.concluida }">
    <span>{{ tarefa.texto }}</span>
    <div class="botoes">
      <button 
        class="toggle-btn" 
        :class="tarefa.concluida ? 'concluida' : 'pending'" 
        @click="$emit('toggle', tarefa.id)">
        {{ tarefa.concluida ? 'Pendente' : 'Concluída' }}
      </button>
      <button class="edit-btn" @click="$emit('edit', tarefa.id)">Editar</button>
      <button class="delete-btn" @click="$emit('delete', tarefa.id)">Deletar</button>
    </div>
  </li>
</template>

<script setup>
const props = defineProps({
  tarefa: {
    type: Object,
    required: true
  }
});
</script>

<style scoped>
li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px;
  margin-bottom: 10px;
  border-radius: 5px;
  background-color: #3a3a3a;
}

.botoes button {
  margin-left: 8px;
  padding: 6px 10px;
  border: none;
  border-radius: 4px;
  background-color: #4a90e2;
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggle-btn.pending:hover {
  background-color: #229e32;
}

.toggle-btn.concluida:hover {
  background-color: #f1c40f;
}

.edit-btn:hover {
  background-color: #07588f;
}

.delete-btn:hover {
  background-color: #f0341f;
}

.concluida span {
  text-decoration: line-through;
  color: #bbb;
}
</style>