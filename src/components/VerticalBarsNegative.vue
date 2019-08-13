<template>
  <svg width="500" height="300" />
</template>

<script>
import * as d3 from "d3";
export default {
  name: "VerticalBarsNegative",
  data() {
    return {
      dataVals: [
        {
          name: "A",
          value: -15
        },
        {
          name: "B",
          value: -20
        },
        {
          name: "C",
          value: -22
        },
        {
          name: "D",
          value: 2
        },
        {
          name: "E",
          value: 10
        }
      ]
    };
  },
  created() {
    this.buildBars(dataVals);
  },
  methods: {
    buildBars(data) {
      const svg = d3.select(this.$el);
      let width = +svg.attr("width");
      let height = +svg.attr("height");
      const margin = { top: 20, left: 40, bottom: 70, rigth: 20 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;



    }
  }
};
</script>

<style>
</style>
