<template>
  <div class="bars-group" ref="container_bars"></div>
</template>
<script>
import * as d3 from "d3";
export default {
  name: "Top10",
  data() {
    return {
      data: [
        { name: "one", val: 10 },
        { name: "two", val: 20 },
        { name: "three", val: 30 },
        { name: "four", val: 40 },
        { name: "five", val: 50 },
        { name: "six", val: 60 },
        { name: "seven", val: 70 },
        { name: "eight", val: 80 },
        { name: "nine", val: 90 },
        { name: "ten", val: 100 }
      ]
    };
  },
  mounted() {
    this.buildTop(this.data);
  },
  methods: {
    buildTop(data) {
      let width = 800;
      let height = 300;
      const margin = { top: 20, left: 40, bottom: 30, rigth: 50 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;

      const svg = d3
        .select(this.$refs.container_bars)
        .append("svg")
        .attr("width", width)
        .attr("height", height);

      let x = d3
        .scaleBand()
        .rangeRound([0, width])
        .padding(0.2)
        .domain(data.map(d => d.name));

      let y = d3
        .scaleLinear()
        .rangeRound([height * 0.3 - 20, 0])
        .domain([0, d3.max(data, d => d.val)]);

      let colorLabels = d3
        .scaleOrdinal()
        .range([
          "#e1edfa",
          "#cbdff5",
          "#b8d5f5",
          "#9bc0e8",
          "#86b3e3",
          "#699ed6",
          "#4f8ac9",
          "#3a75b5",
          "#2664a6",
          "#215891"
        ]);

      const bars = svg.selectAll("g").data(data);
      const elemEnter = bars
        .enter()
        .append("g")
        .attr("transform", `translate(0,20)`);
      /* TOOLTIP---------------------------------------- */
      const div = d3
        .select(this.$refs.container_bars)
        .append("div")
        .attr("class", "tooltip-bar-top")
        .style("opacity", 0)
        .style("pointer-events", "none");
      /* ----------------------------------------------- */
      const bar = elemEnter
        .append("rect")
        .attr("x", d => x(d.name))
        .attr("class", d => d.name)
        .attr("y", d => y(d.val))
        .attr("width", x.bandwidth())
        .attr("height", d => y.range()[0] - y(d.val))
        .style("fill", (d, i) => colorLabels(i))
        .on("mouseover", d => {
          let coord = this.$refs.container_bars.getBoundingClientRect();
          let y = window.scrollY + coord.top;
          let x = window.scrollX + coord.left;

          div
            .transition()
            .duration(200)
            .style("opacity", 0.9);

          div
            .html(`dddd`)
            .style("color", "rgb(53,52,52)")
            .style("left", `${d3.event.pageX - x}px`)
            .style("top", `${d3.event.pageY - y}px`)
            .style("background-color", "white");
        })
        .on("mousemove", () => {
          let coord = this.$refs.container_bars.getBoundingClientRect();
          let y = window.scrollY + coord.top;
          let x = window.scrollX + coord.left;

          div
            .style("left", `${d3.event.pageX - x}px`)
            .style("top", `${d3.event.pageY - y}px`);
        })
        .on("mouseout", () => {
          div
            .transition()
            .duration(500)
            .style("opacity", 0);
        });

      elemEnter
        .append("text")
        .attr("dx", d => x(d.name) + 15)
        .attr("y", 90)
        .text(d => d.name);
    }
  }
};
</script>
<style>
.bars-group {
  position: relative;
  
}
.tooltip-bar-top {
  position: absolute !important;
  color: rgb(53, 52, 52);
}
</style>