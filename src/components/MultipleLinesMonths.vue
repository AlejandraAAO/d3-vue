<template>
  <svg width="500" height="340" />
</template>
<script>
import * as d3 from "d3";
export default {
    name:'MultipleLinesMonths',
    data(){
        return {
            dataVals: [
          {
            timestamp: 'Ene',
            temperature: 28.3184653,
            group:'nugget'
          },
          {
            timestamp: 'Feb',
            temperature: 36.3184664,
            group:'nugget'
          },
          {
            timestamp: 'Mar',
            temperature: 25.3139687,
            group:'nugget'
          },
          {
            timestamp: 'Abr',
            temperature: 27.3182553,
            group:'nugget'
          },
          {
            timestamp: 'May',
            temperature: 21.3184653,
            group:'nugget'
          },
          {
            timestamp: 'Jun',
            temperature: 26.2084653,
            group:'nugget'
          },
          {
            timestamp: 'Jul',
            temperature: 28.5184653,
            group:'nugget'
          },
          {
            timestamp: 'Ago',
            temperature: 25.1084653,
            group:'nugget'
          },
          {
            timestamp: 'Sep',
            temperature: 23.9084653,
            group:'nugget'
          },
          {
            timestamp: 'Oct',
            temperature: 24.7084653,
            group:'nugget'
          },
          {
            timestamp: 'Nov',
            temperature: 25.5084653,
            group:'nugget'
          },
          {
            timestamp: 'Dic',
            temperature: 28.3084653,
            group:'nugget'
          },

          {
            timestamp: 'Ene',
            temperature: 32.3184653,
            group:'wantan'
          },
          {
            timestamp: 'Feb',
            temperature: 40.3184664,
            group:'wantan'
          },
          {
            timestamp: 'Mar',
            temperature: 39.3139687,
            group:'wantan'
          },
          {
            timestamp: 'Abr',
            temperature: 25.3182553,
            group:'wantan'
          },
          {
            timestamp: 'May',
            temperature: 26.3184653,
            group:'wantan'
          },
          {
            timestamp: 'Jun',
            temperature: 29.2084653,
            group:'wantan'
          },
          {
            timestamp: 'Jul',
            temperature: 35.5184653,
            group:'wantan'
          },
          {
            timestamp: 'Ago',
            temperature: 30.1084653,
            group:'wantan'
          },
          {
            timestamp: 'Sep',
            temperature: 26.9084653,
            group:'wantan'
          },
          {
            timestamp: 'Oct',
            temperature: 35.7084653,
            group:'wantan'
          },
          {
            timestamp: 'Nov',
            temperature: 40.5084653,
            group:'wantan'
          },
          {
            timestamp: 'Dic',
            temperature: 45.3084653,
            group:'wantan'
          },
         
        ]
      }
    },
    mounted(){
        this.buildLines(this.dataVals)
    },
    methods:{
        buildLines(data){
const svg = d3.select(this.$el);
        let width = +svg.attr('width');
        let height = +svg.attr('height'); 
        const colorValue =  d => d.group;
        const margin = {top:20,left:40,bottom:35,rigth:20};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
        const circleRadius = 3.5;
        const months = ['Ene', 'Feb','Mar','Abr','May','Jun','Jul','Ago','Sep','Oct','Nov','Dic']
        //grupos que especifican q valores tomo
        const xValue = d => d.timestamp;
        console.log(xValue)
        const yValue = d => d.temperature;
//linear scale bueno para dos cantidades
        const xScale = d3.scaleBand()
          .domain(months) 
          .range([0, innerWidth]);
        
        const yScale = d3.scaleLinear()//nombre de los paises
        //mapea domain y range de los espacios de las barras incluyendo
          .domain(d3.extent(data, yValue)) 
          .range([innerHeight,0]);

        const colorScale = d3.scaleOrdinal()
            .range(['red','blue'])

        const yAxis = d3.axisLeft(yScale)
        //la data del eje y cree lineas q ocupen todo el grafico
          .tickSize(0)
          .tickPadding(10)
          ;
        
       
        const xAxisTickFormat = number => d3.format('.1s')(number);
         //para formatear los valores del eje x
        const xAxis = d3.axisBottom(xScale)
          //.tickFormat(xAxisTickFormat)
          .tickSize(0)
          .tickPadding(15);

        const g = svg.append('g')
          .attr('transform', `translate(${margin.left},${margin.top})`);
        
        //eje y
        g.append('g')
          .call(yAxis)
          //selecciona la linea del eje y y la borra con el remove
          .selectAll('.domain')
            .remove()
          ;
        g.append('g').call(xAxis)
          .attr('transform', `translate(0,${innerHeight})`)
          .selectAll('.domain')
            .remove();
             
        const lineGenerator = d3.line()
          .x(d => xScale(xValue(d))+18)
          .y(d=> yScale(yValue(d)))
          //.curve(d3.curveBasis)
       
        const nested = d3.nest()
          .key(colorValue)
          .entries(data);
        
        colorScale.domain(nested.map(d => d.key))

        g.selectAll('line-path').data(nested)
          .enter().append('path')
            .attr('class','line-path1')
            .attr('d',d => lineGenerator(d.values) )
            .attr('stroke', d => colorScale(d.key))
            .attr('stroke-width',2.5)
          
        const rectangulos =  g
          .selectAll('circle').data(data)
            .enter().append('circle')
            //alinear los circulos a data del eje y
              .attr('cy', d=> yScale(yValue(d)))
              .attr('cx', d => xScale(xValue(d))+18)
              //diametro de cada circulo
              .attr('r',circleRadius) 
              .attr("stroke", "white")
              .attr('stroke-width',1)
              .attr('fill', d => colorScale(d.group))
        }
    }
}
</script>
<style > 
.line-path1 {
    fill:none;
    stroke-linejoin: round

  }
</style>