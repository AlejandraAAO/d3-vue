<template>
  <svg width="650" height="300"/>
</template>

<script>
import * as d3 from "d3";

export default {
  name:'GroupBars',
  data() {
    return {
      dataVals: [
        {
          step: "Mars",
          complexity: -10,
          task: "Task 1"
        },
        {
          step: "Jupiter",
          complexity: 60,
          task: "Task 1"
        },
        {
          step: "Neptune",
          complexity: 10,
          task: "Task 1"
        },
        {
          step: "Venus",
          complexity: 25,
          task: "Task 1"
        },
        {
          step: "Mars",
          complexity: 30,
          task: "Task 2"
        },
        {
          step: "Jupiter",
          complexity: 10,
          task: "Task 2"
        },
        {
          step: "Neptune",
          complexity: 50,
          task: "Task 2"
        },
        {
          step: "Mars",
          complexity: 20,
          task: "Task 3"
        },
        {
          step: "Jupiter",
          complexity: 40,
          task: "Task 3"
        },
        {
          step: "Mars",
          complexity: 45,
          task: "Task 4"
        },
        {
          step: "Jupiter",
          complexity: 60,
          task: "Task 4"
        },
        {
          step: "Mars",
          complexity: 35,
          task: "Task 5"
        },
        {
          step: "Jupiter",
          complexity: 30,
          task: "Task 5"
        },
        {
          step: "Neptune",
          complexity: 50,
          task: "Task 5"
        },

        {
          step: "Mars",
          complexity: 35,
          task: "Task 6"
        },
        {
          step: "Jupiter",
          complexity: 30,
          task: "Task 6"
        },
        {
          step: "Neptune",
          complexity: 50,
          task: "Task 6"
        },
        
        {
          step: "Mars",
          complexity: 35,
          task: "Task 7"
        },
        {
          step: "Jupiter",
          complexity: 30,
          task: "Task 7"
        },
        {
          step: "Neptune",
          complexity: 50,
          task: "Task 7"
        }
      ]
    };
  },
  mounted() {
    this.getValues(this.dataVals);
  },
  methods: {
    getValues(data) {
      console.log("grupos");
      const svg = d3.select(this.$el);
      let width = +svg.attr("width");
      let height = +svg.attr("height");
      const margin = { top: 20, left: 40, bottom: 50, rigth: 10 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;

      /* Legend ---------------------------- */
      let keys = ['LW','L4W','LM','LY'];
      let colorLabels = d3.scaleOrdinal()
        .domain(keys)
        .range(["#b39ddb", "#9fa8da", "#90caf9","#a5d6a7","#c5e1a5","#ffe082","#ffab91"])


      svg.selectAll('mydots')
        .data(keys)
        .enter()
        .append('circle')
          .attr('cx', 100)
          .attr('cy', (d,i) => 100 + i*25)
          .attr('transform',`translate(470,0)`)
          .attr('r',7)
          .style('fill', d => colorLabels(d));

      svg.selectAll('mylabels')
        .data(keys)
        .enter()
        .append('text')
          .attr('x',120)
          .attr('y', (d,i) => 100+ i*25)
          .style('fill',d => colorLabels(d))
          .attr('transform',`translate(470,0)`)
          .text(d => d)
          .attr('text-anchor','left')
          .style('alignment-baseline','middle')


      /* ----------------------------------- */
      const chartContainner = svg.append('g')
        .attr('transform',`translate(${margin.left},${margin.top})`)
        .attr('class','chart');
      
      const groups = d3.nest()
        .key(d => d.task)
        .entries(data);
      console.log(groups)
      /* [{
            key: 'nugget1',
            vals:[
              {step:'bbb',jjj:'ooo}
            ]
          }, {...}
          ] */
      const xGroupScale = d3.scaleBand()
        .domain(groups.map(d => d.key))
        .rangeRound([0,innerWidth])
        .padding(0.1);
      
      const XBarScale = d3.scaleBand()
        .domain(data.map(data => data.step))
        .rangeRound([0,xGroupScale.bandwidth()])
        .padding(0.2);
      
      const yScale = d3.scaleLinear()
        .domain([0,d3.max(data, data => data.complexity)])
        .range([0,innerHeight])
        .nice();

      const yAxisScale = d3.scaleLinear()
        .domain([d3.min(data, d => d.complexity), d3.max(data, d => d.complexity)])
        .range([innerHeight - yScale(d3.min(data, d => d.complexity)),0])

      const colorScale = d3.scaleOrdinal()
        .range(["#b39ddb", "#9fa8da", "#90caf9","#a5d6a7","#c5e1a5","#ffe082","#ffab91"])

        //barras
      const barGroup = chartContainner.append('g')
        .attr('class','bar-groups')
        .selectAll('g')
        .data(groups)
        .enter()
        .append('g')
        .attr('class', data => `${data.key}`)
        .attr('transform', data => `translate(${xGroupScale(data.key)},0)`);

      const bar = barGroup.selectAll('rect')
        .data(d => d.values)
        .enter()
        .append('rect')
        .attr('x', data => XBarScale(data.step))
        .attr('width',XBarScale.bandwidth())
        .attr('y', data => innerHeight - Math.max(0,yScale(data.complexity)))
        .attr('height',data => Math.abs(yScale(data.complexity)))
        .style('fill',data => colorScale(data.step))

      //ejes

      chartContainner.append('g')
        .attr('class','axis axis--x')
        .attr('transform', `translate(0,${innerHeight})`)
        .call(d3.axisBottom(xGroupScale));

      chartContainner.append('g')
        .attr('class','axis axis--y')
        .call(d3.axisLeft(yAxisScale).ticks(8).tickSize(-innerWidth));
      
      chartContainner.selectAll('.axis')
        .selectAll('path')
        .style('stroke','none');

     
    }
  }
};
</script>

<style>
</style>
