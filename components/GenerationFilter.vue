<template>
  <v-select
    :items="generations"
    v-model="internalSelected"
    label="Filtrer par génération"
  />
</template>

<script>
export default {
  props: ['selected'],
  data() {
    return {
      generations: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      internalSelected: this.selected
    }
  },
  watch: {
    selected(newVal) {
      this.internalSelected = newVal
    },
    internalSelected(newVal) {
      this.$emit('change', newVal)
    }
  }
}
</script>
