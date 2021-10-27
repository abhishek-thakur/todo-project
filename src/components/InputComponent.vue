<template>
    <div class="container">
        <div>
            <input 
                v-model="Task" 
                type="text" 
                placeholder="enter the task" 
                class="form-control"
            />
            <button 
                @click="submitTask" 
                class="btn btn-dark mt-2" 
                :disabled="isDisabled"
            >
                <span class="fas fa-plus"></span>
            </button>
        </div>

        <H1 class="mt-3">TODO List</H1>

        <table class="table table-hover table-bordered mt-3">
            <thead>
                <th>Task</th>
                <th>Status</th>
                <th>Update</th>
                <th>Delete</th>
                <th>Category</th>
            </thead>
            <tbody>
                <task-component 
                    v-for="(task, index) in tasks" :key="index" :task="task"
                    @editTask="editTask" @deleteTask="deleteTask" @isCompleted="isCompleted" @changeBg = "changeBg"
                >
                </task-component>
            </tbody>
        </table>
        <!-- <bar-graph></bar-graph> -->
        <pie-chart-container></pie-chart-container>
    </div>
</template>

<script>
import TaskComponent from '../views/TaskComponent.vue'
import firebase from '@/firebaseApi.js'
import PieChartContainer from '../views/PieChartContainer.vue'
//import BarGraph from '../views/BarGraph.vue'

export default {
    name: 'InputComponent',
    components:{
        TaskComponent,
        PieChartContainer,
        
    },
    data(){
        return{
            Task:"",
            tasks:[]
        }
    },
    computed:{
        isDisabled(){
            return this.Task.length == 0;
        }
    },
    methods:{
        //to add new task in todo list
        submitTask(){
            const todoRef = firebase.database().ref("todo");
            const task = {
                name : this.Task,
                completed: false,
                category :"white"
            }
            todoRef.push(task);
            this.Task= "";
        },

        //to edit task name of task
        editTask(id,newTask){
           // this.task = this.tasks[index].name;
           //alert("from ip component");
           const todoRef = firebase.database().ref("todo").child(id);
           todoRef.update({
               name: newTask
           });
           this.Task = "";

        },

        //to delete the task from todo list
        deleteTask(id){
           // this.tasks.splice(index, 1)
           //alert("from ip component");
           const todoRef = firebase.database().ref("todo").child(id);
           todoRef.remove();
        },

        //to change the status of the task
        isCompleted(id){
            //alert("from ip component");
            const todoRef = firebase.database().ref("todo").child(id);
            todoRef.update({
                completed: !todoRef.completed,
            })
        },
        
        //to change the bgcolor of the row
        changeBg(id, newCat){
            const todoRef = firebase.database().ref("todo").child(id);
            todoRef.update({
                category : newCat
            })
        },

        // to get the task from database
        getTodos(){
            const todoRef = firebase.database().ref("todo");
            var vm = this;
            todoRef.on("value",(snapshot)=>{
                const todos = snapshot.val();
                vm.tasks = [];
                for (let id in todos){
                    vm.tasks.push({
                        name : todos[id].name,
                        completed: todos[id].completed,
                        category: todos[id].category,
                        id: id
                    });
                }
            });
        }
    },

    //to render table from the database
    mounted(){
        this.getTodos();
    }
}
</script>

<style>
.pointer{
    cursor: pointer;
}
.finished{
    text-decoration: line-through;
font-weight: bold;
}
</style>