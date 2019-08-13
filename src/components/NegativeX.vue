<template>
  <svg width="500" height="300" />
</template>

<script>
import * as d3 from "d3";
export default {
  name: "NegativeX",
  data() {
    return {
      dataVals: [100, -100, -150, 55, 150, 120, 450, 980, 1200]
    };
  },
  mounted() {
    this.builBars(this.dataVals);
  },
  methods: {
    builBars(data) {
      let svg = d3.select(this.$el);
      console.log(svg);
      let width = +svg.attr("width");
      let height = +svg.attr("height");
      const barWidth = 30;
      const margin = { top: 20, left: 40, bottom: 30, right: 20 };
      const innertWidth = width - (margin.left + margin.right);
      const innerHeight = height - (margin.top + margin.bottom);

      const yScale = d3
        .scaleLinear()
        .domain([0, d3.max(data)])
        .range([0, innerHeight]);

      const yAxisScale = d3
        .scaleLinear()
        .domain([d3.min(data), d3.max(data)])
        .range([innerHeight - yScale(d3.min(data)), 0]);

      svg
        .selectAll("rect")
        .data(data)
        .enter()
        .append("rect")
        .attr("x", (d, i) => margin.left + i * barWidth)
        .attr("y", (d, i) => innerHeight - Math.max(0, yScale(d)))
        .attr("height", d => Math.abs(yScale(d)))
        .attr('width', barWidth)
      .style('fill','grey')
      .style("stroke", "black")
      .style('stroke-width','1px')
      .style('opacity',(d,i) => 1);

       var yAxis = d3.axisLeft(yAxisScale);
       
      svg.append('g')
      .attr('transform', d => `translate(${margin.left},0)`)
      .call(yAxis);

    }
  }
};
</script>

<style>
</style>
