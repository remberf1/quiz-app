<script setup>
import { ref, onMounted,defineEmits,defineExpose} from 'vue';
const time=ref(10 * 1000);
const Timer = ref(time.value);
const intervalId = ref(null)

const getFormattedTime = (time) => {
  const seconds = Math.floor((time / 1000) % 60).toString().padStart(2, "0");
  return `:${seconds}`;
}

const emit = defineEmits(["time-up"]);
const endTimeRef = ref(null);

onMounted(() => {
  startTimer();
});
function startTimer() {
 if (intervalId.value) {
    clearInterval(intervalId.value);
  }


  endTimeRef.value = Date.now() + Timer.value;

  intervalId.value = setInterval(() => {
    const remainingTime = endTimeRef.value - Date.now();

    // If time is up
    if (remainingTime <= 0) {
      clearInterval(intervalId);
      emit("time-up");
    } else {
      Timer.value = remainingTime;
    }
  }, 1000);
}

defineExpose({

resetTimer: () => {
  Timer.value =time.value;
  endTimeRef.value = Date.now() + Timer.value;
  startTimer();
}
})
// defineExpose({
//   resetTimer: () => {
//     Timer.value = 60 * 1000;
//     endTimeRef.value = Date.now() + Timer.value;
//     startTimer();
//   }
// });
</script>

<template>
  <div>
    <h1 class="text-2xl shadow-md px-4 py-2 inline-block">
      Time Left {{ getFormattedTime(Timer) }} <!-- Display formatted remaining time -->
    </h1>
  </div>
</template>
