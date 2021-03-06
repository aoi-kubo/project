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
  },
  data() {
    return {
      options: {
        series: [],
        xAxis: {
          title: {
            text: '年度'
          },
          categories: [1960, 1965, 1970, 1975, 1980, 1985, 1990, 1995, 2000, 2005, 2010, 2015, 2020, 2025, 2030, 2035, 2040, 2045]
        },
        yAxis: {
          title: {
            text: '人口数'
          }
        },
        plotOptions: {
          series: {
            animation: {
              duration: 2000
            }
          }
        },
        title: {
          style: {
            display: "none"
          }
        }
      }
    };
  },
  methods: {
    addSeries: function(id, name, population) {
      this.options.series.push({
        id: id,
        name: name,
        data: population
      });
    },
    removeSeries: function(id) {
      this.options.series = this.options.series.filter(val => val.id !== id);
    }
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

@media screen and (max-width: 480px) {
  .prefectures-container {
    margin-left: 0;
  }
}
</style>
