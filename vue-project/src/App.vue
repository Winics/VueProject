<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue'
import Formulario from './components/Formulario.vue';
import ListaDeTarefa from './components/ListaDeTarefa.vue';
  const estado = reactive({
    filtro: 'todas',
    tarefaTemp:``,
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
        titulo: 'Academia',
        finalizada: true,
      }
    ]
  })

  const getTarefasPendente = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }


  const getTarefasFiltradas = () => {
    const {filtro} = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendente();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  }

  // const castraTarefa = (e) => {
  //   e.preventDefault()
    
  const castraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas = [...estado.tarefas, tarefaNova];
    console.log("Nova lista de tarefas:", estado.tarefas);
    estado.tarefaTemp = '';

  }
</script>

<template>
  <div class="container">
    <Cabecalho 
    :tarefas-pendentes="getTarefasPendente().length"/>
    <Formulario 
    :trocar-filtro="evento => estado.filtro = evento.target.value"
    :tarefa-temp="estado.tarefaTemp"
    :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value"
    :cadastra-tarefa="castraTarefa"
    />
    <ListaDeTarefa 
    :tarefas="getTarefasFiltradas()" />
  </div>
</template>
