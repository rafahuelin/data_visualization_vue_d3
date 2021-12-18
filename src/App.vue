<template>
  <div id="app">
    <Chart :coupons="coupons" />
  </div>
</template>

<script>
import Chart from './components/Chart.vue'
import { coupons } from './assets/json/coupons.json'

console.log(coupons)

export default {
  name: 'App',
  components: {
    Chart
  },
  data() {
    return {
      coupons
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
