<template>
  <svg width="500" height="300" />
</template>

<script>
import * as d3 from "d3";
export default {
  name: "VerticalGroupBars",
  data() {
    return {
      dataVals: [
        {
          dataset: "A",
          value: -15,
          group:'nugget'
        },
        {
          dataset: "B",
          value: -20,
          group:'wantan'
        },
        {
          dataset: "C",
          value: -22,
          group:'nugget'
        },
        {
          dataset: "D",
          value: 2,
          group:'wantan'
        },
        {
          dataset: "E",
          value: 10,group:'nugget'
        }
      ]
    };
  },
  mounted() {
    this.buildBars(this.dataVals);
  },
  methods: {
    buildBars(data) {
      const svg = d3.select(this.$el);
      let width = +svg.attr("width");
      let height = +svg.attr("height");
      const margin = { top: 20, left: 40, bottom: 70, rigth: 20 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;
      
      svg.attr('transform', `translate(${margin.left},${margin.top})`);     
      ////////////////////////////////////////////
      ////////////////   GRUPOS  /////////////////
      ///////////////////////////////////////////
      
      const groups = d3.nest()
        .key(d => d.group)
        .entries(data);
      console.log(groups)

    const yGroupScale = d3.scaleBand()
        .domain(groups.map(d => d.key))
        .rangeRound([])


      //////////////////////////////////////////
      const y = d3.scaleBand()
        .range([innerHeight,0])
        .padding(0.1);
      
      const x = d3.scaleLinear()
        .range([0,innerWidth]);

      x.domain(d3.extent(data, d => d.value))
      y.domain(data.map(d => d.dataset))

      svg.selectAll('.bar')
        .data(data)
        .enter().append('rect')
        .attr('class',d => `bar bar--${(d.value < 0 ? 'negative' : 'positive')}`)
        .attr('x', d => x(Math.min(0,d.value)))
        .attr('y', d => y(d.dataset))
        .attr('width', d => Math.abs(x(d.value) - x(0)))
        .attr('height', y.bandwidth());
      
      svg.append('g')
        .attr('transform',`translate(0,${innerHeight})`)
        .call(d3.axisBottom(x));
      
      svg.append('g')
        .attr('class','y axis')
        .attr('transform',`translate(${x(0)},0)`)
        .call(d3.axisRight(y))




    }
  }
};
</script>

<style>
</style>
