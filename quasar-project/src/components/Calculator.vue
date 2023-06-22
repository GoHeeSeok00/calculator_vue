<template>
  <div>
    <div>계산기</div>
    <div>
      <q-input rounded outlined v-model="calResult" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="deep-orange" label="C" />
      <q-btn class="glossy cal-btn" rounded color="deep-orange" label="<-" />
      <q-btn class="glossy cal-btn" rounded color="teal" label="*" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="teal" label="+" />
      <q-btn class="glossy cal-btn" rounded color="teal" label="-" />
      <q-btn class="glossy cal-btn" rounded color="teal" label="%" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="primary" label="7" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="8" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="9" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="primary" label="4" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="5" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="6" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="primary" label="1" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="2" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="3" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="primary" label="0" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="." />
      <q-btn class="glossy cal-btn" rounded color="teal" label="=" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'VictorCalculator',
  setup() {
    const calResult = ref('')
    return { calResult }
  }
})
</script>

<style scoped lang="scss">
.cal-btn {
  margin: 0.5rem;
  width: 5rem;
  height: 1rem;
}
</style>
