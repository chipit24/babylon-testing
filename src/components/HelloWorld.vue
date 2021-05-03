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
} from '@babylonjs/core';

export default {
  mounted() {
    const canvas = this.$refs.renderCanvas;
    const engine = new Engine(canvas, true, { preserveDrawingBuffer: true, stencil: true });
    const scene = new Scene(engine);

    const camera = new ArcRotateCamera('camera', -Math.PI / 2, Math.PI / 2.5, 3, new Vector3(0, 0, 0), scene);
    camera.attachControl(canvas, true);

    new HemisphericLight('light', new Vector3(10, 10, 0), scene);

    const box = MeshBuilder.CreateBox('box', {}, scene);
    box.position.y = 0.5;
    MeshBuilder.CreateGround('ground', { width: 10, height: 10 });
    engine.runRenderLoop(() => {
      scene.render();
    });

    window.addEventListener('resize', () => {
      engine.resize();
    });
  }
}
</script>

<style>
.canvas {
  width: 100%;
  height: 100%;
  touch-action: none;
}
</style>
