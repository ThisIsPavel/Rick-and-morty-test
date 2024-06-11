<script setup>
import { defineProps } from "vue";
defineProps({
  text: [String, Number],
});
</script>

<template>
  <button class="btn">{{ text }}</button>
</template>

<style scoped>
.btn {
  padding: 1rem;
  border-radius: 0.8rem;
  transition: all 0.3s;
  cursor: pointer;
  background: rgb(60, 62, 68);
  color: white;
}
.btn:hover {
  background: grey;
}
</style>
