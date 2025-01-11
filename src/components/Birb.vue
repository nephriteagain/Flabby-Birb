
<script setup lang="ts">
defineProps<{ x: number; y: number; size: number }>();
</script>

<template>
  <div
    id="birb"
    class="absolute z-10"
    :style="{ 
        left: `${x}px`, 
        top: `${y}px` , 
        width: `${size}px`, 
        height: `${size}px`, 
        backgroundColor:'yellow'
    }"
  ></div>
</template>
