<template>
    <svg width="500" height="300">
     
    </svg>
</template>

<script>
import * as d3 from "d3";
export default {
    name:'PieComplete',
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
      this.renderData2(this.countries)
    },
    methods:{
        renderData2(data){
          //conf inicial
          const svg = d3.select(this.$el);
          let width = +svg.attr('width');
          let height = +svg.attr('height'); 
          let radius = Math.min(width, height) / 2;
          /* Legend ---------------------------------------- */
         
          let keys = data.map(d => d.name);
          console.log('keys',keys);
          let colorLabels = d3.scaleOrdinal()
            .domain(keys)
            .range(["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56"])

          svg.selectAll('mydots')
            .data(keys)
            .enter()
            .append('circle')
              .attr('cx',100)
              .attr('cy',(d,i) => 100 + i*25)
              .attr('transform',`translate(${320},${0})`)
              .attr('r',7)
              .style('fill', d => colorLabels(d))
          
          svg.selectAll('mylabels')
            .data(keys)
            .enter()
            .append('text')
              .attr('x',120)
              .attr('y', (d,i) => 100 + i*25)
              .style('fill', d => colorLabels(d))
              .attr('transform',`translate(${320},${0})`)
              .text(d => d)
              .attr('text-anchor','left')
              .style('alignment-baseline','middle')


          //----------------------------------------
          const margin = {top:20,left:40,bottom:20,rigth:20};
          const innerWidth = width - margin.left - margin.rigth;
          const innerHeight = height - margin.top - margin.bottom

          const g = svg.append('g')
            .attr('transform',`translate(${width/2},${height/2})`);

          const color = d3.scaleOrdinal(["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56"]);
          
          const pie = d3.pie().value(d => d.total);

          const arc = d3.arc()
            .innerRadius(0)
            .outerRadius(radius - 10);
          
          const label = d3.arc()
            .innerRadius(radius - 80)
            .outerRadius(radius);

          const containner = svg.append('g')
         
          
          const arcs = g.selectAll('.arc')
            .data(pie(data))
            .enter()
            .append('g')
            .attr('class','arc')
          
          arcs.append('path')
            .attr('d',arc)
            .attr('fill', (d,i) => color(i));
          
          console.log(arcs);
        
          arcs.append('text')
            .attr('transform',d => `translate(${label.centroid(d)})`)
            .text((d) => {
              console.log('D del text: ', d)
              return d.data.name
            }); 
        }
    }
}
</script>

<style>

</style>
