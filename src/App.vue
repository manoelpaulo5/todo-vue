<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

  const state = reactive({
    filtro: 'todas',
    tarefaTmp: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false,
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false,
      },
      {
        titulo: 'Ir para a academia',
        finalizada: true,
      }
    ]
  })

  const getTarefasPendentes = () => {
    return state.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasFinalizadas = () => {
    return state.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltradas = () => {
    const { filtro } = state;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return state.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tareafaNova = {
      titulo: state.tarefaTmp,
      finalizada: false,
    }
    state.tarefas.push(tareafaNova);
    state.tarefaTmp = '';
  }
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario :trocar-filtro="evento => state.filtro = evento.target.value" :tarefa-tmp="state.tarefaTmp" :edita-tarefa-tmp="evento => state.tarefaTmp = evento.target.value" :cadastra-tarefa="cadastraTarefa" />
    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/>
  </div>
</template>
