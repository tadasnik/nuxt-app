<template>
  <svg
    :viewBox="viewBoxString"
    width="800"
    fill="white"
  >
    <!-- SVG Entry -->
    <pan-zoom
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
      mapheight: 230,

      // range:  [[0, 100], [0, 500]],
      domain: [[0, 1000], [0, 800]],
      transform: {
        k: 1,
        x: 0,
        y: 0
      },
      displayGrid: false
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

<style lang="scss" scoped>
div.main-demo {
  display: grid;
  grid: 100px 1fr / 100%;
  row-gap: 20px;
  position: absolute;
  top: 0;
  left: 50px;
  bottom: 50px;
  right: 10px;
  width: auto;
  height: auto;

  .controls {
    padding: 20px 40px;
    background-color: rgba(#808080, 0.25);

    display: grid;
    grid: auto / 150px 1fr;
    column-gap: 20px;
    align-items: center;

    > h3 {
      padding: 10px 25px;
    }

    &__inner {
      display: flex;
      flex-flow: row nowrap;
      align-items: center;

      > * {
        margin-right: 20px;

        &:last-child {
          margin-right: 0;
          margin-left: auto;
        }
      }
    }
  }

  > .svg {
    width: 100%;
    height: 100%;
    // overflow: hidden;
    position: relative;
    .panner {
      border: 1px solid rgba(#808080, 0.5);
    }
  }
}

svg {
  left: auto;
  top: auto;
  width: 100%;
  height: 100%;
}
</style>
