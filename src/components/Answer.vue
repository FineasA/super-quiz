<template>
  <div class="alert alert-success text-center">
    <h1>Correct! Nice Job!</h1>
    <hr />
    <button class="btn btn-primary" @click="nextQuestion">Next Question</button>
  </div>
</template>

<script>
import { EventBus } from "../main.js";

export default {
  methods: {
    nextQuestion() {
      EventBus.$emit("confirmed");
      this.$emit("confirmed");
    }
  }
};
</script>

<style>
</style>