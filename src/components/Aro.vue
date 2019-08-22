<template>
  <div ref="aro_container"></div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "PieComplete",
  data() {
    return {
      countries: [
        {
          name: "No interesado",
          total: 24377
        },
        {
          name: "No contactado",
          total: 7448
        },
        {
          name: "Oportunidad de venta",
          total: 1450
        },
        {
          name: "No regresar",
          total: 326
        },
        {
          name: "Vendido",
          total: 372
        },
        {
          name:"Pendiente",
          total: 76
        }
      ]
    };
  },
  mounted() {
    this.renderData2(this.countries);
  },
  methods: {
    renderData2(data) {
      //conf inicial
      let width = 500;
      let height = 300;
      let radius = Math.min(width, height) / 2;
      const svg = d3
        .select(this.$refs.aro_container)
        .append("svg")
        .attr("width", width)
        .attr("height", height);
      /* Legend ---------------------------------------- */

      let keys = data.map(d => d.name);
      let nugget = ['o']
      console.log("keys", keys);
      let colorLabels = d3
        .scaleOrdinal()
        .domain(keys)
        .range(["#b39ddb", "#9fa8da",'#9beb8d','#f7ec86','#a3dce6','#e6a3a3','#8f8f8f']);

      svg
        .selectAll("mydots")
        .data(keys)
        .enter()
        .append("circle")
        .attr("cx", 100)
        .attr("cy", (d, i) => 100 + i * 25)
        .attr("transform", `translate(${320},${0})`)
        .attr("r", 7)
        .style("fill", d => colorLabels(d));

      svg
        .selectAll("mylabels")
        .data(keys)
        .enter()
        .append("text")
        .attr("x", 120)
        .attr("y", (d, i) => 100 + i * 25)
        .style("fill", 'black')
        .attr("transform", `translate(${320},${0})`)
        .text(d => d)
        .attr("text-anchor", "left")
        .style("alignment-baseline", "middle");

        svg
          .selectAll("total")
          .data(nugget)
          .enter()
          .append("text")
          .attr("x", 120)
          .attr("y", (d, i) => 100 + i * 25)
          .text(`Total`)
          .style("fill", 'black')
          .style('font-size','2rem')
          .attr("transform", `translate(${100},${0})`)

      //----------------------------------------
      const margin = { top: 20, left: 40, bottom: 20, rigth: 20 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;

      const g = svg
        .append("g")
        .attr("transform", `translate(${width / 2},${height / 2})`);

      const color = d3.scaleOrdinal(["#b39ddb", "#9fa8da",'#9beb8d','#f7ec86','#a3dce6','#e6a3a3','#8f8f8f']);

      const pie = d3.pie().value(d => d.total);

      const arc = d3
        .arc()
        .innerRadius(100)
        .outerRadius(radius - 10);

      const label = d3
        .arc()
        .innerRadius(radius - 80)
        .outerRadius(radius);

      const containner = svg.append("g");

      const arcs = g
        .selectAll(".arc")
        .data(pie(data))
        .enter()
        .append("g")
        .attr("class", "arc");

      arcs
        .append("path")
        .attr("d", arc)
        .attr("fill", (d, i) => color(i));

      console.log(arcs);

      /* arcs
        .append("text")
        .attr("transform", d => `translate(${label.centroid(d)})`)
        .text(d => {
          console.log("D del text: ", d);
          return d.data.name;
        }); */
    }
  }
};
</script>

<style>
</style>
