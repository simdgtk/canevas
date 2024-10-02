<script setup>
import { onMounted, ref } from 'vue';
import * as THREE from 'three';

const width = window.innerWidth, height = window.innerHeight;
const container = ref(null);

onMounted(() => {
  // init
  const camera = new THREE.PerspectiveCamera(70, width / height, 0.01, 10);
  camera.position.z = 1;
  const scene = new THREE.Scene();

  const geometry = new THREE.BoxGeometry(0.2, 0.2, 0.2);
  // const material = new THREE.MeshNormalMaterial();
  const material = new THREE.MeshBasicMaterial({
    color: 0x34a1eb
  });

  const mesh = new THREE.Mesh(geometry, material);
  scene.add(mesh);

  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(width, height);
  renderer.setAnimationLoop(animate);
  container.value.appendChild(renderer.domElement);

  // animation

  function animate(time) {

    mesh.rotation.x = time / 2000;
    mesh.rotation.y = time / 1000;

    renderer.render(scene, camera);

  }
})

</script>

<template>
  <div class="container-full" ref="container">
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/utils/utils.scss';
</style>
