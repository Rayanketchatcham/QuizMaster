<template>
  <div :style="{ display: 'flex', alignItems: 'center', gap: '08px' }">
    <div>
      <slot name="icone"></slot>
    </div>
    <div
      :style="{
        display: 'flex',
        flexDirection: 'row',
        alignItems: 'center',
        gap: '6px',
      }"
    >
      <p :style="{ fontSize: '25px', color: 'green' }">{{ title }}</p>
      <p>{{ subTitle }}</p>
    </div>
  </div>
</template>
<script setup>
defineProps({
  title: String,
  subTitle: String,
});
</script>
