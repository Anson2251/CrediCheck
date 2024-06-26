<script setup lang="ts">
import QuestionForms from '@/components/QuestionForms.vue';
import { darkTheme, NConfigProvider, NButton, NCard, NDrawer, NDrawerContent, NInput, NSpace } from 'naive-ui';
import { ref } from 'vue';

const score = ref(0);
const article = ref("");
const url = ref("");
const active = ref(false);

function genreateContent() {
  return `URL: ${url.value}\n\nARTICLE:${article.value}\n\nSCORE: ${(window as any).questionairScore}`
}

function getScore() {
  return (window as any).questionairScore;
}

function save() {
  const blob = new Blob([genreateContent()], { type: 'text/plain' });

  // Create a URL for the Blob
  const url = URL.createObjectURL(blob);

  // Create a link element
  const link = document.createElement('a');
  link.href = url;
  link.download = 'text_message.txt';

  // Trigger the download
  document.body.appendChild(link);
  link.click();

  // Clean up
  URL.revokeObjectURL(url);
  document.body.removeChild(link);

  active.value = false
}
</script>

<template>
  <n-config-provider :theme="darkTheme">
    <QuestionForms />
    <n-card>
      <n-button @click="active = true">Save Result</n-button>
    </n-card>
    <n-drawer v-model:show="active" :width="502" :placement="'right'">
      <n-drawer-content title="Save Result">
        <n-space vertical>
          <n-input v-model:value="url" type="text" placeholder="Link (URL)" />
          <n-input v-model:value="article" type="textarea" placeholder="" />
          <p> Score: {{ getScore() }}</p>
          <n-button type="primary" @click="save">Save</n-button>
        </n-space>
      </n-drawer-content>
    </n-drawer>
  </n-config-provider>
</template>

<style scoped></style>
