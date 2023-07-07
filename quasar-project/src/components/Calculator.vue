<template>
  <div class="calculator">
    <div class="display">
      <q-input outlined readonly dense input-class="text-right" v-model="dashboard" />
    </div>
    <div class="button_container">
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="deep-orange" label="C" @click="reset" />
        <q-btn class="glossy cal-btn" rounded color="deep-orange" label="<-" @click="deleteLastOne" />
        <q-btn class="glossy cal-btn" rounded color="teal" label="×" @click="multiplicationSign" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="teal" label="+" @click="plusSign" />
        <q-btn class="glossy cal-btn" rounded color="teal" label="-" @click="minusSign" />
        <q-btn class="glossy cal-btn" rounded color="teal" label="÷" @click="divisionSign" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="primary" label="7" @click="pushNumber('7')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="8" @click="pushNumber('8')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="9" @click="pushNumber('9')" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="primary" label="4" @click="pushNumber('4')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="5" @click="pushNumber('5')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="6" @click="pushNumber('6')" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="primary" label="1" @click="pushNumber('1')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="2" @click="pushNumber('2')" />
        <q-btn ref="btn3" class="glossy cal-btn" rounded color="primary" label="3" @click="pushNumber('3')" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="primary" label="0" @click="pushZero()" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="." @click="pushDecimalPoint()" />
        <q-btn class="glossy cal-btn" rounded color="teal" label="=" @click="calculate" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, onUnmounted, ref } from 'vue'

export default defineComponent({
  name: 'VictorCalculator',
  setup() {
    const dashboard = ref<string>('0')
    const memory = ref<string>('')
    const lastSign = ref<string>('')

    const pushNumber = (str: string) => {
      if (dashboard.value[0] === '0' && dashboard.value.length === 1) {
        dashboard.value = str
      } else {
        dashboard.value += str
      }
    }

    const pushZero = () => {
      if (dashboard.value[0] !== '0') {
        dashboard.value += '0'
      } else {
        if (dashboard.value.length !== 1) {
          dashboard.value += '0'
        }
      }
    }

    const pushDecimalPoint = () => {
      if (dashboard.value !== '' && dashboard.value.indexOf('.') === -1) {
        dashboard.value += '.'
      }
    }

    const reset = () => {
      dashboard.value = '0'
      cleanMemoryAndLastSign()
    }

    const deleteLastOne = () => {
      if (dashboard.value[0] !== '0' || dashboard.value.length !== 1) {
        dashboard.value = dashboard.value.slice(0, -1)
      }

      if (dashboard.value === '') {
        dashboard.value = '0'
      }
    }

    const plusSign = () => {
      memory.value = dashboard.value
      dashboard.value = '0'
      lastSign.value = '+'
    }

    const minusSign = () => {
      memory.value = dashboard.value
      dashboard.value = '0'
      lastSign.value = '-'
    }

    const multiplicationSign = () => {
      memory.value = dashboard.value
      dashboard.value = '0'
      lastSign.value = '*'
    }

    const divisionSign = () => {
      memory.value = dashboard.value
      dashboard.value = '0'
      lastSign.value = '/'
    }

    const calculate = () => {
      try {
        if (lastSign.value !== '') {
          let pow = 1

          const memoryDotIndex = memory.value.indexOf('.')
          const dashboardDotIndex = dashboard.value.indexOf('.')

          if (memoryDotIndex !== -1 && memoryDotIndex >= dashboardDotIndex) {
            pow = Math.pow(10, memory.value.length - memoryDotIndex - 1)
          } else if (memoryDotIndex !== -1 && memoryDotIndex < dashboardDotIndex) {
            pow = Math.pow(10, dashboard.value.length - dashboardDotIndex - 1)
          }

          const targetA = Number(memory.value) * pow
          const targetB = Number(dashboard.value) * pow
          console.log(pow, 'target ', targetA, lastSign.value, targetB)
          // 연산
          const calResult = calNaturalNumber(targetA, targetB, pow)
          // 메모리, 연산 초기화
          cleanMemoryAndLastSign()

          console.log('isnon', isNaN(calResult))
          if (isNaN(calResult)) {
            dashboard.value = '숫자아님'
          } else {
            dashboard.value = calResult.toString()
          }
        }
      } catch (e) {
        console.log('calculate', e)
      }
    }

    const calNaturalNumber = (targetA: number, targetB: number, pow: number) => {
      try {
        if (lastSign.value === '+') {
          return (targetA + targetB) / pow
        } else if (lastSign.value === '-') {
          return (targetA - targetB) / pow
        } else if (lastSign.value === '*') {
          return (targetA * targetB) / pow ** 2
        } else if (lastSign.value === '/') {
          if (targetB !== 0) {
            return targetA / targetB
          } else {
            return NaN
          }
        } else {
          return 0
        }
      } catch (e) {
        console.log('calNaturalNumber', e)
        return 0
      }
    }

    const cleanMemoryAndLastSign = () => {
      memory.value = ''
      lastSign.value = ''
    }

    const handleKeyDown = (event: any) => {
      const key = event.key
      const keyCode = event.keyCode
      if (/^[1-9]$/.test(key)) {
        pushNumber(key)
      } else if (/^0$/.test(key)) {
        pushZero()
      } else if (/\+/.test(key)) {
        plusSign()
      } else if (/\-/.test(key)) {
        minusSign()
      } else if (/\*/.test(key)) {
        multiplicationSign()
      } else if (/\//.test(key)) {
        divisionSign()
      } else if (/\=/.test(key) || keyCode === 13) {
        calculate()
      } else if (keyCode === 27) {
        reset()
      } else if (keyCode === 8) {
        deleteLastOne()
      } else if (keyCode === 190) {
        pushDecimalPoint()
      } else {
        console.log('Hi i am calculator')
      }
    }

    onMounted(() => {
      window.addEventListener('keydown', handleKeyDown)
    })

    onUnmounted(() => {
      window.removeEventListener('keydown', handleKeyDown)
    })

    const state = { dashboard }
    const action = {
      pushNumber,
      pushZero,
      pushDecimalPoint,
      reset,
      deleteLastOne,
      plusSign,
      minusSign,
      multiplicationSign,
      divisionSign,
      calculate,
      handleKeyDown
    }
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

.display {
  background-color: white;
  margin: 1.5rem 1.5rem 1rem 1.5rem;
}

.button_container {
  margin-bottom: 0.5rem;
}

.calculator {
  background-color: rgb(255, 251, 245);
  border: 1px solid black;
  border-radius: 5%;
}
</style>
