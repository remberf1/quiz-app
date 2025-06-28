<script setup>
import { ref } from 'vue';
import '../assets/questionCard.css';
import confetti from 'canvas-confetti';
import questions from '../data/questionbank.json';
import Timer from './Timer.vue';

const letters = ['A', 'B', 'C', 'D'];
const score = ref(0);
const currentQuestionIndex = ref(0);
const selectedAnswer = ref(null);
const quizCompleted = ref(false);
const displayResult = ref(false);
const timerRef = ref(null);
const scoreDecremented = ref(false);

// To track if an option has been clicked
const clicked = ref(false);

// Function to move to the next question
function nextQuestion() {
  if (currentQuestionIndex.value < questions.questions.length - 1) {
    currentQuestionIndex.value++;
    selectedAnswer.value = null; // Reset selected answer
    clicked.value = false; // Allow next question's answer selection

  }
  const options = document.querySelectorAll('.options');
  options.forEach((option) => {
    option.style.backgroundColor = '';
    option.classList.remove('disabled');
  });
    if(currentQuestionIndex.value === questions.questions.length - 1) {
        quizCompleted.value = true;
    }

    resetTimer();
    scoreDecremented.value = false;
}

// Display correct/incorrect answer
function displayAnswer(selectedIndex) {
  const correctAnswerText = questions.questions[currentQuestionIndex.value].answer;
  const options = document.querySelectorAll('.options');

  options.forEach((option, index) => {
    const optionText = questions.questions[currentQuestionIndex.value].options[index];

    // If the option matches the correct answer, set green
    if (optionText === correctAnswerText) {
      option.style.backgroundColor = 'green';
    }
    // If the option is the one selected, set red (if incorrect)
    else if (selectedIndex === index) {
      option.style.backgroundColor = 'red';
    }
  });

  // Disable further clicks after the answer is displayed
  clicked.value = true;
}

// Handle the answer selection
function checkAnswer(optionIndex) {
  if (clicked.value) return;

  selectedAnswer.value = optionIndex;
  const correctAnswerText = questions.questions[currentQuestionIndex.value].answer;
  const selectedOption = questions.questions[currentQuestionIndex.value].options[optionIndex];


  // If selected option matches correct answer, increment the score
  if (selectedOption === correctAnswerText) {
    score.value++;
    triggerConfetti();
  }

  disableOptions();

  displayAnswer(optionIndex);
}

function disableOptions() {
  const options = document.querySelectorAll('.options');
  options.forEach(option => {
    option.classList.add('disabled'); // Disable clicking on all options
  });
}

function triggerConfetti() {
  // Configuring the confetti with random bursts and direction
  confetti({
    ticks: 200,                    // Duration of the confetti
    particleCount: 100,            // Number of confetti pieces
    angle: 90,                     // Angle of the burst
    spread: 70,                    // Spread of the confetti
    origin: { x: 0.5, y: 0.5 },    // Origin of the confetti (center of the screen)
    colors: ['#ff0', '#0f0', '#f00'] // Colors for the confetti
  });
}
function decrementScore() {
  if (scoreDecremented.value) return; // Prevent further decrementing if already done
  score.value--;
  scoreDecremented.value = true;  // Mark that the score has been decremented
}

function resetTimer() {
  if (timerRef.value) {
    timerRef.value.resetTimer();
  }
}
</script>

<template >
<div v-if="!displayResult" class="w-[1000px] bg-white/55 p-2 rounded-md">
  <div class="flex justify-between">
    <h1 class=" text-2xl shadow-md px-4 py-2 inline-block ">
      Score : {{ score }}
    </h1>
    <div class="flex justify-center">
    <Timer @time-up="decrementScore" ref="timerRef"/>
  </div>
  </div>
  <!--question title -->
  <div  class="mb-4">
    <h2  class=" text-center text-3xl">
      {{ questions.questions[currentQuestionIndex].question }}
    </h2>
  </div>

  <div class="flex flex-col ml-4">
    <div v-for="(option, optionIndex) in questions.questions[currentQuestionIndex].options "
     :key="optionIndex">
      <p class="items-center align-center text-white">
        <span  class="bg-blue-600 rounded-3xl w-[60px] px-4 py-2  text-white mr-1"
          >
          {{ letters[optionIndex] }}</span>
        <span

  class="options"
  @click="()=>{checkAnswer(optionIndex)}"
>
  {{ option }}
</span>
      </p>
    </div>
  </div>


  <div class="flex justify-between mt-2 items-center">
    <div class=" inline-block  shadow-md px-4 py-2 ml-3">
      {{ currentQuestionIndex + 1 }} of {{ questions.questions.length }} of questions.
    </div>
    <div v-if="!quizCompleted" class=" inline-block  shadow-md px-4 py-2 mr-3 bg-purple-500 hover:cursor-pointer rounded-sm" @click="nextQuestion()">
      Next
    </div>
    <div v-else class=" inline-block  shadow-md px-4 py-2 mr-3 bg-purple-500 hover:cursor-pointer rounded-sm" @click="displayResult = true">
          Finish
    </div>
  </div>
</div>
<div v-else>
  <div class="flex flex-col items-center bg-blue-200 m-auto justify-center  h-screen ">
    <h1 class=" text-2xl shadow-md px-4 py-2 inline-block">
      Your score is : {{ score }}
    </h1>
  </div>
</div>
    </template>
