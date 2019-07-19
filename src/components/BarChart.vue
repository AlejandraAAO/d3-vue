<template>
  <svg width="500" height="300" />
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
          total: 6485454
        },
        {
          name:'EEUU',
          total: 4545419
        },
        {
          name:'Mexico',
          total:1354587
        },
        {
          name:'Brasil',
          total: 1357817
        },
        {
          name:'Perú',
          total:9534891
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
        const svg = d3.select(this.$el);
        let width = +svg.attr('width');
        let height = +svg.attr('height'); 
        const margin = {top:20,left:40,bottom:20,rigth:20};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
        
        

        const xValue = d => d.total;
        const yValue = d => d.name;

        const xScale = d3.scaleLinear()
        //min y max valor del domain
          .domain([0, d3.max(data, xValue)])
          .range([0, width])

        const yScale = d3.scaleBand()
          .domain(data.map(yValue))
          .range([0, height])

        const g = svg.append('g')
          .attr('transform', `translate(${margin.left},${margin.top})`)

        //call para pasar algo dentro de una funcion
        g.append('g').call(d3.axisLeft(yScale))
        g.append('g').call(d3.axisBottom(xScale))
          .attr('transform',`translate(0,${innerHeight})`)

       // *  g.selectAll('rect').data(data)
          .enter().append('rect')
            .attr('y', d => yScale(yValue(d)))
            .attr('width', d => xScale(xValue(d)))
            .attr('height', yScale.bandwidth())
      }
    }
}
</script>

<style>
</style>
