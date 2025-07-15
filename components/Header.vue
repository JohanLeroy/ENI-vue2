<template>
  <v-app-bar app color="deep-purple accent-4" dark>
    <v-toolbar-title @click="$router.push('/')" class="text-white">Mon pokédex</v-toolbar-title>
  </v-app-bar>
</template>
<script>
export default {
  props: ['pokemon']
}
</script>
