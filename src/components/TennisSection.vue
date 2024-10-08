<script setup>
import { onMounted, ref } from 'vue';
import Matter from "matter-js";

const container = ref(null);

onMounted(() => {
  setTimeout(() => {

    // module aliases
    var Engine = Matter.Engine,
      Render = Matter.Render,
      Runner = Matter.Runner,
      Bodies = Matter.Bodies,
      Composite = Matter.Composite,
      Mouse = Matter.Mouse,
      MouseConstraint = Matter.MouseConstraint;

    // create an engine
    var engine = Engine.create();

    // create a renderer
    var render = Render.create({
      element: container.value,
      engine: engine,
      options: {
        width: window.innerWidth,
        height: window.innerHeight,
        wireframes: false,
        background: 'transparent'
      }
    });

    let balls = [];
    let ballNumber = 22;
    let ballSize = 80;
    let xScale = 0.27;
    let yScale = 0.27;
    if (window.innerWidth < 1000) {
      ballNumber = 15;
      ballSize = (50);
      xScale *= 0.625;
      yScale *= 0.625;
    }
    for (let i = 0; i < ballNumber; i++) {
      var ball3 = Bodies.circle(Math.random() * (window.innerWidth), Math.random() * (-2000), ballSize, {
        angle: Math.random() * Math.PI * 2,
        render: {
          sprite: {
            // 600 x 589, donc 600 * valeurYScale / 2 -1 (environ) pour le radius
            texture: '/images/balle_detouree.webp',
            xScale: xScale,
            yScale: yScale,
          },
        }
      });
      ball3.restitution = 0.7;
      Matter.Body.rotate(ball3, Math.random() * 360);
      balls.push(ball3);
    }

    var ground = Bodies.rectangle(
      window.innerWidth / 2,
      window.innerHeight + 500 / 2,
      window.innerWidth,
      500,
      {
        isStatic: true,
        render: {
          fillStyle: 'rgba(0, 0, 0, 0)',
          strokeStyle: 'transparent',
          lineWidth: 0
        }
      }
    );
    var wallLeft = Bodies.rectangle(
      -250,
      window.innerHeight / 2,
      500,
      window.innerHeight * 2,
      {
        isStatic: true,
        render: {
          fillStyle: 'rgba(0, 0, 0, 0)',
          strokeStyle: 'transparent',
          lineWidth: 0
        }
      }
    );
    var wallRight = Bodies.rectangle(
      window.innerWidth + 250,
      window.innerHeight / 2,
      500,
      window.innerHeight * 2,
      {
        isStatic: true,
        render: {
          fillStyle: 'rgba(0, 0, 0, 0)',
          strokeStyle: 'transparent',
          lineWidth: 0
        }
      }
    );
    var wallTop = Bodies.rectangle(
      window.innerWidth / 2,
      -250,
      window.innerWidth * 2,
      500,
      {
        isStatic: true,
        render: {
          fillStyle: 'rgba(0, 0, 0, 0)',
          strokeStyle: 'transparent',
          lineWidth: 0
        }
      }
    );

    Composite.add(engine.world, [ground, wallLeft, wallRight, ...balls]);
    setTimeout(() => {
      Composite.add(engine.world, [wallTop]);
    }, 4000);

    var mouse = Mouse.create(render.canvas);
    var mouseConstraint = MouseConstraint.create(engine, {
      mouse: mouse,
      constraint: {
        stiffness: 0.2,
        render: {
          visible: false
        }
      }
    });
    // add mouse control to the world
    Composite.add(engine.world, mouseConstraint);

    // garde le scroll
    mouse.element.removeEventListener('wheel', mouse.mousewheel);
    mouse.element.removeEventListener('DOMMouseScroll', mouse.mousewheel);

    // keep the mouse in sync with rendering
    render.mouse = mouse;

    // run the renderer
    Render.run(render);

    // create runner
    var runner = Runner.create();

    // run the engine
    Runner.run(runner, engine);
  }, 4000)

});
</script>

<template>
  <div class="tennis-container container-full" ref="container">
    <h1>Tennis</h1>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/utils/utils.scss';


.container-full {
  width: 100%;
  height: 100vh;
  overflow: hidden;
  background: url("/images/tennis_fond.webp") no-repeat center center / cover;
  position: relative;
  overflow-y: hidden;

  &::after {
    content: '';
    position: absolute;
    user-select: none;
    pointer-events: none;
    scroll-behavior: smooth;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(197, 80, 51, 0.3);
  }

  h1 {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    user-select: none;
    pointer-events: none;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    font-family: 'New Title', sans-serif;
    font-weight: bold;
    font-size: 21rem;
    color: #ffffff;
    z-index: 2;
    opacity: 0.8;
    background-color: #ffffff;
    background-blend-mode: normal;
    background-image: url("/images/tennis_fond.webp");
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    color: transparent;
    background-clip: text;
    -webkit-background-clip: text;
    filter: grayscale(100%) sepia(31%) brightness(258%) saturate(226%) contrast(104%);
    -webkit-filter: grayscale(100%) sepia(31%) brightness(158%) saturate(226%) contrast(104%);
    -moz-filter: grayscale(100%) sepia(31%) brightness(158%) saturate(226%) contrast(304%);


    @media screen and (max-width: 768px) {
      font-size: 12rem;
    }
  }
}
</style>
