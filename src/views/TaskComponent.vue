<template>
  <tr :style="{'background': upCategory}">
    <td>
      <span :class="{ finished: task.completed == true }">{{ task.name }}</span>
    </td>
    <td>
      <div class="pointer" @click="isCompletedTC(task)">
        <span
          :class="{
            'fas fa-check': task.completed,
            'fas fa-hourglass-half': !task.completed,
          }"
        ></span>
      </div>
    </td>
    <td>
      <div class="pointer">
        <div @click="showModal"><span class="fas fa-pen-fancy"></span></div>
      </div>
      <b-modal
        ref="my-modal"
        title="Update the Task"
        @ok="editTaskTC(task.id)" centered
      >
        <input type="text" 
          v-model="upTask"
          class="form-control"
        />
      </b-modal>
    </td>
    <td>
      <div @click="deleteTaskTC(task)" class="pointer">
        <span class="fas fa-trash-alt"></span>
      </div>
    </td>
    <td>
      <select v-model="upCategory" v-on:change="changeCat(task.id)">
        <option value="white">default</option>
        <option value="blue">blue</option>
        <option value="green">green</option>
        <option value="red">red</option>
      </select>
    </td>
  </tr>
</template>
<script>
export default {
  name: "TaskComponent",
  data() {
    return {
      upTask:this.task.name,
      upCategory: this.task.category
    };
  },
  props: ["task"],
  methods: {
    showModal() {
        this.$refs['my-modal'].show();
      },
    editTaskTC(id) {
      if (this.upTask.length == 0) {
          return
        }
        this.$emit("editTask",id,this.upTask);
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing')
        })
    },
    changeCat(id){
      this.$emit("changeBg",id,this.upCategory)
    },
    deleteTaskTC(task) {
      this.$emit("deleteTask", task.id);
    },
    isCompletedTC(task) {
      this.$emit("isCompleted", task.id);
    }
  }
};
</script>