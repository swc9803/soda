<template>
  <div class="switch_container">
    <div class="switch_wrapper">
      <button
        v-for="item in flavor"
        :key="item.id"
        @click="selectColor(item.color)"
      >
        {{ item.name }}
      </button>
    </div>
  </div>
</template>

<script setup>
// lang="ts"
import { defineEmits } from "vue";

const flavor = [
  { name: "Grape", color: "#6F2DA8" },
  { name: "Peach", color: "#FFE5B4" },
  { name: "Orange", color: "#FFA500" },
  { name: "Strawberry", color: "#FC5A8D" },
  { name: "Lime", color: "#BFFF00" },
];

const emit = defineEmits(["changeColor"]);
const selectColor = (color) => {
  emit("changeColor", color);
};
</script>

<style lang="scss" scoped>
.switch_container {
  width: 100%;
}
</style>
