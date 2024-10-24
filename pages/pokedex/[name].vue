<script setup lang="ts">

const route = useRoute()

const pokemon = await $fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.name}`, {
    method: 'get'
})

const { name, sprites, types } = pokemon

const formattedName = toCapitalization(name)

useHead({
    title: formattedName
})

const router = useRouter();

const currentPokemonId = pokemon.id;

const prevPokemonId = currentPokemonId > 1 ? currentPokemonId - 1 : null;
const nextPokemonId = currentPokemonId < 1025 ? currentPokemonId + 1 : null;

const navigateToPokemon = async (id) => {
    if (id) {
        const response = await $fetch(`https://pokeapi.co/api/v2/pokemon/${id}`);
        router.push(`/pokedex/${response.name}`);
    }
};

</script>

<template>
    <div class="absolute top-4 left-4">
        <NuxtLink to="/pokedex"
            class="bg-white text-red-600 px-4 py-2 rounded-full shadow-md hover:bg-gray-100 transition-colors duration-200">
            &larr; Back to Pokédex
        </NuxtLink>
    </div>

    <div class="w-full h-screen flex items-center justify-center p-4">
        <div class="bg-red-600 rounded-lg shadow-lg p-6 max-w-4xl w-full">
            <div class="flex flex-col md:flex-row items-center">
                <div class="w-full md:w-1/2 flex justify-center">
                    <img :src="sprites.other['official-artwork'].front_default" :alt="formattedName"
                        class="w-64 h-64 object-contain" />
                </div>
                <div class="w-full md:w-1/2 text-white">
                    <h1 class="text-4xl font-bold mb-2">{{ formattedName }}</h1>
                    <p class="text-xl mb-4">{{ padLeadingZeros(pokemon.id) }}</p>
                    <div class="flex flex-wrap gap-2 mb-4">
                        <span v-for="type in types" :key="type.slot"
                            class="px-3 py-1 rounded-full text-sm font-semibold" :class="[`bg-${type.type.name}`]">
                            {{ toCapitalization(type.type.name) }}
                        </span>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div v-for="stat in pokemon.stats" :key="stat.stat.name"
                            class="bg-white bg-opacity-20 rounded p-2">
                            <p class="text-sm font-semibold">{{ toCapitalization(stat.stat.name) }}</p>
                            <p class="text-lg">{{ stat.base_stat }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="flex justify-between">
        <button v-if="prevPokemonId" @click="navigateToPokemon(prevPokemonId)"
            class="absolute bottom-4 left-4 bg-white text-red-600 px-4 py-2 rounded-full shadow-md hover:bg-gray-100 transition-colors duration-200">
            &larr; Previous
        </button>
        <button v-if="nextPokemonId" @click="navigateToPokemon(nextPokemonId)"
            class="absolute bottom-4 right-4 bg-white text-red-600 px-4 py-2 rounded-full shadow-md hover:bg-gray-100 transition-colors duration-200">
            Next &rarr;
        </button>
    </div>
</template>