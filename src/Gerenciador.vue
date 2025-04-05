<script lang="ts" setup>
import { computed, reactive, ref, watch } from "vue";
import Cards from "./components/Cards.vue";
import ModalTarefa from "@/components/ModalTarefa.vue";

import type { Tarefa } from "@/types/Tarefa";

const modalVisivel = ref(false);
const isEdicao = ref(false);
const indiceEdicao = ref<number | null>(null);

const tarefa = ref<Tarefa>({
  titulo: "",
  descricao: "",
  dt_vencimento: "",
  concluida: false,
});

const tarefasSalvas = ref<Tarefa[]>([]);
const tarefasArmazenadas = localStorage.getItem("tarefas");
if (tarefasArmazenadas) {
  tarefasSalvas.value = JSON.parse(tarefasArmazenadas);
}

watch(
  tarefasSalvas,
  (novas) => {
    localStorage.setItem("tarefas", JSON.stringify(novas));
  },
  { deep: true }
);

function abrirModalNovaTarefa() {
  tarefa.value = {
    titulo: "",
    descricao: "",
    dt_vencimento: "",
    concluida: false,
  };
  indiceEdicao.value = null;
  isEdicao.value = false;
  modalVisivel.value = true;
}

function editarTarefa(index: number) {
  tarefa.value = { ...tarefasSalvas.value[index] };
  indiceEdicao.value = index;
  isEdicao.value = true;
  modalVisivel.value = true;
}

function salvarTarefa(t: Tarefa) {
  if (indiceEdicao.value !== null) {
    tarefasSalvas.value[indiceEdicao.value] = t;
  } else {
    tarefasSalvas.value.push(t);
  }

  modalVisivel.value = false;
  tarefa.value = {
    titulo: "",
    descricao: "",
    dt_vencimento: "",
    concluida: false,
  };
  indiceEdicao.value = null;
  isEdicao.value = false;
}

function excluirTarefa(index: number) {
  tarefasSalvas.value.splice(index, 1);
}

function alternarStatus(index: number) {
  tarefasSalvas.value[index].concluida = !tarefasSalvas.value[index].concluida;
}

const estado = reactive({
  filtroStatus: "todas",
  termoBusca: "",
});

const tarefasFiltradas = computed(() => {
  let filtradas = tarefasSalvas.value;

  if (estado.filtroStatus !== "todas") {
    filtradas = filtradas.filter((t) =>
      estado.filtroStatus === "concluida" ? t.concluida : !t.concluida
    );
  }

  if (estado.termoBusca.trim()) {
    const termo = estado.termoBusca.toLowerCase();
    filtradas = filtradas.filter(
      (t) =>
        t.titulo.toLowerCase().includes(termo) ||
        t.descricao.toLowerCase().includes(termo)
    );
  }

  return filtradas;
});
</script>

<template>
  <div
    class="flex flex-col items-center w-full min-h-screen text-white px-4 py-10"
  >
    <h1 class="text-3xl text-center text-blue-400 mb-8 pb-4">
      Gerenciador de Tarefas
    </h1>

    <div
      class="flex flex-col gap-4 bg-gray-800 p-6 rounded-xl shadow-md w-full max-w-xl mb-10"
    >
      <button
        @click="abrirModalNovaTarefa"
        class="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700 transition"
      >
        Nova Tarefa
      </button>
    </div>

    <div class="w-full max-w-5xl">
      <div class="flex justify-between items-center pb-4 pt-7">
        <h2 class="text-xl font-bold text-white mb-4 text-left pb-4">
          Tarefas Salvas
        </h2>

        <input
          v-model="estado.termoBusca"
          type="text"
          placeholder="Buscar por título ou descrição..."
          class="mb-4 p-2 rounded-full bg-gray-700 text-white w-64"
        />

        <select
          v-model="estado.filtroStatus"
          class="mb-4 p-2 rounded bg-gray-700 text-white rounded-xl"
        >
          <option value="todas">Todas</option>
          <option value="pendente">Pendentes</option>
          <option value="concluida">Concluídas</option>
        </select>
      </div>

      <div
        class="max-h-95 overflow-y-auto pr-2 scroll-smooth flex flex-col gap-4"
      >
        <div v-if="tarefasFiltradas.length === 0" class="text-gray-400 italic">
          {{
            estado.filtroStatus === "todas"
              ? "Nenhuma tarefa salva ainda."
              : estado.filtroStatus === "concluida"
              ? "Nenhuma tarefa concluída ainda."
              : "Nenhuma tarefa pendente ainda."
          }}
        </div>

        <Cards
          v-for="(t, index) in tarefasFiltradas"
          :key="index"
          :titulo="t.titulo"
          :descricao="t.descricao"
          :dt_vencimento="t.dt_vencimento"
          :concluida="t.concluida"
          :onEditar="() => editarTarefa(index)"
          :onExcluir="() => excluirTarefa(index)"
          :onToggleStatus="() => alternarStatus(index)"
        />
        <ModalTarefa
          :visible="modalVisivel"
          :tarefa="tarefa"
          :is-edicao="isEdicao"
          @fechar="modalVisivel = false"
          @salvar="salvarTarefa"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
::-webkit-scrollbar {
  width: 8px;
}
::-webkit-scrollbar-thumb {
  background-color: #4b5563;
  border-radius: 8px;
}
</style>
