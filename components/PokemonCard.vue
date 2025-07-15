<template>
  <v-col cols="12" sm="4" md="3">
    <v-card class="hover:scale-105 transition" @click="$router.push('/pokemon/' + pokemon.pokedex_id)">
      <v-img :src="pokemon.sprites.regular" height="200px h-20" />
      <v-card-title>{{ pokemon.name.fr }}</v-card-title>
    </v-card>
  </v-col>
</template>

<script>
export default {
  props: ['pokemon']
}
</script>
