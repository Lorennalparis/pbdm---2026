<script setup lang="ts">
import { ref, computed } from "vue";

let listaId = 0;
let tarefaId = 0;

const novoTitulo = ref("");

const listas = ref([
  {
    id: listaId++,
    titulo: "Estudos",
    novaTarefa: "",
    esconderConcluidas: false,
    tarefas: [
      { id: tarefaId++, texto: "Aprender HTML", done: true },
      { id: tarefaId++, texto: "Aprender JavaScript", done: false },
      { id: tarefaId++, texto: "Aprender Vue", done: false }
    ]
  }
]);

function criarLista() {
  if (!novoTitulo.value.trim()) return;

  listas.value.push({
    id: listaId++,
    titulo: novoTitulo.value,
    novaTarefa: "",
    esconderConcluidas: false,
    tarefas: []
  });

  novoTitulo.value = "";
}

function adicionarTarefa(lista: any) {
  if (!lista.novaTarefa.trim()) return;

  lista.tarefas.push({
    id: tarefaId++,
    texto: lista.novaTarefa,
    done: false
  });

  lista.novaTarefa = "";
}

function removerTarefa(lista: any, tarefa: any) {
  lista.tarefas = lista.tarefas.filter((t: any) => t.id !== tarefa.id);
}

function removerLista(id: number) {
  listas.value = listas.value.filter((l) => l.id !== id);
}

function tarefasFiltradas(lista: any) {
  return lista.esconderConcluidas
    ? lista.tarefas.filter((t: any) => !t.done)
    : lista.tarefas;
}
</script>

<template>
  <h1>Gerenciador de Listas de Tarefas</h1>

  <div class="nova-lista">
    <input
      v-model="novoTitulo"
      placeholder="Título da nova lista"
    />
    <button @click="criarLista">
      Criar Lista
    </button>
  </div>

  <div
    class="lista"
    v-for="lista in listas"
    :key="lista.id"
  >
    <h2>{{ lista.titulo }}</h2>

    <form @submit.prevent="adicionarTarefa(lista)">
      <input
        v-model="lista.novaTarefa"
        placeholder="Nova tarefa"
        required
      />
      <button>Adicionar</button>
    </form>

    <ul>
      <li
        v-for="tarefa in tarefasFiltradas(lista)"
        :key="tarefa.id"
      >
        <input
          type="checkbox"
          v-model="tarefa.done"
        />

        <span :class="{ done: tarefa.done }">
          {{ tarefa.texto }}
        </span>

        <button @click="removerTarefa(lista, tarefa)">
          X
        </button>
      </li>
    </ul>

    <button
      @click="lista.esconderConcluidas = !lista.esconderConcluidas"
    >
      {{ lista.esconderConcluidas
        ? "Mostrar todas"
        : "Ocultar concluídas" }}
    </button>

    <button class="remover" @click="removerLista(lista.id)">
      Remover Lista
    </button>
  </div>
</template>

<style scoped>
body {
  font-family: Arial, Helvetica, sans-serif;
}

h1 {
  text-align: center;
}

.nova-lista {
  margin-bottom: 20px;
}

.lista {
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 15px;
  margin-bottom: 20px;
}

ul {
  padding-left: 20px;
}

li {
  margin: 8px 0;
}

.done {
  text-decoration: line-through;
  color: gray;
}

button {
  margin-left: 5px;
}

.remover {
  margin-top: 10px;
  background-color: crimson;
  color: white;
}
</style>
