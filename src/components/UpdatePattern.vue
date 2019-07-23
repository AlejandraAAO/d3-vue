<template>
    <svg width="500" height="300"></svg>
</template>

<script>
import * as d3 from "d3";
import { setTimeout } from 'timers';
export default {
    name:'UpdatePattern',
    data(){
      return {
        width:0,
        height:0
      }
    },
    mounted(){
        const svg = d3.select(this.$el);
        let width = +svg.attr('width');
        this.width = width
        let height = +svg.attr('height');
        this.height = height;
 
        /*    const render = (selection, {fruits}) => {
          selection.selectAll('circle').data(fruits)
          //enter es solo data
            .enter().append('circle')
                .attr('cx',(d,i) => i *100 + 60)
                .attr('cy', height/2)
                .attr('fill','#700f0f')
                .attr('r',30);
                
          selection.selectAll('circle').data(fruits)
           .exit().remove();
        } */
        const makeFruit = type => ({
          type,
          id: Math.random()
        });

        let fruits = d3.range(5)
            .map(() => makeFruit('apple'));

        this.renderFruits(svg, {fruits});
        
        setTimeout(() => {
          //eat an apple
          fruits.pop();
          this.renderFruits(svg, {fruits});
        }, 1000);

        //reemplazar con un limon
        setTimeout(() => {
          fruits[2].type = 'lemon'
          this.renderFruits(svg, {fruits});
        }, 2000)
         
        //reemplazar con un limon
        setTimeout(() => {
           fruits = fruits.filter((d,i) => i !==1 );
          this.renderFruits(svg, {fruits});
        }, 3000)
        
    },
    methods:{
      renderFruits(selection, {fruits}){
        
        const xPosition = (d,i) => i *100 + 60;

        const colorScale = d3.scaleOrdinal()
          .domain(['apple','lemon'])
          .range(['#c11d1d','yellow']);
          
        const radiusScale = d3.scaleOrdinal()
          .domain(['apple','lemon'])
          .range([30,15])
        //circles es data join
        const circles = selection.selectAll('circle')
          .data(fruits, d => d.id)
          //enter es solo data
            circles
              .enter().append('circle')
                .attr('cx',xPosition)
                .attr('cy', this.height/2)
                .attr('r', 0)
              .merge(circles)
                .attr('fill',d => colorScale(d.type))
              .transition().duration(1000)
              .attr('cx',xPosition)
                .attr('r',d => radiusScale(d.type));
            
           /*  //el update es el mismo elemento pero es reeeplazado por el merge
            circles
              .attr('fill',d => colorScale(d.type))
              .attr('r',d => radiusScale(d.type)); */    
            circles.exit()
              .transition().duration(1000)
                .attr('r',0)
              .remove();

        const text = selection.selectAll('text')
          .data(fruits)
          //enter es solo data
            text
              .enter().append('text')
                .attr('x',xPosition)
                .attr('y', this.height/2)
            
              .merge(text)
                .text(d => d.type);
            text.exit()
              .remove();
      }
    }
}
</script>

<style scoped>
  text {
    font-size: 1rem;
    text-anchor: middle
  }
</style>

