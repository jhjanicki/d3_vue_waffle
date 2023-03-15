<template>
  <rect
      v-for="d in data"
      :x="(d.x * length) / unitDimension"
      :y="(d.y * length) / unitDimension"
      :width="length / unitDimension"
      :height="length / unitDimension"
      :fill="colorScale(d.category)"
      stroke="white"
      stroke-width="2"
      />
</template>

<script>
export default {
  name: "WaffleViz",
  props: ['data','length','unitDimension','colorScale'],
};
</script>

<style lang="css"></style>
