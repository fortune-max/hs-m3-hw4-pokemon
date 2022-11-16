<script setup>
  import { ref } from 'vue';
  import PokemonCard from './components/PokemonCard.vue';
  
  const textbox = ref("eevee");
  const pokemonName = ref(textbox.value);

  function searchPokemon(){
    pokemonName.value = textbox.value;
  }

</script>

<template>
  <input placeholder="Enter Pokémon name" v-model="textbox"/>
  <button @click="searchPokemon">Search</button>
  <Suspense>
    <PokemonCard v-model:pokemon-name="pokemonName" />
    <template #fallback>
      <div>Loading...</div>
    </template>
  </Suspense>
</template>
