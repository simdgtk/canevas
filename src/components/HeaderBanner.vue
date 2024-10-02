<template>
    <h2>Canevas</h2>
</template>
<style lang="scss" scoped>
@import '../assets/styles/utils/utils.scss';
h2 {
  position: fixed;
  z-index: 3;
  top: 0;
  left: 0;
  color: #ffffff;
  mix-blend-mode: difference;
  font-family: 'New Title', sans-serif;
  font-size: 30vw * 0.2;
  font-weight: 300;
  padding: 1rem 1.5rem;
  user-select: none;

  @media screen and (max-width: 768px) {
    font-size: 40vw * 0.4;
  }
}
</style>