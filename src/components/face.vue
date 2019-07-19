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
            .attr('transform',`translate(${chartWidth/2 - chartWidth/10}, ${chartHeigth /2 - chartWidth/16})`)
            
        this.rigthEye = svg
            .append('circle')
            .attr('r',`${chartHeigth/12}`)
            .attr('transform',`translate(${chartWidth/2 + chartWidth/10}, ${chartHeigth /2 - chartWidth/16})`)    
        
        this.arc = d3.arc()
            .innerRadius(80)
            .outerRadius(90)
            .startAngle(Math.PI /1.5)
            .endAngle(Math.PI * 4 /3)

        this.g = svg
            .append('g')
            .attr('transform',`translate(${chartWidth*0.5},${chartHeigth/2})`)

        this.mounthContainner = this.g
            .append('path')
            .attr('d', this.arc)

        this.eyebrownLeft = this.g
            .append('rect')
            .attr('width',50)
            .attr('height',15)
            .attr('x',-70)
            .attr('y',-80)

        this.eyebrownRigth = this.g
            .append('rect')
            .attr('width',50)
            .attr('height',15)
            .attr('y',-80)
            .attr('x',20)
            .transition().duration(2000)
                .attr('y', -100)
            .transition().duration(2000)
                .attr('y', -80)
    }
}
</script>

<style>

</style>
