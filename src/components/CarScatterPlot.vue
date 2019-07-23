<template>
  <svg width="500" height="300" />
</template>

<script>
import * as d3 from "d3";
export default {
  name: "Cars",
  data() {
    return {
      cars: [
        {
          name: "Chevrolet",
          mpg: 5000,
          cylinders: 10,
          year: 2010
        },
        {
          name: "Ferrari",
          mpg: 4000,
          cylinders: 1,
          year: 2000
        },
        {
          name: "Toyota",
          mpg: 6500,
          cylinders: 5,
          year: 2005
        },
        {
          name: "Range Rover",
          mpg: 1800,
          cylinders: 8,
          year: 1995
        },
        {
          name: "Lamborghini",
          mpg: 3000,
          cylinders: 12,
          year: 2001
        },
        {
          name: "Chevrolet1",
          mpg: 5050,
          cylinders: 20,
          year: 2010
        },
        {
          name: "Ferrari1",
          mpg: 4050,
          cylinders: 21,
          year: 2000
        },
        {
          name: "Toyota1",
          mpg: 6550,
          cylinders: 25,
          year: 2005
        },
        {
          name: "Range Rover1",
          mpg: 1850,
          cylinders: 17,
          year: 1995
        },
        {
          name: "Lamborghini1",
          mpg: 3050,
          cylinders: 28,
          year: 2001
        }
      ]
    };
  },
    mounted(){
     
      this.renderCircle(this.cars);
    },
    methods:{
      renderCircle(data){
        const svg = d3.select(this.$el);
        let width = +svg.attr('width');
        let height = +svg.attr('height'); 
        const margin = {top:20,left:40,bottom:20,rigth:20};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
        const circleRadius = 10
        //grupos que especifican q valores tomo
        const xValue = d => d.mpg;
        const yValue = d => d.cylinders;
//linear scale bueno para dos cantidades
        const xScale = d3.scaleLinear()
        //mapea los valores del domain(min, max valores de la data) 
        //con sus correspondientes range(espacio en la pantalla)
          .domain(d3.extent(data, xValue)) 
          //min(data, xValue)-max(data, xValue)
          .range([0, innerWidth])
          .nice()
        
        const yScale = d3.scaleLinear()//nombre de los paises
        //mapea domain y range de los espacios de las barras incluyendo
          .domain(d3.extent(data, yValue)) 
          .range([0, innerHeight])
          .nice()

          //.fill(d => d3.interpolateCool(Math.random()))

        const yAxis = d3.axisLeft(yScale)
        //la data del eje y cree lineas q ocupen todo el grafico
          .tickSize(-innerWidth)
          .tickPadding(10);
        
       
        const xAxisTickFormat = number => d3.format('.1s')(number);
         //para formatear los valores del eje x
        const xAxis = d3.axisBottom(xScale)
          .tickFormat(xAxisTickFormat)
          .tickSize(-innerHeight)
          .tickPadding(10);

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

        const rectangulos =  g
          .selectAll('circle').data(data)
            .enter().append('circle')
            //alinear los circulos a data del eje y
              .attr('cy', d=> yScale(yValue(d)))
              .attr('cx',d => xScale(xValue(d)))
              //diametro de cada circulo
              .attr('r',circleRadius)
              .attr('fill', d => d3.interpolateViridis(Math.random()))
      }
    }
};
</script>

<style>
</style>
