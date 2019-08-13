<template>
  <div class="grid">
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Donunt Chart</h2>

        <div class="chart-container">
          <d3-donut :height="960" :thickness="90" keyColor="color" :value="parserData"/>
        </div>
        <div class="controls">
          <div v-for="item,key in paises" :key="key">
            <label for>{{item.name}} {{item.value}}</label>
            <input type="checkbox" v-model="item.active">
            <input type="range" min="0" max="100" step="1" v-model="paises[key].value">
          </div>

          <hr>

          <button @click="addPaises">Añadir</button>
        </div>
      </div>
    </div>

    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Line Chart</h2>

        <div class="chart-container">
          <d3-line :dots="true" :text="true" :value="parserData" keyLabel="name" :rotateAxisX="20"></d3-line>
        </div>
      </div>
    </div>

    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Bar Horizontal Chart</h2>

        <div class="chart-container">
          <d3-bar-vertical :value="parserData" keyLabel="name" @select="select">
            <div slot="tooltip" slot-scope="tooltip">{{tooltip.data}}</div>
          </d3-bar-vertical>
        </div>
      </div>
    </div>

    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Bar Vertical Chart</h2>

        <div class="chart-container">
          <d3-bar-horizontal :value="parserData" keyLabel="name"></d3-bar-horizontal>
        </div>
      </div>
    </div>

    <div class="grid-item">
      <div class="card">
        <div class="card-title">Bar Vertical Chart Stack</div>
        <div class="chart-container">
          <d3-bar-horizontal-stack
            :value="parserData"
            keyLabel="name"
            :keysValue="['value','value2','value3']"
          ></d3-bar-horizontal-stack>
        </div>
      </div>
    </div>

    <div class="grid-item">
      <div class="card">
        <div class="card-container">
          <div class="controls">
            <div v-for="item,key in paises" :key="key">
              <label for>{{item.name}} {{item.value2}}</label>
              <input type="checkbox" v-model="item.active">
              <input type="range" min="0" max="100" step="1" v-model="paises[key].value2">
            </div>
          </div>
          <hr>
          <div class="controls">
            <div v-for="item,key in paises" :key="key">
              <label for>{{item.name}} {{item.value3}}</label>
              <input type="checkbox" v-model="item.active">
              <input type="range" min="0" max="100" step="1" v-model="paises[key].value3">
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="grid-item">
      <div class="card">
        <div class="card-container">
          <d3-bar-vertical-stack
            keyLabel="name"
            :keysValue="['value','value2','value3']"
            :value="parserData"
            :colors="['#3d5a80','#98c1d9','#e0fbfc']"
          ></d3-bar-vertical-stack>
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title"> Circle</h2>
        <div class="chart-container">
          <svg ref="circle1" :width="width" :height="height"></svg>

        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">ControlSize</h2>
        <div class="chart-container">
          <svg ref="circle2" :width="width" :height="height"></svg>
          <br>
          <input type="range"
          v-model="circleSize"
          min="1"
          max="100"
          step="1">
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Build Barras</h2>
        <div class="chart-container">
          <svg ref="figure1" :width="width" :height="height"></svg>
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Popular names</h2>
        <div class="chart-container">
          <Pie :data="names"/>
          <br>
          <button v-if="canAddName" @click="addName">Añadir nombre</button>
        </div>
      </div>

    </div>
    <!--   <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Happy Face</h2>
        <div class="chart-container">
          <Face />
        </div>
      </div>
    </div> -->
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Barras</h2>
        <div class="chart-container">
          <Barras />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Barras con Labels</h2>
        <div class="chart-container">
          <BarrasLabel />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Circulos</h2>
        <div class="chart-container">
          <Circulos />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Cars</h2>
        <div class="chart-container">
          <Cars />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Lineas</h2>
        <div class="chart-container">
          <Linea />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Area</h2>
        <div class="chart-container">
          <Area />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Update Pattern</h2>
        <div class="chart-container">
          <UpdatePattern />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Loli</h2>
        <div class="chart-container">
          <Loli/>
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Pie</h2>
        <div class="chart-container">
          <PieComplete/>
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">
          Top
        </h2>
        <div class="chart-container">
          <Top />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">Barras agrupadas</h2>
        <div class="chart-container">
          <GroupBars />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">BubleDiferent</h2>
        <div class="chart-container">
          <BubleDiferent/>
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">MUltipleLine</h2>
        <div class="chart-container">
          <MultipleLine/>
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">ScatterPlotLine</h2>
        <div class="chart-container">
          <ScatterPlotLine />
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">NegativeY</h2>
        <div class="chart-container">
          <NegativeX/>
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">BarChartVertical</h2>
        <div class="chart-container">
          <BarChartVertical/>
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">VerticalBarsNegative</h2>
        <div class="chart-container">
          <VerticalBarsNegative/>
        </div>
      </div>
    </div>
    <div class="grid-item">
      <div class="card">
        <h2 class="card-title">VerticalGroupBars</h2>
        <div class="chart-container">
          <VerticalGroupBars/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";
import D3Donut from "./D3Donut.vue";
import D3Line from "./D3Line.vue";
import D3BarVertical from "./D3BarVertical.vue";
import D3BarHorizontal from "./D3BarHorizontal.vue";
import D3BarHorizontalStack from "./D3BarHorizontalStack.vue";
import D3BarVerticalStack from "./D3BarVerticalStack.vue";
import Pie from "./circle.vue"
import Face from "./face.vue"
import Barras from "./BarChart.vue"
import Circulos from "./ScatterPlot.vue"
import Cars from "./CarScatterPlot.vue"
import Linea from "./Line.vue"
import Area from "./AreaChart"
import UpdatePattern from "./UpdatePattern.vue"
import BarrasLabel from "./BarrasLabel"
import Loli from "./DonaTooltip"
import PieComplete from "./PieChart"
import Top from "./Top"
import GroupBars from "./BarsGroup"
import BubleDiferent from './BubleDiferent'
import MultipleLine from './MultipleLine'
import ScatterPlotLine from './ScatterPlotLine'
import NegativeX from './NegativeX'
import BarChartVertical from './BarChartVertical'
import VerticalBarsNegative from './VerticalBarsNegative'
import VerticalGroupBars from './VerticalGroupBars'


const NAMES = [
  { name: 'Sarah', value: 2502 },
  { name: 'Emma', value: 2005 },
  { name: 'Laura', value: 1968 },
  { name: 'Chloé', value: 1863 },
  { name: 'Marie', value: 1810 },
  { name: 'Emily', value: 1637 },
  { name: 'Léa', value: 1592 },
  { name: 'Camille', value: 1572 },
  { name: 'Anna', value: 1433 },
  { name: 'Manon', value: 1403 }
]
export default {
  name: "HelloWorld",
  components: {
    D3Donut,
    D3Line,
    D3BarVertical,
    D3BarHorizontal,
    D3BarHorizontalStack,
    D3BarVerticalStack,
    Pie,
    Face,
    Barras,
    Circulos,
    Cars,
    Linea,
    Area,
    UpdatePattern,
    BarrasLabel,
    Loli,
    PieComplete,
    Top,
    GroupBars,
    BubleDiferent,
    MultipleLine,
    ScatterPlotLine,
    NegativeX,
    BarChartVertical,
    VerticalBarsNegative,
    VerticalGroupBars
  },
  props: {
    msg: String
  },
  data() {
    return {
      paises: [
        {
          name: "USA",
          value: 95,
          value2: 120,
          value3: 14,
          color: "#3d5a80",
          active: true
        },
        {
          name: "UK",
          value: 20,
          value2: 450,
          value3: 52,
          color: "#98c1d9",
          active: true
        },
        {
          name: "Canada",
          value: 30,
          value2: 142,
          value3: 68,
          color: "#e0fbfc",
          active: true
        },
        {
          name: "Mexico",
          value: 10,
          value2: 240,
          value3: 100,
          color: "#ee6c4d",
          active: true
        }
      ],
      width: '500',
      height: '300',
      circleSize:50,
      data:[
        {name: 'one', val: 100},
        {name: 'two', val: 150},
        {name: 'three', val: 200}
      ],
      names: NAMES.slice(0,3)
    };
  },
  computed: {
    parserData() {
      let d = this.paises.filter(i => {
        return i.active;
      });

      return d.map(i => {
        i.value = parseInt(i.value);
        i.value2 = parseInt(i.value2);
        i.value3 = parseInt(i.value3);
        return i;
      });
    },
    canAddName(){
      return this.names.length  < NAMES.length
    }
  },
  methods: {
    addPaises() {
      let arrayPaises = ["Croacia", "Dinamarca", "Italia", "España", "Francia"];

      let val = Math.floor(Math.random() * (100 - 1)) + 1;
      let val2 = Math.floor(Math.random() * (120 - 1)) + 1;
      let val3 = Math.floor(Math.random() * (90 - 1)) + 1;
      let indexPaises = Math.floor(Math.random() * (5 - 0)) + 0;

      this.paises.push({
        name: arrayPaises[indexPaises],
        value: val,
        value2: val2,
        value3: val3,
        active: true
      });
    },
    select(d, i) {
     // console.log("d: ", d, " : ", i);
    },
    drawCircle(){
      d3.select(this.$refs.circle1)
      .append('circle')
      .attr('cx','250')
      .attr('cy','150')
      .attr('r','100')
    },
    controllingSize(){
      let svg = d3.select(this.$refs.circle2);
      this.circle = svg
        .append('circle')
        .attr('cx','250')
        .attr('cy','150')
        .attr('r', this.circleSize)
    },
    buildBarras(){
      let svg = d3.select(this.$refs.figure1);
      let width = +svg.attr('width');
      let height = +svg.attr('height'); 
      
      //console.log(height);

       let data = [
      {name: 'one', val: 100},
      {name: 'two', val: 150},
      {name: 'three', val: 200}
      ];
      let x = d3.scaleBand()
                .rangeRound([0, width]).padding(0.1)
                .domain(data.map(d => d.name));
      let y = d3.scaleLinear()
                .rangeRound([height * 0.3 - 20, 0])
                .domain([0,d3.max(data, d => d.val)])
            
           
      function addRectsWithName(elem, name){
        elem
          .append('text')
          .text(name)
          .attr('x', width/2)
          .attr('y', 5)
          .attr('text-anchor','middle');

        elem.selectAll('rect')
          .data(data)
          .enter()
            .append('rect')
            .attr('x', d => x(d.name))
            .attr('class', d => d.name)
            .attr('y', d => y(d.val))
            .attr('width', x.bandwidth())
            .attr('height', d => y.range()[0] - y(d.val))
      }

      svg
        .append('g')
        .attr('id', 'bars-style')
        .attr('transform', `translate(0,20)`)
        .call(addRectsWithName, 'Basic styles')
      

    },
    addName(){
      this.names.push(NAMES[this.names.length]);
    }
  },
  mounted(){
    this.drawCircle();
    this.controllingSize();
    this.buildBarras()
  },
  watch:{
    circleSize(newV){
      this.circle
        .attr('r',newV)
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(450px, 1fr));
  width: 100%;
  min-height: 100vh;
  gap: 1em;
}

.card {
  position: relative;
  display: flex;
  flex-direction: column;
  padding: 0.5em 0.75em;
  box-shadow: 0 0 0.25em 0.25em rgba(0, 0, 0, 0.05);
}

.card > .card-title {
  margin: 0;
  font-size: 1.25rem;
  font-family: sans-serif;
}
#bars-style .one {
  fill: #ffc300
}
#bars-style .two {
  fill: #c70039
}
#bars-style .three {
  fill: #571845
}
</style>
