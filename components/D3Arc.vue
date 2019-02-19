<template>
  <g>
    <!-- Arc -->
    <path class='arc-path'
          :fill='fillColor'
          :d='d' />

    <!-- Centroid -->
    <g :transform='trans'>
      <slot :centroid="centroid">
        <text>hello</text>
      </slot>
    </g>
  </g>
</template>

<script>
import * as d3 from 'd3-shape'
export default {
  props: {
    fillColor: {
      type: String,
      default: '#c5c5c5'
    },
    innerRadius: {
      type: Number,
      default: 100
    },
    outerRadius: {
      type: Number,
      default: 300
    },
    cornerRadius: {
      type: Number,
      default: 0
    },
    startAngle: {
      type: Number,
      default: 0
    },
    endAngle: {
      type: Number,
      default: 2
    },
    padAngle: {
      type: Number,
      default: 0
    }
  },
  computed: {
    arcGenerator() {
      return d3
        .arc()
        .innerRadius(this.innerRadius)
        .outerRadius(this.outerRadius)
        .cornerRadius(this.cornerRadius)
        .startAngle(this.startAngle)
        .endAngle(this.endAngle)
        .padAngle(this.padAngle)
    },
    d() {
      return this.arcGenerator()
    },
    centroid() {
      if (this.arcGenerator) {
        const cen = this.arcGenerator.centroid()
        const radians = this.startAngle + ((this.endAngle - this.startAngle) * 0.5)
        return {
          x: cen[0],
          y: cen[1],
          radians,
          degrees: radians / Math.PI * 180
        }
      } else return false;
    },
    trans() {
      if (this.arcGenerator) {
        const centroid = this.arcGenerator.centroid()
        return `translate(${centroid[0]}, ${centroid[1]})`
      } else return false;
    }
  }
}
</script>

<style scoped lang='scss'>
.arc-path {
  &:hover {
    stroke: #e4e4e4;
    stroke-width: 15;
  }
}
</style>
