<template>
  <svg width="500" height="300">
    <circle cx="40" cy="60" r="10" style="fill:#98abc5;"></circle>
    <circle cx="160" cy="60" r="10" style="fill:#8a89a6;"></circle>
    <circle cx="290" cy="60" r="10" style="fill:#7b6888;"></circle>
  </svg>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "Top",
  data() {
    return {
      data: [
        { name: "one", val: 100 },
        { name: "two", val: 150 },
        { name: "three", val: 200 }
      ]
    };
  },
  mounted() {
    //console.log("x");
    this.buildCircles(this.data);
  },
  methods: {
    buildCircles(data) {
      /* const svg = d3.select(this.$el);
      let width = +svg.attr("width");
      let height = +svg.attr("height");
      let radius = Math.min(width, height) / 2;
      const margin = { top: 20, left: 40, bottom: 20, rigth: 20 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;
      const colors = d3.scaleOrdinal([
        "#98abc5",
        "#8a89a6",
        "#7b6888",
        "#6b486b",
        "#a05d56"
      ]); */
      const svg = d3.select(this.$el);

      const circle = svg.selectAll('circle')
        .data(this.data.map(d => d.val))
        .attr('r', d => Math.sqrt(d)*4)
   
    }
  }
};
</script>

<style>
</style>
