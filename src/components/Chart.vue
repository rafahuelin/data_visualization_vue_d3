<template>
  <div>
    <h3>{{title}}</h3>
    <select v-model="selected" >
      <option v-for="company in companies" :key="company" :value="company">{{titleCase(company)}}</option>
    </select>
    <br />
    <svg></svg>
  </div>
</template>

<script>
import * as d3 from "d3"

export default {
  name: 'Chart',
  props: ['axisData', 'companies', 'title'],
  data() {
    return {
      selected: this.companies[0],
    }
  },
  computed: {
    filteredData() {
      return this.axisData.filter(el => el.company === this.selected)
    },
    maxValue() {
      return Math.max(...this.filteredData.map(el => el.y))
    },
  },
  methods: {
    titleCase(title) {
      return title.charAt(0).toUpperCase() + title.slice(1)
    },
    renderChart() {
      const height = 300
      const roundedHeight = Math.ceil((height + 1)/10) * 10
      const width = 800

      // set the ranges
      const xScale = d3
        .scaleBand()
        .domain(this.filteredData.map(dataPoint => dataPoint.x))
        .range([0, width])
        .padding(0.2)

      const yScale = d3
        .scaleLinear()
        .domain([0, this.maxValue])
        .range([roundedHeight, 10])

      const container = d3.select('svg')
        .classed('chart-container', true)
        .style('height', `${roundedHeight}px`)
        .style('width', `${width}px`)

      // eslint-disable-next-line no-unused-vars
      const bar = container
        .selectAll('.bar')
        .data(this.filteredData)
        .enter()
        .append('rect')
        .classed('bar', true)
        .attr('width', xScale.bandwidth())
        .attr('height', data => roundedHeight - yScale(data.y))
        .attr('x', data => (xScale(data.x)))
        .attr('y', data => yScale(data.y))

      // add the x Axis
      container.append("g")
        .attr('transform', "translate(0," + roundedHeight + ")")
        .call(d3.axisBottom(xScale))
        .selectAll('text')
        .attr('transform', "translate(-15, 15) rotate(-45)");

      // add the y Axis
      container.append("g")
        .call(d3.axisLeft(yScale));
    }
  },
  mounted() {
    this.renderChart()
  },
  updated() {
    this.renderChart()
  },
  beforeUpdate() {
    var svg = d3.select("svg")
    svg
      .selectAll('*')
      .remove()
  },
}
</script>

<style>
  .chart-container {
    border: 1px solid navy;
    margin-left: auto;
    margin-right: auto;
    overflow: visible;
  }

  .bar { 
    fill: rgb(255, 0, 212);
  }

  text {
    fill: navy;
    font-size: smaller;
  }

  path.domain {
    stroke: transparent;
  }

  select {
    margin-bottom: 20px;
  }
</style>