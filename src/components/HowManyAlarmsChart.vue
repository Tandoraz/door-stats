<script setup>
import {computed} from 'vue'

const props = defineProps({
  doorEvents: {
    type: Object,
    required: true
  }
})

const FIVE_MIN = 1000 * 60 * 5;

const count = computed(() => {
  let events = props.doorEvents.events;
  if (events.length) {
    let alarms = 0;
    events.forEach((event, i) => {
      if (events.length-1 === i) return;
      let next = events[i + 1];
      if (event.event === "OPEN" && next.event === "CLOSED") {
         if ((next.created_at.getTime() - event.created_at.getTime()) > FIVE_MIN) {
           alarms++;
         }
      }
    });
    return alarms
  }
  return 0;
})

</script>

<template>
  <div style="height: 300px; width: 300px">
    <h4>How many Door alarms the {{doorEvents.selectionLabel}}</h4>
    <span class="count">
      {{count}}
    </span>
  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

h4 {
 text-align: center;
}

.count {
  text-align: center;
  font-size: 8rem;
}
</style>
