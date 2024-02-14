<template>
  <router-view />
</template>

<script setup>
import { onMounted } from "vue";
import Lenis from "@studio-freight/lenis";

const lenis = new Lenis();

const lenisScroll = (time) => {
  lenis.raf(time);
  requestAnimationFrame(lenisScroll);
};

onMounted(() => {
  lenisScroll();
});
</script>

<style lang="scss">
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
