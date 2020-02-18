<template>
  <div class="alert alert-danger text-center" name="fade">
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
.fade-enter-active {
  transition: all 1s ease;
}
.fade-leave-active {
  transition: all 1s ease;
}
.slide-fade-enter,
.slide-fade-leave-to .slide-fade-leave-active {
  opacity: 0;
}
</style>