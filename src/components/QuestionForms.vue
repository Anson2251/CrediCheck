<script setup lang="ts">
import questionair from '@/assets/questionaire.json';
import { onMounted, ref } from 'vue';

import {
    NCard,
    NRadioGroup,
    NRadioButton
} from "naive-ui";

console.log(questionair);

const score = ref(0);

function updateScore() {
    score.value = answers.value.reduce((a, b) => a + b || 0, 0);
    console.log(score.value);
    (window as any).questionairScore = score.value;
}

interface Option {
    label: string;
    mark: number;
}

interface Question {
    question: string;
    options: Option[];
}

const question = ref<Question[]>(questionair);
const answers = ref(new Array(questionair.length).fill(null));

onMounted(() => {
    updateScore();
    document.body.addEventListener('click', updateScore);
})
</script>

<template>
    <n-card v-for="(q, index) in question" :key="index" :title="q.question"
        :content-style="`overflow: auto; margin: 16px`" :style="`margin-top: 16px`">
        <n-radio-group v-model:value="answers[index]" :name="`question-${index}`">
            <n-radio-button v-for="(option, optionIndex) in q.options" :key="`question${index}-option${optionIndex}`"
                :value="option.mark" :label="option.label" />
        </n-radio-group>
    </n-card>
    <n-card title="Result">
        {{ `Score: ${score}` }}
    </n-card>
</template>

<style></style>
