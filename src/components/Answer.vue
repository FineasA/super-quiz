<template>
  <transition name="bounce">
    <div class="alert alert-success text-center">
      <h1>Correct! Nice Job!</h1>
      <hr />
      <button class="btn btn-primary" @click="nextQuestion">
        Next Question
      </button>
    </div>
  </transition>
</template>

<script>
import { EventBus } from "../main.js";

export default {
  methods: {
    nextQuestion() {
      this.playSound(
        "http://soundbible.com/mp3/Elevator Ding-SoundBible.com-685385892.mp3"
      );
      EventBus.$emit("confirmed");
      this.$emit("confirmed");
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
.bounce-enter-active {
  animation: bounce-in 0.5s;
}
.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
</style>