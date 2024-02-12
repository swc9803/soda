<template>
  <div class="switch_container">
    <div class="switch_wrapper">
      <button v-for="item in flavor" :key="item.id">{{ item.name }}</button>
    </div>
  </div>
</template>

<script setup>
// lang="ts"
const flavor = [
  { name: "Grape", color: "#6F2DA8" },
  { name: "Peach", color: "#FFE5B4" },
  { name: "Orange", color: "#FFA500" },
  { name: "Strawberry", color: "#FC5A8D" },
  { name: "Lime", color: "#BFFF00" },
];
</script>

<style lang="scss" scoped>
.switch_container {
  width: 100%;
}
</style>
