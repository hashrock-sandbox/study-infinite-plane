<template>
  <div>
    <svg
      viewBox="0 0 300 300"
      width="300"
      height="300"
      @pointerdown="onPointerDown"
      @pointermove="onPointerMove"
      @pointerup="onPointerUp"
    >
      <circle v-for="p in points" :cx="p.x" :cy="p.y" :key="p.id" r="10" />
    </svg>
  </div>
</template>
<script>
export default {
  computed: {
    points() {
      return this.src.map(p => {
        return {
          ...p,
          x: p.x + this.viewport.x,
          y: p.y + this.viewport.y
        };
      });
    }
  },
  methods: {
    onPointerMove(ev) {
      if (this.grab) {
        this.viewport.x = ev.offsetX - this.grab.x;
        this.viewport.y = ev.offsetY - this.grab.y;
      }
    },
    onPointerDown(ev) {
      this.grab = {
        x: ev.offsetX - this.viewport.x,
        y: ev.offsetY - this.viewport.y
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
        { id: 1, x: 20, y: 30 }
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
}
</style>