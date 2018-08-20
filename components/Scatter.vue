<template id='scatter'>
  <g class='scatter'>
    <circle v-for='(point, index) in data' :key='index' class='point' :cx='xScale(point.x)' :cy='yScale(point.y)' :r='radius' :style='pointStyle'></circle>
  </g>
</template>

<script>
import * as d3 from 'd3';

export default {
  template: '#scatter',
  props: {
    data: {
      type: Array,
      required: true,
    },
    xScale: {
      type: Function,
      required: true,
    },
    yScale: {
      type: Function,
      required: true,
    },
    radius: {
      type: Number,
      default: 4,
    },
    pointStyle: {
      type: Object,
      default: () => ({
        stroke: '#43e742',
        fill: '#ffffff',
        strokeWidth: 2,
      }),
    },
  }
};
</script>