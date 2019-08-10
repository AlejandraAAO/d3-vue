<template>
  <div ref="container" class="padrenugget">
    <svg class="nugget" width="500" height="300" />
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
    name:'Barras',
    data(){
      return {
        countries: [
          {
            name: 'China',
            total: 5000
          },
          {
            name:'EEUU',
            total: 4000
          },
          {
            name:'Mexico',
            total: 6500
          },
          {
            name:'Brasil',
            total: 1800
          },
          {
            name:'Perú',
            total: 3000
          },
        ]
      }
      
    },
    mounted(){
      console.log(this.countries);
      this.renderRectan(this.countries);
    },
    methods:{
      renderRectan(data){
        
        const svg = d3.select('.nugget');
        let width = +svg.attr('width');
        let height = +svg.attr('height'); 
        const margin = {top:20,left:40,bottom:20,rigth:35};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
        //grupos que especifican q valores tomo
        const xValue = d => d.total;
        const yValue = d => d.name;

        const xScale = d3.scaleLinear()
        //mapea los valores del domain(min, max valores de la data) 
        //con sus correspondientes range(espacio en la pantalla)
          .domain([0, d3.max(data, xValue)])
          .range([0, innerWidth]);

        const yScale = d3.scaleBand()//nombre de los paises
        //mapea domain y range de los espacios de las barras incluyendo
          .domain(data.map(yValue))
          .range([0, innerHeight])
          .padding(0.1)
          //.fill(d => d3.interpolateCool(Math.random()))

        const yAxis = d3.axisLeft(yScale);
        //crea un nuevo eje con orientacion a la izq 
       
        const xAxisTickFormat = number => d3.format('.2s')(number);
         //para formatear los valores del eje x
        const xAxis = d3.axisBottom(xScale)
          .tickFormat(xAxisTickFormat)
          .tickSize(-innerHeight)
          ;
        
        const div = d3.select('.padrenugget')
          .append('div')
          .attr('class','tooltip')
          .style('opacity',0)
          .style('position','absolute')
          

        const g = svg.append('g')
          .attr('transform', `translate(${margin.left},${margin.top})`);
        
        //eje y
        g.append('g')
          .call(d3.axisLeft(yScale))
          //selecciona la linea del eje y y la borra con el remove
          .selectAll('.domain, .tick line')
            .remove()
          ;
        g.append('g').call(xAxis)
          .attr('transform', `translate(0,${innerHeight})`)
          .selectAll('.domain')
            .remove();

        const rectangulos =  g
          .selectAll('rect').data(data)
            .enter().append('g');
            
            rectangulos.append('rect')
              .attr('y', d=> yScale(yValue(d)))
              .attr('width',d => xScale(xValue(d)))
              //ancho de cada barra por separado
              .attr('height',yScale.bandwidth())
              .attr('fill', d => d3.interpolateViridis(Math.random()))
              .style('cursor','pointer')
              .on('mouseover', d => {
                let coord = this.$refs.container.getBoundingClientRect();
                console.log('coords',coord);
                let y = window.scrollY + coord.top;
                let x = window.scrollX + coord.left;

                div
                  .transition()
                  .duration(200)
                  .style('opacity',0.9);
                div
                  .html(d.total)
                  .style('left', `${d3.event.pageX -x}px`)
                  .style('top', `${d3.event.pageY- y}px`)
                  .style('background-color','white');
              })
              .on('mousemove',() => {
                let coord = this.$refs.container.getBoundingClientRect();
                let y = window.scrollY + coord.top;
                let x= window.scrollX + coord.left;

                div
                .style('left',`${d3.event.pageX-x}px`)
                .style('top',`${d3.event.pageY-y}px`)
              }
              )
              .on('mouseout', () => {
                div
                  .transition()
                  .duration(500)
                  .style('opacity',0)
              })
              

            rectangulos.append('text')
              .attr('class','label')
              .attr('y',d => yScale(d.name)+ 28)
              .attr('x',d => xScale(d.total))
              .text(d => d.total);

        
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
