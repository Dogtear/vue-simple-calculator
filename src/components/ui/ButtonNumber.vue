<script setup lang="ts">
type ButtonProps = {
  numberDisplay: string
  // isOperator?: boolean
}

type ButtonEmits = {
  (e: 'clickFunction'): void
}

const props = defineProps<ButtonProps>()

const emit = defineEmits<ButtonEmits>()

// const actionAddStr = () => {
//   emit('response', props.numberDisplay)
// }

const actionClick = () => {
  emit('clickFunction')
}
</script>
<template>
  <button
    class="cursor-pointer flex justify-center items-center w-[calc(450/4)] h-20 bg-slate-900 hover:bg-slate-950"
    @click="actionClick"
  >
    <p class="text-2xl">
      {{ props.numberDisplay }}
    </p>
  </button>
</template>
