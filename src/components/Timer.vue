<template>
  <div class="col text-center">
    <start-game v-show="!gameStarted"></start-game>
    <br /><br />
    <button
      v-if="!gameStarted"
      @click="gameStartedFn(gameStarted)"
      class="btn btn-primary"
    >
      Begin Quiz
    </button>

    <h3
      v-if="gameStarted"
      class="row text-center d-flex justify-content-center"
    >
      {{ countdown }} seconds remaining...
    </h3>
    <hr />
    <number-correct v-if="gameStarted"></number-correct>
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
import NumberCorrect from "../components/NumberCorrect.vue";
import { EventBus } from "../main";

export default {
  components: {
    StartGame,
    NumberCorrect
  },
  data() {
    return {
      countdown: 5,
      gameStarted: false,
      gameLost: false,
      width: 100,
      answeredCorrect: false
    };
  },

  methods: {
    timer() {
      let timer = setInterval(() => {
        this.playSound("https://soundbible.com/grab.php?id=2044&type=mp3");
        if (this.answeredCorrect) {
          this.countdown = 5;
          this.width = 100;
          clearInterval(timer);
          return (this.answeredCorrect = false);
        }
        if (this.countdown === 0) {
          clearInterval(timer);
          this.playSound("https://soundbible.com/grab.php?id=499&type=mp3");
          this.width = 100;
          this.gameLost = true;
          this.$emit("game-lost");
          return;
        }
        this.width -= 20;
        this.countdown--;
      }, 1000);
    },
    playSound(sound) {
      if (sound) {
        let audio = new Audio(sound);
        audio.play();
      }
    },
    gameStartedFn(gameStarted) {
      this.playSound(
        "http://soundbible.com/mp3/Air Plane Ding-SoundBible.com-496729130.mp3"
      );
      gameStarted = !gameStarted;
      this.gameStarted = !this.gameStarted;
      this.$emit("started");

      if (gameStarted) {
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
        this.answeredCorrect = true;
        this.width = 100;
        this.countdown = 5;
      }
    });
    EventBus.$on("confirmed", () => {
      this.timer();
    });
  }
};
</script>

<style>
</style>