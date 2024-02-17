<template>
        <h1 class="text-white font-bold py-24 text-center text-3xl">Use Async Data</h1>
        <div class=" flex justify-center gap-7">
        <button class="inline-block rounded bg-emerald-500 px-5 py-3 text-sm font-medium text-white" @click="refresh"> Recarregar Dados</button>
        <select class="bg-emerald-500 px-5 py-3 text-sm font-medium text-white outline-none"  v-model="selectedPokemon" name="pokemnon">
            <option value="charmander">Charmander</option>
            <option value="bulbasaur">Bulbasaur</option>
            <option value="pikachu">Pikachu</option>
        </select>
        </div>
        <div class=" flex justify-center gap-7">

        <div class=" text-emerald-500 my-6" v-if="pending">Carregando</div>
        <div v-else-if="error">Error: {{ error.message }}</div>
        <pre v-else class="text-white">
                {{charmander}}
        </pre>
        </div>
        <back-home/>
       
    </template>
    
    <script setup>    
    const selectedPokemon = ref('charmander');
    
    const { data: charmander, pending, error, refresh } 
    = await useAsyncData(
        "pokemom-info",
        async () =>{
          const [pokemonData, speciesData] = await Promise.all([
              $fetch(`https://pokeapi.co/api/v2/pokemon/${selectedPokemon.value}`),
              $fetch(`https://pokeapi.co/api/v2/pokemon-species/${selectedPokemon.value}`),
          ]);
          return{
            id: pokemonData.id,
            name: pokemonData.name,
            image: pokemonData.sprites.front_default,
            habitat: speciesData.shape?.name
          };
        },
        {
            watch: [selectedPokemon],
            lazy: true,
        }
    );
    </script>
    