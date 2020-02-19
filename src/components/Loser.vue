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
      this.playSound(
        "http://soundbible.com/mp3/Air Plane Ding-SoundBible.com-496729130.mp3"
      );
      EventBus.$emit("reset", 5);
    },
    playSound(sound) {
      if (sound) {
        let audio = new Audio(sound);
        audio.play();
      }
    }
  }
};
</script>

<style>
</style>