<template>
  <svg
    width="100%"
    height="100%"
    @click="addPoint">
    @contextmenu.prevent="points = []">

    <path
      :d="lineGen(points)"
      stroke="white"
      stroke-width="5" />
    <circle
      v-for="(c, index) in points"
      :key="index"
      :cx="c.x"
      :cy="c.y"
      r="10" />
  </svg>
</template>

<script>
import * as d3 from 'd3'

export default {
  data() {
    return {
      points: [
        {
          x: 400,
          y: 200
        }
      ]
    }
  },
  computed: {
    lineGen() {
      return d3.line()
        .x(v => v.x)
        .y(v => v.y)
    }
  },
  methods: {
    addPoint(mouseEvent) {
      const {
        layerX: x,
        layerY: y
      } = mouseEvent

      this.points.push({ x, y })
    }
  }
}
</script>
