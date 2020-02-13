<template>
  <div class="alert alert-success text-center">
    <h1>Correct! Nice Job!</h1>
    <hr />
    <button class="btn btn-primary" @click="nextQuestion">Next Question</button>
  </div>
</template>

<script>
export default {
  methods: {
    nextQuestion() {
      this.$emit("confirmed");
    }
  }
};
</script>

<style>
</style>