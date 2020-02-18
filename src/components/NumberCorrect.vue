<template>
  <div class="row">
    <div class="col-sm">Number Correct: {{ numberCorrect }}</div>
    <div class="col-sm">Number Correct: {{ numberIncorrect }}</div>
  </div>
</template>

<script>
import { EventBus } from "../main.js";

export default {
  data() {
    return {
      numberCorrect: 0,
      numberIncorrect: 0
    };
  },
  created() {
    EventBus.$on("answered", data => {
      if (data) {
        this.numberCorrect++;
      } else {
        this.numberIncorrect++;
      }
    });
  }
};
</script>

<style>
</style>