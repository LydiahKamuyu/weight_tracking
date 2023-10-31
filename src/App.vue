<script setup>
import {ref, shallowRef, computed, watch, nextTick} from 'vue'
import Chart from 'chart.js/auto'

const weights = ref([])
const weightChartE1 = ref(null)
const weightChart = shallowRef(null)
const weightInput = ref(60.0)
const currentweight = computed(() => {
  return weights.value.sort((a,b) => b.date - a.date)[0] || {weight:0}
})

const addWeight = () => {
  weights.value.push({
    weight: weightInput.value,
    date: new Date().getTime()
  })
}

</script>

<template>
<main>
  <h1>Weight Tracker</h1>
  <div class="current">
    <span>{{ currentweight.weight }}</span>
    <small> Current weight (kg)</small>
    <small>Your Weight</small>
  </div>

  <form @submit.prevent="addWeight">
    <input type="number" step="0.1" v-model="weightInput">
    <input type="submit" value="Add Weight">
  </form>

  <div v-if="weights && weights.length > 0">
  <h2>Last 5 days</h2>

  <div class="canvas-box">
    <canvas ref="weightChartE1"> </canvas>
  </div>

  <div class="weight-history">
    <h2>Weight History</h2>
    <ul>
      <li v-for="weight in weights">
        <span>{{ weight.weight }}</span>
        <small>{{ new Date(weight.date).toLocaleDateString() }}</small>
      </li>
    </ul>
  </div>
  </div>

</main>

</template>

<style scoped>

</style>
