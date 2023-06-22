<template>
  <q-page class="row items-center justify-evenly">
    <victor-calculator></victor-calculator>
  </q-page>
</template>

<script lang="ts">
import VictorCalculator from 'src/components/Calculator.vue'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  components: { VictorCalculator },
  setup() {
    return {}
  }
})
</script>
