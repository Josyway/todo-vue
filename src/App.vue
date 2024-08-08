<script setup>
import { reactive } from 'vue';
  
const estado = reactive ({
    filtro: 'todas',
    novaTarefa: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada:false,
      },
      {
        titulo: 'Estudar SASS',
        finalizada:false,
      },
      {
        titulo: 'Ir para a academia',
        finalizada:true,
      },
    ]
  })

  const getTarefaPendente = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefaFinalizada = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefaFiltrada = () => {
    const { filtro } = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefaPendente();
      case 'finalizadas':
        return getTarefaFinalizada();
      default:
        return estado.tarefas;
      }
  }

  const cadastrarTarefa = () => {
    const tarefaNova ={
        titulo: estado.novaTarefa,
        finalizada:false,
      }
    estado.tarefas.push(tarefaNova)
    estado.novaTarefa = '';
  }

</script>

<template>

  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>Você possui {{ getTarefaPendente().length }} tarefas restantes</p>
    </header>
  </div>
  <div class="container">
    <form @submit.prevent="cadastrarTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.novaTarefa" @change="evento => estado.novaTarefa = evento.target.value" required type="text" placeholder="Digite a sua tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas as tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefaFiltrada()">
        <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
        <label :class="{done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.done{
  text-decoration: line-through;
}

</style>
