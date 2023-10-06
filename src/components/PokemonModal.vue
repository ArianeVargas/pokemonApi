<template>
  <div
    class="fixed inset-0 z-50 flex items-center justify-center overflow-x-hidden overflow-y-auto outline-none focus:outline-none"
  >
  <div class="relative w-500px max-w-3xl mx-auto">
      <!-- Modal content -->
      <div
      class="relative flex flex-col w-full bg-white border-0 rounded-lg shadow-lg outline-none focus:outline-none"
      >
      <div
          class="flex items-center justify-center p-6 border-b border-solid border-blueGray-200 rounded-b"
        >
        <h1 class="text-3xl text-center font-semibold">{{ pokemon.nombre }}</h1>
        </div>
        <!-- Contenido horizontal -->
        <div class="p-3 flex items-center justify-between">
          <!-- Columna izquierda (Información básica) -->
          <div class="flex-auto md:w-1/3 ">
            <h2>#ID: {{ pokemon.numero }}</h2>
            <p>Altura: {{ pokemon.altura }}</p>
            <p>Peso: {{ pokemon.peso }}</p>
            <p>Tipo: {{ pokemon.tipo_pk }}</p>
          </div>

          <!-- Columna central (Imagen centrada) -->
          <div class="md:w-1/2 flex items-center justify-center bg-white">
            <img
              :src="pokemon.img"
              alt="Pokemon Image"
              class="w-30 h-30 mx-auto mb-4"
            />
          </div>

          <!-- Columna derecha (Habilidades) -->
          <div class="flex-auto md:w-1/3 ">
            <p>Vida: {{ pokemon.hp }}</p>
            <div class="bg-gray-200 h-4 rounded-lg">
              <div
                class="bg-green-500 h-full rounded-lg"
                :style="{ width: `${(pokemon.hp / 255) * 100}%` }"
              ></div>
            </div>
            <p>Ataque: {{ pokemon.ataque }}</p>
            <div class="bg-gray-200 h-4 rounded-lg">
              <div
                class="bg-red-500 h-full rounded-lg"
                :style="{ width: `${(pokemon.ataque / 255) * 100}%` }"
              ></div>
            </div>
            <p>Defensa: {{ pokemon.defensa }}</p>
            <div class="bg-gray-200 h-4 rounded-lg">
              <div
                class="bg-blue-500 h-full rounded-lg"
                :style="{ width: `${(pokemon.defensa / 255) * 100}%` }"
              ></div>
            </div>
            <p>Velocidad: {{ pokemon.velocidad }}</p>
            <div class="bg-gray-200 h-4 rounded-lg">
              <div
                class="bg-yellow-500 h-full rounded-lg"
                :style="{ width: `${(pokemon.velocidad / 255) * 100}%` }"
              ></div>
            </div>
          </div>
        </div>

        <!-- Footer -->
        <div
          class="flex items-center justify-end p-6 border-t border-solid border-blueGray-200 rounded-b"
        >
          <button
            @click="closeModal"
            class="px-6 py-2 mb-1 mr-1 text-sm font-bold text-red-500 uppercase transition-all duration-150 ease-linear bg-transparent border border-red-500 rounded outline-none hover:bg-red-500 hover:text-white active:bg-red-600 focus:outline-none"
            type="button"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
  
  
  <script setup>
import { ref, defineProps, defineEmits } from "vue";

const props = defineProps(["pokemon"]);
const emit = defineEmits();

const closeModal = () => {
  emit("close-modal");
};
</script>
  