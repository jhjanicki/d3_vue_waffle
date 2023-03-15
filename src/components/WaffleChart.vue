<template>
  <svg :width="length + margin * 2" :height="length + margin * 2">
    <g :transform="`translate(${margin},${margin})`">
      <WaffleViz 
        :data="waffleData[0]" 
        :length="length" 
        :unitDimension = "unitDimension"
        :colorScale = "colorScale"
      />
      <WaffleLabel
        :data="waffleData[0]" 
        :length="length" 
        :unitDimension = "unitDimension"
      />
    </g>
    <g :transform="`translate(${margin},${margin+legendPadding + length})`">
      <WaffleLegend 
      :data="dataSummed" 
      :categoryHeight="categoryHeight"
      :colorScale = "colorScale"
      ></WaffleLegend>
    </g>
  </svg>
</template>

<script>
import * as d3 from "d3";
import data from "../assets/data.json";
import WaffleViz from "./WaffleViz.vue";
import WaffleLabel from "./WaffleLabel.vue";
import WaffleLegend from "./WaffleLabel.vue";

export default {
  name: "WaffleChart",
  data: () => ({
    length: 400,
    margin: 30,
    categoryHeight: 20,
    legendPadding: 20,
    unitDimension:10
  }),
  components: {
    WaffleViz,
    WaffleLabel,
    WaffleLegend,
  },
  computed: {
    data() {
      return data;
    },
    categoriesSorted() {
      return [
        "Critically Endangered",
        "Endangered",
        "Vulnerable",
        "Near Threatened",
        "Least Concern",
        "Data Deficient",
      ];
    },
    colorScale() {
      return d3
        .scaleOrdinal()
        .domain(this.categoriesSorted)
        .range([
          "#b30000",
          "#e34a33",
          "#fc8d59",
          "#fdbb84",
          "#fee8c8",
          "#d9d9d9",
        ]);
    },
    legendHeight(){
      return this.categoryHeight * this.categoriesSorted.length;
    },
    dataSummed() {
      //summarise data & calculate ratio & raw count for each category
      const rolledData = this.data.map((entry) =>
        d3.map(
          d3.rollup(
            this.data,
            (v) => v.length,
            (d) => d.redlistCategory
          ),
          ([category, result]) => ({
            category: category,
            ratio: (result / this.data.length) * 100,
            quantity: result,
          })
        )
      )[0];

      // sort the data based on the order of categories
      const order = {}; // map for efficient lookup of sortIndex
      for (let i = 0; i < this.categoriesSorted.length; i++) {
        order[this.categoriesSorted[i]] = i;
      }

      return rolledData.sort((a, b) => order[a.category] - order[b.category]);
    },
    waffleData() {
      const array = [];
      const max = this.dataSummed.length - 1;
      let index = 0,
        curr = 1,
        accu = Math.round(this.dataSummed[0].ratio),
        waffle = [],
        category = "",
        ratio = "",
        labelX = 0,
        labelY = 0;
  
      for (let y = 0; y < 10; y++)
        for (let x = 0; x < 10; x++) {
          if (curr > accu && index < max) {
            let r = Math.round(this.dataSummed[++index].ratio);
            while (r === 0 && index < max) r = Math.round(this.dataSummed[++index].ratio);
            accu += r;
            labelX = x;
            labelY = y;
          }
          category = this.dataSummed[index].category;
          ratio = this.dataSummed[index].ratio.toFixed(0);
          waffle.push({
            x,
            y,
            index,
            ratio,
            category,
            label: [labelX, labelY],
          });
          curr++;
        }
      array.push(waffle);
      return array;
    },
  },
};
</script>

<style lang="css"></style>
