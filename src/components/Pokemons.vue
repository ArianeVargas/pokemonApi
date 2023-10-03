<script setup>

    const props = defineProps(['resultados', 'buscar'])

    const pokemonSeleccionado = (pokemon) => {
        emit('set-pokemon', pokemon)
    };

</script>

<template>
    
    <div class="mt-6 flex justify-center">
            <div class="bg-yellow-400 rounded-2xl w-96 ">
                <p class="text-center py-10 text-5xl uppercase text-red-700 font-bold ">pokemons</p>

            </div>
        </div>

        <ul class="grid grid-cols-6 gap-4 text-lg mt-10">
            <li v-for="item in props.resultados" @click="pokemonSeleccionado(item)" class="border rounded-xl p-10 shadow">
                <div>
                    <img :src="item.img">
                </div>
                <h1 class="text-center font-semibold">
                    ID: {{ item.numero }}
                </h1>
                <h1 class="text-center font-semibold">
                    {{ item.nombre }}
                </h1>
                <ul class="flex space-x-2 mt-2">
                    <li v-for="(tipo, index) in item.tipo_pk" :key="index" class="rounded bg-blue-500 text-white px-2">
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