<template>
  <div class="container" style="max-width: 1000px">
    <!-- Heading -->
    <h2 class="text-center mt-5">Name Todo List</h2>

    <!-- Add new Task  popup-->
    <div class="d-flex justify-content-center mt-5">
      <b-button
        id="show-btn"
        class="btn btn-info rounded-50 w-50 form-control"
        @click="$bvModal.show('bv-modal-example')"
        >Add New Task</b-button
      >
      <b-modal id="bv-modal-example" hide-footer>
        <template #modal-title>Add New Todo</template>
        <!-- Task name  -->
        <b-form-group
          label="Task Name"
          label-for="name-input"
          invalid-feedback="Name is required"
        >
          <b-form-input id="name-input" v-model="name" required></b-form-input>
        </b-form-group>
        <!-- Task description-->
        <b-form-group
          label="Discrption"
          label-for="name-input"
          invalid-feedback="Name is required"
        >
          <b-form-textarea
            id="name-input"
            v-model="Discrption"
            required
          ></b-form-textarea>
        </b-form-group>
        <!-- Task priority-->
        <b-form-group label="Priority" label-for="input-3">
          <b-form-select
            id="input-3"
            v-model="priority"
            :options="Priority"
            required
          ></b-form-select>
        </b-form-group>

        <!-- Attachment-->
        <div class="form-group">
          <label for="exampleFormControlFile1">Attachment</label>
          <input
            type="file"
            class="form-control-file"
            id="exampleFormControlFile1"
          />
        </div>
        <b-button class="btn btn-info mt-3" block @click="AddTask()"
          >Add</b-button
        >
      </b-modal>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-5">
      <!-- table header  -->
      <thead>
        <tr>
          <th scope="col" class="bg-secondary text-white">Task Status</th>
          <th scope="col" class="bg-secondary text-white">Task</th>
          <th scope="col" class="bg-secondary text-white text-center">
            Priority
          </th>
          <th scope="col" class="bg-secondary text-white text-center">
            Delete
          </th>
          <th scope="col" class="bg-secondary text-white text-center">Edit</th>
        </tr>
      </thead>
      <!-- table body-->
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td
            class="pointer noselect"
            @click="changeStatus(index)"
            :class="{
              color3: task.status === 'To do',
              color1: task.status === 'Done',
              color2: task.status === 'In Progress',
            }"
          >
            <span>
              {{ task.status }}
            </span>
          </td>
          <td class="pointer noselect" @click="showTodo(index)">
            <span
              :class="{
                'line-through': task.status === 'Done',
              }"
            >
              {{ task.name }}
            </span>
          </td>
          <td class="text-center">
            <div>
              {{ task.Priority }}
            </div>
          </td>
          <td class="text-center">
            <div class="pointer noselect" @click="deleteTask(index)">
              <span class="fas fa-times-circle"></span>
            </div>
          </td>
          <td class="text-center">
            <div class="pointer noselect" @click="OpenModal(task)">
              <p class="fas fa-edit"></p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <edit_todolist
      :todoobject="taskItem"
      @updateItem="updateTodoItem"
    ></edit_todolist>
  </div>
</template>

<script>
import edit_todolist from "./edit_todolist.vue";

export default {
  name: "HelloWorld",
  components: { edit_todolist },
  props: {
    msg: String,
  },
  data() {
    return {
      Discrption: "",
      priority: null,
      name: "",
      task: "",
      taskItem: {},
      editedTask: null,
      statuses: ["To do", "In Progress", "Done"],
      Priority: ["Very Low", "Low", "Medium", "High", "Very High"],

      /* Status could be: 'to-do' / 'in-progress' / 'finished' */
      tasks: [
        {
          id: 123,
          name: "Steal bananas from the supermarket.",
          description: "ahhahahhahhhahha",
          status: "To do",
          Priority: "Very Low",
        },
        {
          id: 12,
          name: "Eat 1 kg chocolate in 1 hour.",
          description: "ahhahahhahhhahha",
          status: "In Progress",
          Priority: "Medium",
        },
        {
          id: 1253,
          name: "Create YouTube video.",
          description: "ahhahahhahhhahha",
          status: "Done",
          Priority: "Low",
        },
        {
          id: 15423,
          name: "Watch Naruto.",
          description: "ahhahahhahhhahha",
          status: "Done",
          Priority: "High",
        },
      ],
    };
  },

  methods: {
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },

    /**
     * Deletes task by index
     */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    /**
     * Edit task
     */
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    addTask() {},

    /**
     * Add / Update task
     */
    submitTask() {
      if (this.task.length === 0) return;

      /* We need to update the task */
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        /* We need to add new task */
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },

    OpenModal(task) {
      this.taskItem = task;
      this.$bvModal.show("bv-modal-example2");
    },

    updateTodoItem(targetTodo) {
      this.$bvModal.hide("bv-modal-example2");
      this.findTodoItemByID(targetTodo);
    },

    findTodoItemByID(targetTodo) {
      this.tasks.forEach((todo, index) => {
        if (todo.id == targetTodo.id) this.$set(this.tasks, index, targetTodo);
      });
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}

.line-through {
  text-decoration: line-through;
}

.color1 {
  background-color: #69f0ae;
}
.color2 {
  background-color: #fff59d;
}
.color3 {
  background-color: #ff6e40;
}
</style>
