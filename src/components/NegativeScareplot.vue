<template>
<div class="container-div-p" ref="container_p"></div>
  <!-- <svg width="500" height="300" /> -->
</template>

<script>
import * as d3 from "d3";
export default {
    name:'NegativeScareplot',
    data(){
      return {
        countries: [
          {
            name: 'LM',
            total: 5000,
            group: 'wantan'
          }, 
          {
            name: 'LM',
            total: 1900,
            group: 'nugget'
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
            name:'L4W',
            total: -1500,
            group: 'nugget'
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
    
      this.renderCircle(this.countries);
    },
    methods:{
      renderCircle(data){
        //const svg = d3.select(this.$el);
        let width = 500;
        let height = 300; 
        const margin = {top:20,left:40,bottom:40,rigth:20};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
      
        const svg = d3.select('.container-div-p')
          .append('svg')
          .attr('width', width)
          .attr('height',height)
        //grupos que especifican q valores tomo
        const xValue = d => d.total;
        const yValue = d => d.name;

        const colorBubles = d3.scaleOrdinal()
            .domain(['nugget','wantan'])
            .range(['blue', 'red']);

        const xScale = d3.scaleLinear()
        //mapea los valores del domain(min, max valores de la data) 
        //con sus correspondientes range(espacio en la pantalla)
          .domain([-d3.max(data, xValue), d3.max(data, xValue)])
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
        /* TOOLTIP-----------------------------------------------*/
        const div = d3.select('.container-div-p')
          .append('div')
          .attr('class','tooltip')
          .style('opacity',0)
          .style('pointer-events','none');

        /* ----------------------------------------------------- */
        //eje y
        g.append('g')
        //.attr('transform', `translate(-${innerHeight},${innerWidth/2})`)
          .call(yAxis)
          //selecciona la linea del eje y y la borra con el remove
          
          ;
        g.append('g').call(xAxis)
          .attr('transform', `translate(0,${innerHeight})`)
          

        const circulos =  g
          .selectAll('circle').data(data)
            .enter().append('g');
            
            circulos.append('circle')
            //alinear los circulos a data del eje y
              .attr('cy', d=> yScale(yValue(d)))
              .attr('cx',d => xScale(xValue(d)))
              //diametro de cada circulo
              .attr('r',20)
              .attr('fill',d => colorBubles(d.group))
              .style('opacity','0.6')
              .style('cursor','pointer')
    
              .on('mouseover', d => {
                let coord = this.$refs.container_p.getBoundingClientRect();
                let y = window.scrollY + coord.top;
                let x = window.scrollX + coord.left;

                div
                  .transition()
                  .duration(200)
                  .style('opacity',0.9)
                div
                  .html(`${d.total}`)
                  .style('color','rgb(53,52,52)')
                  .style('left',`${d3.event.pageX - x +12}px`)
                  .style('top',`${d3.event.pageY - y}px`)
                  .style('background-color','white');
              })
              .on('mousemove',() => {
                let coord = this.$refs.container_p.getBoundingClientRect();
                let y = window.scrollY + coord.top;
                let x = window.scrollX + coord.left;

                div
                  .style('left',`${d3.event.pageX - x+12}px`)
                  .style('top',`${d3.event.pageY - y}px`)
              })
              .on('mouseout',() => {
                div
                  .transition()
                  .duration(500)
                  .style('opacity',0)
              })
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

.container-div-p {
  position: relative;
}
.tooltip {
   position: absolute !important;
  color: rgb(53, 52, 52);
}
</style>
