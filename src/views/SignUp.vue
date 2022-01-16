<template>
  <div class="about">
    <h1>SignIn</h1>
    <button @click="goToHome">Go to Home</button>
  </div>
</template>

<script>
export default {
  name: "SignUp",
  methods: {
    goToHome() {
      this.$router.push({ name: "Home" });
    },
  },
};
</script>
