<template>
  <g class='axis'>
    <text class='axis--label' :style='{transform: `translate(${-dimensions.margin.left / 1.5}px, ${dimensions.height / 2}px) rotate(-90deg)`}'>{{ labels.y }}</text>
    
    <text class='axis--label' :style='{transform: `translate(${dimensions.width / 2}px, ${dimensions.height + dimensions.margin.bottom}px)`}'>{{ labels.x }}</text>
    <g class='axis--y' ref='yAxis'></g>
    <g class='axis--x' :style='{transform: `translate(0px, ${this.dimensions.height}px)`}' ref='xAxis'></g>
  </g>
</template>

<script>
import * as d3 from 'd3';

export default {
  props: {
    data: {
      type: Array,
    },
    labels: {
      type: Object,
      default:() => ({x: 'Axis X', y: 'Axis y'}),
    },
    xScale: {
      type: Function,
      required: true,
    },
    yScale: {
      type: Function,
      required: true,
    },
    dimensions: {
      type: Object,
      required: true,
    },
  },
  computed: {
    xAxis() {
      return d3.axisBottom(this.xScale).ticks(this.tick);
    },
    yAxis() {
      return d3.axisLeft(this.yScale).ticks(this.tick);
    },
    tick() {
      switch (this.$vuetify.breakpoint.name) {
         case 'xs': return 5;
         case 'sm': return 10;
         case 'md': return 15;
         case 'lg': return 20;
         case 'xl': return 25;
       };
    },
  },
  methods: {
    updateAxes() {
      const t = d3.transition().duration(750);
      d3.select(this.$refs.xAxis)
        .transition(t)
        .call(this.xAxis);
      d3.select(this.$refs.yAxis)
        .transition(t)
        .call(this.yAxis)
    },
  },
  mounted() {
    this.updateAxes();
  },
  watch: {
    data: {
      deep: true,
      handler() {
        this.updateAxes();
      },
    },
    tick() {
      this.updateAxes();
    },
  },
};
</script>