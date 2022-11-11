<template>
  <div id="app">
    {{ this.prefectures }}
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      prefectures: null,
    };
  },
  mounted() {
    var prefectures_url =
      "https://opendata.resas-portal.go.jp/api/v1/prefectures";
    axios
      .get(prefectures_url, {
        headers: { "X-API-KEY": "8OSsW1I7j1SPrT1Ne3zZmLpXfPd6cPjRtGA3WuHM" },
      })
      .then((response) => (this.prefectures = response.data.result));
  },
};
</script>
