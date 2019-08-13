<template>
  <svg width="500" height="400" />
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
            total: 5000
          },
          {
            name:'L4W',
            total: 4000
          },
          {
            name:'LM',
            total: 6500
          },
          {
            name:'LY',
            total: -1800
          },
          {
            name:'Perú',
            total: 3000
          },
          {
            name:'Brasil',
            total: 1000
          },
          {
            name:'Japon',
            total:-1000
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
        const svg = d3.select(this.$el);
        let width = +svg.attr('width');
        let height = +svg.attr('height'); 
        const margin = {top:20,left:40,bottom:70,rigth:20};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
        //grupos que especifican q valores tomo
        const x = d3.scaleBand()
          .range([0,innerWidth])
          .padding(0.1);
        
        const y = d3.scaleLinear()
          .range([innerHeight,0]);
        //negativos
        const yScale = d3.scaleLinear()
          .domain([0,d3.max(data, d => d.total)])
          .range([0,innerHeight]);

        const yAxisScale = d3.scaleLinear()
          .domain([d3.min(data, d => d.total),d3.max(data, d => d.total)])
          .range([innerHeight - yScale(d3.min(data, d => d.total)),0])

        
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
          .attr('y',(d,i) => {
            console.log('yscale d y',yScale(i))
            return innerHeight - Math.max(0, yScale(d.total))
          })//menos de lo q es
          .attr('height',d => Math.abs(yScale(d.total)))

        g.append('g')
          .attr('transform',`translate(0,${innerHeight})`)
          .call(d3.axisBottom(x));
        g.append('g')
          .call(d3.axisLeft(yAxisScale))
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
'x