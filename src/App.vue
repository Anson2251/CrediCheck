<script setup lang="ts">
import QuestionForms from '@/components/QuestionForms.vue';
import { darkTheme, NConfigProvider } from 'naive-ui';
</script>

<template>
  <n-config-provider :theme="darkTheme">
    <QuestionForms />
  </n-config-provider>
</template>

<style scoped></style>
