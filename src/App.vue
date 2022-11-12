<template>
  <div id="app">
    <div class="main-contents">
      <h1>都道府県別グラフ</h1>
    <div class="prefectures-wrapper">
      <h2>都道府県</h2>
      <Prefectures onAddSeries="addSeries" onRemoveSeries="removeSeries" />
    </div>
    <Highcharts :options="options" />
    </div>
  </div>
</template>

<script>
import {Chart} from "highcharts-vue";
import Prefectures from "./Prefectures.vue";

export default {
  components: {
    Highcharts: Chart,
    Prefectures: Prefectures
  }
};
</script>
<style scoped>
#app {
  max-width: 100%;
  margin: 0 auto;
}

.main-contents {
  width:100%;
  max-width: 992px;
  margin:0 auto;
}
.prefectures-wrapper {
  padding:5px 10px 20px;
  margin:4vh auto;
}

</style>
