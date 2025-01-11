
<script setup lang="ts">
defineProps<{ score: number; best:number}>();

const emit = defineEmits(["handleRestart"])
</script>

<template>
  <div
    id="game-over"
    class="absolute z-20 top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 flex flex-col items-center h-[400px] w-[400px] gap-y-12"
  >
  <div class="flex flex-col gap-y-4">
    <div class="flex flex-col items-center gap-y-3 bg-blue-300 px-8 py-4 rounded-xl shadow-md">
        <h2 class="text-xl text-orange-500 font-semibold">Score</h2>
        <h2 class="text-3xl font-bold text-white"
      style="text-shadow: -1px 1px 0 #000, 1px 1px 0 #000, 1px -1px 0 #000, -1px -1px 0 #000;"
        >{{ score }}</h2>
    </div>
    <div class="flex flex-col items-center gap-y-3 bg-blue-300 px-8 py-4 rounded-xl shadow-md">
        <h2 class="text-xl text-orange-500 font-semibold">Best</h2>
        <h2 class="text-3xl font-bold text-white"
      style="text-shadow: -1px 1px 0 #000, 1px 1px 0 #000, 1px -1px 0 #000, -1px -1px 0 #000;"
        >{{ best }}</h2>
    </div>
  </div>
  <div class="bg-white p-2 border-2 border-b-4 border-orange-950">
      <button @click="$emit('handleRestart')"
      class="bg-orange-600 text-white text-3xl font-bold px-3 py-1"
      >
        Restart
        </button>
  </div>
  </div>
</template>
