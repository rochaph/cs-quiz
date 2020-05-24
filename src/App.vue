<template>
  <div id="app">
    <Header :total="total" :numCorrect="numCorrect" />
    <b-container>
      <b-row>
        <b-col cols="12" lg="6" offset-lg="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :correct="correct"
            :reset="reset"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
export default {
  name: "App",
  components: { Header, QuestionBox },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      total: 0
    };
  },
  methods: {
    next() {
      if (this.index < 9) {
        this.index++;
      } else {
        fetch(
          "https://opentdb.com/api.php?amount=10&category=18&type=multiple",
          {
            method: "get"
          }
        )
          .then(response => response.json())
          .then(data => (this.questions = data.results))
          .then(() => (this.index = 0))
          .catch();
      }
    },
    correct(isCorrect) {
      if (isCorrect) this.numCorrect++;
      this.total++;
    },
    reset() {
      this.numCorrect = 0;
      this.total = 0;
      this.next();
    }
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=18&type=multiple", {
      method: "get"
    })
      .then(response => response.json())
      .then(data => (this.questions = data.results))
      .catch();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 2rem;
}
</style>
