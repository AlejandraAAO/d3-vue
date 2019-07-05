<template>
    <svg width="500" height="300"></svg>
</template>

<script>
import * as d3 from "d3";
export default {
    name:'Face',

    mounted(){
        const svg = d3.select(this.$el);
  
        let width = +svg.attr('width');
        let height = +svg.attr('height');
        let margin = {top:30,left:0,bottom:30,rigth:0};

        let chartWidth = width -(margin.left + margin.rigth);
        let chartHeigth = height - (margin.top + margin.bottom);

        this.happyFace = svg
            .append('circle')
            .attr('r',`${chartHeigth/2}`)
            .attr('transform', `translate(${chartWidth/2}, ${chartHeigth/2})`)
            .attr('fill','yellow')
        
        this.leftEye = svg
            .append('circle')
            .attr('r',`${chartHeigth/12}`)
            .attr('transform',`translate(${chartWidth/2 - chartWidth/10}, ${chartHeigth /2 - chartWidth/10})`)
            
        this.rigthEye = svg
            .append('circle')
            .attr('r',`${chartHeigth/12}`)
            .attr('transform',`translate(${chartWidth/2 + chartWidth/10}, ${chartHeigth /2 - chartWidth/10})`)    
        
        this.arc = d3.arc()
            .innerRadius(80)
            .outerRadius(100)
            .startAngle(0)
            .endAngle(3.14)

        this.g = svg
            .append('g')
            .attr('transform',`translate(${chartWidth/12},${chartHeigth/12})`)

        this.mounthContainner = g
            .append('path')
            .attr('d', this.arc)
            
            
    }
}
</script>

<style>

</style>
