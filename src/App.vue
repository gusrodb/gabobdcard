<template>
  <HelloWorld/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import * as THREE from "three";

export default {
  name: 'App',
  components: {
    HelloWorld
  }
};

function boxScene() {
  const scene = new THREE.Scene();
  scene.background = new THREE.Color(0xffffff);

  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

  const renderer = new THREE.WebGLRenderer();
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);

  const geometry = new THREE.PlaneGeometry(6.40, 9.8);

  camera.position.z = 10;

  let plains = [];

  const loadManager = new THREE.LoadingManager();
  const loader = new THREE.TextureLoader(loadManager);

  const material = new THREE.MeshBasicMaterial({map: loader.load('./card_front.png')});
  const materialBack = new THREE.MeshBasicMaterial({map: loader.load('./card_back.png')});

  let animate = function () {
  };

  loadManager.onLoad = () => {
    plains.push(new THREE.Mesh(geometry, material));
    plains.push(new THREE.Mesh(geometry, materialBack));

    plains[1].rotation.y = -3.141875;

    scene.add(plains[0]);
    scene.add(plains[1]);

    animate = function (yDiff) {
      plains[0].rotation.y = Math.min(0, Math.max(-3.141875, -yDiff / 2));
      plains[1].rotation.y = -3.141875 + Math.min(0, Math.max(-3.141875, -yDiff / 2));

      const limit = 6.5;
      if (yDiff > limit) {
        let yPos = yDiff - limit;
        plains[0].position.y = yPos * 2;
        plains[1].position.y = yPos * 2;
      }

      renderer.render(scene, camera);
    };

    renderer.render(scene, camera);
  };

  const doc = document.documentElement;
  document.body.onscroll = () => {
    let scrollTop = (window.scrollY || doc.scrollTop) - (doc.clientTop || 0);

    let visible = scrollTop < 15000;
    document.getElementsByTagName('canvas')[0].style.visibility = visible ? "initial" : "hidden";

    scrollTop *= 0.0025;
    animate(scrollTop);
  }
}

boxScene();
</script>

<style>
canvas {
  position: fixed;
  top: 0;
  left: 0;
}

main {
  width: 100vw;
  height: 840vh;
  position: absolute;
}
</style>
