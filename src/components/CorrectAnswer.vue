<template>
    <div class="container">
      <h1 class="rightAnswer">That's right!</h1>
      <button class="next-question" @click="nextQuestion">Next Question</button>
    </div>
</template>

<script>
import { appBus } from '../main.js';
export default {
  methods: {
    nextQuestion() {
      appBus.$emit('nextQuestion');
    }
  }
}
</script>

<style scoped>
  * {
    box-sizing :border-box;
  }

  .container {
    padding: 5px 0;
  }

  .rightAnswer {
    padding: 15px;
    border-bottom: 1px solid rgba(128, 128, 128, 0.24);
    color: green;
  }

  .next-question {
    border: none;
    border-radius: 5px;
    padding: 10px 15px;
    background-color: rgb(112, 197, 237);
    color: white;
    outline: none;
    cursor: pointer;
  }
</style>
