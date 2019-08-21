<template>
  <svg
    width="100%"
    height="100%">
    <circle
      v-for="(item, index) in h.descendants()"
      :key="index"
      :r="item.r"
      :cx="item.x"
      :cy="item.y"
      stroke="white"
      stroke-width="2">
      <title>
        {{ item.data.key }}: {{ item.value }}
      </title>
    </circle>
  </svg>
</template>

<script>
import * as d3 from 'd3'

export default {
  data: () => ({
    width:      100,
    height:     100,
    padding:    2,
    h:          d3.hierarchy({}),
    groupOrder: [
      'region',
      'subregion'
    ],
    dataset: []
  }),
  computed: {
    layout() {
      return d3.pack()
        .size([this.width, this.height])
        .padding(this.padding)
    },
    nester() {
      const n = d3.nest()

      this.groupOrder.forEach(v => {
        n.key(node => node[v])
      })

      return n
    },
    nestedData() {
      return {
        key:    'root',
        values: this.nester.entries(this.dataset)
      }
    }
  },
  watch: {
    layout() {
      this.layout(this.h)
    },
    nestedData(val) {
      // * Add Hierarchy to nested data
      const h = d3.hierarchy(val, v => v.values)

      // ! Calculate Totals and sort
      h.sum(v => v.value)
      h.sort((a, b) => d3.ascending(a.value, b.value))

      /**
       * We must assign properties to hierarchy
       * before we pass it on to vue, so that they are observed
       */
      this.layout(h)

      /** Finally pass it to Vue for observing */
      this.h = h
    }
  },
  async mounted() {
    // 1. Assign Sizes
    this.updateSize()

    // 2. Load the raw data
    const data = await d3.json('/datasets/populations.json')

    // 3. Assign data to our dataset object
    this.dataset = Object.freeze(data)

    window.addEventListener('resize', this.updateSize)
  },
  methods: {
    updateSize() {
      const {
        width,
        height
      } = this.$el.getBoundingClientRect()
      this.width = width
      this.height = height
    }
  }
}
</script>

<style scoped>
circle {
  transition: all 1200ms ease;
}
</style>
