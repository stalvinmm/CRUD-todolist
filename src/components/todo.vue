<template>
  <div class="container my-5 p-5 rounded">
    <h2 class="text-center mt-5">Car Service</h2>

    <!-- ------for input------- -->
    <div class="d-flex mt-5">
      <input
        v-model="task"
        type="text"
        placeholder="ADD TASK"
        class="form-control"
        value="hello"
        
      />
      <button class="btn btn-info" @click="submitTask(tasks)">
        SUBMIT
      </button>
    </div>

    <!-- ----------for task-------------- -->
    <table class="table table-hover table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">edit</th>
          <th scope="col" class="text-center">drop</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ dashed: task.status === 'finished' }">
              {{ task.name }}
            </span>
          </td>
          <td style="width: 120px">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-danger': task.status === 'To-Do',
                'text-warning': task.status === 'In-Progress',
                'text-success': task.status === 'Finished',
              }"
            >
              {{ task.status }}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen pointer"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>




<script>
export default {
  data() {   
    return {
      task: "",
      editedTask: null,
      statuses: ["To-Do", "In-Progress", "Finished"],
      tasks: [
        // {
        //   name: "Wheels",
        //   status: "To-Do",
        // },
        // {
        //   name: "Oil",
        //   status: "To-Do",
        // },
      ],
    };
  },

  methods: {
    submitTask(tasks) {
      if (this.task.length === 0) return; 

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "To-Do",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = "";
      // ----------------local storage----------------

      localStorage.setItem("tasks",JSON.stringify(tasks));
      tasks = localStorage.getItem("tasks");
      console.log(tasks)
      

    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
  },
};
 
</script>






<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto+Condensed&display=swap");
* {
  font-family: "Roboto Condensed", sans-serif;
}
.pointer {
  cursor: pointer;
}
.dashed {
  text-decoration: line-through;
}
.container {
  background-color: rgb(243, 237, 231);
}
</style>