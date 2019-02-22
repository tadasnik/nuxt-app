<template>
  <section class="section">
    <div class="container">
      <h1 class="title">
        indonesia maps
      </h1>
      <h2 class="subtitle">
        what not
      </h2>
    <div class="tile is-ancestor">
      <div class="tile">

          <base-map :zoom-transform.sync="zoomTransform"/>
        <!-- Add content or other tiles -->
      </div>
      <div class="tile">

          <base-map :zoom-transform.sync="zoomTransform"/>
        <!-- Add content or other tiles -->
      </div>
      <div class="tile">

          <base-map :zoom-transform.sync="zoomTransform"/>
        <!-- add content or other tiles -->
      </div>


    </div>
      </div>
  </section>
</template>

<script>
import BaseMap from '@/components/BaseMap_mod.vue'
export default {
  components: {
    BaseMap: BaseMap
  },
   data() {
       return {
  zoomTransform: {
        k: 1,
        x: 0,
        y: 0
      },
    }
   }
  }

</script>
