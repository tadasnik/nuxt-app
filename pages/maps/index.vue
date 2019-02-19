<template>
    <div class="tile is-ancestor">
      <div class="tile is-6 is-vertical is-parent">
        <div class="tile is-child box">
            <indomap></indomap>
        </div>
        <div class="tile is-child box">
          <p class="title">Two</p>
        </div>
      </div>
      <div class="tile is-parent">
        <div class="tile is-child box">
          <p class="title">Three</p>
          <svg>
            <d3arc></d3arc>
        </svg>
        </div>
      </div>
    </div>
 
</template>

<script>

import D3Arc from "@/components/D3Arc.vue";
import IndoMap from "@/components/IndoMap.vue"
import * as d3 from 'd3'
export default {
  mounted () {
      var svg = d3.select('h4')
                .append('svg')
                .attr('width', 200)
                .attr('height', 100)
       svg.append('rect')
        .attr('width', '100%')
        .attr('height', '100%')
        .attr('fill', 'pink')
  },
    components: {
        d3arc: D3Arc,
        indomap: IndoMap
    }
}
</script>
