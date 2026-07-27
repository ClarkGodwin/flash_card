<script setup lang="ts">
import { ref, computed } from 'vue';
import { ChevronRight, ChevronLeft } from '@lucide/vue';
import Questions from '@/data.json';

let activeQuestionId = ref(0);
let showAnswer = ref(false);

const isFirstQuestion = computed(() => activeQuestionId.value === 0 ? true : false);
const isLastQuestion = computed(() => activeQuestionId.value === Questions.length - 1 ? true : false);

// The question and answer update automatically based on the ID
const activeQuestion = computed(() => Questions[activeQuestionId.value]!.question);
const activeAnswer = computed(() => Questions[activeQuestionId.value]!.answer);

// The displayed text adapts automatically depending on the button state
const displayText = computed(() => showAnswer.value ? activeAnswer.value : activeQuestion.value);
const switchQuestionAnswer = computed(() => showAnswer.value ? 'Show Question' : 'Show Answer');

function nextQuestion() {
  activeQuestionId.value++;
  showAnswer.value = false; // Reset to the question for the next step
}
function previousQuestion() {
  activeQuestionId.value--;
  showAnswer.value = false; // Reset to the question for the previous step
}

function switchDisplayQuestionAnswer() {
  showAnswer.value = !showAnswer.value;
}

</script>

<template>
    <div
        class=" *:w-fluid-card-width  *:rounded-2xl *:border *:border-frosted *:mx-auto *:px-[1em]  *:mt-6 *:bg-surface-100">
        <div
            class="h-[clamp(12.5rem,10.648rem+9.259vw,18.75rem)] text-text-main text-[clamp(0.875rem,0.764rem+0.556vw,1.25rem)] text-center grid place-items-center ">
            {{ displayText }}
        </div>

        <div
            class="h-[clamp(2.5rem,2.315rem+0.926vw,3.125rem)] flex justify-between items-center text-value-nav *:hover:text-frosted *:hover:cursor-pointer *:disabled:text-text-muted *:disabled:cursor-not-allowed">
            <button @click="previousQuestion" :disabled="isFirstQuestion" class="flex items-center">
                <ChevronLeft class="text-frosted size-(--text-value-nav)" />
                <span>Previous</span>
            </button>
            <button :disabled="isFirstQuestion || isLastQuestion" @click="switchDisplayQuestionAnswer" >{{ switchQuestionAnswer }}</button>
            <button :disabled="isLastQuestion" @click="nextQuestion" class="flex items-center">
                <span>Next</span>
                <ChevronRight class=" text-frosted  size-(--text-value-nav)" />
            </button>
        </div>
    </div>
</template>
