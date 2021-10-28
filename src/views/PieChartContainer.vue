<template>
    <div class="container">
        <h3>Pie Chart </h3>
        <pie-chart
            v-if="loaded"
            :chartData="chartData"
            :options="options"
        ></pie-chart>
    </div>
</template>

<script>
import firebase from '@/firebaseApi.js'
import PieChart from '../views/PieChart.vue'
export default {
    name:'PieChartContainer',
    components:{PieChart},
    data(){
        return{
            loaded: false,
            chartData: null,
            options:{
                borderWidth: "10px",
                hoverBackgroundColor: "red",
                hoverBorderWidth: "10px"
            }
        }
    },
    mounted () {
    this.loaded = false
    try {
        const todoRef = firebase.database().ref("todo");
        todoRef.on("value",(snapshot)=>{
            var todos = snapshot.val();
            console.log(Object.keys(todos));
            var category =[];
            for(let value of Object.values(todos)){
                var cat = value.category;
                category.push(cat);
            }
            console.log(category);
        })
      this.loaded = true
    } catch (e) {
      console.error(e)
    }
  }
}
</script>