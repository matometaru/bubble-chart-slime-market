<template>
  <g class='scatter'>
    <template v-for='point in data'>
      <circle :key='"circle-" + point.id' class='point' :cx='xScale(point.x)' :cy='yScale(point.y)' :r='point.radius' :style='point.style'></circle>
      <text :key='"text-" + point.id' :x='xScale(point.x)' :y='yScale(point.y)'>{{point.name}}</text>
    </template>
  </g>
</template>

<script>
export default {
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
  }
};
</script>