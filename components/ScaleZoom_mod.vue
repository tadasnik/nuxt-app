<template>
    <div>
  <svg
    ref="svg"
    class="pan-zoom"
    :zoom-transform="this.zoomTransform"
    :viewBox="viewBoxString"

  >
    <!-- Inner Content -->
    <svg
      v-if="$slots.default"
      style="overflow: hidden;"
      width="100%"
      height="100%"
      class="pan-content"
    >

      <g v-bind="transformProp">

        <slot />
      </g>
    </svg>

  </svg>
  </div>
</template>

<script>
import bounds from '@/mixins/bounds.js'
import {
  zoom,
  event,
  // eslint-disable-next-line no-unused-vars
  D3ZoomEvent,
  // eslint-disable-next-line no-unused-vars
  ZoomBehavior,
  zoomIdentity
} from 'd3'
import * as d3 from 'd3'
export default {

  mixins: [bounds],
  inheritAttrs: false,
  props: {
      viewBoxString: {
          type: String,
          default: `0 0 100 100`
      },

    scaleContent: {
      type: Boolean,
      default: true
   },

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
    showGrid: Boolean
  },
  data() {
    return {
      transformProp: null,
      selection: null,
      zooming: false,
      watcher: null
    }
  },
  computed: {
    /** @returns {ZoomBehavior} */
    zoom() {
      return zoom().scaleExtent([1, 3])
    },
  },
  mounted() {
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
  beforeDestroy() {
    if (this.watcher) this.watcher()
    this.zoom.on('zoom', null)
  },
  methods: {
    onZoom() {
      /** @type {D3ZoomEvent} */
      const e = event
      //if (!e.transform.rescaleX) return
      //this.rescaleX = e.transform.rescaleX(this.scaleX)
      //this.rescaleY = e.transform.rescaleY(this.scaleY)

      if (this.scaleContent) {
        this.transformProp = {
          transform: e.transform.toString()
        }
      }
        console.log(this.transformProp.transform)
      // debugger
      // }

      this.$emit('zoomTransform', e.transform)
        console.log('emit', e.transform)
    },
    reset() {
      this.$emit('zoomTransform', zoomIdentity)
    },
    updateZoom() {
      console.log('shoulve updates')
      this.onZoom()
    }
  }
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
