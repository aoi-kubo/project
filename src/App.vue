<template>
  <div id="app">
    <h1>都道府県別の総人口推移グラフ</h1>
    <div class="prefectures-container">
      <h2>都道府県名</h2>
      <Prefectures 
        @onAddSeries="addSeries" @onRemoveSeries="removeSeries" />
    </div>
    <Highcharts :options="options" />
  </div>
</template>

<script>
import { Chart } from "highcharts-vue";
import Prefectures from "./Prefecture.vue";

export default {
  components: {
    Highcharts: Chart,
    Prefectures: Prefectures
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  max-width: 1000px;
  margin: 0 auto;
  h1 {
    text-align: center;
    font-size: 23px;
  }
  h2 {
    font-size: 17px;
  }
  .prefectures-container {
    margin-left: 5%;
  }
}
</style>
