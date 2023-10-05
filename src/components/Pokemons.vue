<script setup>

    const props = defineProps(['resultados', 'buscar'])

    const pokemonSeleccionado = (pokemon) => {
        emit('set-pokemon', pokemon)
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
    
    <div class="mt-6 flex justify-center">
            <div class="bg-yellow-400 rounded-2xl w-96 ">
                <p class="text-center py-10 text-5xl uppercase text-red-700 font-bold ">pokemons</p>

            </div>
        </div>

        <ul class="grid grid-cols-6 gap-4 text-lg mt-10">
            <li v-for="item in props.resultados" :key="item.numero" @click="pokemonSeleccionado(item)" class="border rounded-xl p-10 shadow cursor-pointer">
                <div>
                    <img :src="item.img">
                </div>
                <h1 class="text-center font-semibold">
                    ID: {{ item.numero }}
                </h1>
                <h1 class="text-center font-semibold">
                    {{ item.nombre }}
                </h1>
                <ul class="flex justify-center space-x-2 mt-2">
                    <li v-for="(tipo, index) in item.tipo_pk" :key="index" :class="getColorClass(tipo)" class="rounded bg-blue-500 text-white px-2">
                        {{ tipo }}
                    </li>
                </ul>
            </li>

            <li v-if="resultados.length == 0">    
                <span class="text-red-600">
                    El pokemon {{ buscar.value }} no existe
                </span>
            </li>

        </ul>

</template>