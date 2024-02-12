<template>
  <div class="container">
    <div ref="canvasRef" class="wrapper" />
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
// import { RGBELoader } from "three/examples/jsm/loaders/RGBELoader.js"; 동적환경맵?
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

const canvasRef = ref();
let camera;
let raf;
let can;

const scene = new THREE.Scene();
const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
const gltfLoader = new GLTFLoader();

const zoomFit = (model, camera) => {
  // 모델의 경계 박스
  const box = new THREE.Box3().setFromObject(model);

  // 모델의 경계 박스 대각 길이
  const sizeBox = box.getSize(new THREE.Vector3()).length();

  // 모델의 경계 박스 중심 위치
  const centerBox = box.getCenter(new THREE.Vector3());

  // 모델 크기의 절반값
  const halfSizeModel = sizeBox / 2;

  // 카메라의 fov의 절반값
  const halfFov = THREE.MathUtils.degToRad(camera.fov / 2);

  // 모델을 화면에 꽉 채우기 위한 거리
  const distance = halfSizeModel / Math.tan(halfFov);

  // 모델 중심에서 카메라 위치로 향하는 방향 단위 벡터 계산
  const direction = new THREE.Vector3()
    .subVectors(camera.position, centerBox)
    .normalize();

  // 단위 방향 벡터 방향으로 모델 중심 위치에서 distance 거리에 대한 위치
  const position = direction.multiplyScalar(distance).add(centerBox);
  camera.position.copy(position);

  // 모델의 크기에 맞춰 카메라의 near, far 값 조정
  camera.near = sizeBox / 100;
  camera.far = sizeBox * 100;

  // 카메라 기본 속성 변경에 따른 투영행렬 업데이트
  camera.updateProjectionMatrix();

  camera.lookAt(centerBox);
};

gltfLoader.load("/can.glb", (gltf) => {
  can = gltf.scene;

  can.traverse((node) => {
    if (node.isMesh) {
      node.castShadow = true;
      node.receiveShadow = true;
    }
  });

  scene.add(can);
  zoomFit(can, camera);
});

// light
const light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(1, 1, 1);
scene.add(light);

const init = () => {
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(canvasRef.value.offsetWidth, canvasRef.value.offsetHeight);
  canvasRef.value.appendChild(renderer.domElement);

  const controls = new OrbitControls(camera, renderer.domElement);
  controls.minDistance = 3;
  controls.maxDistance = 6;
  controls.maxPolarAngle = Math.PI / 2 - 0.1;
  controls.update();
};

const animate = () => {
  camera.updateMatrixWorld();
  renderer.render(scene, camera);

  raf = requestAnimationFrame(animate);
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
    1000
  );

  init();
  animate();

  window.addEventListener("resize", onResize);
});

onBeforeUnmount(() => {
  cancelAnimationFrame(raf);
  renderer.dispose();

  window.removeEventListener("resize", onResize);
});
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
  height: 100vh;
  .wrapper {
    position: absolute;
    width: 100%;
    height: 100%;
  }
}
</style>
