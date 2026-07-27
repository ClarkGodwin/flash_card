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

const progressPercentage = computed(() => Math.round(((activeQuestionId.value + 1) / Questions.length) * 100));

</script>

<template>
    <div class="*:w-fluid-card-width *:rounded-2xl *:mx-auto *:mb-2">
        <!-- Container de la barre (Fond gris/sombre) -->
        <div>
            <div class="bg-surface-200 h-2.5 overflow-hidden my-4">
                <!-- Remplissage dynamique de la barre -->
                <div class="bg-frosted h-2.5 rounded-full transition-all duration-300 ease-out"
                    :style="{ width: `${progressPercentage}%` }"></div>
            </div>
            
            <!-- Optionnel : texte affichant l'avmx-auto ancement -->
            <div class="text-[10px] text-frosted text-right mt-1">
                {{ activeQuestionId + 1 }} / {{ Questions.length }} ({{ progressPercentage }}%)
            </div>
        </div>

        <div
            class="  p-3 *:rounded-2xl border border-frosted *:px-[1em]  *:m-1 *:bg-surface-200">

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
                <button :disabled="isFirstQuestion || isLastQuestion" @click="switchDisplayQuestionAnswer">{{
                    switchQuestionAnswer }}</button>
                <button :disabled="isLastQuestion" @click="nextQuestion" class="flex items-center">
                    <span>Next</span>
                    <ChevronRight class=" text-frosted  size-(--text-value-nav)" />
                </button>
            </div>
        </div>
    </div>
</template>
