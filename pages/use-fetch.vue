<template>
    <h1 class="text-white font-bold py-24 text-center text-3xl">Use Fetch</h1>
    <div class=" flex justify-center gap-7">
        <button class="inline-block rounded bg-emerald-500 px-5 py-3 text-sm font-medium text-white" @click="refresh">
            Recarregar Dados</button>
        <select class="bg-emerald-500 px-5 py-3 text-sm font-medium text-white outline-none" v-model="selectedPokemon"
            name="pokemnon">
            <option value="charmander">Charmander</option>
            <option value="bulbasaur">Bulbasaur</option>
            <option value="pikachu">Pikachu</option>
        </select>
    </div>
    <div class=" flex justify-center gap-7">
        <div class=" text-emerald-500 my-6" v-if="pending">Carregando</div>
        <div v-else-if="error">Error: {{ error.message }}</div>
        <pre v-else class="text-white">
                {{ charmander }}
        </pre>
    </div>
    <back-home />
</template>
    
<script setup>
import { computed } from "vue";

const selectedPokemon = ref('charmander');
const endpoint = computed(() => {
    return `https://pokeapi.co/api/v2/pokemon/${selectedPokemon.value}`;
});

const { data: charmander, pending, error, refresh }
    = await useFetch(endpoint, {
        method: "GET",
        headers: {
            "Content-Type": "application/json",
        },
        transform: (data) => ({
            id: data.id,
            name: data.name,
            Image: data.sprites.front_default,
        })
    });
</script>