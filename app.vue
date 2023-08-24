<template>
  <div class="w-full min-h-screen text-gray-100 bg-gray-950">
    <canvas id="canvas" />
    <div class="bg-gray-950 top-0 left-0 fixed z-40 w-full min-h-screen flex text-center justify-center items-center">
      <div id="loading" class="text-4xl">Please Wait</div>
    </div>
    <div class="top-0 left-0 fixed z-40 w-full min-h-screen flex text-center justify-center items-center">
      <div id="result" class="flex-1 font-mono text-[7px] leading-[5px]" />
    </div>
    <div
      class="fixed z-50 left-0 px-10 text-center flex justify-center items-center w-full flex-col"
      :style="{
        top: '50%',
        transform: 'translateY(-50%)'
      }"
    >
      <div id="subtitle-top" class="transition-all duration-300 bg-gray-900/60 py-2 px-4 rounded-lg text-2xl"></div>
      <div id="subtitle-bottom" class="transition-all duration-300 bg-gray-900/60 py-2 px-4 rounded-lg text-xs mt-4"></div>
    </div>
    <div class="fixed bottom-0 left-0 z-50 bg-gray-950/80 rounded-tr-xl">
      <button id="btn-video-pause" class="text-xs px-4 py-2">pause</button>
    </div>
    <div v-if="!isRunning" class="text-gray-200 z-50 bg-gray-950 w-full min-h-screen top-0 left-0 fixed flex flex-col justify-center items-center">
      <div class="text-4xl pb-16 flex gap-8 items-center">
        <span>PROJECT MLY</span>
        <div class="heart" :style="{ '--width': '20px', '--height': '20px' }" />
      </div>
      <button @click="start" class="flex gap-2 items-center">
        <span>CLICK TO START.</span>
      </button>
    </div>
  </div>
</template>

<script setup>
const isRunning = ref(false)
const beloved = new ProjectMly()
const firstLoading = ref(false)

const analyze = () => {
  const loadingEl = document.getElementById('loading')
  const resultEL = document.getElementById('result')
  const b = document.getElementById('subtitle-top')
  const a = document.getElementById('subtitle-bottom')

  // if result have text
  const resText = resultEL.innerText?.replaceAll('\n', '').replaceAll(' ', '')
  if (resText && resText.length > 10) {
    loadingEl.style.display = 'none'
    firstLoading.value = true
    b.style.display = 'block'
    a.style.display = 'block'
  } else {
    loadingEl.style.display = 'block'
    b.style.display = 'none'
    a.style.display = 'none'
  }

  if (!firstLoading.value) setTimeout(analyze, 100)
}

onMounted(() => {
  analyze()
})

const start = () => {
  beloved.run(
    document.getElementById('canvas'),
    document.getElementById('result'),
    document.getElementById('subtitle-top'),
    document.getElementById('subtitle-bottom'),
    document.getElementById('btn-video-pause')
  )
  isRunning.value = true
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

* {
  font-family: 'Press Start 2P', cursive;
}

// init root vars
:root {
  --width: 100px;
  --height: 100px;
}

.heart {
  width: var(--width);
  height: var(--height);
  position: relative;
  background-color: red;
  transform: rotate(-45deg);
  margin: calc(var(--height) / 2) auto;
}

.heart::before,
.heart::after {
  content: "";
  width: var(--width);
  height: var(--height);
  background-color: red;
  border-radius: var(--width);
  position: absolute;
}

.heart::before {
  left: 0;
  top: calc(-1 * var(--height) / 2);
}

.heart::after {
  left: calc(var(--width) / 2);
  top: 0;
}
</style>