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
import { ref, defineEmits } from "vue";

const flavor = [
  { name: "Grape", color: "#4EBC38" },
  { name: "Strawberry", color: "#FC5A8D" },
  { name: "Peach", color: "#FFE5B4" },
  { name: "Orange", color: "#FFA500" },
  { name: "Lime", color: "#BFFF00" },
];

const currentColor = ref("#4EBC38");
const preventClick = ref(false);
const emit = defineEmits(["changeColor"]);
const selectColor = (color) => {
  if (!preventClick.value && currentColor.value !== color) {
    preventClick.value = true;
    currentColor.value = color;
    emit("changeColor", color);
    setTimeout(() => {
      preventClick.value = false;
    }, 800);
  }
};
</script>

<style lang="scss" scoped>
.switch_container {
  width: 100%;
}
</style>
