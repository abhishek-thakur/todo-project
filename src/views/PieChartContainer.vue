<template>
  <div class="container" style="background-color: rgba(197, 227, 77)">
    <h3>Pie Chart</h3>
    <pie-chart
      v-if="loaded"
      :key="componentKey"
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
      componentKey: 0,
      options: {
        legend: {
          display: true,
        },
        responsive: true,
        maintainAspectRatio: false,
      },
    };
  },
  methods: {
    forceRerender() {
      this.componentKey += 1;
    },
  },
  computed: {
    chartData() {
      return {
        borderWidth: 1,
        labels: [],
        datasets: [
          {
            label: "data one",
            backgroundColor: [],
            data: null,
          },
        ],
      };
    },
  },
  mounted() {
    const todoRef = firebase.database().ref("todo");
    todoRef.on("value", (snapshot) => {
      var todos = snapshot.val();
      //console.log(Object.keys(todos));
      var categoryArr = [];
      for (let value of Object.values(todos)) {
        var cat = value.category;
        categoryArr.push(cat);
      }
      //console.log(categoryArr);
      const category = {};
      categoryArr.forEach((x) => {
        category[x] = (category[x] || 0) + 1;
      });
      //console.log(category);
      var catKey = [];
      for (let key of Object.keys(category)) {
        catKey.push(key);
      }
      this.chartData.labels = catKey;
      this.chartData.datasets[0].backgroundColor = catKey;
      //console.log(catKey);
      var catCount = [];
      for (let value of Object.values(category)) {
        catCount.push(value);
      }
      //console.log(catCount);
      this.chartData.datasets[0].data = catCount;
      this.loaded = true;
      this.forceRerender();
      // this.$nextTick(function(){
      //   this.$forceUpdate();
      // })
    });
  },
};
</script>