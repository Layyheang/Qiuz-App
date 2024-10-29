<script setup>
import Header from './components/Header.vue';
import Qiuz from './components/Qiuz.vue';
import { ref, onMounted} from 'vue';
import axios from 'axios';

// Variables to store quiz state and data
const totalQiuz = ref([]); // Variable to store questions
const crruIndexofQiuz = ref(0); // Variable to track the current question index, starting from 0
const quizClick= ref([]); // Variable to store user clicks on answers
const intervalId = ref(null); // Variable to set timer and check when countdown reaches 0 to move to next question
const daurationTime = 30; // Variable to set duration time for each question (30 seconds)
const counter = ref(daurationTime); // Variable to start counting duration time
const isStartQuiz = ref(false); // Variable to control visibility of the quiz form
const isSubmitQuiz = ref(false); // Variable to control the state when the user clicks on submit quiz
const totalScore = ref(0); // Variable to store the total score of the quiz

// Function to start the quiz
const startQuiz = () => { 
  crruIndexofQiuz.value = 0; // Reset to the first question
  isStartQuiz.value = true; // Show the quiz form
  isSubmitQuiz.value = false; 
  totalScore.value = 0;
  counter.value = daurationTime; // Reset the counter
  quizClick.value = [];
  
}

// Function to set up the timer
const setTimer = () => { 
  intervalId.value = setInterval(() => {
    if (counter.value !== 0) { // If counter is not 0, decrement it
      counter.value--; 
    } else { // If time is up, move to the next question
      nextQuiz();
    }
  }, 1000); // Timer counts down every second
}

// Function to clear and reset the timer
const clearTimer = () => { 
  if (intervalId.value) { // If there is an active interval, clear it
    clearInterval(intervalId.value);
    counter.value = daurationTime; // Reset the counter
    setTimer(); // Start the timer again
  }
}

// Function to fetch quiz data when the component is mounted
onMounted(async () => { 
  try {
    const res = await axios.get('http://localhost:3000/js-questions'); // Fetch data from API using axios
    totalQiuz.value = res.data.data; // Store fetched data in totalQiuz variable
    handleClickQuiz({ answerIndex: 1, questionId: 2 }); // Example call to handleClickQuiz with dummy data
    setTimer(); // Start the timer
  } catch (error) {
    console.log(error); // Log any errors
  }
})

// Function to handle user clicks on quiz answers
const handleClickQuiz = ({ questionId, answerIndex } = {}) => { 
  const index = quizClick.value.findIndex((quiz) => quiz.questionId === questionId); // Find index of the clicked question
  if (index === -1) { // If the question has not been answered yet
    quizClick.value.push({ questionId, answerIndex }); // Add the answer to quizClick array
  } else { // If the question has already been answered
    quizClick.value[index] = { questionId, answerIndex }; // Update the answer in quizClick array
  }
  console.log(quizClick.value);
}


// Function to move to the next question
const nextQuiz = () => { 
  if (crruIndexofQiuz.value < totalQiuz.value.length - 1) { // If not the last question
    clearTimer(); // Clear and reset the timer
    crruIndexofQiuz.value++; // Move to the next question
  }
}

// Function to check if the current question is the last one
const isLastQiuz = () => crruIndexofQiuz.value === totalQiuz.value.length - 1;

// Function to submit the quiz
const Submit = async () => {
  try {
    const response = await axios.post('http://localhost:3000/js-questions', quizClick.value); // Submit answers to the API
    console.log(quizClick.value); 
    if (response.status === 200) { // If submission is successful
      totalScore.value = response.data.data.score; // Store the score
      isStartQuiz.value = false; // Hide the quiz form
      isSubmitQuiz.value = true; // Show the submission state
    }
  } catch (error) {
    console.log(error); // Log any errors
  }
}
</script>

<template>
  <div class="flex flex-col border-[1px] rounded-[2px] p-2 border-1 shadow-md ml-[60px] mr-[60px] min-h-full">
    <Header @startQuiz="startQuiz" :isSubmitQuiz="isSubmitQuiz" :totalScore="totalScore" :isStartQuiz="isStartQuiz" />
    <template v-if="isStartQuiz">
      <Qiuz @clickAnswer="handleClickQuiz" :qiuz="totalQiuz[crruIndexofQiuz]" :count="counter"></Qiuz>
      <div class="mt-5 flex justify-between">
        <p>Question <span>{{ crruIndexofQiuz + 1 }}</span> of <span>{{ totalQiuz.length }}</span></p>
        <button v-if="!isLastQiuz()" type="submit" class="w-[30%] h-12 border-[1px] bg-[#7077A1] text-white rounded-lg focus:outline-none focus:shadow-outline hover:bg-[#424769]" @click="nextQuiz">Next Question</button>
        <button v-else type="submit" class="w-[30%] h-12 border-[1px] bg-[#7077A1] text-white rounded-lg focus:outline-none focus:shadow-outline hover:bg-[#424769]" @click="Submit">Submit</button>
      </div>
    </template>
  </div>
</template>

<style scoped>
</style> 
