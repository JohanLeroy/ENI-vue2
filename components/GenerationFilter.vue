<template>
  <v-btn-toggle
    v-model="internalSelected"
    mandatory
    class="mb-4 overflow-x-auto flex no-scrollbar"
  >
    <v-btn
      :value="0"
      class="flex-shrink-0 min-w-[80px] sm:min-w-[100px]"
    >
      Tout
    </v-btn>

    <v-btn
      v-for="gen in generations"
      :key="gen"
      :value="gen"
      class="flex-shrink-0 min-w-[80px] sm:min-w-[100px]"
    >
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

<style>
.no-scrollbar::-webkit-scrollbar {
  display: none;
}
.no-scrollbar {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
</style>
