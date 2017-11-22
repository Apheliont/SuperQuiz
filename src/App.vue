<template>
  <div class="wrapper">
    <h1>The Super Quiz</h1>
    <hr>
    <div class="quiz-container">
      <div class="quiz-box" ref="quiz-box">
      <transition name="myTransition" mode="out-in" appear>
        <div class="quiz-question" v-if="isShown" key="question">
          <app-quiz :min="min" :max="max" :maxAnswers="maxAnswers"></app-quiz>
        </div>
        <div class="quiz-response" v-if="!isShown" key="response">
          <app-correct-answer></app-correct-answer>
        </div>
      </transition>
      </div>
    </div>
  </div>
</template>

<script>
  import { appBus } from './main.js';
  import Quiz from './components/Quiz.vue';
  import CorrectAnswer from './components/CorrectAnswer.vue';
  export default {
    data() {
      return {
        isShown: true,
        min: 1,
        max: 100,
        maxAnswers: 4
      }
    },
    components: {
      appQuiz: Quiz,
      appCorrectAnswer: CorrectAnswer
    },
    created() {
      appBus.$on('nextQuestion', () => {
        this.isShown = true;
      });
      appBus.$on('rightAnswer', () => {
        this.isShown = false;
      });
      appBus.$on('wrongAnswer', () => {
        this.$refs['quiz-box'].classList.add('quiz-box--shake');
        setTimeout(() => {
          this.$refs['quiz-box'].classList.remove('quiz-box--shake');
        }, 200);
      });
    }
  }
</script>

<style>
  @keyframes shake {
    from {
      transform: translateX(-5px);
    }
    20% {
      transform: translateX(10px);
    }
    40% {
      transform: translateX(-15px);
    }
    60% {
      transform: translateX(20px);
    }
    80% {
      transform: translateX(-15px);
    }
    100% {
      transform: translateX(0);
    }
  }

  .wrapper {
    margin-top: 50px;
    text-align: center;
  }

  .quiz-container {
    max-width: 960px;
    margin: 40px auto;
    display: flex;
    flex-flow: row nowrap;
    justify-content: center;
  }

  .quiz-box {
    height: 180px;
    width: 500px;
    padding: 0;
  }

  .quiz-question {
    height: 100%;
    border-radius: 5px;
    border: 1px solid rgba(128, 128, 128, 0.44);
    padding: 0;
  }

  .quiz-response {
    height: 100%;
    border-radius: 5px;
    background-color: rgba(144, 238, 144, 0.25);
  }

  .quiz-box--shake {
    animation: shake 0.2s;
  }

  .myTransition-enter {
    transform: scaleX(0);
  }

  .myTransition-enter-active {
    transition: transform 0.5s;
    transition-timing-function: ease-in-out;
  }

  .myTransition-leave {

  }

  .myTransition-leave-active {
    transform: scaleX(0);
    transition: transform 0.5s;
    transition-timing-function: ease-in-out;
  }
</style>

