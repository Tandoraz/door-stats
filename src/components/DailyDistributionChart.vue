<script setup>
import {Scatter} from 'vue-chartjs'
import {computed} from 'vue'

const props = defineProps({
  doorEvents: {
    type: Object,
    required: true
  }
})

const data = computed(() => {
  let events = props.doorEvents.events;
  if (events.length) {
    return {
      datasets: [{
        label: "Closed",
        data: events.filter(event => event.event === 'CLOSED').map(event => {
          return {x: event.created_at.getHours()*60 + event.created_at.getMinutes(), y: 1.4}
        }),
      }, {
        label: "Open",
        data: events.filter(event => event.event === 'OPEN').map(event => {
          return {x: event.created_at.getHours()*60 + event.created_at.getMinutes(), y: 1.6}
        }),
      }]
    }
  }
  return {
    datasets: [{
      label: "Closed",
      data: [],
    }, {
      label: "Open",
      data: []
    }]
  }
})

const options = {
  responsive: true,
  scales: {
    x: {
      max: 24*60,
      min: 0,
      type: 'linear',
      ticks: {
        callback: function(value) {
          return `${Math.floor(value / 60)}:${value % 60}`
        }
      }
    },
    y: {
      display: false,
      max: 2,
      min: 1
    }
  },
  plugins: {
    legend: {
      position: 'top',
    },
    title: {
      display: true,
      text: 'daily distribution Chart'
    }
  }
}

</script>

<template>
  <div style="width: 600px">
    <Scatter
        :data="data"
        :options="options"
    ></Scatter>
  </div>
</template>

<style scoped>

</style>
