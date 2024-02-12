<template>
  <div class="container">
    <div ref="canvasRef" class="wrapper" />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
// import { RGBELoader } from "three/examples/jsm/loaders/RGBELoader.js"; 동적환경맵?
import gsap from "gsap";

const canvasRef = ref();
let camera;
let can;

const scene = new THREE.Scene();
const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
const gltfLoader = new GLTFLoader();

gltfLoader.load("/can.glb", (gltf) => {
  can = gltf.scene;

  can.traverse((node) => {
    if (node.isMesh) {
      node.castShadow = true;
      node.receiveShadow = true;
    }
  });

  //   can.rotation.set(-Math.PI / 2, 0, 0);

  const box = new THREE.Box3().setFromObject(can);
  const center = new THREE.Vector3();
  box.getCenter(center);
  can.position.sub(center);
  can.scale.set(0.8, 0.8, 0.8);

  scene.add(can);
});

setTimeout(() => {
  can.traverse((node) => {
    if (node instanceof THREE.Mesh) {
      if (node.name === "model") {
        const randomColor = {
          r: Math.random(),
          g: Math.random(),
          b: Math.random(),
        };
        gsap.to(node.material.color, {
          r: randomColor.r,
          g: randomColor.g,
          b: randomColor.b,
          duration: 1.5,
        });
      }
    }
  });
}, 1000);

// light
const light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(1, 1, 1);
scene.add(light);

const init = () => {
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(canvasRef.value.offsetWidth, canvasRef.value.offsetHeight);
  canvasRef.value.appendChild(renderer.domElement);
};

const animate = () => {
  camera.updateMatrixWorld();
  renderer.render(scene, camera);

  requestAnimationFrame(animate);
};

const onResize = () => {
  camera.aspect = canvasRef.value.offsetWidth / canvasRef.value.offsetHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(canvasRef.value.offsetWidth, canvasRef.value.offsetHeight);
};

onMounted(() => {
  camera = new THREE.PerspectiveCamera(
    75,
    canvasRef.value.offsetWidth / canvasRef.value.offsetHeight,
    0.1,
    100
  );

  camera.position.set(0, 0, 5);

  init();
  animate();

  window.addEventListener("resize", onResize);
});
</script>

<style lang="scss" scoped>
.container {
  position: fixed;
  width: 100%;
  height: 100vh;
  .wrapper {
    position: absolute;
    width: 100%;
    height: 100%;
  }
}
</style>