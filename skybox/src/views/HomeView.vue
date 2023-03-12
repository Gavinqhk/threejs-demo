<template>
  <div id="container"></div>
</template>

<script setup lang="ts">
import * as THREE from "three";
import { onMounted } from "vue";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import img6 from "../assets/images/skybox/6.png";
import img5 from "../assets/images/skybox/5.png";
import img4 from "../assets/images/skybox/4.png";
import img3 from "../assets/images/skybox/3.png";
import img2 from "../assets/images/skybox/2.png";
import img1 from "../assets/images/skybox/1.png";
// 初始化renderer
const renderer = new THREE.WebGLRenderer();
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);

// 初始化场景
const scene = new THREE.Scene();

// 初始化相机
const camera = new THREE.PerspectiveCamera(
  60,
  window.innerWidth / window.innerHeight,
  1,
  1000
);
camera.position.setZ(5);

const controls = new OrbitControls(camera, renderer.domElement);

// 初始化天空盒
const urls: any = [img6, img3, img2, img1, img5, img4];

const textCube = new THREE.CubeTextureLoader().load(urls);
scene.background = textCube;

// 灯光
const light = new THREE.AmbientLight(0xff8800, 0.3);
scene.add(light);

// 窗口自适应
window.addEventListener("resize", () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
});

// 渲染
function animate() {
  controls.update();
  renderer.render(scene, camera);
  requestAnimationFrame(animate);
}

onMounted(() => {
  const container = document.getElementById("container") as HTMLElement;
  container.appendChild(renderer.domElement);
  animate();
});
</script>
