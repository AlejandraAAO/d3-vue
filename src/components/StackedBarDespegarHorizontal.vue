<template>
  <div class="container_stackedbar" ref="container_stackedbar">
    <svg ref="stackedbar" class="class_stackedbar" />
  </div>
</template>
<script>
import * as d3 from "d3";
export default {
  name: "StackedBarDespegar",
  data() {
    return {
      dataRows: [
        { day: "Mon", apricots: 120, blueberries: 180},
        /*{ day: "Tue", apricots: 60, blueberries: 185},
        { day: "Wed", apricots: 100, blueberries: 215},
        { day: "Thu", apricots: 150, blueberries: 330} ,
        { day: "Fri", apricots: 120, blueberries: 240} */
      ]
    };
  },
  mounted() {
    this.buildStackedBar(this.dataRows);
  },
  methods: {
    buildStackedBar(data) {
      let width = 400;
      let height = 200;
      const margin = { top: 20, left: 170, bottom: 5, rigth: 10 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;
      const colors = ["#00D7D2", "#FF4436", "#313c53"];

      
      /* tooltip */
      const tooltipStacked = d3
      .select(this.$refs.container_stackedbar)
      .append('div')
      .attr('class','tooltipstacked')
      .attr('opacity',0)
      .style('pointer-events','none');
      /* tooltip */

      let svg = d3
        .select(this.$refs.stackedbar)
        .attr("width", width)
        .attr("height", height)
        .attr("viewBox", "0 0 400 250");

      let keysObject = Object.keys(data[0]).slice(1);
      /* ["redDelicious", "mcintosh", "oranges", "pears"] */
      /* console.log("keysObject", keysObject); */
      
      /* legend */
      let colorLabels = d3
        .scaleOrdinal()
        .domain(keysObject)
        .range(colors)

      svg
        .selectAll('mydots')
        .data(keysObject)
        .enter()
        .append('circle')
        .attr('cx',100)
        .attr('cy',(d,i) => 100 + i * 25)
        .attr('transform',`translate(${-130},${-70})`)
        .attr('r',7)
        .style('fill', d => colorLabels(d));

      svg
        .selectAll('mylabels')
        .data(keysObject)
        .enter()
        .append('text')
        .attr('x',120)
        .attr('y', (d,i) => 100+ i * 25)
        .style('fill', "rgb(53, 52, 52")
        .attr('transform',`translate(${-130},${-70})`)
        .text(d => d)
        .attr('text-anchor','left')
        .style('alignment-baseline','middle')
      /* legend */
      let stack = d3.stack().keys(keysObject);
      /* console.log("stack", stack); */

      let stackedSeries = stack(data);
      console.log("stackedSeries", stackedSeries);

      const g = svg
        .append("g")
        .attr("transform", `translate(${margin.left},${margin.top})`)
        .attr('class','big-container-stacked');

      let gr = svg
        .select("g")
        .selectAll("g.series")
        .data(stackedSeries)
        .enter()
        .append("g")
        .classed("series", true)
        .style("fill", (d, i) => {
          return colors[i];
        });
        const x = d3.scaleLinear().rangeRound([0, innerWidth]);
        const y = d3.scaleBand().rangeRound([innerHeight, 0])
        .padding(0.1);
      /* const x = d3
        .scaleBand()
        .rangeRound([0, innerWidth])
        .padding(0.1);

      const y = d3.scaleLinear().range([innerHeight, 0]); */

      y.domain(data.map(d => d.day));
      x.domain([
        0,
        d3.max(stackedSeries, d => {
          return d3.max(d, d => {
            console.log("d[0] = " + d[0], "d[1] = " + d[1]);
            return d[0] + d[1];
          });
        })
      ]);

      gr.append("g")
        .attr("class", "x axis")
        .attr("transform", `translate(0,${innerHeight})`)
        .call(d3.axisBottom(x));

      gr.append("g")
        .attr("class", "y axis")
        .call(d3.axisLeft(y));

    let _self = this;
      const rectangulos = gr
        .selectAll("rect")
        .data(d => d)
        .enter()
        .append("rect")
        .attr("height", d => {
          return /* y(d[0]) - y(d[1]); */y.bandwidth()
        })
        .attr("y", d => {
          return /* (d[1]) */y(d.data.day);
        })
        .attr("x", (d, i) => {
          return /* x(d.data.day)  */x(d[0]);
        })
        .attr("width",(d,i) => {
            return x(d[1]) - x(d[0])
        }/*  x.bandwidth() */)
        .on('mouseover', function(d,i){
          let coord = _self.$refs.container_stackedbar.getBoundingClientRect();
          let y = window.scrollY + coord.top;
          let x = window.scrollX + coord.left;

          tooltipStacked
          .transition()
          .duration(200)
          .style('opacity',0.9);
          tooltipStacked
            .html(`nugget ${d[1] - d[0]}`)
            .style('color', 'rgb(53, 52, 52)')
            .style('left', `${d3.event.pageX - x+10}px`)
            .style('top', `${d3.event.pageY - y}px`)
            .style('background-color', 'white')
        })
        .on('mousemove', function(d,i){
          let coord = _self.$refs.container_stackedbar.getBoundingClientRect();
          let y = window.scrollY + coord.top;
          let x = window.scrollX + coord.left;

          tooltipStacked
            .style('left',`${d3.event.pageX - x+10}px`)
            .style('top', `${d3.event.pageY - y}px`)
        })
        .on('mouseout', function(d,i){
          tooltipStacked
            .transition()
            .duration(500)
            .style('opacity',0);
        });

     
      /* let dataset = d3.map(data, d => d.day).keys(); */
      /* valores de otro eje */
      /*  ["2006", "2007", "2008", "2009", "2010", "2011", "2012", "2013", "2014", "2015", "2016"]*/
    }
  }
};
</script>
<style >
  .container_stackedbar {
  position: relative;
}  
  .container_stackedbar .tooltipstacked {
   position: absolute !important;
  color: rgb(53, 52, 52);
  }

</style>