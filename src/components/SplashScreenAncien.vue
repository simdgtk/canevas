<script setup>
import { gsap } from 'gsap'
import { onMounted } from 'vue'
onMounted(() => {
  function variableFont() {
    let tl = gsap.timeline({ repeat: -1, repeatDelay: 3 })
    tl.to('.h1', {
      duration: 1.5, // Durée de l'animation en secondes
      fontWeight: 100, // Passe de 200 (light) à 900 (bold)
      ease: 'expo.out', // Type d'effet de transition
      repeat: 1, // Répète l'animation à l'infini
      yoyo: true // Retourne à l'état initial après chaque boucle
    })
    return tl
  }
  variableFont()

  gsap.to('.link-container', {
    duration: 1,
    delay: 1.5,
    height: 40,
    ease: 'power3.inOut'
  })
})
</script>
<template>
  <div class="h1-container">
    <h1 class="h1">Canevas</h1>
    <div class="link-container">
      <a href="http://simondaguetkargl.fr/" target="_blank">.simondaguetkargl.fr</a>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/utils/utils.scss';

.h1-container {
  overflow: hidden;
  display: flex;
  height: 100vh;
  width: 100vw;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  h1 {
    user-select: none;
    z-index: -1;
    font-size: 30vw;
    font-weight: 900;
    font-family: 'New Title', sans-serif;
    transition: font-weight 0.5s ease;
    position: relative;
    color: $color-gray;

    &::before {
      @keyframes path {
        0% {
          clip-path: polygon(0 100%, 100% 100%, 100% 100%, 0 100%);
        }

        100% {
          clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
        }
      }

      content: 'Canevas';
      position: absolute;
      // clip-path: polygon(0 70%, 100% 70%, 100% 100%, 0 100%);
      animation: path 2.5s ease-in-out;
      z-index: 999;
      top: 0;
      left: 0;
      color: $color-black;
      z-index: 1;
      transition: clip-path 0.5s ease;
    }
  }

  .link-container {
    height: 0;
    overflow: hidden;
  }

  a {
    font-family: 'Futura LT', sans-serif;
    font-weight: 300;
    text-decoration: none;
    font-size: 1.5rem;
  }
}
</style>