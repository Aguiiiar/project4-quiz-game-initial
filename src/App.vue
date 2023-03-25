<template>
  <div>
    <Scoreboard :winCount="winCount" :loseCount="loseCount" />
    <template v-if="question">
      <div>
        <h1 v-html="question"></h1>
      </div>
      <template v-for="(answer, index) in answers" :key="index">
        <input :disabled="answerSubmitted" type="radio" name="options" :value="answer" v-model="chosenAnswer">
        <label v-html="answer"></label><br>
      </template>

      <button class="send" type="button" @click="submitAnswer" v-if="!answerSubmitted">Send</button>

      <section class="result" v-if="answerSubmitted">
        <template v-if="chosenAnswer === correctAnswer">
          <h4 v-html="'&#9989; Congratulations, the answer ' + correctAnswer + 'is correct.'"></h4>
        </template>
        <template v-else>
          <h4 v-html="'&#10060; I`m sorry, you picked wrong answer. The correct   is ' + correctAnswer + '.'"></h4>
        </template>
        <button class="send" type="button" @click="getNewQuestion()">Next question</button>
      </section>
    </template>
  </div>
</template>

<script>
const api = "https://opentdb.com/api.php?amount=1&category=18"
import Scoreboard from './components/Scoreboard.vue';

export default {

  name: 'App',
  components: { Scoreboard },
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0,
    }
  },
  computed: {
    answers() {
      let answers = JSON.parse(JSON.stringify(this.incorrectAnswers));

      answers.splice(Math.round(Math.random() * answers.length),0,this.correctAnswer)
      return answers;
    }
  },
  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) {
        return alert('Pick one of the options');
      }
      this.answerSubmitted = true;

      if (this.chosenAnswer !== this.correctAnswer) {
        this.loseCount++
        return;
      }

      this.winCount++;
    },


    getNewQuestion() {

      this.answerSubmitted = false;
      this.chosenAnswer = undefined;
      this.question = undefined;

      this.axios.get(api).then((res) => {
        this.question = res.data.results[0].question;
        this.incorrectAnswers = res.data.results[0].incorrect_answers;
        this.correctAnswer = res.data.results[0].correct_answer;
      })
    }
  },
  created() {
    this.getNewQuestion();
  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type=radio] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
