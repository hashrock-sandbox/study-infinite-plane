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
      <input type="range" v-model.number="viewport.zoom" min="0.1" max="2" step="0.05" />
    </div>
  </div>
</template>
<script>
export default {
  computed: {
    points() {
      return this.src.map(p => {
        return {
          ...p,
          x: (p.x + this.viewport.x) * this.viewport.zoom,
          y: (p.y + this.viewport.y) * this.viewport.zoom
        };
      });
    }
  },
  methods: {
    onPointerMove(ev) {
      if (this.grab) {
        this.viewport.x = ev.offsetX / this.viewport.zoom - this.grab.x;
        this.viewport.y = ev.offsetY / this.viewport.zoom - this.grab.y;
      }
    },
    onPointerDown(ev) {
      this.$refs.canvas.setPointerCapture(ev.pointerId);

      this.grab = {
        x: ev.offsetX / this.viewport.zoom - this.viewport.x,
        y: ev.offsetY / this.viewport.zoom - this.viewport.y
      };
    },
    onPointerUp() {
      this.grab = null;
    }
  },
  data() {
    return {
      grab: null,
      src: [
        {
          id: 0,
          x: 10,
          y: 10
        },
        { id: 1, x: 20, y: 30 },
        {
          id: 2,
          x: 50,
          y: 30
        },
        {
          id: 3,
          x: 80,
          y: 90
        }
      ],
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