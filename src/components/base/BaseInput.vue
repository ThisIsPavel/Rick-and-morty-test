<script setup>
import { defineProps, defineEmits } from "vue";

defineProps({
  id: Number,
  label: String,
  type: String,
  placeholder: String,
  modelValue: String,
});

const emit = defineEmits("update:modelValue");

const onInput = (e) => {
  emit("update:modelValue", e.target.value);
};
</script>

<template>
  <div class="form-input">
    <label v-if="label" :for="id" class="label">
      {{ label }}
    </label>
    <input
      :id="id"
      :type="type"
      :placeholder="placeholder"
      :value="modelValue"
      @change="onInput"
      class="input"
    />
  </div>
</template>

<style scoped>
.form-input {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1rem;
}
.label {
  height: 100%;
  font-size: 1.8rem;
  color: white;
  margin-right: 1rem;
}
.input {
  padding: 0.5rem;
  height: 100%;
}
@media (max-width: 548px) {
  .form-input {
    width: 100%;
  }
  .input {
    width: 100%;
  }
}
</style>
