<template id='zoom'>
  <g>
    <rect ref='zoom' style='fill: whitesmoke;' :width='dimensions.width' :height='dimensions.height'></rect>
  </g>
</template>

<script>
import * as d3 from 'd3';

export default {
  template: '#zoom',
  props: {
    dimensions: {
      type: Object,
    },
  },
  computed: {
    zoom() {
      return d3.zoom().on('zoom', this.zoomed);
    },
  },
  mounted () {
    console.log("#zoom");
  },
  mounted() {
    d3.select(this.$refs.zoom)
      .call(this.zoom);
  },
  methods: {
    zoomed() {
      console.log('zoomed');
    },
  },
};
</script>