<template>
  <div class="container">
    <h3>Bar Graph showing Task Completion</h3>
    <bar-graph v-if="loaded"
    :graphdata="datacollection"
    :options="options"
    :key="componentKey"
    > </bar-graph>
  </div>
</template>

<script>
import BarGraph from "./BarGraph.vue";
import firebase from "@/firebaseApi.js";
export default {
  name: "BarGraphComponent",
  components: { BarGraph },
  data() {
    return {
      loaded: false,
      componentKey: 0,
      options: {
        scales: {
          xAxes: [
            {
              ticks: {
                beginAtZero: true,
              },
              gridLines: {
                display: false,
              },
            },
          ],
          yAxes: [
            {
              ticks: {
                beginAtZero: true,
              },
              // gridLines: {
              // 	display: true
              // }
            },
          ],
        },
      },
    };
  },
  methods:{
    forceRerender() {
      this.componentKey += 1;
    },
  },
  computed: {
    datacollection() {
      return {
        labels: ["Completed","not completed"],
        datasets: [
          {
            label: ["Task Completed"],
            barPercentage: 1,
            barThickness: 50,
            maxBarThickness: 50,
            minBarLength: 2,
            data: [],
            backgroundColor: [
              "rgba(255, 99, 132, 0.2)",
              "rgba(75, 192, 192, 0.2)",
            ],
            borderColor: ["rgba(255, 99, 132, 1)", "rgba(75, 192, 192, 1)"],
            borderWidth: 1,
          },
        ],
      };
    }
  },
  mounted(){
    const todoRef = firebase.database().ref("todo");
    todoRef.on("value", (snapshot) => {
      var todos = snapshot.val();
      //console.log(Object.keys(todos));
      var completedArr = [];
      for(let value of Object.values(todos)){
        var cmpd = value.completed;
        completedArr.push(cmpd);
      }
      // console.log(completedArr);
      const completed = {};
      completedArr.forEach((x) => {
        completed[x] = (completed[x] || 0) + 1;
      });
      console.log(completed);
      const compKey = [];
      for(let key of Object.keys(completed)){
        compKey.push(key);
      }
      console.log(compKey);
      this.datacollection.labels = compKey;
      const compCount = []
      for(let value of Object.values(completed)){
        compCount.push(value);
      }
      console.log(compCount);
      this.datacollection.datasets[0].data = compCount;
      this.loaded = true;
      this.forceRerender();
    })
  }
}
</script>