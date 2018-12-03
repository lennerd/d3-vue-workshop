<template>
  <div>
    <LineGraph
      v-for="(curve, index) in curves"
      :key="index"
      :curve="curve"
      :data="data"
      width="400"
      height="400"
    />
    <div style="text-align: center;">
      <button @click="shuffle" style="font-size: 2em">Shuffle</button>
    </div>
  </div>
</template>

<script>
import LineGraph from './components/LineGraph.vue';
import { createIncomeData } from './data';
import * as d3 from 'd3';

export default {
  name: 'app',
  components: {
    LineGraph
  },

  data() {
    return {
      data: createIncomeData(),
      curves: [
        d3.curveLinear,
        d3.curveMonotoneX,
        d3.curveBasis,
        d3.curveStepAfter,
      ]
    }
  },

  methods: {
    shuffle() {
      this.data = createIncomeData();
    }
  }
}
</script>

