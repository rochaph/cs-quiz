<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        <span v-html="currentQuestion.question"></span>
      </template>

      <hr class="my-4" />

      <b-list-group class="mb-4">
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
          :disabled="answered"
        >
          <span v-html="answer"></span>
        </b-list-group-item>
      </b-list-group>

      <b-button
        @click="submitAnswers"
        class="mr-4"
        variant="primary"
        :disabled="selectedIndex=== null || answered"
      >Submit</b-button>
      <b-button class="mr-4" @click="next" variant="success">Next</b-button>
      <b-button @click="reset" variant="danger">Reset</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    correct: Function,
    reset: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      answered: false
    };
  },
  methods: {
    selectAnswer: function(index) {
      this.selectedIndex = index;
    },
    submitAnswers: function() {
      let isCorrect = false;
      this.selectedIndex === this.correctIndex ? (isCorrect = true) : isCorrect;
      this.correct(isCorrect);
      this.answered = true;
    },
    answerClass: function(index) {
      let answerClass = "";
      !this.answered && this.selectedIndex === index
        ? (answerClass = "selected")
        : this.answered && this.correctIndex === index
        ? (answerClass = "correct")
        : this.answered &&
          this.selectedIndex === index &&
          this.correctIndex !== index
        ? (answerClass = "incorrect")
        : answerClass;
      return answerClass;
    }
  },
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      answers = _.shuffle(answers);
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.correctIndex = this.answers.indexOf(
          this.currentQuestion.correct_answer
        );
        this.answered = false;
        this.selectedIndex = null;
      }
    }
  }
};
</script>

<style scoped>
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}
.list-group-item {
  color: black !important;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen !important;
}
.incorrect {
  background-color: lightcoral !important;
}
</style>