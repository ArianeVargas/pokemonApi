<script setup>
import { ref, defineProps, defineEmits } from "vue";
import PokemonModal from "./PokemonModal.vue";

const props = defineProps(["resultados"]);
const { emit } = defineEmits();

const isOpen = ref(false);
const selectedPokemon = ref(null);

const openPokemonModal = (pokemon) => {
  selectedPokemon.value = pokemon;
  isOpen.value = true;
};

const closeModal = () => {
  isOpen.value = false;
};

/* color para los tipos */
function getColorClass(tipo) {
  switch (tipo) {
    case "fire":
      return "bg-red-500 text-white";
    case "grass":
      return "bg-green-500 text-white";
    case "water":
      return "bg-blue-500 text-white";
    case "electric":
      return "bg-yellow-500 text-black"; // Tipo eléctrico
    case "ice":
      return "bg-blue-200 text-black"; // Tipo hielo
    case "fighting":
      return "bg-red-700 text-white"; // Tipo lucha
    case "poison":
      return "bg-purple-500 text-white"; // Tipo veneno
    case "bug":
      return "bg-orange-900 text-white"; // Tipo insecto
    case "flying":
      return "bg-teal-300 text-white"; // Tipo volador
    case "ground":
      return "bg-green-900 text-white"; // Tipo suelo
    // Agrega más casos según los tipos que existan en la API
    default:
      return "bg-gray-500 text-white"; // Clase predeterminada en caso de que el tipo no tenga una clase específica
  }
}
</script>


<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-6 gap-4 text-lg mt-10">
    <div
      v-for="pokemon in resultados"
      :key="pokemon.numero"
      @click="openPokemonModal(pokemon)"
      class="cursor-pointer"
    >
      <div
        class="border p-4 rounded shadow hover:shadow-lg transition duration-300"
      >
        <img :src="pokemon.img" alt="Pokemon Image" class="mx-auto" />
        <h2 class="text-center font-semibold text-lg">{{ pokemon.nombre }}</h2>
        <!-- Muestra el tipo_pk con colores -->
        <div class="flex justify-center space-x-2 mt-2">
          <span
            v-for="(tipo, index) in pokemon.tipo_pk"
            :key="index"
            :class="getColorClass(tipo)"
            class="rounded bg-blue-500 text-white px-2"
          >
            {{ tipo }}
          </span>
        </div>
      </div>
    </div>
  </div>

  <!-- Display the modal when isOpen is true -->
  <PokemonModal
    v-if="isOpen"
    :pokemon="selectedPokemon"
    @close-modal="closeModal"
  />
</template>