<template>
  <button
    @click="$emit('clickButton')"
    style="
      padding: 8px;
      border-radius: 8px;
      color: green;
      font-weight: bold;
      font-size: 16px;
      background-color: white;
      border: 2px solid green;
    "
  >
    {{ label }}
  </button>
</template>
<script setup>
defineProps({
  label: String,
});
defineEmits(["clickButton"]);
</script>
