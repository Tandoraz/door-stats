<script setup>
import {onMounted} from "vue";

const props = defineProps({
  activeOption: {
    type: Number,
    required: true
  }
});

const emit = defineEmits(['option-changed']);

const options = [
  {id: 0, text: "last month", date: dateMinusDays(30)},
  {id: 1, text: "last 7 days", date: dateMinusDays(7)},
  {id: 2, text: "last day", date: dateMinusDays(1)},
]

onMounted(() => {
  changeOption(options[props.activeOption]);
})

function dateMinusDays(days) {
  let date = new Date();
  let daysInMillis = (24*60*60*1000) * days;
  date.setTime(date.getTime() - daysInMillis);
  return date;
}

function changeOption(option) {
  console.debug("selected option:", option);
  emit('option-changed', {date: option.date, label: option.text});
}
</script>

<template>
  <div class="options">
    <span v-for="option in options" @click="changeOption(option); activeOption = option.id" :class="{ active: activeOption === option.id}">{{ option.text }}</span>
  </div>
</template>

<style scoped>
.options {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.options span {
  padding: .4rem 1rem;
  border: 1px solid var(--color-text);
  border-radius: 999px;
  cursor: pointer;
}

.options span:hover {
  border-color: hsla(160, 100%, 37%, 1);
}

.options span.active {
  border-color: hsla(160, 100%, 37%, 1);
  color: hsla(160, 100%, 37%, 1);
}
</style>
