<template>
  <div class="home">
    <TodoAppVue></TodoAppVue>
  </div>
</template>

<script>
// @ is an alias to /src
import TodoAppVue from "../components/TodoApp.vue";

export default {
  name: "Home",
  components: {
    TodoAppVue,
  },
};
</script>
