<template>
  <div class="wrapper">
    <h2>都道府県一覧</h2>
    <div class="prefectures-box">
    <div
      v-for="prefecture in prefectures"
      :key="prefecture.id"
      class="prefecture"
    >
      <label :for="prefecture.id">
        <input
          type="checkbox"
          :id="prefecture.id"
          :checked="prefecture.isChecked"
          v-on:click="toggleChart(prefecture.id, prefecture.name, prefecture.isChecked)"
        />
        {{ prefecture.name }}
      </label>
    </div>
  </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      prefectures: [],
    };
  },
  mounted() {
    this.initPrefectures();
  },
  methods: {
    /* access API */
    fetchAPI: function (path) {
      const response = axios.get(`${process.env.VUE_APP_BASE_URL}/${path}`, {
        headers: {
          // Check with POSTMAN
          "X-API-KEY": process.env.VUE_APP_PREFECTURE_ACCESS_TOKEN,
          "Content-Type": "application/json;charset=UTF-8",
        },
      });
      return response;
    },

    // Display prefectures
    initPrefectures: function () {
      const path = "prefectures";
      try {
        this.fetchAPI(path)
          .then((response) => {
            // APIデータを返す；コンソールで確認
            console.log("initPrefectures response ===", response.data);
            this.prefectures =
              response.data &&
              response.data.result &&
              response.data.result.map((val) => {
                return {
                  id: val["prefCode"],
                  name: val["prefName"],
                  isChecked: false,
                };
              });
          })
          .catch((err) => {
            console.log("initPrefectures response err ===", err);
          });
      } catch (error) {
        console.error(error.message);
      }
    },

    /* get the graph */
    addChart: function (id, name) {
      const path = `population/composition/perYear?cityCode=-&prefCode=${id}`;
      try {
        this.fetchAPI(path)
          .then((response) => {
            console.log("addChart response ===", response.data);
            const population =
              response.data &&
              response.data.result &&
              response.data.result.data[0] &&
              response.data.result.data[0].data &&
              response.data.result.data[0].data.map((val) => val["value"]);
            this.$emit("dispSeries", id, name, population);
            // 1-47ではなくて0-46にするため - get the result of the index
            this.prefectures[id - 1].isChecked = true;
          })
          .catch((err) => {
            console.log("addChart response err ===", err);
          });
      } catch (error) {
        console.error(error.message);
      }
    },

    /* delete the graph */
    deleteChart: function (id) {
      this.$emit("hideSeries", id);
      // 1-47ではなくて0-46にするため - get the result of the index
      this.prefectures[id - 1].isChecked = false;
    },

    /* toggle graph on and off */
    toggleChart: function (id, name, isChecked) {
      if (isChecked) {
        this.deleteChart(id);
      } else {
        this.addChart(id, name);
      }
    },
  },
};
</script>
<style scoped>

.wrapper {
  background:antiquewhite;
  padding:2vh 0;
  width:90%;
  max-width:640px;
  margin:0 auto;
}

.wrapper h2 {
  font-size: 20px;
  text-align: center;
  margin:0;
}
.prefectures-box {
  padding:10px 0;
  font-size:0.9em;
  display:flex;
  flex-wrap: wrap;
}

.prefectures-box .prefecture {
  width:calc(100%/6);
  margin-bottom: 10px;
}

@media screen and (max-width:576px) {

  .wrapper {
    width:100%;
    max-width: 540px;
  }
  .prefectures-box {
    font-size: 0.8em;
  }

  .prefectures-box .prefecture {
    width:20%;
  }

}

@media screen and (max-width:500px) {
  .prefectures-box {
    font-size: 0.7em;
  }

  .prefectures-box .prefecture {
    width:25%;
  }
}


</style>
