<template>
  <div class="card">
    <div>
      <h3 class="card-title text-center" style="margin-top: 25px">
        {{ question }}
      </h3>
    </div>
    <div class="card-body">
      <div class="col text-center">
        <button
          class="btn btn-primary"
          style="margin: 10px"
          @click="answer(btnData[0].correct, (btnData[0].picked = true))"
          :style="[btnData[0].picked ? incorrectClass : null]"
        >
          {{ btnData[0].answer }}
        </button>
        <button
          class="btn btn-primary"
          style="margin: 10px"
          @click="answer(btnData[1].correct, (btnData[1].picked = true))"
          :style="[btnData[1].picked ? incorrectClass : null]"
        >
          {{ btnData[1].answer }}
        </button>
        <button
          class="btn btn-primary"
          style="margin: 10px"
          @click="answer(btnData[2].correct, (btnData[2].picked = true))"
          :style="[btnData[2].picked ? incorrectClass : null]"
        >
          {{ btnData[2].answer }}
        </button>
        <button
          class="btn btn-primary"
          style="margin: 10px"
          @click="answer(btnData[3].correct, (btnData[3].picked = true))"
          :style="[btnData[3].picked ? incorrectClass : null]"
        >
          {{ btnData[3].answer }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { EventBus } from "../main.js";

export default {
  data() {
    return {
      question: "Oops an error occurred",
      randomNum1: 0,
      randomNum2: 0,
      mode: null,
      incorrectChoicePicked: false,
      incorrectClass: {
        backgroundColor: "red"
      },
      correctAnswer: 0,
      btnData: [
        {
          correct: true,
          answer: 0,
          picked: false
        },
        {
          correct: false,
          answer: 0,
          picked: false
        },
        {
          correct: false,
          answer: 0,
          picked: false
        },
        {
          correct: false,
          answer: 0,
          picked: false
        }
      ]
    };
  },
  methods: {
    //randomly pick whether or not the problem will be addition or subtraction
    pickMode() {
      let random = Math.floor(Math.random() * 2);
      switch (random) {
        case 0:
          this.mode = "Add";
          break;
        case 1:
          this.mode = "Subtract";
          break;
        default:
          this.mode = "Add";
      }
    },
    //generate the question and button answers
    generateQuestion(mode) {
      let modeStr = "";
      console.log("mode: ", this.mode);
      //generate random numbers for expression
      this.randomNum1 = this.generateRandomNumbers(this.randomNum1);
      this.randomNum2 = this.generateRandomNumbers(this.randomNum2);

      //string determination between addition and subtraction
      if (mode === "Add") {
        modeStr = "+";
        this.correctAnswer = this.additionCase(
          this.randomNum1,
          this.randomNum2
        );
      } else if (mode === "Subtract") {
        modeStr = "-";
        this.correctAnswer = this.subtractionCase(
          this.randomNum1,
          this.randomNum2
        );
      }
      //generate answers that are close to the actual number
      this.generateTrickyAnswers(this.correctAnswer);

      //assign the correct answer to the answer property within the object
      this.btnData[0].answer = this.correctAnswer;

      //shuffle the data so the actual answer is not on the same button
      this.shuffleData(this.btnData);
      console.log(this.btnData);

      //return the string that displays what the problem is
      return (this.question = `What is ${this.randomNum1} ${modeStr} ${this.randomNum2}?`);
    },
    //generate Random number function between 1 and 100
    generateRandomNumbers(number) {
      number = Math.floor(Math.random() * (100 - 1) + 1);
      return number;
    },
    answer(isCorrect, picked) {
      if (picked && !isCorrect) {
        this.incorrectChoicePicked = true;
      }
      EventBus.$emit("answered", isCorrect);
      this.$emit("answered", isCorrect);
    },
    //addition function
    additionCase(a, b) {
      return a + b;
    },
    //subtraction function
    subtractionCase(a, b) {
      return a - b;
    },
    //generate tricky answers based on the correct answer
    generateTrickyAnswers(correctAnswer) {
      this.btnData[1].answer = correctAnswer + this.generateBetween1And10();
      this.btnData[2].answer = correctAnswer - this.generateBetween1And10();
      this.btnData[3].answer =
        correctAnswer +
        this.generateBetween1And10() +
        this.generateBetween1And10();
      console.log(
        this.correctAnswer,
        this.btnData[1].answer,
        this.btnData[2].answer,
        this.btnData[3].answer
      );
    },
    //added this function just for some extra variety within the tricky answers function
    generateBetween1And10() {
      return Math.floor(Math.random() * (10 - 1) + 1);
    },
    //shuffle the data of the array for randomness of button answers
    shuffleData(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }
  },
  //upon creation of the vue instance, a mode will be picked at random and then a question will be generated based on
  //the mode that is picked
  created() {
    this.pickMode();
    this.generateQuestion(this.mode);
  }
};
</script>

<style>
</style>