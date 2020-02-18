<template>
  <div class="row text-center d-flex justify-content-center">
    <h5>
      Number Correct
      <p style="color: green">({{ numberCorrect }})</p>
    </h5>
    <p>- - -</p>
    <h5>
      Number Incorrect
      <p style="color: red">({{ numberIncorrect }})</p>
    </h5>
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