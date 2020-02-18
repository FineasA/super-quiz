<template>
  <div class="alert alert-danger text-center">
    <h1>You lose! Try again?</h1>
    <hr />
    <button class="btn btn-primary" @click="reset">Reset</button>
  </div>
</template>

<script>
import { EventBus } from "../main.js";

export default {
  methods: {
    reset() {
      EventBus.$emit("reset", 5);
    }
  }
};
</script>

<style>
</style>