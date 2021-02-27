<template>
  <div class="prefectures-area">
    <div v-for="prefecture in prefectures" :key="prefecture.id" class="prefecture">
      <label :for="prefecture.id">
        <input
          type="checkbox"
          :id="prefecture.id"
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
    getAPI: function(key) {
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
        const response = await this.getAPI(key);
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
    }
  }
};
</script>
<style>
.prefectures-area {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
}
.prefectures {
  font-size: 15px;
}
label {
  cursor: pointer;
}
</style>