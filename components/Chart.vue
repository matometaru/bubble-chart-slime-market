<template>
  <div>
    <svg ref='chart' width='100%' viewbox='0 0 960 600'>
      <defs>
        <clipPath id="myClip">
          <rect :width='chartWidth' :height='chartHeight'></rect>
        </clipPath>
      </defs>
      
      <g :style='{transform: `translate(${margin.left}px, ${margin.top}px)`}'>
        <v-zoom :dimensions='dimensions' />
        
        <v-scatter clip-path="url(#myClip)" :x-scale='xScale' :y-scale='yScale' :data='data' :radius='radius' />
        
        <v-axis :data='data' :x-scale='xScale' :y-scale='yScale' :dimensions='dimensions' :labels='labels' />
      </g>
    </svg>
  </div>
</template>

<script>
import * as d3 from 'd3';
import Axis from '~/components/Axis.vue';
import Scatter from '~/components/Scatter.vue';
import Zoom from '~/components/Zoom.vue';

export default {
  props: {
    data: {
      type: Array,
      required: true,
    },
    labels: {
      type: Object,
    },
    radius: {
      type: Number,
      default: 4,
    },
  },
  components: {
    'v-scatter': Scatter,
    'v-axis': Axis,
    'v-zoom': Zoom,
  },
  mounted () {
    console.log("#chart");
  },
  data: () => ({
    height: 600,
    width: 960,
    margin: {
      top: 20,
      right: 20,
      bottom: 50,
      left: 70,
    },
  }),
  computed: {
    chartWidth() {
      return this.width - this.margin.left - this.margin.right;
    },
    chartHeight() {
      return this.height - this.margin.top - this.margin.bottom;
    },
    xScale() {
      console.log(this.chartWidth);
      return d3.scaleLinear()
        .range([0, this.chartWidth])
        .domain(d3.extent(this.data, d => d.x))
        .nice();
    },
    yScale() {
      console.log(this.chartHeight);
      return d3.scaleLinear()
        .range([this.chartHeight, 0])
        .domain(d3.extent(this.data, d => d.y))
        .nice();
    },
    dimensions() {
      return {
        width: this.chartWidth,
        height: this.chartHeight,
        margin: this.margin,
      };
    },
  },
};
</script>