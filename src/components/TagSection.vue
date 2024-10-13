<script setup>
import { onMounted, ref } from 'vue';

const canvas = ref(null);
let dataURL = ref(null);
let painting = false;
onMounted(() => {
  canvas.value.width = window.innerWidth;
  canvas.value.height = window.innerHeight * 0.98;
  const ctx = canvas.value.getContext("2d");
  if (localStorage.getItem('dataURL')) {
    var image = new Image();
    image.onload = function () {
      ctx.drawImage(image, 0, 0);
    }
    image.src = localStorage.getItem('dataURL');
  }
})


const startPosition = (e) => {
  painting = true;
  const ctx = canvas.value.getContext("2d");
  ctx.beginPath();
  ctx.moveTo(e.clientX, e.clientY);
};

const finishedPosition = () => {
  painting = false;
  dataURL.value = canvas.value.toDataURL();
  localStorage.setItem('dataURL', dataURL.value);
};

const draw = (e) => {
  if (!painting) return;
  const ctx = canvas.value.getContext("2d");
  ctx.lineTo(e.clientX, e.clientY);
  ctx.strokeStyle = "#af131380";
  ctx.lineWidth = 20;
  ctx.lineCap = "round";
  ctx.stroke();
};

onMounted(() => {
  const ctx = canvas.value.getContext("2d");
  ctx.fillStyle = "white";
  ctx.fillRect(0, 0, canvas.value.width, canvas.value.height);
});
</script>

<template>
  <div class="section">
    <div class="container-full container-image"></div>
    <canvas class="canvas" ref="canvas" @mousedown="startPosition" @mouseup="finishedPosition" @mousemove="draw">
    </canvas>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/utils/utils.scss';
.section {position: relative;}
.container-image {
  background-image: url('/images/wall.webp');
  background-repeat: no-repeat;
  background-size: cover;

  @media screen and (min-width: 1920px) {
    height: 100vh;
    width: 100%;
    background-repeat: round;
  }
}

canvas {
  width: 100vw;
  height: 98vh;
  overflow: hidden;
}

.canvas {
  position: absolute;
  top: 0;
  left: 0;
  user-select: all;
  pointer-events: all;
  z-index: 2;
  background-color: blue;
  mix-blend-mode: multiply;
  filter: blur(6px) contrast(120%);
  clip-path: polygon(0% 22%, 43% 22%, 100% 24%, 100% 100%, 0% 100%);
  background-size: cover;

  @media screen and (min-width: 1920px) {
    height: 100vh;
    width: 100%;
    background-size: cover;
  }
}
</style>