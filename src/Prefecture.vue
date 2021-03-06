<template>
  <div class="prefecture">
    <div v-for="prefecture in prefectures" :key="prefecture.id" class="prefecture-area">
      <label :for="prefecture.id">
        <input
          type="checkbox"
          :id="prefecture.id"
          :checked="prefecture.isChecked"
          @click="changeChart(prefecture.id, prefecture.name, prefecture.isChecked)"
        />
        {{ prefecture.name }}
      </label>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const ACCESS_KEY = process.env.VUE_APP_KEY;

export default {
  data() {
    return {
      prefectures: []
    };
  },
  mounted() {
    this.initPrefectures();
  },
  methods: {
    /* APIの取得 */
    fetchAPI: function(key) {
      const response = axios.get(
        `https://opendata.resas-portal.go.jp/api/v1/${key}`,
        {
          headers: { "X-API-KEY": ACCESS_KEY }
        }
      );
      return response;
    },
    /* 都道府県の初期表示 */
    initPrefectures: async function() {
      const key = "prefectures";
      try {
        const response = await this.fetchAPI(key);
        this.prefectures = response.data.result.map(val => {
          return {
            id: val["prefCode"],
            name: val["prefName"],
            isChecked: false
          };
        });
      } catch (error) {
        console.error(error);
      }
    },
    // グラフの描写
    setChart: async function(id, name) {
      const key = `population/composition/perYear?cityCode=-&prefCode=${id}`;
      try {
        const response = await this.fetchAPI(key);
        const population = response.data.result.data[0].data.map(
          val => val["value"]
        );
        this.$emit("onAddSeries", id, name, population);
        this.prefectures[id - 1].isChecked = true;
      } catch (error) {
        console.error(error);
      }
    },
    // グラフの削除
    offChart: function(id) {
      this.$emit("onRemoveSeries", id);
      this.prefectures[id - 1].isChecked = false;
    },
    // グラフの切り替え
    changeChart: function(id, name, isChecked) {
      if (isChecked) {
        this.offChart(id);
      } else {
        this.setChart(id, name);
      }
    }
  }
};
</script>
<style lang="scss">
.prefecture {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  &-area {
    font-size: 15px;
  }
}
label {
  cursor: pointer;
  input {
    margin-right: 5px;
    margin-bottom: 10px;
  }
}
</style>