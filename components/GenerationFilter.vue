<template>
  <v-select
    :items="generations"
    v-model="internalSelected"
    label="Filtrer par génération"
    multiple
  />
</template>

<script>
export default {
  props: {
    selected: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      generations: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      internalSelected: []
    }
  },
  created() {
    this.internalSelected = this.selected.length ? [...this.selected] : [...this.generations]
  },
  watch: {
    selected(newVal) {
      if (JSON.stringify(newVal) !== JSON.stringify(this.internalSelected)) {
        this.internalSelected = [...newVal]
      }
    },
    internalSelected(newVal) {
      this.$emit('change', newVal)
    }
  }
}
</script>
