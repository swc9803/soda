<template>
  <div class="switch_container">
    <div class="background" :style="{ backgroundColor: currentColor }" />
    <div class="name">{{ currentName }}</div>
    <div class="switch_wrapper">
      <button
        v-for="item in flavor"
        :key="item.id"
        @click="selectFlavor(item.color, item.name)"
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
const currentName = ref("Grape");
const preventClick = ref(false);
const emit = defineEmits(["changeColor"]);
const selectFlavor = (color, name) => {
  if (!preventClick.value && currentColor.value !== color) {
    preventClick.value = true;
    currentColor.value = color;
    currentName.value = name;
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
  height: 100vh;
  .background {
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: -1;
  }
  .name {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate3d(-50%, -50%, 0);
    color: white;
    font-size: 4em;
    font-weight: 600;
    z-index: 1;
  }
  .switch_wrapper {
    position: absolute;
    bottom: 10%;
    left: 50%;
    transform: translate3d(-50%, -50%, 0);
    display: flex;
    justify-content: space-between;
    width: 60%;
  }
}
</style>
