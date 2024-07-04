<script setup lang="ts">
import { ref } from 'vue'
import ButtonNumber from '@/components/ui/ButtonNumber.vue'

const stringCal = ref('0')

const operatorList = ['*', '/', '+', '-', '%']

function clearAll() {
  stringCal.value = '0'
}

const handleResponse = (value: string) => {
  const isFirstStringZero = stringCal.value[0] === '0'

  const stringArr = splitStringWithOperators(stringCal.value, operatorList)

  const lastStrItem = stringArr[stringArr.length - 1]

  if (stringCal.value === '0' && operatorList.includes(value)) {
    return (stringCal.value = '0')
  }

  if (operatorList.includes(lastStrItem) && operatorList.includes(value)) {
    return
  }

  if (isFirstStringZero) {
    return (stringCal.value = value)
  }

  stringCal.value += value
  // console.log('string value :', stringCal.value)
}

const eraseLastChar = () => {
  const valStr = stringCal.value
  if (valStr.length === 1) {
    return (stringCal.value = '0')
  }
  stringCal.value = valStr.substring(0, valStr.length - 1)
}

const calucate = () => {
  const stringVal = stringCal.value.includes('%')
    ? stringCal.value.replace(`%`, '/100')
    : stringCal.value

  const calucateString = eval(stringVal).toString()

  stringCal.value = calucateString
}

// const postiveAndNegative = () => {
//   const dataArr = splitStringWithOperators(stringCal.value, operatorList)
//   const lastItem = dataArr[dataArr.length - 1]

//   if (stringCal.value === '0') {
//     return
//   }

//   if (!isNaN(+lastItem) && dataArr.length === 2) {
//     return (stringCal.value = dataArr[dataArr.length - 1])
//   }

//   if (!isNaN(+lastItem) && dataArr.length > 2) {
//     dataArr.pop()

//     if (!isNaN(+lastItem)) {
//       // console.log([...dataArr, `-${lastItem}`].join(''))
//       return (stringCal.value = [...dataArr, `-${lastItem}`].join(''))
//     }

//     return stringCal.value
//   }

//   if (isNaN(+lastItem)) {
//     return stringCal.value
//   }

//   return (stringCal.value = `-` + stringCal.value)
// }

function splitStringWithOperators(inputString: string, operators: string[]) {
  // Escape special characters in operators and join them into a regex-ready string
  const escapedOperators = operators
    .map((op) => op.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'))
    .join('|')
  const operatorRegex = new RegExp(`(${escapedOperators})`)

  // Split the string using the constructed regex and filter out empty strings
  const splitArray = inputString.split(operatorRegex).filter((str) => str !== '')

  return splitArray
}
</script>

<template>
  <div class="h-screen flex flex-col justify-center items-center">
    <div class="bg-slate-700 w-[450px]">
      <div class="h-20 flex items-center justify-end px-5">
        <p class="text-2xl">
          {{ stringCal }}
        </p>
      </div>
      <div class="grid grid-cols-4 bg-slate-700">
        <!-- <button
          class="cursor-pointer flex justify-center items-center w-[calc(450/4)] h-10 bg-slate-900 hover:bg-slate-950"
          @click="test('1')"
        >
          1
      </button > -->

        <!-- Fix this child  -->
        <ButtonNumber :numberDisplay="'C'" @click-function="clearAll" />
        <ButtonNumber :numberDisplay="'%'" @click-function="() => handleResponse('%')" />
        <ButtonNumber :numberDisplay="'/'" @click-function="() => handleResponse('/')" />
        <ButtonNumber :numberDisplay="'<'" @click-function="eraseLastChar()" />

        <ButtonNumber :numberDisplay="'1'" @click-function="() => handleResponse('1')" />
        <ButtonNumber :numberDisplay="'2'" @click-function="() => handleResponse('2')" />
        <ButtonNumber :numberDisplay="'3'" @click-function="() => handleResponse('3')" />
        <ButtonNumber :numberDisplay="'+'" @click-function="() => handleResponse('+')" />

        <ButtonNumber :numberDisplay="'4'" @click-function="() => handleResponse('4')" />
        <ButtonNumber :numberDisplay="'5'" @click-function="() => handleResponse('5')" />
        <ButtonNumber :numberDisplay="'6'" @click-function="() => handleResponse('6')" />
        <ButtonNumber :numberDisplay="'-'" @click-function="() => handleResponse('-')" />

        <ButtonNumber :numberDisplay="'7'" @click-function="() => handleResponse('7')" />
        <ButtonNumber :numberDisplay="'8'" @click-function="() => handleResponse('8')" />
        <ButtonNumber :numberDisplay="'9'" @click-function="() => handleResponse('9')" />
        <ButtonNumber :numberDisplay="'*'" @click-function="() => handleResponse('*')" />

        <ButtonNumber :numberDisplay="' '" />
        <ButtonNumber :numberDisplay="'0'" @click-function="() => handleResponse('0')" />
        <ButtonNumber :numberDisplay="' '" />
        <ButtonNumber :numberDisplay="'='" @click-function="calucate" />
      </div>
    </div>
  </div>
</template>
