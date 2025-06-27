<script setup>
import { ref } from 'vue'
import QuestionCard from './components/QuestionCard.vue'


document.title = 'quiz_app';

const startQuiz = ref(false);
</script>


<template>
<div v-if="!startQuiz" class="flex flex-col items-center  m-auto justify-center  h-screen  ">
<div class=" text-center p-6 text-1xl shadow-2xl p-4">
  <p><b class="text-4xl">Welcome to the Quiz App!</b></p><p> This quiz will test your knowledge across various topics. You will be presented with a series of questions, each followed by multiple-choice options.</p>
  <p>Your task is to select the correct answer for each question.</p>

  <h3 class="mt-4 font-bold text-xl">How It Works:</h3>
  <ul class="list-inside list-disc">
    <li>You will be shown a question along with four options.</li>
    <li>Choose the option you think is correct by clicking on it.</li>
    <li>Continue this process until you've completed the quiz.</li>
  </ul>

  <h3 class="mt-4 font-bold text-xl">Time & Scoring:</h3>
  <ul class="list-inside list-disc">
    <li>Time Limit: There is a 60-second time limit for each question. If you run out of time, your score will be subtracted.</li>
    <li>Score: Your score will be calculated based on the number of correct answers you provide.</li>
  </ul>

  <h3 class="mt-4 font-bold text-xl">Clicking Next</h3>
  <ul class="list-inside list-disc">
    <li>This skips the current question and you will not be able to go back.</li>
    <li>Scores are not affected by skipping a question.</li>
  </ul>
</div>
  <div class=" text-4xl block px-5 py-3 bg-purple-800/70 rounded-sm hover:cursor-pointer" @click="startQuiz = !startQuiz">
    start
  </div>
</div>
  <div v-else class="flex flex-col items-center bg-blue-200 m-auto justify-center  h-screen ">
    <QuestionCard />
  </div>
</template>
