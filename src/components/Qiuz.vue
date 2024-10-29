<script setup>
 
const prop = defineProps ({
    qiuz: {
        type : Object, 
        default : null 
    },
    count: {
        type : Number, 
        default : 0 
    }

})


const emit = defineEmits(['clickAnswer']) 

function clickAnswer(index) {
  emit('clickAnswer',{questionId:prop.qiuz.id,answerIndex:index}) // we pass the data to the person 
}


</script>

<template>
    <div id="Checkdata" class="min-w-o min-h-0 border-[1px] rounded-[2px] p-2 m-auto bg-gray-200 shadow-md shadow-slate-400 ">
        <div class="p-1 ml-4 mr-4" >
            <p class="text-amber-500 text-[17px] mb-2">Time Remaning: {{ count }}<span class="setTimer"></span></p>
            <p class="font-[12px] text-[30px] mb-3"><span>{{ qiuz?.id }}. </span>{{ qiuz?.question }} </p>
            <form class="option" >
                <div v-for="(answer,index) in qiuz?.answer" :key=" answer">
                    <input type="radio" :id="'q-'+index" name="answer" hidden class="peer"  >
                    <label  :for="'q-'+index"
                             class="block p-4 mt-3 border-2 border-transparent rounded cursor-pointer transition-colors duration-200ms hover:bg-[#BED7DC] hover:border-[#070F2B] peer-checked:bg-[#B6C7AA] peer-checked:border-[#5A639C]"
                             @click="clickAnswer(index)"
                             >{{ answer }}</label>
                </div>
            </form>     
                   
        </div>
        
    </div>

</template>

<style scoped>

</style>