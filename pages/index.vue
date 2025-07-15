<template>
  <div class="p-4">
    <GenerationFilter :selected="selectedGen" @change="onGenChange" />
    <v-container>
      <v-row>
        <PokemonCard
          v-for="poke in pokemons"
          :key="poke.id"
          :pokemon="poke"
        />
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios'
import PokemonCard from '@/components/PokemonCard.vue'
import GenerationFilter from '@/components/GenerationFilter.vue'

export default {
  components: { PokemonCard, GenerationFilter },
  data: () => ({
    selectedGen: 1,
    pokemons: [],
  }),
  async mounted() {
    await this.loadPokemons()
  },
  methods: {
    async loadPokemons() {
      const res = await axios.get('https://tyradex.vercel.app/api/v1')
      const all = res.data
      this.pokemons = all.filter(p => p.generation.id === this.selectedGen)
    },
    async onGenChange(gen) {
      this.selectedGen = gen
      await this.loadPokemons()
    },
  }
}
</script>
