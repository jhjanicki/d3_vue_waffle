<template>
  <g
    v-for="d in data"
    :transform="`translate(0,${i * this.categoryHeight})`"
  >
    <text transform="translate(15,9)"> {{ d.category }} </text>
    <rect :fill="colorScale(d.category)" width="10" height="10"></rect>
  </g>
</template>

<script>
export default {
  name: "WaffleLegend",
  data: () => ({}),
  props: ["data", "categoryHeight", "colorScale"],
};
</script>

<style lang="css"></style>
