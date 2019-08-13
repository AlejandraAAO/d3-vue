<template>
  <svg width="500" height="300" />
</template>

<script>
import * as d3 from "d3";
export default {
    name:'Circulos',
    data(){
      return {
        countries: [
          {
            name: 'LM',
            total: 5000,
            group: 'wantan'
          },
          {
            name:'LW',
            total: 4000,
            group: 'nugget'
          },
          {
            name:'L4W',
            total: 6500,
            group: 'wantan'
          },
          {
            name:'MTD',
            total: 1800,
            group: 'nugget'
          }
        ]
      }
      
    },
    mounted(){
      console.log(this.countries);
      this.renderCircle(this.countries);
    },
    methods:{
      renderCircle(data){
        const svg = d3.select(this.$el);
        let width = +svg.attr('width');
        let height = +svg.attr('height'); 
        const margin = {top:20,left:40,bottom:20,rigth:20};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
        //grupos que especifican q valores tomo
        const xValue = d => d.total;
        const yValue = d => d.name;

        const colorBubles = d3.scaleOrdinal()
            .domain(['nugget','wantan'])
            .range(['blue', 'red']);

        const xScale = d3.scaleLinear()
        //mapea los valores del domain(min, max valores de la data) 
        //con sus correspondientes range(espacio en la pantalla)
          .domain([0, d3.max(data, xValue)])
          .range([0, innerWidth])
          .nice()
          //scalePoint para manejar los circulos y attr ordenados 
        const yScale = d3.scalePoint()//nombre de los paises
        //mapea domain y range de los espacios de las barras incluyendo
          .domain(data.map(yValue))
          .range([0, innerHeight])
          .padding(0.5)
          //.fill(d => d3.interpolateCool(Math.random()))

        const yAxis = d3.axisLeft(yScale)
        //la data del eje y cree lineas q ocupen todo el grafico
          .tickSize(-innerWidth);
        
       
        const xAxisTickFormat = number => d3.format('.1s')(number);
         //para formatear los valores del eje x
        const xAxis = d3.axisBottom(xScale)
          .tickFormat(xAxisTickFormat)
          .tickSize(-innerHeight);

        const g = svg.append('g')
          .attr('transform', `translate(${margin.left},${margin.top})`);
        
        //eje y
        g.append('g')
          .call(yAxis)
          //selecciona la linea del eje y y la borra con el remove
          .selectAll('.domain')
            .remove()
          ;
        g.append('g').call(xAxis)
          .attr('transform', `translate(0,${innerHeight})`)
          .selectAll('.domain')
            .remove();

        const circulos =  g
          .selectAll('circle').data(data)
            .enter().append('circle')
            //alinear los circulos a data del eje y
              .attr('cy', d=> yScale(yValue(d)))
              .attr('cx',d => xScale(xValue(d)))
              //diametro de cada circulo
              .attr('r',20)
              .attr('fill',d => colorBubles(d.group))
              .style('opacity','0.6')
      }
    }
}
</script>

<style>
.tick text {
  fill: rgb(53, 52, 52)
}
.tick line {
stroke: #C0C0BB
}
</style>
