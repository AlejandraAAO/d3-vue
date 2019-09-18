<template>
  <div class="container-div-bar-vertical" ref="container_bar-vertical">
    <svg ref="verticalbarspositive" />
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "TwoGraphics",
  data() {
    return {
      countries: [
        {
          name: "ROM",
          total: 100
        },
        {
          name: "SMO",
          total: 200
        },
        {
          name: "JRT",
          total: 300
        },
        {
          name: "VCO",
          total: 400
        },
        {
          name: "VPM",
          total: 450
        }
      ],
      line: [
        {
          name: "ROM",
          total: 200
        },
        {
          name: "SMO",
          total: 300
        },
        {
          name: "JRT",
          total: 150
        },
        {
          name: "VCO",
          total: 250
        },
        {
          name: "VPM",
          total: 400
        }
      ]
    };
  },
  mounted() {
    console.log(this.countries);
    this.renderRectan(this.countries, this.line);
  },
  methods: {
    renderRectan(data, line) {
      let width = 500;
      let height = 400;
      const margin = { top: 20, left: 200, bottom: 70, rigth: 20 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;

      const svg = d3
        .select(this.$refs.verticalbarspositive)
        .attr("width", width)
        .attr("height", height)
        .attr("viewBox", "0 0 500 400");

      /* LEYEND */
      let keys = data.map(d => d.name);
      let colorLabels = d3
        .scaleOrdinal()
        .domain(keys)
        .range(["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56"]);

      svg
        .selectAll("mydots")
        .data(keys)
        .enter()
        .append("circle")
        .attr("cx", 100)
        .attr("cy", (d, i) => 100 + i * 25)
        .attr("transform", `translate(${-90},${-70})`)
        .attr("r", 7)
        .style("fill", d => colorLabels(d));

      svg
        .selectAll("mylabels")
        .data(keys)
        .enter()
        .append("text")
        .attr("x", 120)
        .attr("y", (d, i) => 100 + i * 25)
        .style("fill", "rgb(53, 52, 52")
        .attr("transform", `translate(${-90},${-70})`)
        .text(d => d)
        .attr("text-anchor", "left")
        .style("alignment-baseline", "middle");
      /*  */

      const xValue = line => line.name;
      const yValue = line => line.total;

      //grupos que especifican q valores tomo
      const x = d3
        .scaleBand()
        .range([0, innerWidth])
        .padding(0.1);

      const y = d3.scaleLinear().range([innerHeight, 0]);
      //negativos
      //------------------
      const g = svg
        .append("g")
        .attr("transform", `translate(${margin.left},${margin.top})`);

      x.domain(data.map(d => d.name));
      y.domain([0, d3.max(data, d => d.total)]);

      let barras = g
        .selectAll(".bar")
        .data(data)
        .enter()
        .append("g");

      barras
        .append("rect")
        .attr("class", "bar")
        .attr("x", d => x(d.name))
        .attr("width", x.bandwidth)
        .style("fill", (d, i) => colorLabels(i))
        .attr("y", (d, i) => {
          return y(d.total);
        }) //menos de lo q es
        .attr("height", d => innerHeight - y(d.total));
      g.append("g")
        .attr("transform", `translate(0,${innerHeight})`)
        .call(d3.axisBottom(x));
      g.append("g").call(d3.axisLeft(y));

      let lineGenerator = d3
        .line()
        .x(d => x(xValue(d))+15)
        .y(d => y(yValue(d)));

      barras
        .append("path")
        .attr("class", "line-two")
        .attr("d", lineGenerator(line));

    

      barras
        .append("circle")
        .attr("class", "dot")
        .style('fill','red')
        .attr("cx", function (d, i) {
          console.log('linedentro',line[i])
          return x(xValue(line[i]))+15;
        })
        .attr("cy", function (d, i) {
          console.log('linedentro',line[i])
          return y(yValue(line[i]));
        })
        .attr("r", 5);
    }
  }
};
</script>

<style>
.tick text {
  fill: rgb(53, 52, 52);
}
.tick line {
  stroke: #c0c0bb;
}

.line-two {
  fill: none;
  stroke: red;
  stroke-linejoin: round;
}
</style>
