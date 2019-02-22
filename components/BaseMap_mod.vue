<template>
    <div>
  <svg
    ref="svg"
    class="pan-zoom"
    :zoom-transform="zoomTransform"
    :viewBox="viewBoxString"

  >
    <!-- Inner Content -->
    <svg
      style="overflow: hidden;"
      width="100%"
      height="100%"
      class="pan-content"
    >

      <g v-bind="transformProp">

    <rect
      width="100%"
      height="100%"
      x="0%"
      y="0%"
      fill="#909090"
      opacity="0.5"
    />

   <path
      fill="#1cdd87"
      stroke="white"
      stroke-width="0.5"
      :d="d()"
    />


      </g>
    </svg>
  </svg>
    </div>
</template>

<script>
import * as d3 from 'd3'
import {
    event,
  zoom,
  zoomIdentity
} from 'd3'
import * as topojson from 'topojson-client'
import bounds from '@/mixins/bounds.js'
import dataset from '@/assets/geo/ind_regions_sim.json'
// const land = topojson.feature(dataset, dataset.objects.land)
const land = topojson.feature(dataset, dataset.objects.IDN_adm1_lon_lat)

export default {

  mixins: [bounds],


props: {
    zoomTransform: {
      type: Object,
      default() {
        return {
          k: 1,
          x: 0,
          y: 0
        }
      }
    },
},

  data() {
    return {
      transformProp: null,
      selection: null,
      zooming: false,
      watcher: null,

      mapwidth: 610,
      mapheight: 225,
      transform: {
        k: 1,
        x: 0,
        y: 0
      },
      d: null,
      projection: d3
        .geoEquirectangular()
        .scale(1)
        .translate([0, 0]),
      displayGrid: true
    }
  },

  computed: {
    zoom() {
      return zoom().scaleExtent([1, 3])
    },

    viewBoxString() {
      console.log(`0 0 ${this.mapwidth} ${this.mapheight}`)
      return `0 0 ${this.mapwidth} ${this.mapheight}`
    },
    realProjection() {
      return d3
        .geoEquirectangular()
        .translate(this.translator)
        .scale(this.scaler)
        .precision(0.1)
    },
  },

  created() {
    console.log('created', this)
    this.path = d3.geoPath().projection(this.projection)
    this.d = () => this.path(land)
  },

  mounted() {
    // determine scale and translation for the plot
    console.log('mounted')
    const b = this.path.bounds(land)
    this.scaler =
      0.99 /
      Math.max(
        (b[1][0] - b[0][0]) / this.mapwidth,
        (b[1][1] - b[0][1]) / this.mapheight
      )
    this.translator = [
      (this.mapwidth - this.scaler * (b[1][0] + b[0][0])) / 2,
      (this.mapheight - this.scaler * (b[1][1] + b[0][1])) / 2
    ]
    this.projection = this.realProjection
    this.path = d3.geoPath().projection(this.projection)
    this.d = () => this.path(land)

    this.selection = d3.select(this.$el)
    this.zoom.on('zoom', this.onZoom)
    this.zoom.on('start', () => {
      if (this.watcher) this.watcher()
      this.zooming = true
    })
    this.zoom.on('end', () => {
      this.zooming = false
      this.watcher = this.$watch(
        () => {
          return {
            w: this.dimensions.width,
            h: this.dimensions.height,
            k: this.zoomTransform.k,
            x: this.zoomTransform.x,
            y: this.zoomTransform.y
          }
        },
        () => {
          if (this.selection)
            this.zoom.transform(this.selection, this.zoomTransform)
        }
      )
    })
    this.zoom(this.selection)


  },
   Updated() {
       this.onZoom()
   },

  beforeDestroy() {
    if (this.watcher) this.watcher()
    this.zoom.on('zoom', null)
  },

    methods: {
    onZoom() {
      /** @type {D3ZoomEvent} */
    const e = event,
    tx = Math.min(0, Math.max(e.transform.x, this.mapwidth - this.mapwidth * e.transform.k)),
    ty = Math.min(0, Math.max(e.transform.y, this.mapheight - this.mapheight * e.transform.k))
    e.transform.x = tx
    e.transform.y = ty
    this.transformProp = {
          transform: e.transform.toString()
    }
        console.log('here', e.transform)

      this.$emit('update:zoomTransform', e.transform)
    },
    reset() {
      this.$emit('zoomTransform', zoomIdentity)
    },
    },
}
</script>

<style scoped lang="scss">
.pan-zoom {
  overflow: visible;
  position: relative;
}

.pan-content {
  clip-path: rect(0 0 0 20px);
}
</style>
