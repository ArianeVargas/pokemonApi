<script setup>

import { onMounted, ref } from 'vue'
import Pokemons from './components/Pokemons.vue';
import Pokemon from './components/Pokemon.vue';

const pokemons = ref([]);
const resultados = ref([]);
const buscar = ref('');

const filtrar = () => {

    if (buscar.value) {
        
        resultados.value = pokemons.value.filter(value => {
            return value.nombre == buscar.value 
        })

    }else{
        resultados.value = pokemons.value
    }
};

//este metodo se ejecuta cuando se carga el componente
onMounted(() => {

    for (let index = 1; index < 51; index++) {

        fetch(`https://pokeapi.co/api/v2/pokemon/${index}/`)
            .then(response => response.json())
            .then(data => {
                let object = {
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
                }

                pokemons.value.push(object)

            });

    }

    resultados.value = pokemons.value

})


</script>

<template>

    <div class="p-4">

        <div class="flex justify-end space-x-4">

            <input v-model="buscar" type="text" placeholder="Nombre del pokemon" class="rounded-full border py-3 px-6 text-xl w-96">

            <button @click="filtrar()" class="px-10 py-1 bg-green-600 text-white rounded-full">
                Buscar
            </button>

        </div>
        <!-- llamar el emit -->

        <Pokemons :resultados="resultados" :buscar="buscar"  />

        <Pokemon />
        
    </div>

</template>
