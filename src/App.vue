<template>
  <div class="container d-flex justify-content-center vertical-center">
    <div class="row">
      <div class="card" style="width: 25rem;">
        <div class="card-body">
          <h1 class="card-title text-center" style="color: #0068D9">
            Super Quiz
          </h1>
          <hr />
          <transition name="slide-fade">
            <component
              v-if="gameStarted"
              :is="mode"
              @answered="answered($event)"
              @confirmed="mode = 'question'"
            ></component>
          </transition>
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
    answered(isCorrect) {
      if (isCorrect) {
        this.mode = "answer";
      } else {
        this.mode = "question";
        alert("Wrong! Try again!");
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
  min-height: 100%; /* Fallback for browsers do NOT support vh unit */
  min-height: 100vh; /* These two lines are counted as one :-)       */

  display: flex;
  align-items: center;
}
.slide-fade-enter-active {
  transition: all 1s ease;
}
</style>
