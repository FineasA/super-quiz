<template>
  <div class="col-xs-12 col-sm-6 text-center">
    <start-game v-show="!gameStarted"></start-game>
    <button
      v-if="!gameStarted"
      @click="gameStartedFn(gameStarted)"
      class="btn btn-primary"
    >
      Begin Quiz
    </button>

    <h3 v-if="gameStarted" class="row text-center panel-default">
      {{ countdown }} seconds remaining...
    </h3>
    <br />
    <br />
    <div class="progress">
      <div
        class="progress-bar progress-bar-striped progress-bar-animated"
        role="progressbar"
        aria-valuenow="75"
        aria-valuemin="0"
        aria-valuemax="100"
        :style="{ width: width + '%' }"
      ></div>
    </div>
  </div>
</template>

<script>
import StartGame from "../components/StartGame.vue";
import { EventBus } from "../main";

export default {
  components: {
    StartGame
  },
  data() {
    return {
      countdown: 5,
      gameStarted: false,
      gameLost: false,
      width: 100
    };
  },

  methods: {
    timer() {
      let timer = setInterval(() => {
        if (this.countdown === 0) {
          clearInterval(timer);
          this.width = 100;
          this.gameLost = true;
          this.$emit("game-lost");
          return;
        }
        this.width -= 20;
        this.countdown--;
      }, 1000);
    },
    gameStartedFn(gameStarted) {
      gameStarted = !gameStarted;
      this.gameStarted = !this.gameStarted;
      this.$emit("started");

      if (gameStarted) {
        console.log("yee");
        this.timer();
      }
    }
  },
  created() {
    EventBus.$on("reset", data => {
      this.countdown = data;
      this.timer();
    });
    EventBus.$on("answered", data => {
      if (data) {
        this.width = 100;
        this.countdown = 5;
      }
    });
  }
};
</script>

<style>
</style>