<template>
  <canvas ref="renderCanvas" class="canvas"/>
</template>

<script>
import {
  Engine,
  Scene,
  ArcRotateCamera,
  Vector3,
  HemisphericLight,
  MeshBuilder,
  Sound,
  Tools,
  StandardMaterial,
  Color3,
  Texture,
} from '@babylonjs/core';
import villageSound from '/village_sound.mp3';
import roofTexture from '@/textures/roof.jpeg';
import brickTexture from '@/textures/brick.jpeg';

export default {
  mounted() {
    const canvas = this.$refs.renderCanvas;
    const engine = new Engine(canvas, true, { preserveDrawingBuffer: true, stencil: true });
    const scene = new Scene(engine);

    const camera = new ArcRotateCamera('camera', -Math.PI / 2, Math.PI / 2.5, 3, new Vector3(0, 0, 0), scene);
    camera.attachControl();
    camera.wheelPrecision = 25;

    new HemisphericLight('light', new Vector3(10, 10, 0), scene);

    const box = MeshBuilder.CreateBox('box', {}, scene);
    box.position = new Vector3(-5, 2, 5);
    box.scaling = new Vector3(8, 4, 6);
    box.rotation = new Vector3(0, Tools.ToRadians(45), 0);
    const boxMaterial = new StandardMaterial('boxMaterial', scene);
    boxMaterial.diffuseTexture = new Texture(brickTexture, scene);
    box.material = boxMaterial;

    const roof = MeshBuilder.CreateCylinder('roof', { tessellation: 3 });
    roof.scaling = new Vector3(2, 4, 7);
    roof.rotation = new Vector3(0, Tools.ToRadians(45), Tools.ToRadians(90));
    roof.position = new Vector3(-5, 4.5, 5);
    const roofMaterial = new StandardMaterial('roofMaterial', scene);
    roofMaterial.diffuseTexture = new Texture(roofTexture, scene);
    roof.material = roofMaterial;

    const ground = MeshBuilder.CreateGround('ground', { width: 20, height: 20 });
    const groundMaterial = new StandardMaterial('groundMaterial', scene);
    groundMaterial.diffuseColor = Color3.FromHexString('#608038');
    ground.material = groundMaterial;

    new Sound('name', villageSound, scene, null, { loop: true, autoplay: true });

    engine.runRenderLoop(() => {
      scene.render();
    });

    window.addEventListener('resize', () => {
      engine.resize();
    });
  },
};
</script>

<style>
#app,
.canvas {
  width: 100%;
  height: 100%;
  touch-action: none;
}
</style>
