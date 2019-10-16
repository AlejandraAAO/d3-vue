<template>
  <div class="container_barhorizontalline" ref="container_barhorizontalline">
    <svg ref="barhorizontalline" class="class_barhorizontalline" />
  </div>
</template>
<script>
import * as d3 from "d3";
export default {
  name: "PaintByValue",
  props: [
    "socio",
    "bars",
    "line",
    "nameroute",
    "type",
    "sup",
    "labelKey",
    "valueKey",
    "hp",
    "value"
  ],
  data() {
    return {};
  },
  methods: {
    renderRectanStandarBIG(data, line, ruta) {
      console.log("data de la funcion de las barras", data);
      let width = 390;
      let height = 390;
      const margin = { top: 15, left: 150, bottom: 40, rigth: 10 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;
      //grupos que especifican q valores tomo
      const svg = d3
        .select(this.$refs.barhorizontalline)
        .attr("width", width)
        .attr("height", height)
        .attr("viewBox", "0 0 390 390");

      const xValue = d => d[this.valueKey];
      const yValue = d => d[this.labelKey];

      const xValueLine = line => line[this.valueKey];
      const yValueLine = line => line[this.labelKey];

      console.log("valores de line x", xValueLine);
      console.log("valores de linea y", yValueLine);

      const xScale = d3
        .scaleLinear()
        //mapea los valores del domain(min, max valores de la data)
        //con sus correspondientes range(espacio en la pantalla)
        .domain([0, d3.max(data, xValue)])
        .range([0, innerWidth]);

      const yScale = d3
        .scaleBand() //nombre de los paises
        //mapea domain y range de los espacios de las barras incluyendo
        .domain(data.map(yValue))
        .range([0, innerHeight])
        .padding(0.1);
      //.fill(d => d3.interpolateCool(Math.random()))

      /* escala de colores */
      const interpolateColors = d3.interpolate("#8FC0A9", "#9FC5A0");

      let arrColorsSupervisor = [
        "#215891",
        "#2664a6",
        "#3a75b5",
        "#4f8ac9",
        "#699ed6",
        "#86b3e3",
        "#9bc0e8",
        "#b8d5f5",
        "#cbdff5",
        "#e1edfa"
      ];
      let colorLabels = d3
        .scaleOrdinal()
        .domain(xValue)
        .range(arrColorsSupervisor);
      /* TOOLTIP */
      const div = d3
        .select(this.$refs.container_barhorizontalline)
        .append("div")
        .attr("class", "tooltiphorizontal")
        .attr("opacity", 0)
        .style("pointer-events", "none");
      const divCircle = d3
        .select(this.$refs.container_barhorizontalline)
        .append("div")
        .attr("class", "tooltipcircle")
        .style("opacity", 0)
        .style("pointer-events", "none");
      /*---------------------------------*/
      const yAxis = d3.axisLeft(yScale);
      //crea un nuevo eje con orientacion a la izq

      const xAxisTickFormat = number => d3.format("~s")(number);
      //para formatear los valores del eje x
      const xAxis = d3
        .axisBottom(xScale)
        .tickFormat(xAxisTickFormat)
        .tickSize(-innerHeight);

      const g = svg
        .append("g")
        .attr("transform", `translate(${margin.left},${margin.top})`);

      //eje y
      g.append("g")
        .call(d3.axisLeft(yScale).tickPadding(4))
        //selecciona la linea del eje y y la borra con el remove
        .selectAll(".domain, .tick line")
        .remove();
      g.append("g")
        .call(xAxis)
        .attr("transform", `translate(0,${innerHeight})`)
        .selectAll(".domain")
        .remove();

      const rectangulos = g
        .selectAll("rect")
        .data(data)
        .enter()
        .append("g");
      let _self = this;

      /*  colorLabels(i)*/
      rectangulos
        .append("rect")
        .attr("y", d => yScale(yValue(d)))
        .attr("width", d => xScale(xValue(d)))
        //ancho de cada barra por separado
        .attr("height", yScale.bandwidth())
        .attr("fill", function(d){
            if(d.value > 1990){
                return 'purple'
            } else if( d.value <= 1990){
                return 'green'
            }
        })
        .on("mouseover", function(d) {
          let coord = _self.$refs.container_barhorizontalline.getBoundingClientRect();
          let y = window.scrollY + coord.top;
          let x = window.scrollX + coord.left;

          d3.select(this).attr("stroke", "red");
          div
            .transition()
            .duration(200)
            .style("opacity", 0.9);
          div
            .html(`Total : ${d[_self.valueKey]}`)
            .style("color", "rgb(53, 52, 52)")
            .style("left", `${d3.event.pageX - x + 10}px`)
            .style("top", `${d3.event.pageY - y}px`)
            .style("background-color", "white");
        })
        /* .on('click', function(d,i){
          _self.$router.push({
            name: ruta,
            params: {
              territorio: `${d.name}`
            }
          })
        }) */
        .on("mousemove", function(d, i) {
          let coord = _self.$refs.container_barhorizontalline.getBoundingClientRect();
          let y = window.scrollY + coord.top;
          let x = window.scrollX + coord.left;

          div
            .style("left", `${d3.event.pageX - x + 10}px`)
            .style("top", `${d3.event.pageY - y}px`);
        })
        .on("mouseout", function(d, i) {
          div
            .transition()
            .duration(500)
            .style("opacity", 0);

          d3.select(this).attr("stroke", colorLabels(i));
        });

      const lineGenerator = d3
        .line()
        .x(d => xScale(xValueLine(d)))
        .y(d => yScale(yValueLine(d)) + 15);

      rectangulos
        .append("path")
        .attr("class", "line-two")
        .attr("d", lineGenerator(line));

      rectangulos
        .append("circle")
        .attr("class", "dot")
        .style("fill", "black")
        .attr("cx", function(d, i) {
          return xScale(xValueLine(line[i]));
        })
        .attr("cy", function(d, i) {
          return yScale(yValueLine(line[i])) + 15;
        })
        .attr("r", 5)
        .on("mouseover", function(d, i) {
          let coord = _self.$refs.container_barhorizontalline.getBoundingClientRect();
          let y = window.scrollY + coord.top;
          let x = window.scrollX + coord.left;

          divCircle
            .transition()
            .duration(200)
            .style("opacity", 0.9);

          divCircle
            .html(`Meta de ${d.name}: ${line[i].total}`)
            .style("color", "rgb(53,52,52)")
            .style("left", `${d3.event.pageX - x + 20}px`)
            .style("top", `${d3.event.pageY - y}px`)
            .style("background-color", "white");
        })
        .on("mousemove", function(d, i) {
          let coord = _self.$refs.container_barhorizontalline.getBoundingClientRect();
          let y = window.scrollY + coord.top;
          let x = window.scrollX + coord.left;

          divCircle
            .style("left", `${d3.event.pageX - x + 20}px`)
            .style("top", `${d3.event.pageY - y}px`);
        })
        .on("mouseout", function(d, i) {
          divCircle
            .transition()
            .duration(500)
            .style("opacity", 0);
        });
    }
  },
  mounted() {
    this.renderRectanStandarBIG(this.bars, this.line, this.nameroute);
  }
};
</script>
<style>
svg {
  width: 100%;
  height: auto;
}
.container_barhorizontalline {
  position: relative;
  }
 .container_barhorizontalline .tooltiphorizontal {
    position: absolute !important;
    color: rgb(53, 52, 52);
  }
 .container_barhorizontalline .tooltipcircle {
    position: absolute !important;
    color: rgb(53, 52, 52);
  }

</style>