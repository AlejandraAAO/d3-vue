<template>
  <svg width="500" height="300" />
  
</template>

<script>
import * as d3 from "d3";
export default {
  name: "Linea",
  data() {
    return {
      temperature: [
          {
            timestamp: '2019-01-01T10:00:00.000',
            temperature: 28.3184653,
          },
          {
            timestamp: '2019-01-01T11:00:00.000',
            temperature: 36.3184664
          },
          {
            timestamp: '2019-01-01T12:00:00.000',
            temperature: 0
          },
          {
            timestamp: '2019-01-02T13:00:00.000',
            temperature: 27.3182553
          },
          {
            timestamp: '2019-01-02T14:00:00.000',
            temperature: 21.3184653
          },
          {
            timestamp: '2019-01-02T15:00:00.000',
            temperature: 26.2084653
          },
          {
            timestamp: '2019-01-03T16:00:00.000',
            temperature: 28.5184653
          },
          {
            timestamp: '2019-01-03T17:00:00.000',
            temperature: 25.1084653
          },
          {
            timestamp: '2019-01-03T18:00:00.000',
            temperature: 23.9084653
          },
          {
            timestamp: '2019-01-03T19:00:00.000',
            temperature: 24.7084653
          },
          {
            timestamp: '2019-01-03T20:00:00.000',
            temperature: 25.5084653
          },
          {
            timestamp: '2019-01-04T10:00:00.000',
            temperature: 28.3084653
          },
          {
            timestamp: '2019-01-04T11:00:00.000',
            temperature: 26.6084653
          },
          {
            timestamp: '2019-01-04T12:00:00.000',
            temperature: 27.2084653
          },
          {
            timestamp: '2019-01-05T13:00:00.000',
            temperature: 22.6084653
          },
          {
            timestamp: '2019-01-05T14:00:00.000',
            temperature: 21.4084653
          },
          {
            timestamp: '2019-01-06T15:00:00.000',
            temperature: 27.3084653
          },
          {
            timestamp: '2019-01-07T17:00:00.000',
            temperature: 21.4084653
          },
          {
            timestamp: '2019-01-07T18:00:00.000',
            temperature: 20.7084653
          },
          {
            timestamp: '2019-01-07T19:00:00.000',
            temperature: 21.5084653
          },
          {
            timestamp: '2019-01-08T20:00:00.000',
            temperature: 28.3084653
          }
      ]
    };
  },
    mounted(){
      this.renderCircle(this.temperature);
    },
    methods:{
      renderCircle(data){
        const svg = d3.select(this.$el);
        let width = +svg.attr('width');
        let height = +svg.attr('height'); 
        const margin = {top:20,left:40,bottom:20,rigth:20};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
        const circleRadius = 3
        //grupos que especifican q valores tomo
        const xValue = d => new Date(d.timestamp);
        console.log(xValue)
        const yValue = d => d.temperature;
//linear scale bueno para dos cantidades
        const xScale = d3.scaleTime()
        //mapea los valores del domain(min, max valores de la data) 
        //con sus correspondientes range(espacio en la pantalla)
          .domain(d3.extent(data, xValue)) 
          //min(data, xValue)-max(data, xValue)
          .range([0, innerWidth])
          .nice()
        
        const yScale = d3.scaleLinear()//nombre de los paises
        //mapea domain y range de los espacios de las barras incluyendo
          .domain(d3.extent(data, yValue)) 
          .range([innerHeight,0])
          .nice()

          //.fill(d => d3.interpolateCool(Math.random()))

        const yAxis = d3.axisLeft(yScale)
        //la data del eje y cree lineas q ocupen todo el grafico
          .tickSize(-innerWidth)
          .tickPadding(10);
        
       
        const xAxisTickFormat = number => d3.format('.1s')(number);
         //para formatear los valores del eje x
        const xAxis = d3.axisBottom(xScale)
          //.tickFormat(xAxisTickFormat)
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
        const lineGenerator = d3.line()
          .x(d => xScale(xValue(d)))
          .y(d=> yScale(yValue(d)))
         //.curve(d3.curveBasis)

        g.append('path')
          .attr('class','line-path')
          .attr('d',lineGenerator(data) )

        const rectangulos =  g
          .selectAll('circle').data(data)
            .enter().append('circle')
            //alinear los circulos a data del eje y
              .attr('cy', d=> yScale(yValue(d)))
              .attr('cx', d => xScale(xValue(d)))
              //diametro de cada circulo
              .attr('r',circleRadius)
              .attr('fill', 'orange')
      }
    }
};
</script>

<style scope>
  .line-path {
    fill:none;
    stroke: orange;
    stroke-linejoin: round

  }
</style>
