<script setup lang="ts">
useHead({
  titleTemplate: (titlePage) => {
    return titlePage
      ? `${titlePage} | Pokémon`
      : 'Pokémon'
  }
})
</script>

<template>
  <div>
    <NuxtPage />
  </div>
</template>
