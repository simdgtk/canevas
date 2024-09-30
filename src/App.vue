<script setup>
import SplashScreen from './components/SplashScreen.vue';
import TennisSection from './components/TennisSection.vue';
import { ref, onMounted } from 'vue';
import { gsap } from "gsap";

const isLoaded = ref(false);
const splash = ref(null);

onMounted(() => {
  console.log(splash.value);
  if (splash.value) {
    setTimeout(() => {
      gsap.to(
        splash.value.$el,
        {
          y: -800,
          duration: 1.5,
          ease: 'power3.inOut'
        }
      );
    }, 3000);
    setTimeout(() => {
      isLoaded.value = true;
    }, 3800);
  }
});
</script>

<template>
  <SplashScreen ref="splash" v-show="!isLoaded" />

  <TennisSection v-if="isLoaded" />
</template>

<style lang="scss">
@import '../src/assets/styles/main.scss';
</style>
