<template>
  <div class="p-4">
    <GenerationFilter :selected="selectedGen" @change="onGenChange" />

    <v-container>
      <v-row>
        <v-col cols="12" class="text-center" v-if="loading">
          <v-progress-circular indeterminate color="primary" size="48" />
        </v-col>

        <PokemonCard
          v-for="poke in pokemons"
          :key="poke.pokedex_id"
          :pokemon="poke"
          v-if="!loading"
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
      selectedGen: 0,
      pokemons: [],
      loading: false,
    }
  },
  async mounted() {
    await this.loadPokemons()
  },
  methods: {
    async loadPokemons() {
      this.loading = true
      try {
        const res = await axios.get('https://tyradex.vercel.app/api/v1/pokemon')
        const all = res.data
        if (this.selectedGen === 0) {
          this.pokemons = all
        } else {
          this.pokemons = all.filter(p => p.generation === this.selectedGen)
        }
      } catch (error) {
        console.error('Erreur lors du chargement des pokémons :', error)
        this.pokemons = []
      } finally {
        this.loading = false
      }
    },
    async onGenChange(gen) {
      this.selectedGen = gen
      await this.loadPokemons()
    }
  }
}
</script>
