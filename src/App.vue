<script setup>
import { onMounted, ref } from "vue";
import Pokemons from "./components/Pokemons.vue";

const pokemons = ref([]);
const resultados = ref([]);
const buscar = ref("");
const tipoSeleccionado = ref("");
const tiposPokemon = ref([]);

const cargarTiposPokemon = async () => {
  try {
    const response = await fetch("https://pokeapi.co/api/v2/type/");
    if (!response.ok) {
      throw new Error("No se pudo cargar la lista de tipos de Pokémon");
    }
    const data = await response.json();
    tiposPokemon.value = data.results.map((tipo) => tipo.name);
  } catch (error) {
    console.error(error);
  }
};

const filtrar = () => {
  if (buscar.value || tipoSeleccionado.value) {
    resultados.value = pokemons.value.filter((value) => {
      const porNombre = !buscar.value || value.nombre.includes(buscar.value);
      const porTipo =
        !tipoSeleccionado.value ||
        (value.tipo_pk && value.tipo_pk.includes(tipoSeleccionado.value));
      return porNombre && porTipo;
    });
  } else {
    resultados.value = pokemons.value;
  }
};

onMounted(async () => {
  await cargarTiposPokemon();

  for (let index = 1; index < 51; index++) {
    try {
      const response = await fetch(
        `https://pokeapi.co/api/v2/pokemon/${index}/`
      );
      if (!response.ok) {
        throw new Error(`No se pudo cargar el Pokémon número ${index}`);
      }
      const data = await response.json();
      const object = {
        img: data.sprites.other["official-artwork"].front_default,
        numero: data.id,
        nombre: data.name,
        altura: data.height,
        peso: data.weight,
        tipo_pk: data.types.map((pk) => pk.type.name),
        hp: data.stats[0].base_stat,
        ataque: data.stats[1].base_stat,
        defensa: data.stats[2].base_stat,
        ataque_especial: data.stats[3].base_stat,
        defensa_especial: data.stats[4].base_stat,
        velocidad: data.stats[5].base_stat,
      };
      pokemons.value.push(object);
    } catch (error) {
      console.error(error);
    }
  }

  resultados.value = pokemons.value;
});

const volverAInicio = () => {
  resultados.value = pokemons.value; // Restablece los resultados a la lista completa de Pokémon.
  buscar.value = ""; // Limpia el campo de búsqueda.
  tipoSeleccionado.value = ""; // Restablece la selección de tipo.
};
</script>

<template>

  <div>
    <!-- Botón de Inicio -->
    <button @click="volverAInicio" class="px-4 py-2 m-6 bg-blue-500 text-white rounded-full mt-4">
      Inicio
    </button>
  </div>
  <div class="p-4">
    <div class="mt-6 flex justify-center">
      <div class="bg-yellow-400 rounded-2xl w-96">
        <p class="text-center py-10 text-5xl uppercase text-red-700 font-bold">
          pokemons
        </p>
      </div>
    </div>
    <div
      class="flex flex-col justify-end sm:flex-row px-6 sm:items-center space-y-0 mt-6 sm:space-y-0 sm:space-x-4"
    >
      <input
        v-model="buscar"
        type="text"
        placeholder="Nombre del Pokémon"
        class="rounded-full border py-3 px-6 text-xl w-full sm:w-96"
      />
      <select
        v-model="tipoSeleccionado"
        class="rounded-full border py-3 px-6 text-xl w-full sm:w-auto"
      >
        <option value="">Todos los tipos</option>
        <option v-for="tipo in tiposPokemon" :value="tipo" :key="tipo">
          {{ tipo }}
        </option>
      </select>
      <button
        @click="filtrar"
        class="px-10 py-1 bg-green-600 text-white rounded-full"
      >
        Buscar
      </button>
    </div>
    <Pokemons :resultados="resultados" :buscar="buscar" />
  </div>
</template>
  