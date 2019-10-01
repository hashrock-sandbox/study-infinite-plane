<template>
  <div>
    <svg viewBox="0 0 300 300" width="300" height="300">
      <circle v-for="p in points" :cx="p.x" :cy="p.y" :key="p.id" r="10" />
    </svg>
  </div>
</template>
<script>
export default {
  data() {
    return {
      points: [
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