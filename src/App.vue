<template>
  <div id="app">
    <h1>Data visualization</h1>
    <Chart
      :axisData="axisData"
      :coupons="coupons"
      title="Tickets vs. Value"
      :companies="companies"
    />
  </div>
</template>

<script>
import Chart from "./components/Chart.vue";
import { coupons } from "./assets/json/coupons.json";

export default {
  name: "App",
  components: {
    Chart,
  },
  data() {
    return {
      coupons,
      axisData: coupons
                  .filter(el => el.promotion_type === 'percent-off')
                  .map(el => ({x: el.coupon_id, y: el.value, company: el.webshop_id}))
    };
  },
  computed: {
    companies() {
      return [...new Set(this.coupons.map(el => el.webshop_id))]
    },
  }
};
</script>

<style>
body {
  background-color: aquamarine;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: navy;
  margin-top: 60px;
}
</style>
