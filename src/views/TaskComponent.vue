<template>
  <tr>
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
        <b-button v-b-modal.modal-prevent-closing><span class="fas fa-pen-fancy"></span></b-button>
      </div>
      <b-modal id="modal-prevent-closing"
        ref="modal"
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
  </tr>
</template>
<script>
export default {
  name: "TaskComponent",
  data() {
    return {
      upTask:this.task.name
    };
  },
  props: ["task"],
  methods: {
      // handleOk(bvModalEvt) {
      //   bvModalEvt.preventDefault()
      //   this.handleSubmit()
      // },
      // handleSubmit() {
      //   if (this.upTask.length == 0) {
      //     return
      //   }
      //   this.$nextTick(() => {
      //     this.$bvModal.hide('modal-prevent-closing')
      //   })
      // },
    editTaskTC(id) {
      //alert("editing")
      if (this.upTask.length == 0) {
          return
        }
        this.$emit("editTask",id);
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing')
        })
    },
    deleteTaskTC(task) {
      //alert("deleting");
      this.$emit("deleteTask", task.id);
    },
    isCompletedTC(task) {
      //alert("completed");
      this.$emit("isCompleted", task.id);
    },
  },
};
</script>