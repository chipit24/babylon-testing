<template>
  <canvas ref="renderCanvas" class="canvas"/>
</template>

<script>
import * as BABYLON from 'babylonjs';

export default {
  name: 'HelloWorld',
  mounted() {
    const canvas = this.$refs.renderCanvas;
    const engine = new BABYLON.Engine(canvas, true, { preserveDrawingBuffer: true, stencil: true });
    const scene = new BABYLON.Scene(engine);
    const camera = new BABYLON.ArcRotateCamera('camera', -Math.PI / 2, Math.PI / 2.5, 3, new BABYLON.Vector3(0, 0, 0), scene);

    camera.attachControl(canvas, true);

    new BABYLON.HemisphericLight('light', new BABYLON.Vector3(0, 1, 0), scene);
    BABYLON.MeshBuilder.CreateBox('box', {}, scene);

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
