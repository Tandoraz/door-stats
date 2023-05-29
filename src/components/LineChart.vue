<script setup>
import {Line} from 'vue-chartjs'
defineProps({
  doorEvents: {
    type: Object,
    required: true
  }
})

const data = {
  labels: ['January', 'February', 'March'],
  datasets: [{data: [40, 20, 12]}]
}

const options = {
  responsive: true
}

</script>

<template>
  <div style="width: 600px">
    <Line
        :data="data"
        :options="options"
    ></Line>
  </div>
</template>

<style scoped>

</style>
