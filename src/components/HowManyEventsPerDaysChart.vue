<script setup>
import {Bar} from 'vue-chartjs'
import {computed} from 'vue'

const props = defineProps({
  doorEvents: {
    type: Object,
    required: true
  }
})

function getDateString(date) {
  return `${("0" + date.getDate()).slice(-2)}.${("0" + date.getMonth()+1).slice(-2)}.${date.getFullYear()}`;
}

function addToMap(map, key, closed) {
  let val = map.get(key) || { closed: 0, open: 0 };
  map.set(key, {
    closed: val.closed + (closed ? 1 : 0),
    open: val.open + (closed ? 0 : 1)
  })
}

const data = computed(() => {
  let events = props.doorEvents.events;
  if (events.length) {
    let eventsPerDay = new Map()
    events.forEach((event) => addToMap(eventsPerDay, getDateString(event.created_at), event.event === 'CLOSED'));

    return {
      labels: Array.from(eventsPerDay.keys()),
      datasets: [{
        label: "Closed",
        data: Array.from(eventsPerDay.values()).map(e => e.closed),
        borderWidth: 2,
        borderRadius: 5
      }, {
        label: "Open",
        data: Array.from(eventsPerDay.values()).map(e => e.open),
        borderWidth: 2,
        borderRadius: 5
      }]
    }
  }
  return {
    labels: [getDateString(new Date())],
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
      text: 'How many Open/Closed events per day Chart'
    }
  }
}

</script>

<template>
  <div style="width: 600px">
    <Bar
        :data="data"
        :options="options"
    ></Bar>
  </div>
</template>

<style scoped>

</style>
