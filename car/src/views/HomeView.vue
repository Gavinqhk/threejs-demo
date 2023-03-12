<template>
  <div id="container"></div>
</template>

<script setup lang="ts">
import * as THREE from "three"
import { onMounted, ref } from "vue"
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls"
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader"

let renderer: any = null
let scene: any = null
let camera: any = null
let controls: any = null
// 初始化renderer
const initRenderer = () => {
  renderer = new THREE.WebGLRenderer()
  renderer.setSize(window.innerWidth, window.innerHeight)
  document.querySelector('#container')?.appendChild(renderer.domElement)
}
// 初始化场景
const initScene = () => {
  scene = new THREE.Scene()
}
// 初始化相机
const initCamera = () => {
  camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 1000)
  camera.position.set(510, 128, 0)
}

const initCar = () => {
  const loader = new GLTFLoader();

  loader.load(
    'src/assets/car/scene.gltf',
    // called when the resource is loaded
    function ( gltf ) {
      scene.add( gltf.scene );
    },
    // called while loading is progressing
    function ( xhr ) {
      console.log( ( xhr.loaded / xhr.total * 100 ) + '% loaded' );
    },
    // called when loading has errors
    function ( error ) {
      console.log( 'An error happened' );
    }
);
}






// 初始化控制器（用户交互）
const initControls = () => {
  controls = new OrbitControls(camera, renderer.domElement)
  controls.enableZoom = true
}
// 初始化灯光
const initLight = () => {
  const directionalLight = new THREE.DirectionalLight(0xffffff, 0.5)
  directionalLight.position.set(-4, 8, 4)
  const dhelper = new THREE.DirectionalLightHelper(directionalLight, 5, 0xff0000)
  const hemisphereLight = new THREE.HemisphereLight(0xffffff, 0xffffff, 0.4)
  hemisphereLight.position.set(0, 8, 0)
  const hHelper = new THREE.HemisphereLightHelper(hemisphereLight, 5)
  scene.add(directionalLight)
  scene.add(hemisphereLight)
}

// 窗口抖动
const onWindowResize = () => {
  camera.aspect = innerWidth / innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize( innerWidth, innerHeight );
  renders();
}

// 渲染 
const renders = () => {
  renderer.clear()
  renderer.render(scene, camera)
}
// 动画
const animate = () => {
  requestAnimationFrame(() => {
    if (controls) controls.update();
    renders();
    animate();
  })
}

onMounted(async () => {
  initRenderer()
  initScene()
  initCamera()
  initControls()
  initLight()
  initCar()
  renders()
  animate()
  window.addEventListener('resize', onWindowResize, false)
})
</script>
