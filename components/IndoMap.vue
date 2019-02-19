<template>
<div id="map-wrap" style="height: 100%">
</div>
</template>

<script>
import * as d3 from 'd3-geo'
//import * as topojson from 'topojson-client'
import dataset from '@/assets/geo/ind_regions_sim.json'
//const projection = d3.geoEquirectangular();

//const land = topojson.feature(dataset, dataset.objects.land)
export default {
  mounted() {
    const land = d3.json(dataset)
    this.d = () => this.path(land)
  }
}

</script>
<style>
</style>
