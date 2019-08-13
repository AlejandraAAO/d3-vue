<template>
  <svg width="500" height="300" />  
</template>
<script>
import * as d3 from "d3";
export default {
    name:'ScatterPlotLine',
    data(){
        return {
            countries: [
          {
            name: 'LM',
            total: 5000,
            group: 'wantan'
          },
          {
            name:'LW',
            total: 4000,
            group: 'nugget'
          },
          {
            name:'L4W',
            total: 6500,
            group: 'wantan'
          },
          {
            name:'MTD',
            total: 1800,
            group: 'nugget'
          }
        ]
        }
    }
}
</script>
<style>
    
</style>