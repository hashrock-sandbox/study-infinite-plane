<template>
  <div>
    <svg
      ref="canvas"
      viewBox="0 0 300 300"
      width="300"
      height="300"
      @pointerdown="onPointerDown"
      @pointermove="onPointerMove"
      @pointerup="onPointerUp"
    >
      <circle v-for="p in points" :cx="p.x" :cy="p.y" :key="p.id" :r="10 * viewport.zoom" />
    </svg>
    <div>
      <button @click="zoomIn">+</button>
      <button @click="zoomOut">-</button>
    </div>
    <div>{{viewport.x}} {{viewport.y}} {{viewport.zoom}}</div>
  </div>
</template>
<script>
import { example } from "./example.js";

export default {
  computed: {
    points() {
      return this.src.map(p => {
        return {
          ...p,
          x: (p.x - this.viewport.x) * this.viewport.zoom,
          y: (p.y - this.viewport.y) * this.viewport.zoom
        };
      });
    }
  },
  methods: {
    onPointerMove(ev) {
      if (this.grab) {
        this.viewport.x = -(ev.offsetX - this.grab.x) / this.viewport.zoom;
        this.viewport.y = -(ev.offsetY - this.grab.y) / this.viewport.zoom;
      }
    },
    zoomIn() {
      //中点を原点に移動
      const zoomRatio = 1.05;

      //

      // const px = this.viewport.x + 150 / this.viewport.zoom;
      // console.log(px);

      // this.viewport.x -= this.viewport.x + 150 / this.viewport.zoom;
      // this.viewport.y -= 150 / this.viewport.zoom;
      // this.viewport.x *= zoomRatio;
      // this.viewport.y *= zoomRatio;
      // this.viewport.zoom *= zoomRatio;
      // this.viewport.x += 150 * this.viewport.zoom;
      // this.viewport.y += 150 * this.viewport.zoom;
    },
    zoomOut() {
      this.viewport.zoom /= 1.1;
    },
    onPointerDown(ev) {
      this.$refs.canvas.setPointerCapture(ev.pointerId);

      //viewport矩形内のつかみ位置
      this.grab = {
        x: this.viewport.x + ev.offsetX / this.viewport.zoom,
        y: this.viewport.y + ev.offsetY / this.viewport.zoom
      };
    },
    onPointerUp() {
      this.grab = null;
    }
  },
  data() {
    return {
      grab: null,
      src: example,
      viewport: {
        x: 0,
        y: 0,
        zoom: 1
      }
    };
  }
};
</script>
<style>
body {
  background: #999;
}
svg {
  background: white;
  cursor: grabbing;
}
circle {
  transition: fill 0.1s;
  fill: rebeccapurple;
  cursor: pointer;
}
circle:hover {
  stroke: rebeccapurple;
  fill: white;
}
</style>