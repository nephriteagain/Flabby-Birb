<script setup lang="ts">
defineProps<{ score: number }>();
</script>

<template>
  <div class="absolute top-[100px] left-1/2 -translate-x-1/2 z-50">
    <h1 
      class="text-5xl text-white font-bold" 
      style="text-shadow: -1px 1px 0 #000, 1px 1px 0 #000, 1px -1px 0 #000, -1px -1px 0 #000;"
    >
      {{ score }}
    </h1>
  </div>
</template>
