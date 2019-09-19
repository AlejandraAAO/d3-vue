<template>
  <div ref="container_barhorizontalline">
    <svg ref="barhorizontalline" />
  </div>
</template>

<script>
import * as d3 from "d3";
export default {
  name: "Barras",
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
    const orden = this.countries.sort((a, b) => b.total - a.total);
    this.renderRectan(orden);
  },
  methods: {
    renderRectan(data) {
      let width = 500;
      let height = 300;
      const margin = { top: 20, left: 40, bottom: 20, rigth: 20 };
      const innerWidth = width - margin.left - margin.rigth;
      const innerHeight = height - margin.top - margin.bottom;
      //grupos que especifican q valores tomo
      const svg = d3
        .select(this.$refs.barhorizontalline)
        .attr("width", width)
        .attr("height", height)
        .attr("viewBox", "0 0 500 300");

      const xValue = d => d.total;
      const yValue = d => d.name;

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

      const yAxis = d3.axisLeft(yScale);
      //crea un nuevo eje con orientacion a la izq

      const xAxisTickFormat = number => d3.format(".1s")(number);
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
        .call(d3.axisLeft(yScale))
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

      rectangulos
        .append("rect")
        .attr("y", d => yScale(yValue(d)))
        .attr("width", d => xScale(xValue(d)))
        //ancho de cada barra por separado
        .attr("height", yScale.bandwidth())
        .attr("fill", d => d3.interpolateViridis(Math.random()));
      
      const lineGenerator = d3.line()
        .x(d => xScale(xValue(d)))
        .y(d =>  yScale(yValue(d))+20);
        
      rectangulos
        .append('path')
        .attr('class','line-two')
        .attr('d', lineGenerator(data))
      rectangulos
      .append("circle")
        .attr("class", "dot")
        .style('fill','red')
        .attr('cx', function(d,i){
            return xScale(xValue(d))
        })
        .attr('cy', function(d,i){
            return yScale(yValue(d))+20
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
</style>
