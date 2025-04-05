<script lang="ts" setup>
defineProps<{
  titulo: string;
  descricao: string;
  dt_vencimento: string;
  concluida: boolean;
  onEditar: () => void;
  onExcluir: () => void;
  onToggleStatus: () => void;
}>();
</script>

<template>
  <div
    class="relative bg-gray-800 p-5 rounded-xl shadow border border-gray-700 text-white"
  >
    <div class="absolute top-2 right-2 flex gap-2">
      <button
        @click="onEditar"
        class="text-yellow-400 hover:text-yellow-300 transition"
        title="Editar"
      >
        ✏️
      </button>
      <button
        @click="onExcluir"
        class="text-red-500 hover:text-red-400 transition"
        title="Excluir"
      >
        🗑️
      </button>
    </div>

    <p><strong>Título:</strong> {{ titulo }}</p>
    <p><strong>Descrição:</strong> {{ descricao }}</p>
    <p>
      <strong>Vencimento:</strong>
      {{ new Date(dt_vencimento).toLocaleDateString("pt-BR") }}
    </p>

    <div class="pt-2">
      <button
        @click="onToggleStatus"
        class="mt-4 text-sm px-3 py-1 rounded-full border transition"
        :class="
          concluida
            ? 'border-green-600 text-green-400 hover:bg-green-600 hover:text-white'
            : 'border-gray-400 text-gray-300 hover:bg-gray-600 hover:text-white'
        "
      >
        {{ concluida ? "Desfazer" : "Concluir" }}
      </button>
    </div>
  </div>
</template>
