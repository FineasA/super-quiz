<template>
  <p>
    Number Correct: ({{ numberCorrect }}) -- Number Incorrect: ({{
      numberIncorrect
    }})
  </p>
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