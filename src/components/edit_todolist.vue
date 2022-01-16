<template>
  <b-modal id="bv-modal-example2" hide-footer>
    <template #modal-title>Add New Todo</template>
    <!-- Task name  -->
    <b-form-group
      label="Task Name"
      label-for="name-input"
      invalid-feedback="Name is required"
    >
      <b-form-input
        id="name-input"
        :value="todoobject.name"
        @input="updateTaskName"
        required
      ></b-form-input>
    </b-form-group>
    <!-- Task description-->
    <b-form-group
      label="description"
      label-for="name-input"
      invalid-feedback="Name is required"
    >
      <b-form-textarea
        id="name-input"
        :value="todoobject.description"
        @input="updateDescription"
        required
      ></b-form-textarea>
    </b-form-group>
    <!-- Task priority-->
    <b-form-group label="Priority" label-for="input-3">
      <b-form-select
        id="input-3"
        :value="todoobject.Priority"
        :options="Priority"
        @change="updatePriority"
        required
      ></b-form-select>
    </b-form-group>
    <b-button class="btn btn-info mt-3" block @click="AddTask()">Add</b-button>
  </b-modal>
</template>

<script>
export default {
  name: "edit_todolist",
  props: {
    todoobject: {
      type: Object,
      require: true,
    },
  },
  data() {
    return {
      Priority: ["Very Low", "Low", "Medium", "High", "Very High"],
      taskName: "",
      description: "",
      priority: "",
    };
  },
  methods: {
    updateTaskName(val) {
      this.taskName = val;
    },
    updateDescription(val) {
      this.description = val;
    },
    updatePriority(val) {
      this.priority = val;
    },
    AddTask() {
      this.$emit("updateItem", {
        id: this.todoobject.id,
        name: this.taskName || this.todoobject.name,
        description: this.description || this.todoobject.description,
        Priority: this.priority || this.todoobject.Priority,
        status: this.todoobject.status,
      });
    },
  },
};
</script>

<style scoped></style>
