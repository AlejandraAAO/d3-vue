<template>
  <svg width="500" height="400" />
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
          group:'nugget'
        },
        {
          dataset: "C",
          value: -22,
          group:'nugget'
        },
        {
          dataset: "D",
          value: 2,
          group:'nugget'
        },
        {
          dataset: "E",
          value: 10,
          group:'nugget'
        },
        ////otro
        {
          dataset: "A",
          value: -15,
          group:'wantan'
        },
        {
          dataset: "B",
          value: -20,
          group:'wantan'
        },
        {
          dataset: "C",
          value: -22,
          group:'wantan'
        },
        {
          dataset: "D",
          value: 2,
          group:'wantan'
        },
        {
          dataset: "E",
          value: 10,
          group:'wantan'
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
      const margin = { top: 100, left: 40, bottom: 70, rigth: 20 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;
      
      const chartContainner = svg.append('g')
        .attr('transform', `translate(${margin.left},${margin.top})`)
        .attr('class','chart');     
      ////////////////////////////////////////////
      ////////////////   GRUPOS  /////////////////
      ///////////////////////////////////////////
      
      const groups = d3.nest()
        .key(d => d.group)
        .entries(data);
      console.log(groups)

    const yGroupScale = d3.scaleBand()
        .domain(groups.map(d => d.key))
        .rangeRound([innerHeight,0])
        .padding(0.1);
    
    const yBarScale = d3.scaleBand()
      .domain(data.map(data => data.dataset))
      .rangeRound([[0,yGroupScale.bandwidth()]])
      .padding(0.2);

    const xScale = d3.scaleLinear()
     .domain(d3.extent(data, d => d.value))
     .range([0,innerWidth]);
    
    const colorScale = d3.scaleOrdinal()
        .range([
          'rgb(32, 213, 210)',
          'rgb(169, 112, 255)',
          'rgb(0, 88, 161)',
          'rgb(108, 202, 255)',
          'rgb(238, 83, 139)',
          'rgb(0, 97, 97)',
          'rgb(216, 216, 216)'
        ])
    const barGroup = chartContainner.append('g')
      .attr('class','bar-groups')
      .selectAll('g')
      .data(groups)
      .enter()
      .append('g')
      .attr('class', data => `${data.key}`)
      .attr('transform', data => `translate(${yGroupScale(data.key)},0)`)

    const bar = barGroup.selectAll('rect')
      .data(d => d.values)
      .enter()
      .append('rect')
      .attr('x', data => xScale(data.value))
      .attr('y', data => {
        console.log(data);
        console.log(xScale(data.value));
        console.log(yBarScale(data.dataset))
        return yBarScale(data.dataset)
      })
      .attr('width', data => Math.abs(xScale(data.value) - xScale(0)))
      .attr('height', 5)
      .style('fill', data => colorScale(data.dataset))

      chartContainner.append('g')
        .attr('class','axis axis--x')
        .attr('transform',`translate(0,${innerHeight})`)
        .call(d3.axisBottom(xScale).ticks(8))

      chartContainner.append('g')
        .attr('class','axis axis--y')
        .call(d3.axisRight(yGroupScale))

      //////////////////////////////////////////
     /*  const y = d3.scaleBand()
        .range([innerHeight,0])
        .padding(0.1);
      
      const x = d3.scaleLinear()
        .range([0,innerWidth]);

      x.domain(d3.extent(data, d => d.value))
      y.domain(data.map(d => d.dataset))

      chartContainner.selectAll('.bar')
        .data(data)
        .enter().append('rect')
        .attr('class',d => `bar bar--${(d.value < 0 ? 'negative' : 'positive')}`)
        .attr('x', d => x (Math.min(0,d.value)))
        .attr('y', d => y(d.dataset))
        .attr('width', d => Math.abs(x(d.value) - x(0)))
        .attr('height', y.bandwidth());
      
      chartContainner.append('g')
        .attr('transform',`translate(0,${innerHeight})`)
        .call(d3.axisBottom(x));
      
      chartContainner.append('g')
        .attr('class','y axis')
        .attr('transform',`translate(${x(0)},0)`)
        .call(d3.axisRight(y))

 */


    }
  }
};
</script>

<style>
</style>
