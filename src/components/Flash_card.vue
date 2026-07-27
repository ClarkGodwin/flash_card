<script setup lang="ts">
import { ref } from 'vue';
import Questions from  '@/data.json'

let  activeQuestionId = ref(1); //id of  the actual question displayed
let  activeQuestionIdValue = activeQuestionId.value - 1;
let  activeQuestion = Questions[activeQuestionIdValue]!['question']; // the actual question
let  activeAnswer = Questions[activeQuestionIdValue]!['answer']; // the actual answer
let  displayedText =  activeQuestion //the text displayed on the card

function nextQuestion(){
    activeQuestionId.value++
}

</script>

<template>
    <div class=" *:w-fluid-card-width  *:rounded-2xl *:border *:border-frosted *:mx-auto *:px-[1em]  *:grid *:place-items-center *:mt-6 ">
        <div class="h-[clamp(12.5rem,10.648rem+9.259vw,18.75rem)] bg-surface-300 text-text-main text-[clamp(0.875rem,0.764rem+0.556vw,1.25rem)] text-center">
            {{ displayedText }}
        </div>
    
        <div class="h-[clamp(2.5rem,2.315rem+0.926vw,3.125rem)]">
            test
        </div>
    </div>
</template>
