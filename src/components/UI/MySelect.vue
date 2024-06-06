<template>
  <select :value="modelValue" @change="changeOption">
    <option disabled value="">Выберите из списка</option>
    <option v-for="option in options" :key="option.value" :value="option.value">
      {{ option.value }}
    </option>
  </select>
</template>

<script setup>
const props = defineProps({ modelValue: String, options: Array });

const emit = defineEmits(["update:modelValue"]);

function changeOption(event) {
  emit("update:modelValue", event.target.value);
}
</script>

<style scoped></style>
