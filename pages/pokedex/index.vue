<script setup lang="ts">

useHead({
    title: 'Pokédex'
})

const { results: pokemons } = await $fetch('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=1025', {
    method: 'get'
})
const pokemonList = pokemons.map((pokemon: any, index: number) => ({ name: pokemon.name, id: index + 1 }))

const searchQuery = ref('')

const filteredPokemons = computed(() => {
    return pokemonList.filter((pokemon: any) => 
        pokemon.name.toLowerCase().includes(searchQuery.value.toLowerCase())
    )
})

</script>

<template>
    <div class="w-full max-w-7xl mx-auto p-4">

        <div class="flex justify-center items-center my-8">
            <div class="flex flex-col items-center">
                <h1 class="text-4xl font-bold text-red-600">Pokédex</h1>
            </div>
        </div>

        <div class="mb-4">
            <input v-model="searchQuery" type="text" placeholder="Search Pokémon..."
                class="w-full p-2 border rounded-lg" />
        </div>

        <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
            <div v-for="(pokemon) in filteredPokemons" :key="pokemon.id">
                <NuxtLink :to="`/pokedex/${pokemon.name}`"
                    class="flex flex-col items-center justify-center transition-all hover:bg-gray-100">
                    <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.id}.png`"
                        width="96" height="96" alt="">

                    <span class="text-sm font-bold">{{ padLeadingZeros(pokemon.id) }}</span>
                    <span class="text-md font-bold">{{ toCapitalization(pokemon.name) }}</span>
                </NuxtLink>
            </div>
        </div>
    </div>
</template>
