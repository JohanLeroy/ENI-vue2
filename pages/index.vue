<template>
  <div class="p-4" @scroll.passive="onScroll" ref="scrollContainer" style="height: 80vh; overflow-y: auto;">
    <GenerationFilter :selected="selectedGen" @change="onGenChange" />
    <v-container>
      <v-row>
        <PokemonCard
          v-for="poke in displayedPokemons"
          :key="poke.pokedex_id"
          :pokemon="poke"
        />
        <v-col cols="12" class="text-center" v-if="loadingMore">
          <v-progress-circular indeterminate color="primary" size="48" />
        </v-col>
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
      allPokemons: [],
      displayedPokemons: [],
      loading: false,
      loadingMore: false,
      batchSize: 20,
      offset: 0,
      noMoreData: false,
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
        this.allPokemons = this.selectedGen === 0 ? all : all.filter(p => p.generation === this.selectedGen)
        this.offset = 0
        this.noMoreData = false
        this.displayedPokemons = []
        this.loadMore()
      } catch (error) {
        console.error('Erreur lors du chargement des pokémons :', error)
        this.allPokemons = []
        this.displayedPokemons = []
      } finally {
        this.loading = false
      }
    },
    loadMore() {
      if (this.noMoreData) return

      this.loadingMore = true
      setTimeout(() => {
        const nextBatch = this.allPokemons.slice(this.offset, this.offset + this.batchSize)
        this.displayedPokemons.push(...nextBatch)
        this.offset += this.batchSize
        if (this.offset >= this.allPokemons.length) {
          this.noMoreData = true
        }
        this.loadingMore = false
      }, 500)
    },
    async onGenChange(gen) {
      this.selectedGen = gen
      await this.loadPokemons()
    },
    onScroll() {
      const container = this.$refs.scrollContainer
      if (!container) return
      if (container.scrollTop + container.clientHeight >= container.scrollHeight - 100) {
        if (!this.loadingMore && !this.noMoreData) {
          this.loadMore()
        }
      }
    }
  }
}
</script>
