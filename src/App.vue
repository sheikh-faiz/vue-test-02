<template>
  <div id="app">
    <b-container>
      <b-row class="mb-4">
        <b-col md="3">
          <b-card>
            <b-card-body>
              <b-card-text class="font-weight-bolder">
                {{ summary("1. open") }}
              </b-card-text>
            </b-card-body>
          </b-card>
        </b-col>
        <b-col md="3">
          <b-card>
            <b-card-body>
              <b-card-text class="font-weight-bolder">
                {{ summary("2. high") }}
              </b-card-text>
            </b-card-body>
          </b-card>
        </b-col>
        <b-col md="3">
          <b-card>
            <b-card-body>
              <b-card-text class="font-weight-bolder">
                {{ summary("3. low") }}
              </b-card-text>
            </b-card-body>
          </b-card>
        </b-col>
        <b-col md="3">
          <b-card>
            <b-card-body>
              <b-card-text class="font-weight-bolder">
                {{ summary("4. close") }}
              </b-card-text>
            </b-card-body>
          </b-card>
        </b-col>
      </b-row>
      <b-row>
        <b-col md="12">
          <status-table class="mb-4" :data="tableData"></status-table>
          <stick-chart
            :chartSeries="[{ name: 'Graph Title', data: chartSeries }]"
          ></stick-chart>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import {
  BCol,
  BContainer,
  BCard,
  BCardBody,
  BRow,
  BCardText,
} from "bootstrap-vue";

import StickChart from "./components/StickChart.vue";
import StatusTable from "./components/Table.vue";
import axios from "axios";
export default {
  name: "App",
  components: {
    BCol,
    BContainer,
    BCard,
    BCardBody,
    BRow,
    BCardText,
    StatusTable,
    StickChart,
  },
  data() {
    return {
      tableDate: [],
      tableData: [],
      chartSeries: [],
    };
  },
  methods: {
    summary(key) {
      const v = this.tableData.reduce(function (sum, current) {
        return sum + parseFloat(current[key]);
      }, 0);
      console.log(v);
      return v.toFixed(2);
    },
  },
  mounted() {
    axios
      .get(
        "https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=IBM&apikey=demo"
      )
      .then((response) => {
        const { data } = response;
        const dailyData = data["Time Series (Daily)"];
        this.tableDate = Object.keys(dailyData);
        this.tableData = Object.values(dailyData);
        this.tableData.map((e, index) => {
          this.chartSeries.push({
            x: this.tableDate[index],
            y: [
              e["1. open"],
              e["2. high"],
              e["3. low"],
              e["4. close"],
              e["5. volume"],
            ],
          });
          e.date = this.tableDate[index];
        });
      });
  },
};
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
