<template>
  <div class="container d-flex justify-content-center vertical-center">
    <div class="row">
      <div class="card" style="width: 25rem">
        <div class="card-body">
          <h1 class="card-title text-center" style="color: #0068D9">
            Super Quiz
          </h1>
          <hr />
          <component
            v-if="gameStarted"
            :is="mode"
            @answered="answered($event)"
            @confirmed="mode = 'question'"
          ></component>
        </div>

        <div class="card-body">
          <timer
            @started="gameStarted = !gameStarted"
            @game-lost="mode = 'loser'"
          ></timer>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import { EventBus } from "./main.js";
import StartGame from "./components/StartGame.vue";
import Question from "./components/Question.vue";
import Answer from "./components/Answer.vue";
import Timer from "./components/Timer.vue";
import Loser from "./components/Loser.vue";

export default {
  components: {
    Question,
    Answer,
    Timer,
    StartGame,
    Loser
  },
  data() {
    return {
      mode: "question",
      gameStarted: false
    };
  },
  methods: {
    playSound(sound) {
      if (sound) {
        let audio = new Audio(sound);
        audio.play();
      }
    },
    answered(isCorrect) {
      if (isCorrect) {
        this.playSound(
          "http://soundbible.com/mp3/Air Plane Ding-SoundBible.com-496729130.mp3"
        );
        this.mode = "answer";
      } else {
        this.mode = "question";
      }
    },
    gameStartedFn(event) {
      console.log("event");
      if (event) {
        this.gameStarted = !this.gameStarted;
      }
    },
    initialState() {
      console.log("in it");
      return {
        mode: "question",
        gameStarted: true
      };
    }
  },
  created() {
    EventBus.$on("reset", () => {
      console.log("testing");
      this.mode = "question";
    });
  }
};
</script>

<style>
.vertical-center {
  min-height: 100%;
  min-height: 100vh;

  display: flex;
  align-items: center;
}
</style>
