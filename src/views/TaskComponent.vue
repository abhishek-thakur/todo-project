<template>
    <tr>
        <td>
            <span :class="{'finished': task.completed == true}">{{task.name}}</span>
        </td>
        <td>
            <div class="pointer" @click="isCompletedTC(task)">
                <span :class="{'fas fa-check':task.completed, 'fas fa-hourglass-half':!task.completed }"></span>
            </div>
        </td>
        <td>
            <div @click="editTaskTC(task)" class="pointer">
                <span class="fas fa-pen-fancy"></span>
            </div>
        </td>
        <td>
            <div @click="deleteTaskTC(task)" class="pointer">
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
        editTaskTC(task){
            //alert("editing")
            this.$emit('editTask', task.id);
        },
        deleteTaskTC(task){
            //alert("deleting");
            this.$emit('deleteTask', task.id);
        },
        isCompletedTC(task){
            //alert("completed");
            this.$emit('isCompleted', task.id);
        }
    }
}
</script>