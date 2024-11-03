<script setup>
import { onMounted, ref } from 'vue';
import { gsap } from 'gsap';
import Draggable from 'gsap/Draggable';
import Matter from "matter-js";

const canvas = ref(null);
const section = ref(null);
const color = ref("#4a6b9e80");
const isDrawing = ref(false);

// const targetIsVisible = useElementVisibility(section);
// onMounted(() => {
//   watch(targetIsVisible.value, () => {
//     canvas.value.scrollIntoView({ behavior: "smooth", block: "end", inline: "nearest" });
//   });
// });

let dataURL = ref(null);
onMounted(() => {
  canvas.value.width = window.innerWidth;
  canvas.value.height = window.innerHeight;
  const ctx = canvas.value.getContext("2d");
  if (localStorage.getItem('dataURL')) {
    const image = new Image();
    image.onload = () => ctx.drawImage(image, 0, 0);
    image.src = localStorage.getItem('dataURL');
  }
});

const startPosition = (e) => {
  const ctx = canvas.value.getContext("2d");
  ctx.beginPath();
  ctx.fillStyle = color.value;
  ctx.globalAlpha = 0.5;
  ctx.moveTo(e.clientX, e.clientY);
};

const finishedPosition = () => {
  dataURL.value = canvas.value.toDataURL();
  localStorage.setItem('dataURL', dataURL.value);
  const ctx = canvas.value.getContext("2d");
  ctx.closePath();
};

const draw = (e) => {
  const ctx = canvas.value.getContext("2d");
  ctx.lineTo(e.clientX, e.clientY);
  ctx.strokeStyle = color.value;
  ctx.lineWidth = 25;
  ctx.lineCap = "round";
  ctx.stroke();
};

onMounted(() => {
  const ctx = canvas.value.getContext("2d");
  ctx.fillStyle = "white";
  ctx.fillRect(0, 0, canvas.value.width, canvas.value.height);
});

// Matter.js setup
const matter = ref(null);
onMounted(() => {
  const { Engine, Render, Runner, Bodies, Composite, Mouse, MouseConstraint, Constraint } = Matter;

  const engine = Engine.create();
  engine.world.gravity.y = -1;

  const render = Render.create({
    element: matter.value,
    engine,
    options: { width: window.innerWidth, height: window.innerHeight, wireframes: false, background: 'transparent' }
  });

  const boxA = Bodies.rectangle(window.innerWidth - 0.05 * window.innerWidth, window.innerHeight - 0.3 * window.innerHeight, 90, 153, {
    render: { sprite: { texture: '/images/hand.webp', xScale: 0.235, yScale: 0.235 } }
  });

  const constraint = Constraint.create({
    pointA: { x: window.innerWidth - 0.05 * window.innerWidth, y: window.innerHeight + 0.1 * window.innerHeight },
    bodyB: boxA,
    pointB: { x: -10, y: 70 },
    stiffness: 0.003,
    damping: 2,
    render: { lineWidth: 30, strokeStyle: '#223cff', type: 'line' }
  });

  Composite.add(engine.world, [boxA, constraint]);

  const mouse = Mouse.create(render.canvas);
  const mouseConstraint = MouseConstraint.create(engine, { mouse, constraint: { stiffness: 0.2, render: { visible: false } } });
  Composite.add(engine.world, mouseConstraint);

  Matter.Events.on(mouseConstraint, "mousedown", (event) => {
    if (mouseConstraint.body === boxA) {
      color.value = "#FFF";
      startPosition({ clientX: event.mouse.position.x, clientY: event.mouse.position.y });
    } else {
      startPosition({ clientX: event.mouse.position.x, clientY: event.mouse.position.y });
      isDrawing.value = true;
    }
  });

  Matter.Events.on(mouseConstraint, 'mousemove', (event) => {
    if (mouseConstraint.body === boxA) {
      draw({ clientX: event.mouse.position.x, clientY: event.mouse.position.y });
    } else if (isDrawing.value) {
      draw({ clientX: event.mouse.position.x, clientY: event.mouse.position.y });
    }
  });

  Matter.Events.on(mouseConstraint, 'mouseup', () => {
    isDrawing.value = false;
    finishedPosition();
  });

  Render.run(render);
  Runner.run(Runner.create(), engine);

  // gsap.registerPlugin(Draggable);
  // Draggable.create("#sprayBlue", {
  //   bounds: canvas.value,
  // });
});

const updateColor = (newColor) => {
  color.value = newColor;
};
</script>

<template>
  <div id="main">
    <div class="section" ref="section">
      <div class="container-full container-image"></div>
      <canvas class="canvas" ref="canvas" @mousedown="startPosition" @mouseup="finishedPosition"
        @mousemove="draw"></canvas>
      <div class="sprays">
        <div id="sprayBlue" class="spray" @click="updateColor('#4a6b9e80'), toogleClass()">
          <img src="/images/spray_bleu.webp" alt="spray-blue" />
        </div>
        <div id="sprayRed" class="spray" @click="updateColor('#9e4a6b80'), toogleClass()">
          <img src="/images/spray_rouge.webp" alt="spray-red" />
        </div>
      </div>
      <div class="" id="question">
      </div>


    </div>
    <div id="matter-js" ref="matter"></div>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/utils/utils.scss';

#main {
  position: relative;
  overflow: hidden;
}

#matter-js {
  position: absolute;
  z-index: 2;
  top: 0;
  left: 0;
  width: 100vw;
  height: 101vh;
}

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

.canvas {
  position: absolute;
  top: 0;
  left: 0;
  user-select: all;
  pointer-events: all;
  z-index: 2;
  mix-blend-mode: multiply;
  filter: blur(6px) contrast(120%);
  clip-path: polygon(0% 22%, 43% 22%, 100% 24%, 100% 100%, 0% 100%);
  background-size: cover;

  @media screen and (min-width: 1920px) {
    height: 100vh;
    width: 100%;
  }
}

.sprays {
  user-select: none;
  padding-left: 2rem;
  z-index: 3;
  display: flex;
  gap: 1rem;
  position: absolute;
  bottom: 0;
  left: 0;


  .spray {
    transition: transform 0.2s ease-in-out;
    transform-origin: bottom center;

    &:hover {
      cursor: pointer;
      transform: scale(1.1);
    }

    &.active {
      transform: scale(1.1);
    }
  }

  img {
    width: 3.5vw;
    height: auto;
  }
}

#question {
  padding: 1.5rem;
  box-sizing: border-box;
  display: flex;
  position: absolute;
  align-items: center;
  justify-content: center;
  font-family: sans-serif;
  border-radius: 999px;
  z-index: 3;
  top: 0;
  right: 0;
  color: black;
  background-color: white;
  margin: 1rem;
  opacity: 0.6;
  clip-path: polygon(0% 0, 100% 0, 100% 100%, 0% 100%);
  transition: all 0.2s ease-in-out;

  &::before {
    content: '?';
    font-size: 2rem;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: sans-serif;
    position: absolute;
    width: 100%;
    height: 100%;
    opacity: 1;
    transition: opacity 0.2s ease-in-out;
  }

  &::after {
    content: '';
    font-size: 1rem;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: sans-serif;
    position: absolute;
    width: 100%;
    height: 100%;
    opacity: 0;
    clip-path: polygon(100% 0, 100% 0, 100% 100%, 100% 100%);
    transition: clip-path 0.4s ease-in-out, opacity 0.4s ease-out;
  }

  &:hover {
    opacity: 0.9;
    padding-left: 20rem;

    &::before {
      opacity: 0;
    }

    &::after {
      content: 'Choississez votre couleur avec les bombes, dessinez et effacez en glissant la main';
      transform: translateX(-9.2rem);
      clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
      opacity: 1;
    }
  }
}
</style>
