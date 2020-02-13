<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1 class="text-center">Super Quiz</h1>
      </div>
      <hr />
      <div class="row">
        <div
          class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3"
        >
          <component
            :is="mode"
            @answered="answered($event)"
            @confirmed="mode = 'question'"
          ></component>
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
</style>
