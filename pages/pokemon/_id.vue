<template>
  <v-container class="py-10">
    <v-card v-if="pokemon" class="mx-auto max-w-xl p-6">
      <h1 class="text-3xl font-bold mb-4">{{ pokemon.name.fr }}</h1>
      <v-carousel hide-delimiter-background height="400" class="mb-6">
        <v-carousel-item
          v-for="(sprite, index) in availableSprites"
          :key="index"
        >
          <img :src="sprite.url" :alt="sprite.label" class="mx-auto h-80" />
          <div class="text-center mt-2 font-semibold">{{ sprite.label }}</div>
        </v-carousel-item>
      </v-carousel>
      <p><strong>Génération :</strong> {{ pokemon.generation }}</p>
      <div class="mb-4">
        <strong>Types :</strong>
        <span v-for="type in pokemon.types" :key="type.name" class="inline-flex items-center mr-3">
          <img :src="type.image" alt="" class="w-6 h-6 mr-1" />
          {{ type.name }}
        </span>
      </div>
      <div class="mb-4">
        <strong>Talents :</strong>
        <ul>
          <li v-for="talent in pokemon.talents" :key="talent.name">
            {{ talent.name }} <span v-if="talent.tc">(Talent caché)</span>
          </li>
        </ul>
      </div>
      <div class="mb-4">
        <strong>Stats :</strong>
        <v-simple-table dense>
          <tbody>
          <tr><td>HP</td><td>{{ pokemon.stats.hp }}</td></tr>
          <tr><td>Attaque</td><td>{{ pokemon.stats.atk }}</td></tr>
          <tr><td>Défense</td><td>{{ pokemon.stats.def }}</td></tr>
          <tr><td>Attaque Spéciale</td><td>{{ pokemon.stats.spe_atk }}</td></tr>
          <tr><td>Défense Spéciale</td><td>{{ pokemon.stats.spe_def }}</td></tr>
          <tr><td>Vitesse</td><td>{{ pokemon.stats.vit }}</td></tr>
          </tbody>
        </v-simple-table>
      </div>
      <div class="mb-4">
        <strong>Résistances :</strong>
        <div class="flex flex-wrap gap-3">
          <span
            v-for="res in pokemon.resistances"
            :key="res.name"
            :class="resistanceClass(res.multiplier)"
            class="px-2 py-1 rounded text-white font-semibold"
            :title="`Multiplicateur : ${res.multiplier}`"
          >
            {{ res.name }}
          </span>
        </div>
      </div>
      <div class="mb-4">
        <strong>Taille :</strong> {{ pokemon.height }}<br />
        <strong>Poids :</strong> {{ pokemon.weight }}<br />
        <strong>Sexe :</strong> ♂ {{ pokemon.sexe.male }}% / ♀ {{ pokemon.sexe.female }}%<br />
        <strong>Taux de capture :</strong> {{ pokemon.catch_rate }}<br />
        <strong>Groupes d'œufs :</strong> {{ pokemon.egg_groups.join(', ') }}
      </div>
    </v-card>
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  async asyncData({ params }) {
    const { data } = await axios.get(`https://tyradex.vercel.app/api/v1/pokemon/${params.id}`)
    return { pokemon: data }
  },
  computed: {
    availableSprites() {
      const sprites = []
      if (this.pokemon.sprites.regular)
        sprites.push({ url: this.pokemon.sprites.regular, label: 'Normal' })
      if (this.pokemon.sprites.shiny)
        sprites.push({ url: this.pokemon.sprites.shiny, label: 'Shiny' })
      if (this.pokemon.sprites.gmax)
        sprites.push({ url: this.pokemon.sprites.gmax, label: 'Gigamax' })
      return sprites
    }
  },
  methods: {
    resistanceClass(multiplier) {
      if (multiplier === 0) return 'bg-red-700'
      if (multiplier < 1) return 'bg-green-600'
      if (multiplier === 1) return 'bg-gray-500'
      if (multiplier > 1) return 'bg-yellow-600'
      return 'bg-gray-500'
    }
  }
}
</script>

<style scoped>
/* Pour adapter la hauteur du carousel */
.v-carousel-item img {
  object-fit: contain;
}
</style>
