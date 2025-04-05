<template>
  <div
    v-if="visible"
    class="fixed inset-0 bg-black bg-opacity-60 flex justify-center items-center z-50"
  >
    <div class="bg-gray-800 p-6 rounded-xl shadow-lg w-full max-w-md">
      <h2 class="text-xl font-bold mb-4 text-white pb-2">
        {{ isEdicao ? "Editar Tarefa" : "Nova Tarefa" }}
      </h2>

      <div class="flex flex-col gap-4">
        <input
          v-model="localTarefa.titulo"
          type="text"
          placeholder="Título"
          class="p-2 rounded bg-gray-700 text-white border border-gray-600"
        />
        <input
          v-model="localTarefa.descricao"
          type="text"
          placeholder="Descrição"
          class="p-2 rounded bg-gray-700 text-white border border-gray-600"
        />
        <input
          v-model="localTarefa.dt_vencimento"
          type="date"
          class="p-2 rounded bg-gray-700 text-white border border-gray-600"
        />
      </div>

      <div class="mt-6 flex justify-end gap-2 pt-3">
        <button
          class="px-4 py-2 bg-gray-600 rounded hover:bg-gray-700 text-white"
          @click="$emit('fechar')"
        >
          Cancelar
        </button>
        <button
          class="px-4 py-2 bg-blue-600 rounded hover:bg-blue-700 text-white"
          @click="emitirSalvar"
        >
          {{ isEdicao ? "Atualizar" : "Salvar" }}
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, watch, defineProps, defineEmits } from "vue";
import type { Tarefa } from "@/types/Tarefa";

const props = defineProps<{
  tarefa: Tarefa;
  visible: boolean;
  isEdicao: boolean;
}>();

const emit = defineEmits<{
  (e: "salvar", tarefa: Tarefa): void;
  (e: "fechar"): void;
}>();

const localTarefa = ref<Tarefa>({ ...props.tarefa });

watch(
  () => props.tarefa,
  (nova) => {
    localTarefa.value = { ...nova };
  }
);

function emitirSalvar() {
  if (
    !localTarefa.value.titulo ||
    !localTarefa.value.descricao ||
    !localTarefa.value.dt_vencimento
  ) {
    alert("Preencha todos os campos!");
    return;
  }

  emit("salvar", { ...localTarefa.value });
}
</script>

<style>
input[type="date"]::-webkit-calendar-picker-indicator {
  filter: invert(1);
}
</style>
