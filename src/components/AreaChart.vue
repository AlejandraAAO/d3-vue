<template>
  <svg width="500" height="300" />
  
</template>

<script>
import * as d3 from "d3";
export default {
  name: "Area",
  data() {
    return {
      years: [
          {
            year: '1995',
            population: 445479
          },
          {
            year: '1996',
            population: 442862
          },
          {
            year: '1997',
            population: 453147
          },
          {
            year: '1998',
            population: 462741
          },
          {
            year: '1999',
            population: 473159
          },
          {
            year: '2000',
            population: 489123
          },
          {
            year: '2001',
            population: 490589
          },
          {
            year: '2002',
            population: 515632
          },
          {
            year: '2003',
            population: 542036
          },
          {
            year: '2004',
            population: 566350
          },
          {
            year: '2005',
            population: 591085
          },
          {
            year: '2006',
            population: 620320
          },
          {
            year: '2007',
            population: 691036
          },
          {
            year: '2008',
            population: 703058
          },
          {
            year: '2009',
            population:761943
          },
          {
            year: '2011',
            population: 852048
          },
          {
            year: '2012',
            population: 863020
          },
          {
            year: '2013',
            population: 890708
          },
          {
            year: '2014',
            population: 998510
          },
          {
            year: '2015',
            population: 986532
          },
          {
            year: '2016',
            population: 1306090
          }
      ]
    };
  },
    mounted(){
      this.renderCircle(this.years);
    },
    methods:{
      renderCircle(data){
        const svg = d3.select(this.$el);
        let width = +svg.attr('width');
        let height = +svg.attr('height'); 
        const margin = {top:20,left:40,bottom:20,rigth:20};
        const innerWidth = width - margin.left - margin.rigth;
        const innerHeight = height - margin.top - margin.bottom;
        const circleRadius = 3
        //grupos que especifican q valores tomo
        const xValue = d => new Date(d.year);
      
        const yValue = d => d.population * 10;
        //linear scale bueno para dos cantidades
        const xScale = d3.scaleTime()
        //mapea los valores del domain(min, max valores de la data) 
        //con sus correspondientes range(espacio en la pantalla)
          .domain(d3.extent(data, xValue)) 
          //min(data, xValue)-max(data, xValue)
          .range([0, innerWidth])
          
        
        const yScale = d3.scaleLinear()//nombre de los paises
        //mapea domain y range de los espacios de las barras incluyendo
          .domain([0, d3.max(data, yValue)]) 
          .range([innerHeight,0])
          .nice()
          //.fill(d => d3.interpolateCool(Math.random()))
        const yAxisTickFormat = number => d3.format('.1s')(number);

        const yAxis = d3.axisLeft(yScale)
        //la data del eje y cree lineas q ocupen todo el grafico
          .tickFormat(yAxisTickFormat)
          .tickSize(-innerWidth)
          .tickPadding(10);
        
       
        
         //para formatear los valores del eje x
        const xAxis = d3.axisBottom(xScale)
          //.tickFormat(xAxisTickFormat)
          .tickSize(-innerHeight)
          .tickPadding(10);

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
        const areaGenerator = d3.area()
          .x(d => xScale(xValue(d)))
          .y0(innerHeight)
          .y1(d=> yScale(yValue(d)))
         .curve(d3.curveBasis)

        g.append('path')
          .attr('class','line-path2')
          .attr('d',areaGenerator(data) )

    /*     const rectangulos =  g
          .selectAll('circle').data(data)
            .enter().append('circle')
            //alinear los circulos a data del eje y
              .attr('cy', d=> yScale(yValue(d)))
              .attr('cx', d => xScale(xValue(d)))
              //diametro de cada circulo
              .attr('r',circleRadius)
              .attr('fill', 'orange') */
      }
    }
};
</script>

<style scope>
  .line-path2 {
    fill:rgb(243, 183, 70);
    stroke: orange;
    stroke-linejoin: round

  }
</style>
