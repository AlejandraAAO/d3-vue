<template>
<div class="container-div-bar" ref="container_bar">
  <svg ref="verticalbars"/>
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
            name: 'LW',
            total: -0.005
          },
          {
            name:'L4W',
            total: -0.004
          },
          {
            name:'LM',
            total: -0.003
          },
          {
            name:'LY',
            total: 0
          },
          {
            name:'Perú',
            total: -0.001
          },
          {
            name:'Brasil',
            total: -0.0015
          },
          {
            name:'Japon',
            total:-0.001
          }
        ]
      }
      
    },
    mounted(){
      console.log(this.countries);
      this.renderRectan(this.countries);
    },
    methods:{
      renderRectan(data){
        let width = 280;
        let height = 300; 
        const margin = {top:20,left:40,bottom:70,rigth:20};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
        const colors = d3.scaleOrdinal(["#ffd740","rgb(255,100,100)",'#5c6bc0','#7e57c2']);
        let div = d3.select(this.$refs.container_bar)
          .append('div')
          .attr('class','tooltip-bar')
          .style('opacity',0)
          .style('pointer-events','none');

        const svg = d3.select(this.$refs.verticalbars)
          .attr('width', width)
          .attr('height',height)
          .attr('viewBox','0 0 280 300');
        //grupos que especifican q valores tomo
        const x = d3.scaleBand()
          .range([0,innerWidth])
          .padding(0.1);
        
        const y = d3.scaleLinear()
          .range([innerHeight,0]);
        //negativos
        const yScale = d3.scaleLinear()
          .domain([d3.min(data, d => d.total),d3.max(data, d => d.total)])
          .range([innerHeight,0]);
        
        const yAxisTickFormat = number => d3.format(".2%")(number);
        
        const yAxisScale = d3.scaleLinear()
          .domain([0,d3.min(data, d => d.total)])
          .range([0,innerHeight ])
        //------------------
        const g = svg.append('g')
            .attr('transform',`translate(${margin.left},${margin.top})`);
        
        x.domain(data.map(d => d.name));
        y.domain([0,d3.max(data, d => d.total)]);

        g.selectAll('.bar')
          .data(data)
          .enter().append('rect')
          .attr('class','bar')
          .attr('x', d => x(d.name))
          .attr('width', x.bandwidth)
          .attr('fill', (d,i) => colors(i))
          .attr('y',(d,i) => {
            console.log('yscale d y',yScale(i))/* innerHeight - Math.max(0, yScale(d.total)) */
            return 0
          })//menos de lo q es
          .attr('height',d => Math.abs(yScale(d.total)))
          .on('mouseover', d => {
            let coord = this.$refs.container_bar.getBoundingClientRect();
            let y = window.scrollY + coord.top;
            let x = window.scrollX + coord.left;

            div
              .transition()
              .duration(200)
              .style('opacity',0.9)
            /* `Antes : ${d.total*100}% <br/> Ahora : ${d.total+2*10}%` */
            div
              .html(`Nugget ${d.name}`)
              .style('color','rgb(53,52,52)')
              .style('left',`${d3.event.pageX - x}px`)
              .style('top',`${d3.event.pageY - y}px`)
              .style('background-color','white');
          })
          .on('mousemove',() => {
            let coord = this.$refs.container_bar.getBoundingClientRect();
            let y = window.scrollY + coord.top;
            let x = window.scrollX + coord.left;
            div
              .style('left', `${d3.event.pageX - x}px`)
              .style('top',`${d3.event.pageY - y}px`)
          })
          .on('mouseout',() => {
            div
              .transition()
              .duration(500)
              .style('opacity',0)
          })

        g.append('g')
          .attr('transform',`translate(0,${innerHeight})`)
          .call(d3.axisBottom(x));
        g.append('g')
          .call(d3.axisLeft(yAxisScale).tickFormat(yAxisTickFormat))
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
.container-div-bar {
  position: relative;
 } 
.container-div-bar >  .tooltip-bar {
   position: absolute !important;
  color: rgb(53, 52, 52);
  }

</style>
