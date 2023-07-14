<template>
  <div class="calculator">
    <div class="display">
      <q-input outlined readonly dense input-class="text-right" v-model="dashboard" />
    </div>
    <div class="button_container">
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="deep-orange" label="C" @click="calculator('C')" />
        <q-btn class="glossy cal-btn" rounded color="deep-orange" label="<-" @click="calculator('<')" />
        <q-btn class="glossy cal-btn" rounded color="teal" label="×" @click="calculator('*')" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="teal" label="+" @click="calculator('+')" />
        <q-btn class="glossy cal-btn" rounded color="teal" label="-" @click="calculator('-')" />
        <q-btn class="glossy cal-btn" rounded color="teal" label="÷" @click="calculator('/')" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="primary" label="7" @click="calculator('7')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="8" @click="calculator('8')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="9" @click="calculator('9')" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="primary" label="4" @click="calculator('4')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="5" @click="calculator('5')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="6" @click="calculator('6')" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="primary" label="1" @click="calculator('1')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="2" @click="calculator('2')" />
        <q-btn ref="btn3" class="glossy cal-btn" rounded color="primary" label="3" @click="calculator('3')" />
      </div>
      <div class="button_content">
        <q-btn class="glossy cal-btn" rounded color="primary" label="0" @click="calculator('0')" />
        <q-btn class="glossy cal-btn" rounded color="primary" label="." @click="calculator('.')" />
        <q-btn class="glossy cal-btn" rounded color="teal" label="=" @click="calculator('=')" />
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
    let history = ''

    const NUMBER_LIST = ['1', '2', '3', '4', '5', '6', '7', '8', '9']
    const SIGN_LIST = ['+', '-', '*', '/']

    const calculator = (input: string) => {
      if (input === '0') {
        pushZero()
      } else if (input === 'C') {
        reset()
      } else if (input === '.') {
        pushDecimalPoint()
      } else if (input === '<') {
        deleteLastOne()
      } else if (input === '=') {
        calculate()
      } else if (NUMBER_LIST.includes(input)) {
        pushNumber(input)
      } else if (SIGN_LIST.includes(input)) {
        sign(input)
      }

      history += input
      console.log(history)
    }

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

    const sign = (sign: string) => {
      history += sign
      console.log(history, history.slice(-1))
      memory.value = dashboard.value
      dashboard.value = '0'

      if (sign === '+') {
        plusSign()
      } else if (sign === '-') {
        minusSign()
      } else if (sign === '*') {
        multiplicationSign()
      } else if (sign === '/') {
        divisionSign()
      }
    }

    const plusSign = () => {
      lastSign.value = '+'
    }

    const minusSign = () => {
      lastSign.value = '-'
    }

    const multiplicationSign = () => {
      lastSign.value = '*'
    }

    const divisionSign = () => {
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
        calculator(key)
      } else if (/^0$/.test(key)) {
        calculator('0')
      } else if (/\+/.test(key)) {
        calculator('+')
      } else if (/\-/.test(key)) {
        calculator('-')
      } else if (/\*/.test(key)) {
        calculator('*')
      } else if (/\//.test(key)) {
        calculator('/')
      } else if (/\=/.test(key) || keyCode === 13) {
        calculator('=')
      } else if (keyCode === 27) {
        calculator('C')
      } else if (keyCode === 8) {
        calculator('<')
      } else if (keyCode === 190 || keyCode === 110) {
        calculator('.')
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
    const action = { calculator }
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
