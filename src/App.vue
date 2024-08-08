<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';
  
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
        finalizada:false,
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
    <Cabecalho :tarefas-pendentes="getTarefaPendente().length" />
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :nova-tarefa="estado.novaTarefa" :edita-tarefa="evento => estado.novaTarefa = evento.target.value" :cadastrar-tarefa="cadastrarTarefa"/>
    <ListaDeTarefas :tarefas="getTarefaFiltrada()"/>
  </div>
</template>