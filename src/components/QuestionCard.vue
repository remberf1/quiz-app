<script setup>
import { onMounted, ref } from 'vue'
 const letters = ['A', 'B', 'C', 'D']
import questions from '../data/questionbank.json'
const score = ref(0)
const currentQuestionIndex = ref(0)
const selectedAnswer = ref(null)
const quizCompleted = ref(false)
// console.log(questions)
function nextQuestion() {
  if(currentQuestionIndex.value < questions.questions.length - 1) {
    currentQuestionIndex.value++
  }
}

function displayCorrectanswer() {
  // console.log(questions.questions[currentQuestionIndex.value].answer)

}

  function checkAnswer(answer,index) {
    selectedAnswer.value = index
    console.log(selectedAnswer)
    if(answer === questions.questions[currentQuestionIndex.value].answer) {
      score.value++
      // console.log('correct')
    }else {
      // console.log('incorrect')
      displayCorrectanswer()
    }
  }
</script>

<template>
<div class="w-[1000px] bg-white/55 p-2 rounded-md">
  <div>
    <h1 class=" text-2xl shadow-md px-4 py-2 inline-block ">
      Score : {{ score }}
    </h1>
  </div>
  <!--question title -->
  <div  class="mb-4">
    <h2  class=" text-center">
      {{ questions.questions[currentQuestionIndex].question }}
    </h2>
  </div>

  <div class="flex flex-col ">
    <div v-for="(option, optionIndex) in questions.questions[currentQuestionIndex].options "
     :key="optionIndex"
     class="mb-2 bg-purple-600/65 rounded-xl px-4 py-2">
      <p class="items-center align-center text-white">
        <span  class="bg-blue-600 rounded-3xl w-[60px] px-4 py-2  text-white mr-4 hover:cursor-pointer"
          @click="checkAnswer(option,optionIndex)">
          {{ letters[optionIndex] }}</span>
        {{ option }}
      </p>
    </div>
  </div>


  <div class="flex justify-between mt-2 items-center">
    <div class=" inline-block  shadow-md px-4 py-2 ml-3">
      {{ currentQuestionIndex + 1 }} of {{ questions.questions.length }} of questions.
    </div>
    <div class=" inline-block  shadow-md px-4 py-2 mr-3 bg-purple-500 hover:cursor-pointer rounded-sm" @click="nextQuestion()">
      Next
    </div>
  </div>
</div>
</template>
