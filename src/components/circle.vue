<template>
    <svg width="500" height="300"></svg>  
</template>

<script>
import * as d3 from "d3";
export default {
    name: 'Pie',
    props:['data'],
    watch:{
        data(newData){
            this.drawChart(newData)
        }
    },
    mounted(){
        let svg = d3.select(this.$el);
        let width = +svg.attr('width');
        let height = +svg.attr('height');
        let margin = {top:20,left:0,bottom:30,right:0};

        let chartWidth = width - (margin.left +margin.right);
        let chartHeigth = height -(margin.top + margin.bottom);

        this.chartLayer = svg
            .append('g')
            .attr('transform', `translate(${margin.left}, ${margin.top})`);
        
        this.arc = d3.arc()
            .outerRadius(chartHeigth / 2)
            .innerRadius(chartHeigth / 4)
            .padAngle(0.03)
            .cornerRadius(8)
        
        this.pieG = this.chartLayer
            .append('g')
            .attr('transform', `translate(${chartWidth / 2}, ${chartHeigth / 2})`)

        this.drawChart(this.data);
    },
    methods:{
        drawChart(data){
            let arcs = d3.pie()
                .sort(null)
                .value(d => d.value)
                (data)
            
            let block = this.pieG.selectAll('.arc')
                .data(arcs)
                block.select('path').attr('d',this.arc)
            
            let newBlock = block
                .enter()
                .append('g')
                .classed('arc',true)

                newBlock.append('path')
                    .attr('d',this.arc)
                    .attr('id', (d,i) => 'arc-'+ i)
                    .attr('stroke','gray')
                    .attr('fill', d => d3.interpolateCool(Math.random())) 
                    
                 newBlock.append('text')
                    .attr('dx', 10)
                    .attr('dy', -5)
                    .append('textPath')
                    .attr('xlink:href', (d,i) => '#arc-' + i)
                    .text(d => d.data.name) 

        }
    }

}
</script>

<style>

</style>
