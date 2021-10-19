<template>
    <div class="container">
        <div>
            <input 
                v-model="task" 
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
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <td><span :class="{'finished': task.completed == true}">{{task.name}}</span></td>
                    <td>
                        <div class="pointer" @click="isCompleted(index)">
                            <span :class="{'fas fa-check':task.completed, 'fas fa-hourglass-half':!task.completed }"></span>
                        </div>
                    </td>
                    <td>
                        <div @click="editTask(task.id)" class="pointer">
                            <span class="fas fa-pen-fancy"></span>
                        </div>
                    </td>
                    <td>
                        <div @click="deleteTask(task.id)" class="pointer">
                            <span class="fas fa-trash-alt"></span>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import firebase from '@/firebaseApi.js'
export default {
    name: 'InputComponent',
    data(){
        return{
            task:"",
            tasks:[]
        }
    },
    computed:{
        isDisabled(){
            return this.task.length == 0;
        },
        // isCompleted(index){
        //     alert("changing")
        //     return this.task[index].completed = !this.task[index].completed;
        // }
    },
    methods:{
        submitTask(){
            const todoRef = firebase.database().ref("todo");
            const task = {
                name : this.task,
                completed: false
            }
            todoRef.push(task);
            this.task= "";
        },
        editTask(id){
           // this.task = this.tasks[index].name;
           const todoRef = firebase.database().ref("todo").child(id);
           todoRef.update({
               name: this.task
           });
           this.task = "";

        },
        deleteTask(id){
           // this.tasks.splice(index, 1)
           const todoRef = firebase.database().ref("todo").child(id);
           todoRef.remove();
        },
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
                        id: id
                    });
                }
            });
        }
    },
    mounted(){
        //const todoRef = firebase.database().ref('todo');
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
    color: green;
}
</style>