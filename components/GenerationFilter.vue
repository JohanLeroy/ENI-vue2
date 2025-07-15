<template>
  <v-btn-toggle
    v-model="internalSelected"
    mandatory
    class="mb-4"
  >
    <v-btn :value="0">Tout</v-btn>
    <v-btn v-for="gen in generations" :key="gen" :value="gen">
      Génération {{ gen }}
    </v-btn>
  </v-btn-toggle>
</template>

<script>
export default {
  props: {
    selected: {
      type: [Number],
      default: 0
    }
  },
  data() {
    return {
      generations: [1,2,3,4,5,6,7,8,9],
      internalSelected: this.selected
    }
  },
  watch: {
    selected(newVal) {
      if(newVal !== this.internalSelected) {
        this.internalSelected = newVal
      }
    },
    internalSelected(newVal) {
      this.$emit('change', newVal)
    }
  }
}
</script>
