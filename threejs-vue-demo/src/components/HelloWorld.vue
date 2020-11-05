<template>
    <div id="container"></div>
</template>

<script>
import * as Three from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

export default {
  name: 'ThreeTest',
  data() {
    return {
      camera: null,
      scene: null,
      renderer: null,
      mesh: null
    }
  },
  methods: {
    init: function() {
        let container = document.getElementById('container');
        this.camera = new Three.PerspectiveCamera(50, container.clientWidth/container.clientHeight, 0.01, 2000);

        this.scene = new Three.Scene();
        // Set background color
        this.scene.background = new Three.Color( 0xC8FCF7 );

        const scene = this.scene;

        // Add some light
        const ambientLight = new Three.AmbientLight( 0x404040, 2);
        this.scene.add( ambientLight );

        const pointLight1 = new Three.PointLight( 0x404040, 10, 100 );
        pointLight1.position.set( 3, 3.5, -3 );
        this.scene.add( pointLight1 );

       // Load a glTF resource
        const loader = new GLTFLoader();
        loader.load(
          // resource URL
          'room.glb',
          // called when the resource is loaded
          function ( gltf ) {
            scene.add( gltf.scene );

            gltf.animations; // Array<THREE.AnimationClip>
            gltf.scene; // THREE.Group
            gltf.scenes; // Array<THREE.Group>
            gltf.cameras; // Array<THREE.Camera>
            gltf.asset; // Object
          },
          // called while loading is progressing
          function ( xhr ) {
            console.log( ( xhr.loaded / xhr.total * 100 ) + '% loaded' );
          },
          // called when loading has errors
          function ( error ) {
            console.log( error );
          }
        );

        this.renderer = new Three.WebGLRenderer({antialias: true});
        this.renderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(this.renderer.domElement);

        const controls = new OrbitControls( this.camera, this.renderer.domElement );
        this.camera.position.set( 5, 5, 10 );

        controls.update();


    },
    animate: function() {
        requestAnimationFrame(this.animate);


        this.renderer.render(this.scene, this.camera);
    }
  },
  mounted() {
      this.init();
      this.animate();
  }
}
</script>

<style scoped>
    #container{
      width: 100%;
      height: 500px;
    }
</style>