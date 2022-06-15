<template>
  <div class="container">
    <h2 class="text-center mt-5">To doLists with Vue</h2>

    <!-- Input -->
    <div class="f-flex">
      <input v-model="task" type="text" placeholder="Enter task" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">Submint</button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th colspan="2" class="text-center">Operation</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{'finished': task.status === 'finished'}">{{ task.name }}</span>
            </td>
          <td style="width: 120px;">
            <span @click="changeStatus(index)" class="pointer"
            :class="{'text-danger': task.status ===  'to-do',
            'text-warning': task.status === 'in-progress',
            'text-success': task.status === 'finished'}">
              {{ firstCharUpper(task.status) }}</span>
          </td>
          <td class="text-center" @click="editTask(index)" >
             <button class="btn btn-warning">Edit</button>
          </td>
          <td class="text-center" @click="deleteTask(index)">
            <button class="btn btn-danger">Delete</button>
          </td>
        </tr>

      </tbody>
    </table>    
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  props: {
    msg: String
  },

  data(){
    return {
      task: '',
      editedTask: null,
      availableStatuses: ['to-do', 'in-progress','finished'],
      tasks: [
        {
          name: 'Steal bananas from the store',
          status: 'to-do',
        },
        {
          name: 'Eat apple 10 boz',
          status: 'in-progress',
        }
      ]
    }
  },

  methods: {
    submitTask(){
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } 
      // after finish one task to clear input
      this.task = '';
    },

    deleteTask(index) {
      this.tasks.splice(index,1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },

    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
};
</script>

<style scoped>
  .pointer {
    cursor: pointer;
  }
  .finished {
    text-decoration: line-through;
  }
</style>