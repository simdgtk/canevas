<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue'
onMounted(() => {

  function variableFont() {
    let tl = gsap.timeline()
    tl.to('.h1', {
      delay: 1.5,
      duration: 1.5, // Durée de l'animation en secondes
      fontWeight: 900, // Passe de 200 (light) à 900 (bold)
      ease: 'expo.out', // Type d'effet de transition
    })
    return tl
  }
  variableFont()
  // var rule = CSSRulePlugin.getRule(".title::before"); //get the rule
  // gsap.to(rule, { duration: 3, cssRule: { color: "blue" } });
  gsap.to('.link-container', {
    duration: 1,
    delay: 1.5,
    height: 40,
    ease: 'power3.inOut'
  })
  let tl2 = gsap.timeline({
    delay: 3,
  })
  let scale = 0.2;
  if (window.innerWidth < 768) {
    scale = 0.4;
  }
  tl2.to('.h1', {
    delay: 1.5,
    scale: scale,
    margin: 0,
    ease: "expo.inOut",
  }).to('.link-container', {
    display: 'none',
    duration: 0,
  }, '<').to('.background', {
    y: '-100%',
    ease: 'power3.inOut',
    duration: 1,
    stagger: 0.1,
  }, '<').to(".title", {
    color: 'transparent',
  }, '<').to('.h1-container', {
    backgroundColor: 'transparent',
    duration: 0.2,
  }, '<').to('.h1', {
    display: 'none',
    delay: 0
  }, '<')
  gsap.to('.h1', {
    duration: 2,
    delay: 2.5,
    ease: 'power3.inOut',
  })
})
</script>
<template>
  <div class="h1-container">
    <div class="h1-div">
      <h1 class="h1 title">Canevas</h1>
    </div>
    <div class="link-container">
      <a href="http://simondaguetkargl.fr/" target="_blank">.simondaguetkargl.fr</a>
    </div>
    <div class="background"></div>
    <div class="background"></div>
    <div class="background"></div>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/utils/utils.scss';

.h1-container {
  z-index: 99999;
  user-select: none;
  pointer-events: none;
  background-color: #fff;
  position: fixed;
  width: 100vw;
  top: 0;
  left: 0;
  box-sizing: border-box;
  padding: 1rem 1.5rem;
  overflow-y: hidden;
  display: flex;
  height: 100vh;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  .h1-div {
    width: 100%;
    height: 100vh;
    margin: 0 auto;
    display: flex;
    align-items: flex-start;
  }

  h1,
  .title {
    // height: 100%;
    display: flex;
    align-items: center;
    // height: 100vh;
    width: fit-content;
    margin: 0 auto;
    margin: auto auto;
    transform-origin: top left;
    user-select: none;
    z-index: -1;
    font-size: 30vw;
    font-weight: 100;
    overflow: hidden;
    font-family: 'New Title', sans-serif;
    transition: font-weight 0.5s ease;
    position: relative;
    color: $color-gray;

    @media screen and (max-width: 768px) {
      font-size: 40vw;
    }

    &::after {
      & {
        @keyframes path {
          0% {
            clip-path: polygon(0 100%, 100% 100%, 100% 100%, 0 100%);
          }

          100% {
            clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
          }
        }
      }

      content: 'Canevas';
      width: 100vw;
      height: 100vh;
      // text-align: center;
      position: absolute;
      overflow: hidden;
      // clip-path: polygon(0 70%, 100% 70%, 100% 100%, 0 100%);
      animation: path 2.5s ease-in-out;
      z-index: 999;
      top: 0;
      left: 0;
      z-index: 1;
      transition: clip-path 0.5s ease;
      color: white;
      mix-blend-mode: difference;

      @media screen and (max-width: 768px) {
        font-size: 40vw;
      }
    }
  }


  a {
    font-family: 'Futura LT', sans-serif;
    font-weight: 300;
    text-decoration: none;
    font-size: 1.5rem;
  }

  .link-container {
    pointer-events: all;
    height: 0;
    overflow: hidden;
  }

  .background {
    position: fixed;
    width: 100vw;
    height: 100vh;
    top: 0;
    left: 0;
    z-index: -11111;

    &:nth-child(3) {
      background-color: #fff;
    }

    &:nth-child(4) {
      background-color: #ffffff92;
      z-index: -111111;
    }

    &:nth-child(5) {
      background-color: #ffffff1b;
      z-index: -1111111;
    }
  }
}
</style>
