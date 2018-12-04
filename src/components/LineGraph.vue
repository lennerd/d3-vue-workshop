<template>
  <svg :width="width" :height="height">
    <path :d="line(data)" stroke="black" stroke-width="1" fill="none" style="transition: d 400ms" />
    <g ref="xAxis" :transform="`translate(0, ${height - margin.bottom})`" />
    <g ref="yAxis" :transform="`translate(${margin.left}, 0)`" />
  </svg>
</template>

<script>
import * as d3 from 'd3';

export default {
  name: 'LineGraph',

  props: ['data', 'curve', 'width', 'height'],

  data() {
    return {
      margin: {
        top: 15,
        right: 15,
        bottom: 30,
        left: 30,
      }
    }
  },
  
  computed: {
    x() {
      return d3
        .scaleLinear()
        .domain(d3.extent(this.data, d => d.year))
        .range([this.margin.left, this.width - this.margin.right]);
    },
    y() {
      return d3
        .scaleLinear()
        .domain([0, d3.max(this.data, d => d.income)])
        .nice()
        .range([this.height - this.margin.bottom, this.margin.top]);
    },
    line() {
      return d3
        .line()
        .curve(this.curve)
        .x(d => this.x(d.year))
        .y(d => this.y(d.income));
    },
  },

  mounted() {
    this.$nextTick(() => {
      this.addAxes();
    });
  },
  
  updated() {
    this.addAxes();
  },

  methods: {
    addAxes() {
      const xAxis = d3.axisBottom(this.x)
        .tickFormat(d3.format("d"));

      d3.select(this.$refs.xAxis)
        .call(xAxis);

      const yAxis = d3.axisLeft(this.y)

      d3.select(this.$refs.yAxis)
        .call(yAxis);
    }
  }
}
</script>

