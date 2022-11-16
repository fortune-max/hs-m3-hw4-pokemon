<script setup>
    import { ref, watch } from 'vue';

    const props = defineProps(["pokemonName"]);

    const response = ref({});
    const searching = ref(false);
    const errorOccurred = ref(false);
    const invalidPokemon = ref(false);

    watch(()=>props.pokemonName, async ()=>{
        searching.value = true;
        response.value = await getPokemon(props.pokemonName);
        searching.value = false;
    });

    async function getPokemon(pokemon){
        let res = await fetch(`https://pokeapi.co/api/v2/pokemon/${pokemon}`);
        invalidPokemon.value = res.status === 404;
        errorOccurred.value = !res.ok;
        if (res.ok) return await res.json();
        return {};
    }

    response.value = await getPokemon(props.pokemonName);
    
</script>

<template>
    <div v-if="searching">
        Loading...
    </div>
    <div v-else-if="invalidPokemon">
        Invalid Pokémon 🧐
    </div>
    <div v-else-if="errorOccurred">
        Some error occured
    </div>
    <div v-else>
        <img :src="response.sprites.other['official-artwork'].front_default"><br/>
        Name: {{response.name}}<br/>
        Height: {{response.height}}dm<br/>
        Weight: {{response.weight}}hg<br/>
        Type: {{response.types[0].type.name}}<br/>
        Moves list:
        <ul>
            <li v-for="ability in response.abilities">{{ability.ability.name}}</li>
        </ul>
    </div>
</template>

<style scoped>
    ul {
        margin: 0;
    }
</style>