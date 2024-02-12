<template>
  <div class="container">
    <Model :selectedColor="selectedColor" />
    <Switch @changeColor="changeColor" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import Model from "@/components/Model.vue";
import Switch from "@/components/Switch.vue";

const selectedColor = ref("6F2DA8");

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
