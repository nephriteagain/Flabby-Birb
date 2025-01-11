<script setup lang="ts">
defineProps<{ x: number; topY: number; bottomY: number; id: number }>();
</script>

<template>
  <div
    :id="`top-pipe-${id}`"
    class="absolute"
    :style="{ left: `${x}px`, top: 0 , width: '100px', height: `${600-topY}px`, backgroundColor: 'green', opacity: 0.8}"
  ></div>
  <div
    :id="`bot-pipe-${id}`"
    class="absolute"
    :style="{ left: `${x}px`, bottom: 0, width: '100px', height: `${bottomY}px`, backgroundColor: 'green' }"
  ></div>
</template>
