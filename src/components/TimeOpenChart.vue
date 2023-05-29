<script setup>
import {Doughnut} from 'vue-chartjs'
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
    [{created_at: props.doorEvents.startDate, event: "CLOSED"}, ...events].reduce((acc, current) => {
      if (acc.event === current.event) return acc;
      let time = current.created_at.getTime() - acc.created_at.getTime();
      acc.event === "CLOSED" ? closed += time : open += time;
      return current;
    });
    return {
      labels: ['Closed', 'Open'],
          datasets: [{data: [Math.floor(closed / 1000), Math.floor(open / 1000)]}]
    }
  }
  return {
    labels: ['Closed', 'Open'],
    datasets: [{data: [0, 0]}]
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
      text: 'Time Open/Closed in (s) Chart'
    }
  }
}

</script>

<template>
  <div style="height: 400px">
    <Doughnut
        :data="data"
        :options="options"
    ></Doughnut>
  </div>
</template>

<style scoped>

</style>
