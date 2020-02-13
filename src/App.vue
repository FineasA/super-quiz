<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1 class="text-center">Super Quiz</h1>
      </div>
      <hr />
      <br /><br />
      <div class="row">
        <div
          class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3"
        >
          <transition name="slide-fade">
            <component
              :is="mode"
              @answered="answered($event)"
              @confirmed="mode = 'question'"
            ></component>
          </transition>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Question from "./components/Question.vue";
import Answer from "./components/Answer.vue";

export default {
  components: {
    Question,
    Answer
  },
  data() {
    return {
      mode: "question"
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
    }
  }
};
</script>

<style>
.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>
