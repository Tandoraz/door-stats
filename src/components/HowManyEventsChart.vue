<script setup>
import {Bar} from 'vue-chartjs'
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
    let open = 0;
    let closed = 0;
    events.forEach((event) => event.event === 'CLOSED' ? closed++ : open++);
    return {
      labels: [props.doorEvents.selectionLabel],
      datasets: [{
        label: "Closed",
        data: [closed],
        borderWidth: 2,
        borderRadius: 5
      }, {
        label: "Open",
        data: [open],
        borderWidth: 2,
        borderRadius: 5
      }]
    }
  }
  return {
    labels: [props.doorEvents.selectionLabel],
    datasets: [{
      label: "Closed",
      data: [0],
      borderWidth: 2,
      borderRadius: 5
    }, {
      label: "Open",
      data: [0],
      borderWidth: 2,
      borderRadius: 5
    }]
  }
})

const options = {
  responsive: true,
  plugins: {
    legend: {
      position: 'top',
    },
    title: {
      display: true,
      text: 'How many Open/Closed events Chart'
    }
  }
}

</script>

<template>
  <div style="width: 500px">
    <Bar
        :data="data"
        :options="options"
    ></Bar>
  </div>
</template>

<style scoped>

</style>
