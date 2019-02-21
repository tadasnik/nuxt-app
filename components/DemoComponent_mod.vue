<template>
  <svg
    :viewBox="viewBoxString"
    fill="white"
  >
    <!-- SVG Entry -->
    <pan-zoom v-if="displayGrid"
      ref="panzoom"
      class="panner"
      :zoom-transform.sync="transform"
      :h-domain="domain[0]"
      :v-domain="domain[1]"
      :scale-content="true"
      :tick-amount="tickAmount"
      :axis-display="['bottom', 'top', 'left']"
      :show-grid="displayGrid"
    >
      <!-- Display a custom tick value -->
      <template
        slot="tick"
        slot-scope="d"
      >
        <text>
          {{ d.value | formatted }}
        </text>
      </template>

      <circle
        cx="25"
        cy="25"
        r="20"
        fill="#808080"
      />

      <circle
        cx="50"
        cy="50"
        r="10"
        stroke-width="1"
        stroke="#000000"
        fill="#505050"
      />

      <indo-map />
    </pan-zoom>
  </svg>
</template>

<script>
import ScaleZoom from '@/components/ScaleZoom.vue'
import { format } from 'd3'
import bounds from '@/mixins/bounds.js'
import * as gsap from 'gsap'
import IndoMap from '@/components/IndoMap.vue'
const formatter = format('~s')
export default {
  components: {
    PanZoom: ScaleZoom,
    IndoMap: IndoMap
  },
  filters: {
    formatted(num) {
      return formatter(+num)
    }
  },
  mixins: [bounds],
  data() {
    return {
      mapwidth: 610,
      mapheight: 225,

      // range:  [[0, 100], [0, 500]],
      domain: [[0, 610], [0, 225]],
      transform: {
        k: 1,
        x: 0,
        y: 0
      },
      displayGrid: true
    }
  },
  computed: {
    viewBoxString() {
      return `0 0 ${this.mapwidth} ${this.mapheight}`
    },
    tickAmount() {
      return Math.max(3, Math.floor(this.dimensions.width / 90))
    }
  },
  methods: {
    reset() {
      if (gsap.TweenMax.isTweening(this.transform)) {
        return
      }

      gsap.TweenMax.to(this.transform, 1.25, {
        k: 1,
        x: 0,
        y: 0
      }).eventCallback = type => {
        console.log('type', type)
      }
    }
  }
}
</script>

<style>
</style>
