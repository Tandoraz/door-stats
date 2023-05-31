<script setup>
import {ArcElement, BarElement, CategoryScale, Chart as ChartJS, Colors, Legend, LinearScale, LineElement, PointElement, Title, Tooltip} from 'chart.js'
import {supabase} from "@/supabase.service";
import {reactive, ref} from 'vue'
import TimeOpenChart from "@/components/TimeOpenChart.vue";
import Error from "@/components/Error.vue";
import OptionsSelector from "@/components/OptionsSelector.vue";
import HowManyEventsChart from "@/components/HowManyEventsChart.vue";
import HowManyAlarmsChart from "@/components/HowManyAlarmsChart.vue";
import HowManyEventsOverDaysChart from "@/components/HowManyEventsPerDaysChart.vue";
import DailyDistributionChart from "@/components/DailyDistributionChart.vue";

ChartJS.register(Title, Tooltip, Legend, CategoryScale, LinearScale, PointElement, ArcElement, LineElement, Colors, BarElement);

const errorMessage = ref("");
const doorEvents = reactive({
  events: [],
  startDate: new Date(),
  selectionLabel: ""
});

async function loadEvents(event) {
  const {data, error} = await supabase
      .from("doorEvents")
      .select()
      .gte("created_at", event.date.toISOString());

  if (error) {
    console.error("failed to load door events.", error);
    errorMessage.value = "Failed to load door events: " + error.message;
    return;
  }
  doorEvents.events = data.map(d => {
    return {created_at: new Date(d.created_at), event: d.event}
  });
  doorEvents.startDate = event.date;
  doorEvents.selectionLabel = event.label;
}

</script>

<template>
  <header>
    <h1>Door Stats</h1>
    <OptionsSelector :active-option="1" @option-changed="loadEvents"></OptionsSelector>
    <Error :msg="errorMessage"></Error>
  </header>

  <main>
    <DailyDistributionChart :doorEvents="doorEvents"></DailyDistributionChart>
    <HowManyEventsOverDaysChart :doorEvents="doorEvents"></HowManyEventsOverDaysChart>
    <TimeOpenChart :doorEvents="doorEvents"></TimeOpenChart>
    <HowManyEventsChart :door-events="doorEvents"></HowManyEventsChart>
    <HowManyAlarmsChart :door-events="doorEvents"></HowManyAlarmsChart>
  </main>
</template>

<style scoped>

h1 {
  text-align: center;
  margin-bottom: 1.5rem;
}

main {
  display: flex;
  place-items: center;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
