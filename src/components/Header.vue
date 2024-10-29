<script setup>
const emit = defineEmits(['startQuiz']) //
defineProps ({
    isSubmitQuiz:{
        type: Boolean,default:false
    },
    totalScore:{
        type: Number,default:0
    },
    isStartQuiz:{
        type: Boolean,default:false
    },
})

</script>

<template>
    <div class="text-center font-bold">
        <h1 class="text-green-700 text-[35px]">Geek For Geeks</h1>
        <h3 class="text-[25px]">Quiz App</h3>
    </div> 
    <div class="text-center mt-4 mb-2 ">
        
            <h1 v-if="isSubmitQuiz" class="text-[20px] mb-[2px] font-bold">{{'total Score:'+ totalScore}}</h1>
            <button v-if="!isStartQuiz" 
                    type="submit" 
                    class=" p-2 h-12 text-[17px] border mt-3 bg-[#E1F7F5] text-black rounded-lg focus:outline-none focus:shadow-outline hover:bg-[#9AC8CD]" 
                    @click="emit('startQuiz')" >{{ isSubmitQuiz ? 'Retest': 'Start Test' }}</button>
    </div>
  

</template>

<style scoped>

</style>
