<template>
  <div>
    <ScoreBoard :winCount="winCount" :loseCount="loseCount" />

    <h1 v-html="question"></h1>

    <template v-for="answer in answers" :key="answer">
      <input
        :disabled="answerSubmitted"
        type="radio" 
        name="options" 
        :value="answer"
        v-model="chosenAnswer"
      />

      <label v-html="answer"></label><br />
    </template>

    <button v-if="!answerSubmitted" @click="submitAnswer" class="send" type="button">Send</button>
  
    <section v-if="answerSubmitted">
      <h4 v-if="chosenAnswer == correctAnswer"
        v-html='`&#9989; Congratulations, the answer "`+ correctAnswer + `" is correct.`'>
      </h4>

      <h4 v-else 
        v-html="`&#10060; I'm sorry, you picked the wrong answer. The correct is &quot` + correctAnswer + `&quot.`">
      </h4>

      <button @click="getNewQuestion">Next question</button>
    </section>
  
  </div>
</template>

<script lang="ts">
import ScoreBoard from './components/ScoreBoard.vue'

export default {

  name: "App",
  components: {
    ScoreBoard
  },

  data () {
    return {
      question: undefined,
      incorrectAnswers: [],
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0
    }
  },
  computed: {
    answers() {
      var answers = [...this.incorrectAnswers];
      if(this.correctAnswer) {
        answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
        return answers;
      }
    }
  },
  methods: {
    submitAnswer() {
      if(!this.chosenAnswer) {
        alert("Pick one of the options");
      } else {
        this.answerSubmitted = true;
        if (this.chosenAnswer == this.correctAnswer){
          this.winCount++
        } else {
          this.loseCount++
        }
      }
    },

    getNewQuestion() {

      this.answerSubmitted = false;
      this.chosenAnswer = undefined;
      this.question = undefined;

      this.axios
      .get("https://opentdb.com/api.php?amount=10&category=18")
      .then((response: any) => {
        this.question = response.data.results[0].question;
        this.incorrectAnswers = response.data.results[0].incorrect_answers;
        this.correctAnswer = response.data.results[0].correct_answer;
        console.log(response.data.results)
      })
    }
  },
  created() {
    this.getNewQuestion()
  }
}
</script>


<style lang="scss">
body {
  background-color: #222;
}

#app {
  font-family: Arial, Helvetica, sans-serif;
  text-align: center;
  color: white;
  margin: 60px auto;
  max-width: 960px;

  input {
    margin: 10px 5px;
  }

  button {
    font-size: 16px;
    margin-top: 10px;
    background-color: blue;
    color: white;
    padding: 15px 80px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
}
</style>