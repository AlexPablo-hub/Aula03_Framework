<template>
  <div class="container">
    <h1>Gerenciador de Tarefas</h1>
    <input 
      v-model="novaTarefa" 
      @keyup.enter="salvarTarefa" 
      placeholder="Nova tarefa..." />
    <button class="add-btn" @click="salvarTarefa">
      {{ editingTarefaId ? 'Salvar' : 'Adicionar' }}
    </button>
    
    <!-- Filtros -->
    <div class="filtros">
      <button @click="filtro = 'todas'" :class="{ ativo: filtro === 'todas' }">Todas</button>
      <button @click="filtro = 'pendentes'" :class="{ ativo: filtro === 'pendentes' }">Pendentes</button>
      <button @click="filtro = 'concluidas'" :class="{ ativo: filtro === 'concluidas' }">Concluídas</button>
    </div>
    
    <ul>
      <TarefaItem 
        v-for="tarefa in tarefasFiltradas" 
        :key="tarefa.id" 
        :tarefa="tarefa" 
        @toggle="alternarTarefa" 
        @edit="editarTarefa"
        @delete="deletarTarefa" />
    </ul>
    
    <p>{{ tarefasPendentes }} tarefas pendentes</p>
    <p>{{ tarefasConcluidas }} tarefas concluídas</p>
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue';
import TarefaItem from './components/TarefaItem.vue';

const novaTarefa = ref('');
const tarefas = ref([]);
const filtro = ref('todas');
const editingTarefaId = ref(null);

function salvarTarefa() {
  if (novaTarefa.value.trim() === '') return;
  
  if (editingTarefaId.value) {
    const tarefa = tarefas.value.find(t => t.id === editingTarefaId.value);
    if (tarefa) {
      tarefa.texto = novaTarefa.value;
    }
    editingTarefaId.value = null;
  } else {
    tarefas.value.push({
      id: Date.now(),
      texto: novaTarefa.value,
      concluida: false
    });
  }
  
  novaTarefa.value = '';
}

function alternarTarefa(id) {
  const tarefa = tarefas.value.find(t => t.id === id);
  if (tarefa) {
    tarefa.concluida = !tarefa.concluida;
  }
}

function editarTarefa(id) {
  const tarefa = tarefas.value.find(t => t.id === id);
  if (tarefa) {
    novaTarefa.value = tarefa.texto;
    editingTarefaId.value = id;
  }
}

function deletarTarefa(id) {
  tarefas.value = tarefas.value.filter(t => t.id !== id);
  if (editingTarefaId.value === id) {
    editingTarefaId.value = null;
    novaTarefa.value = '';
  }
}

const tarefasPendentes = computed(() => 
  tarefas.value.filter(t => !t.concluida).length
);
const tarefasConcluidas = computed(() => 
  tarefas.value.filter(t => t.concluida).length
);

const tarefasFiltradas = computed(() => {
  if (filtro.value === 'pendentes') {
    return tarefas.value.filter(t => !t.concluida);
  } else if (filtro.value === 'concluidas') {
    return tarefas.value.filter(t => t.concluida);
  }
  return tarefas.value;
});

watch(tarefas, () => {
  localStorage.setItem('tarefas', JSON.stringify(tarefas.value));
}, { deep: true });

onMounted(() => {
  const tarefasSalvas = JSON.parse(localStorage.getItem('tarefas')) || [];
  tarefas.value = tarefasSalvas;
});
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 40px auto;
  padding: 20px;
  background-color: #2c2c2c;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  color: #f0f0f0;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

input {
  width: calc(100% - 130px);
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #3a3a3a;
  color: #f0f0f0;
}

.add-btn {
  width: 110px;
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #4a90e2;
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin-left: 10px;
}

.add-btn:hover {
  background-color: #357ABD;
}

.filtros {
  text-align: center;
  margin: 20px 0;
}

.filtros button {
  margin: 0 5px;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  background-color: #3a3a3a;
  color: #f0f0f0;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.filtros button.ativo {
  font-weight: bold;
  text-decoration: underline;
  background-color: #4a90e2;
}

.filtros button:hover {
  background-color: #357ABD;
}

ul {
  list-style: none;
  padding: 0;
}

p {
  text-align: center;
  margin-top: 20px;
}
</style>