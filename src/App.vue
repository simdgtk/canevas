<script setup>
import SplashScreen from './components/SplashScreen.vue';
import TennisSection from './components/TennisSection.vue';
import StatueSection from './components/StatueSection.vue';
import HeaderBanner from './components/HeaderBanner.vue';
import { ref, onMounted } from 'vue';
import { gsap } from 'gsap';
import { ScrollTrigger } from "gsap/ScrollTrigger";
import Lenis from 'lenis';

onMounted(() => {
  gsap.registerPlugin(ScrollTrigger);
  const lenis = new Lenis()

  lenis.on('scroll', ScrollTrigger.update)

  gsap.ticker.add((time) => {
    lenis.raf(time * 1000)
  })

  gsap.ticker.lagSmoothing(0)
});
</script>

<template>
  <!-- v-show="!isLoaded" -->
  <SplashScreen />
  <!-- <TennisSection v-if="isLoaded" /> -->
  <HeaderBanner />
  <TennisSection />
  <StatueSection />


</template>

<style lang="scss">
@import '../src/assets/styles/main.scss';

html.lenis,
html.lenis body {
  height: auto;
}

.lenis.lenis-smooth {
  scroll-behavior: auto !important;
}

.lenis.lenis-smooth [data-lenis-prevent] {
  overscroll-behavior: contain;
}

.lenis.lenis-stopped {
  overflow: hidden;
}

.lenis.lenis-smooth iframe {
  pointer-events: none;
}
</style>
