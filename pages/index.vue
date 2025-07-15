<template>
  <div class="p-4">
    <GenerationFilter :selected="selectedGen" @change="onGenChange" />
    <v-container>
      <v-row>
        <PokemonCard
          v-for="poke in pokemons"
          :key="poke.pokedex_id"
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
  data() {
    return {
      selectedGen: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      pokemons: [],
    }
  },
  async mounted() {
    await this.loadPokemons()
  },
  methods: {
    async loadPokemons() {
      try {
        const res = await axios.get('https://tyradex.vercel.app/api/v1/pokemon')
        const all = res.data
        this.pokemons = all.filter(p => this.selectedGen.includes(p.generation))
      } catch (error) {
        console.error('Erreur lors du chargement des pokémons :', error)
      }
    },
    async onGenChange(gen) {
      this.selectedGen = Array.isArray(gen) ? gen : [gen]
      await this.loadPokemons()
    },
  }
}
</script>
