<template>
  <div class="container">
    <Model :selectedColor="selectedColor" />
    <Switch @changeColor="changeColor" />
    <!-- lenis -->
  </div>
</template>

<script setup>
import { ref } from "vue";
import Model from "@/components/Model.vue";
import Switch from "@/components/Switch.vue";

const selectedColor = ref("#4EBC38");

const changeColor = (color) => {
  selectedColor.value = color;
  console.log(color);
};
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
}
</style>
