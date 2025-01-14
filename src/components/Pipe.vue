<script setup lang="ts">
defineProps<{ x: number; topY: number; bottomY: number; id: number }>();
</script>

<template>
  <div
    :id="`top-pipe-${id}`"
    class="absolute border border-black"
    :style="{ left: `${x}px`, top: 0 , width: '100px', height: `${600-topY}px`, backgroundImage: 'linear-gradient(to right, rgb(74 222 128), rgb(21 128 61))', opacity: 0.8}"
  >
  <div class="absolute border-2 border-black w-full h-8 bottom-0 bg-gradient-to-r from-green-300 to-green-800"
  >
  </div>
</div>
  <div
    :id="`bot-pipe-${id}`"
    class="absolute border border-black"
    :style="{ left: `${x}px`, bottom: 0, width: '100px', height: `${bottomY}px`, backgroundImage: 'linear-gradient(to right, rgb(74 222 128), rgb(21 128 61))', }"
  >
  <div class="absolute border-2 border-black w-full h-8 top-0 bg-gradient-to-r from-green-300 to-green-800"
  >
  </div>
  </div>
</template>
