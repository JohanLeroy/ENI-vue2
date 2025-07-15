<template>
  <v-container class="py-10">
    <v-card v-if="pokemon" class="mx-auto max-w-xl p-6">
      <h1 class="text-3xl font-bold mb-4">{{ pokemon.name.fr }}</h1>
      <img :src="pokemon.sprites.regular" class="w-64 mx-auto mb-4" />
      <p><strong>Génération :</strong> {{ pokemon.generation.name }}</p>
      <p><strong>Types :</strong> {{ pokemon.types.map(t => t.name).join(', ') }}</p>
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  async asyncData({ params }) {
    const { data } = await axios.get(`https://tyradex.vercel.app/api/v1/pokemon/${params.id}`)
    return { pokemon: data }
  }
}
</script>
