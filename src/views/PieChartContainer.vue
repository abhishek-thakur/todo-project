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
            //console.log(Object.keys(todos));
            var categoryArr =[];
            for(let value of Object.values(todos)){
                var cat = value.category;
                categoryArr.push(cat);
            }
            console.log(categoryArr);
            const category = {};
            categoryArr.forEach((x)=>{
                category[x] =(category[x] || 0)+1;
            });
            console.log(category);
            var catKey=[];
            for(let key of Object.keys(category)){
                catKey.push(key)
            }
            console.log(catKey);
            var catCount=[];
            for(let value of Object.values(category)){
                catCount.push(value);
            }
            console.log(catCount);
            //this.chartData = catCount;
        });
        
        this.loaded = true;
    } catch (e) {
      console.error(e)
    }
  }
}
</script>