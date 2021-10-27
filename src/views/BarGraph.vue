<template>
    <button class="btn btn-Success" @click="showBar">Bar Graph</button>
</template>
<script>
import { Bar } from 'vue-chartjs'

export default {
    name:'BarGraph',
    props:['data','options'],
    extends: Bar,
    methods:{
        showBar(){
            this.renderChart(data, options)
        }
    },
    // mounted () {
    //     this.renderChart(data, options)
    // }
}
</script>