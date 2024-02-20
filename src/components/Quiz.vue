<script setup>

import { ref } from 'vue'

const questions = [
  {
    question: "How many times Messi has won The Ballon d'Or trophy?",
    options: ["8", "15", "3", "6"],
    answer: "8",
  },
  {
    question: "How many times Messi has won the Champions league?",
    options: ["2", "5", "4", "1"],
    answer: "4",
  },
  // Add more questions as needed
];

const selectedAnswer = ref('')
const currentScore = ref(0)
const questionsIndex = ref(0)
const isCorrect = ref(false)
const showScore = ref(false)
const isShow = ref(false)
const isAnswerSubmitted = ref(false)

const showQuestion = ref(questions[questionsIndex.value].question)
const showOptions = ref(questions[questionsIndex.value].options)

const selectAnswer = (option) => {
  selectedAnswer.value = option
}

const checkAnswer = () => {
  isCorrect.value = questions[questionsIndex.value].answer === selectedAnswer.value;
  isCorrect.value === true 
    ? currentScore.value++ 
    : currentScore.value
};

const submit = () => {
  if(questionsIndex.value === 0) {
    console.log(questionsIndex.value)
    checkAnswer()
  } else {
    // console.log(questionsIndex.value)
    checkAnswer()
  }
  isShow.value = true
  isAnswerSubmitted.value = true
}

const nextQuestion = () => {
  if (questionsIndex.value < questions.length - 1) {
    questionsIndex.value++;
    showQuestion.value = questions[questionsIndex.value].question;
    showOptions.value = questions[questionsIndex.value].options;
    isAnswerSubmitted.value = false
    isShow.value = false
  } else {
    showScore.value = true
  }
};


</script>

<template>
  <main class="flex flex-col justify-center items-center p-40 h-screen">
    <h1 class="mb-20 text-2xl font-bold">Multiple Choice App</h1>
    <h3 class="mb-5 text-xl">{{ showQuestion }}</h3>
    <ul class="mb-5">
      <li v-for="(option, index) in showOptions" :key="index" 
        @click="isAnswerSubmitted ? null : selectAnswer(option)"
        :class="{ 'bg-slate-400': selectedAnswer === option }"
        class="hover:bg-slate-400 p-2 text-center w-36 rounded mb-1"
      >{{ option }}</li>
    </ul>
    <div>
      <button 
        @click="submit"
        :disabled="selectedAnswer === ''"
        v-show="!isShow"
        class="bg-violet-600 hover:bg-violet-500 rounded text-white font-bold p-3 pl-10 pr-10 mb-5 w-48"
      >
      Submit
      </button>
    </div>
    <div flex flex-row w-sreen>
      <button 
        @click="nextQuestion"
        class="bg-violet-600 hover:bg-violet-500 rounded text-white font-bold p-3 pl-10 pr-10 mb-5 w-48"
        v-show="isShow"
      >
      Next Question
      </button>
      <span v-show="isShow"
          :class="{ 'text-green-400': isCorrect, 'text-red-400': !isCorrect }"
          class="ml-2 font-bold"
          > {{ isCorrect ? 'Correct!' : 'Incorrect' }}</span>
    </div>
    <div v-show="showScore" class="absolute bg-violet-600 w-screen h-screen flex items-center justify-center">
      <p class="text-white text-2xl">Your Score is: {{ currentScore }} out of {{ questions.length }}</p>
    </div>
  </main>
</template>

<style>

li {
  cursor: pointer
}

/* li.cursor-not-allowed {
  opacity: 0.6;
  pointer-events: none;
} */

</style>
