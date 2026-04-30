
<script setup lang="ts">
defineProps<{ 
  x: number; 
  y: number; 
  size: number; 
  jump: boolean; 
  start: boolean;
  end: boolean;
}>();


</script>



<template>
  <div
    id="birb"
    :class="[
    end ? 'dead' : '', 
    'absolute', 
    'z-10', 
    'rounded-full', 
    jump ? 'jumping' : ''
    ]"
    :style="{ 
        left: `${x}px`, 
        top: `${y}px` , 
        width: `${size + 10}px`, 
        height: `${size}px`, 
        backgroundColor:'yellow'
    }"
  >
  <!-- eyes -->
  <div class="absolute right-0 w-5 h-5 rounded-full bg-white border border-black flex items-center justify-center">
    <span v-if="end" class="text-[10px] font-bold leading-none translate-x-1">✕</span>
    <div v-else class="w-1 h-1 bg-black rounded-full translate-x-1"></div>
  </div>
  <!-- eyes -->
   <!-- beak -->
    <div class="w-7 h-4 absolute -right-2 bottom-1">
      <div class="bg-orange-600 w-full h-2 rounded-e-full border border-black"></div>
      <div class="bg-orange-600 w-full h-2 rounded-e-full border border-black -translate-x-[2px]"></div>
    </div>
   <!-- beak -->
    <!-- wing -->
     <div 
     :id="start ? 'flapping' : ''" 
     class="absolute w-7 hover h-5 bg-yellow-300 border border-black top-4 rounded-br-full rounded-bl-md "></div>
    <!-- wing -->
  </div>
</template>


<style scoped>
.jumping {
  animation: jumping 0.5s linear;
}

.dead {
  animation: over 4s linear;
}

#flapping {
  animation:  flap 0.3s linear infinite;
}

@keyframes jumping {
  0% {
    transform: rotate(0deg);
  }
  50% {
    transform: rotate(-40deg);
  }
  100% {
    transform: rotate(0deg);
  }
}

@keyframes over {
  0% {
    transform: rotate(0deg);
  }
  10% {
    transform: rotate(90deg);
  }
  100% {
    transform: rotate(90deg);
  }
}

@keyframes flap {
  0% {
    transform: scaleY(-1) translateY(100%);
    
  }
  50% {
    transform: scaleY(1);
  }
  100% {
    transform: scaleY(-1) translateY(100%);
  }
}


</style>