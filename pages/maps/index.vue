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

          <indo-map />
        <!-- Add content or other tiles -->
      </div>
      <div class="tile">

          <indo-map />
        <!-- Add content or other tiles -->
      </div>
    </div>
    </div>
  </section>
</template>

<script>
import IndoMap from '@/components/IndoMap.vue'
export default {
  components: {
    IndoMap: IndoMap
  }
}
</script>
