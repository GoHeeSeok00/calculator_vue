<template>
  <div>
    <div>계산기</div>
    <div>
      <q-input rounded outlined readonly v-model="dashboard" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="deep-orange" label="C" @click="resetDashboard" />
      <q-btn class="glossy cal-btn" rounded color="deep-orange" label="<-" @click="deleteLastOne" />
      <q-btn class="glossy cal-btn" rounded color="teal" label="*" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="teal" label="+" />
      <q-btn class="glossy cal-btn" rounded color="teal" label="-" />
      <q-btn class="glossy cal-btn" rounded color="teal" label="%" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="primary" label="7" @click="pushNumber('7')" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="8" @click="pushNumber('8')" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="9" @click="pushNumber('9')" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="primary" label="4" @click="pushNumber('4')" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="5" @click="pushNumber('5')" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="6" @click="pushNumber('6')" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="primary" label="1" @click="pushNumber('1')" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="2" @click="pushNumber('2')" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="3" @click="pushNumber('3')" />
    </div>
    <div>
      <q-btn class="glossy cal-btn" rounded color="primary" label="0" @click="pushZero()" />
      <q-btn class="glossy cal-btn" rounded color="primary" label="." @click="pushDecimalPoint()" />
      <q-btn class="glossy cal-btn" rounded color="teal" label="=" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'VictorCalculator',
  setup() {
    const dashboard = ref<string>('')
    const memory = ref<string>('')
    const lastSign = ref<string>('')

    const pushNumber = (str: string) => {
      dashboard.value += str
      console.log('dashboard ', dashboard.value)
    }

    const pushZero = () => {
      if (dashboard.value !== '') {
        dashboard.value += '0'
      }
      console.log('dashboard ', dashboard.value)
    }

    const pushDecimalPoint = () => {
      console.log(dashboard.value.indexOf('.'))
      if (dashboard.value !== '' && dashboard.value.indexOf('.') === -1) {
        dashboard.value += '.'
        console.log('dashboard ', dashboard.value)
      }
    }

    const resetDashboard = () => {
      dashboard.value = ''
      console.log('dashboard ', dashboard.value)
    }

    const deleteLastOne = () => {
      dashboard.value = dashboard.value.slice(0, -1)
      console.log('dashboard ', dashboard.value)
    }

    const plusSign = () => {
      memory.value = dashboard.value
      dashboard.value = ''
      lastSign.value = '+'
    }

    const minusSign = () => {
      memory.value = dashboard.value
      dashboard.value = ''
      lastSign.value = '-'
    }

    const multiplicationSign = () => {
      memory.value = dashboard.value
      dashboard.value = ''
      lastSign.value = '*'
    }

    const divisionSign = () => {
      memory.value = dashboard.value
      dashboard.value = ''
      lastSign.value = '/'
    }

    const state = { dashboard }
    const action = { pushNumber, pushZero, pushDecimalPoint, resetDashboard, deleteLastOne, plusSign, minusSign, multiplicationSign, divisionSign }
    return { ...state, ...action }
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
