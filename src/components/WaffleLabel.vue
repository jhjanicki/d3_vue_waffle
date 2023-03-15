<template>
  <text v-for="d in data"
        :x="d.label?(d.label[0] * length) / unitDimension:''"
        :y="d.label?(d.label[1] * length) / unitDimension:''"
        :dx="d.ratio > 9 ? 5:9"
        :dy="d.label?length / unitDimension/1.6:''"
        fill="white"
      >
      {{d.ratio + "%"}}
    </text>
</template>

<script>
export default {
  name: "WaffleLabel",
  data: () => ({}),
  props: ['data','length','unitDimension'],
};
</script>

<style lang="css"></style>
