<template>
  <svg width="500" height="300" />
</template>

<script>
import * as d3 from "d3";
export default {
  name: "Dona",
  data() {
    return {
      countries: [
        {
          name: "China",
          total: 5000
        },
        {
          name: "EEUU",
          total: 4000
        },
        {
          name: "Mexico",
          total: 6500
        },
        {
          name: "Brasil",
          total: 1800
        },
        {
          name: "Perú",
          total: 3000
        }
      ]
    };
  },
  mounted() {
    this.drawChart(this.countries)
  },
  methods: {
    drawChart(data) {
      let svg = d3.select(this.$el);
      let width = +svg.attr("width");
      let height = +svg.attr("height");
      const margin = { top: 20, left: 40, bottom: 30, right: 20};
      const innertWidth = width - (margin.left + margin.right);
      const innerHeight = height - (margin.top + margin.bottom);
      
      const xValue = d => d.name;
      const yValue = d => d.total;

      const g = svg.append('g')
        .attr('transform',`translate(${margin.left},${margin.top})`);
      const xAxisG = g.append('g')
          .attr('transform', `translate(0, ${innerHeight})`);
      const yAxisG = g.append('g')
      

      const xScale = d3.scaleBand();
      const yScale = d3.scaleLinear();

      const xAxis = d3.axisBottom()
        .scale(xScale)
        .tickPadding(15)
        .tickSize(-innerHeight);
      
      const yTicks = data.length;
      const yAxis = d3.axisLeft()
        .scale(yScale)
        .ticks(yTicks)
        .tickPadding(10)
        .tickFormat(d3.format('.0s'))
        .tickSize(-innerWidth);

      xScale
          .domain(data.map(xValue))
          .range([0, innerWidth])
          .padding(0.95)
         ;

      yScale
          .domain(d3.extent(data, yValue))
          .range([innerHeight, 0])
          .nice(yTicks);  

      g.selectAll('circle').data(data)
          .enter().append('circle')
            .attr('cx', d => xScale(xValue(d)) + xScale.bandwidth() / 2)
            .attr('cy', d => yScale(yValue(d)))
            .attr('fill', 'black')
            .attr('fill-opacity', 0.6)
            .attr('r', 8);
      g.selectAll('rect').data(data)
          .enter().append('rect')
            .attr('x', d => xScale(xValue(d)))
            .attr('y', d => yScale(yValue(d)))
            .attr('width', d => xScale.bandwidth())
            .attr('height', d => innerHeight - yScale(yValue(d)))
            .attr('fill', 'black')
            .attr('fill-opacity', 0.6);
        

      xAxisG.call(xAxis);
      yAxisG.call(yAxis);  
    }
  }
};
</script>

<style>
 .domain {
        display: none;
      }
</style>
