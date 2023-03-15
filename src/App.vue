<template>
  <div id="chart">
    <WaffleChart />
  </div>
</template>

<script>
import WaffleChart from "./components/WaffleChart.vue";

export default {
  name: "app",
  components: {
    WaffleChart,
  },
};
</script>

<style lang="css">
#chart {
  text-align: center;
  font-family: "Source Sans Pro", sans-serif;
}
</style>
