<template>
  <div class="container">
    <h3>Bar Graph showing Task Completion</h3>
    <bar-graph :graphdata="datacollection" :options="options"> </bar-graph>
  </div>
</template>

<script>
import BarGraph from "./BarGraph.vue";
//import firebase from "@/firebaseApi.js";
export default {
  name: "BarGraphComponent",
  components: { BarGraph },
  data() {
    return {
      options: {
        scales: {
          y: {
            beginAtZero: true,
          },
        },
      },
    };
  },
  computed: {
    datacollection() {
      return {
        labels: ["Not Completed", "Completed"],
        datasets: [
          {
            label: "Task Completed",
            data: [12, 19],
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
  }
};
</script>