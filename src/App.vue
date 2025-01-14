<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';
import Pipe from './components/Pipe.vue';
import Birb from './components/Birb.vue';
import GameOver from './components/GameOver.vue';
import Score from "./components/Score.vue"

document.title = "Flabby Birb"

const pipePos = ref<{x: number; topY: number; bottomY:number; id: number; scored?: boolean}[]>([{x: 1_100, topY: 350, bottomY: 200, id: 0 }])
const birbPos = ref(280)
const speed = ref(5)
const isFalling = ref(false)
const isJumping = ref(false)
const initialJumpPosition = ref(birbPos.value)
const gameOver = ref(false)
const score = ref(0)
const best = ref(Number(localStorage.getItem("best")) ?? 0)
const gameStart = ref(false)

const BIRBX = 200
const SIZE = 40


function generateNewPipe() {
  if (gameOver.value) return
  // x 500 to insure it won't react the floor
  const lastPipeId = pipePos.value[pipePos.value.length - 1].id!
  const topY = Math.floor((Math.random() * 450) + 150)
  const newPipe = {
    x: 1_100,
    topY: topY,
    bottomY: Math.max(topY - 150, 0),
    id: lastPipeId + 1
  }
  pipePos.value = [...pipePos.value, newPipe]
}

function movePipeLeftwards() {
  if (gameOver.value) return
    for (const pipe of pipePos.value) {
      if (pipe.x > -100) {
        pipe.x -= speed.value
      }
    }
    requestAnimationFrame(movePipeLeftwards)
}


function jumpBirb() {
  if (gameOver.value) return;
  if (birbPos.value > initialJumpPosition.value - 80) {
    birbPos.value -= 7

    isJumping.value = true
    isFalling.value = false
    requestAnimationFrame(jumpBirb)
  } else {
    isJumping.value = false
    isFalling.value = true
  }
}

function fall() {
  if (!isFalling.value) {
    return;
  }
  if (!isJumping.value) {
    birbPos.value += 7
  }
  if (birbPos.value < 640) {
    requestAnimationFrame(fall)
  }
}

function checkCollision() {
  if (gameOver.value) return;
  if (birbPos.value > 600) {
    gameOver.value = true
  }
  const birb = document.getElementById("birb")!.getBoundingClientRect();
  for (const pipe of pipePos.value) {
    const topPipePos = document.getElementById(`top-pipe-${pipe.id}`)!.getBoundingClientRect();
    const botPipePos = document.getElementById(`bot-pipe-${pipe.id}`)!.getBoundingClientRect();

    // Check collision with the top pipe
    const collisionWithTopPipe =
      birb.right > topPipePos.left &&
      birb.left < topPipePos.right &&
      birb.top < topPipePos.bottom;birb
    //birbcollision with the bottom pipe
    const collisionWithBottomPipe =
      birb.right > botPipePos.left &&
      birb.left < botPipePos.right &&
      birb.bottom > botPipePos.top;

    // If collision is detected with either pipe, set game over
    if (collisionWithTopPipe || collisionWithBottomPipe) {
      isFalling.value = true;
      isJumping.value = false;
      gameOver.value = true;
      gameStart.value = false;
      console.log(collisionWithTopPipe ? `collided with top-pipe-${pipe.id}` : `collided with bot-pipe-${pipe.id}`)
      console.log("Game Over!");
      if (score.value > best.value) {
        best.value = score.value;
        localStorage.setItem("best", score.value.toString())
      }
      return;
    }
  }

  // Continue checking for collisions in the next frame
  requestAnimationFrame(checkCollision);
}

function restartGame() {
  console.log("restart game")
  pipePos.value = [{x: 1_100, topY: 350, bottomY: 200, id: 0 }];
  birbPos.value = 280;
  isFalling.value = false;
  isJumping.value = false;
  initialJumpPosition.value = birbPos.value;
  gameOver.value = false;
  score.value = 0;
}

function incrementScore() {
  if (gameOver.value) return;


  const birb = document.getElementById("birb")!.getBoundingClientRect();

  for (const pipe of pipePos.value) {
    const topPipePos = document.getElementById(`top-pipe-${pipe.id}`)!.getBoundingClientRect();

    // Check if the pipe has passed completely (right of pipe is less than the left of bird)
    const didScore = topPipePos.right < birb.left;
    if (didScore && !pipe.scored!) {
      score.value += 1; // Increment the score
      pipe.scored = true; // Mark this pipe as scored to avoid counting it multiple times
    }
  }

  // Continue checking for score changes in the next frame
  requestAnimationFrame(incrementScore);
}


onMounted(()=> {
  setInterval(() => {
    const lastPipe = pipePos.value[pipePos.value.length -1]
    if (lastPipe.x < (Math.random() * 200) + 700) {
      generateNewPipe()
    }
    const firstPipe = pipePos.value[0];
    if (firstPipe.x === -100 ) {
      pipePos.value = [...pipePos.value.slice(1)]
      console.log("block deleted")
    }
  }, 100)
})

onMounted(() => {
  document.addEventListener("keypress", e => {
    if (e.key === " ") {
      if (gameOver.value) {
        restartGame()
        return
      }
      gameStart.value = true
      isFalling.value = true
      initialJumpPosition.value = birbPos.value
      requestAnimationFrame(jumpBirb)
    }
  })
})

watch(isFalling, () => {
  if (isFalling.value) {
    console.log("is falling")
    requestAnimationFrame(fall)
  } else {
    console.log("not falling")
  }
}, {immediate: true})

watch([gameOver, gameStart], () => {
  if (!gameOver.value && gameStart.value) {
    console.log("play")
    requestAnimationFrame(movePipeLeftwards)
    requestAnimationFrame(checkCollision)
    requestAnimationFrame(incrementScore)
  }
}, {immediate: true})


const grassArr = Array.from({length: 68})

</script>

<template>
  <div class="w-screen h-screen flex flex-col items-center justify-center">
    <div class="relative w-[1000px] h-[600px] bg-blue-200 overflow-hidden">
      <Score v-if="!gameOver" :score=score />
      <Pipe 
        v-for="(pipe, index) in pipePos" 
        :key="index" 
        :x="pipe.x" 
        :topY="pipe.topY" 
        :bottomY="pipe.bottomY" 
        :id="pipe.id"
      />
      <Birb 
      :x=BIRBX 
      :y=birbPos 
      :size=SIZE 
      :jump=isJumping
      :start=gameStart
      :end=gameOver
      />
      <GameOver v-if="gameOver" @handleRestart="restartGame" :score="score" :best="best" />
    </div>    
    <div class="w-[1000px] overflow-hidden">
      <div class="w-full flex flex-row h-6 " :id="gameStart ?'grass' : ''">
        <div class="w-[2000px] flex flex-row bg-brown">
          <template v-for="(, index) in grassArr" :key="index">
            <div class="w-4 max-w-4 h-10 bg-green-600 rotate-12 -translate-y-2 -translate-x-1"></div>
            <div class="w-4 max-w-4 h-10 bg-green-500 rotate-12 -translate-y-2 -translate-x-1"></div>
          </template>
        </div>
      </div>
      <div class="w-full h-16 bg-[#964B00]"></div>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

#grass {
  animation: grass 6s linear infinite;
}

@keyframes grass {
  from {transform: translateX(0%);}
  to {transform: translateX(-100%)}
}
</style>


