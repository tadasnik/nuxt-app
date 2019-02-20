<template>
  <svg :viewBox="viewBoxString"
      fill="white">
    <rect width="100%"
          height="100%"
          x="0%"
          y="0%"
          fill="#909090"
          opacity=0.5
          />
    <path fill="#1cdd87"
            stroke="white"
            stroke-width="0.5"
          :d="d()" />
  </svg>
</template>

<script>
import * as d3 from 'd3'
import * as topojson from 'topojson-client'
import dataset from '@/assets/geo/ind_regions_sim.json'
//const land = topojson.feature(dataset, dataset.objects.land)
const land = topojson.feature(dataset, dataset.objects.IDN_adm1_lon_lat)

export default {
  data() {

    return {

      mapwidth: 610,
      mapheight: 230,

      d: null,
      projection: d3
        .geoEquirectangular()
        .scale(1)
        .translate([0, 0]),

      tickAmount: Number,
      showGrid: Boolean
    }
  },
  beforeMount() {
    this.path = d3.geoPath().projection(this.projection)
    this.d = () => this.path(land)
  },
  mounted() {
    //determine scale and translation for the plot
    var b = this.path.bounds(land)
    this.scaler =
      0.99 /
      Math.max(
        (b[1][0] - b[0][0]) / this.mapwidth,
        (b[1][1] - b[0][1]) / this.mapheight
      )
     this.translator = [(this.mapwidth - this.scaler * (b[1][0] + b[0][0])) / 2, (this.mapheight - this.scaler * (b[1][1] + b[0][1])) / 2];
    this.projection = this.realProjection
    this.path = d3.geoPath().projection(this.projection)
    this.d = () => this.path(land)
    },
  computed: {
    viewBoxString () {
      return `0 0 ${this.mapwidth} ${this.mapheight}`
    },
    realProjection() {
      return d3[
        'geoEquirectangular'
      ]()
        .translate(this.translator)
        .scale(this.scaler)
        .precision(0.1)
    }
  }
}
</script>
