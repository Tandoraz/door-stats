<script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="error" v-if="msg">
    <span>{{ msg }}</span>
    <span class="close" @click="msg = ''">×</span>
  </div>
</template>

<style scoped>
.error {
  display: flex;
  margin: 1.5rem 0;
  border: 1px solid hsl(351, 100%, 37%);
  border-radius: 1rem;
  padding: 1rem 1.5rem;
  color: hsl(351, 100%, 37%);
}

.error span:first-child {
  flex-grow: 1;
}

.close {
  cursor: pointer;
  font-size: 2rem;
  line-height: 24px;
}
</style>
