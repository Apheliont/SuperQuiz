<template>
    <div class="container">
      <h3 class="question">What's {{firstNumber}} {{sign}} {{secondNumber}}?</h3>
      <ul class="answers">
        <li class="answers__item" v-for="(answer, index) in answers" @click="checkAnswer(index)">{{answer}}</li>
      </ul>
    </div>
</template>

<script>
import { appBus } from '../main.js';
export default {
  props: ['min', 'max', 'maxAnswers', 'isShown'],
  data() {
    return {
      firstNumber: '',
      secondNumber: '',
      sign: '',
      answers: []
    }
  },
  methods: {
    makeRandomElement({min = 1, max = 100, sign = false} = {}) {
      const randNum = Math.max(Math.floor(Math.random() * (max + 1)), min);
      if (sign) {
        const borderNum = Math.floor((max - min) / 2);
        return randNum > min + borderNum ? '-' : '+';
      }
      return randNum;
    },
    makeAnswers() {
      this.answers = [];
      const answerIndex = this.makeRandomElement({min: 0, max: this.maxAnswers - 1});
      console.log(this.rightAnswer);
      console.log(answerIndex);
      for (let counter = 0; counter < this.maxAnswers; counter++) {
        if (counter === answerIndex) {
          this.answers.push(this.rightAnswer);
          continue;
        }
        this.answers.push(this.makeRandomElement({min: this.min, max: this.max}));
      }
    },
    newGame() {
      this.firstNumber = this.makeRandomElement({min: this.min, max: this.max});
      this.secondNumber = this.makeRandomElement({min: this.min, max: this.max});
      this.sign = this.makeRandomElement({min: this.min, max: this.max, sign: true});
      this.makeAnswers();
      console.log(this.answers);
    },
    checkAnswer(index) {
      if (this.answers[index] === this.rightAnswer) {
        appBus.$emit('rightAnswer');
      } else {
        appBus.$emit('wrongAnswer');
      }
    }
  },
  computed: {
    rightAnswer() {
      return this.sign === '-' ? this.firstNumber - this.secondNumber : this.firstNumber + this.secondNumber;
    }
  },
  created() {
    this.newGame();
    appBus.$on('nextQuestion', this.newGame);
  }
}
</script>

<style scoped>
  * {
    box-sizing: border-box;
  }

  .container {
    display: flex;
    flex-flow: column nowrap;
    width: 100%;
    height: 100%;
    padding: 0;
  }

  .question {
    margin: 0;
    padding: 10px 0;
    border-bottom: 1px solid rgba(128, 128, 128, 0.44);
  }

  .answers {
    height: 100%;
    list-style: none;
    padding: 10px;
    display: flex;
    flex-flow: row wrap;
    justify-content: space-around;
    align-items: center;
  }

  .answers__item {
    flex-basis: 15%;
    padding: 10px 20px;
    border: 1px solid rgba(128, 128, 128, 0.44);
    border-radius: 5px;
    cursor: pointer;
    background-color: rgb(112, 197, 237);
    color: white;
  }
</style>
