<template>
  <div class="container">
    <h3>Pie Chart</h3>
    <pie-chart
      v-if="loaded"
      :chartData="chartData"
      :options="options"
    ></pie-chart>
  </div>
</template>

<script>
import firebase from "@/firebaseApi.js";
import PieChart from "../views/PieChart.vue";
export default {
  name: "PieChartContainer",
  components: { PieChart },
  data() {
    return {
      loaded: false,
      options: {
        legend: {
          display: true,
        },
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  computed: {
    chartData() {
      return {
        borderWidth: 1,
        borderColor: [
          "rgba(255,99,132,1)",
          "rgba(54, 162, 235, 1)",
          "rgba(255, 206, 86, 1)",
          "rgba(75, 192, 192, 1)",
        ],
        backgroundColor: [
          "rgba(255, 99, 132, 0.2)",
          "rgba(54, 162, 235, 0.2)",
          "rgba(255, 206, 86, 0.2)",
          "rgba(75, 192, 192, 0.2)",
        ],
        labels: null,
        dataSet: [
          {
            label: "data one",
            backgroundColor: "#f87979",
            data: null,
          },
        ],
      };
    },
  },
  mounted() {
    this.loaded = false;
    const todoRef = firebase.database().ref("todo");
    todoRef.on("value", (snapshot) => {
      var todos = snapshot.val();
      //console.log(Object.keys(todos));
      var categoryArr = [];
      for (let value of Object.values(todos)) {
        var cat = value.category;
        categoryArr.push(cat);
      }
      console.log(categoryArr);
      const category = {};
      categoryArr.forEach((x) => {
        category[x] = (category[x] || 0) + 1;
      });
      console.log(category);
      var catKey = [];
      for (let key of Object.keys(category)) {
        catKey.push(key);
      }
      this.chartData.labels = catKey;
      console.log(catKey);
      var catCount = [];
      for (let value of Object.values(category)) {
        catCount.push(value);
      }
      console.log(catCount);
      this.chartData.dataSet[0].data = catCount;
    });
    this.loaded = true;
  },
};
</script>