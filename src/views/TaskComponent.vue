<template>
    <tr>
        <td>
            <span :class="{'finished': task.completed == true}">{{task.name}}</span>
        </td>
        <td>
            <div class="pointer" @click="isCompleted(task.id)">
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

</template>
<script>
//import firebase from '@/firebaseApi.js'
export default {
    name: 'TaskComponent',
    props: ['task'],
    methods:{
        editTaskTC(id){
        //    const todoRef = firebase.database().ref("todo").child(id);
        //    todoRef.update({
        //        name: this.task
        //    });
        //    this.task = "";
            this.$parent.$emit('editTask',id);
        },
        deleteTaskTC(id){
        //    const todoRef = firebase.database().ref("todo").child(id);
        //    todoRef.remove();
            this.$parent.$emit('deleteTask', id);
        },
        isCompletedTC(id){
            // const todoRef = firebase.database().ref("todo").child(id);
            // todoRef.update({
            //     completed: !todoRef.completed,
            // })
            this.$parent.$emit('isCompleted', id);
        }
    }
}
</script>