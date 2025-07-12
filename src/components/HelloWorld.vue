<template>
  <div>
    <h1>{{ props.msg }}</h1> <p>Backend Message: {{ backendMessage }}</p>
    <p v-if="errorMessage" style="color: red;">Error: {{ errorMessage }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted, defineProps } from 'vue'; // defineProps는 script setup에서 import할 필요 없음.

const props = defineProps({
  msg: String
});

const backendMessage = ref('Loading message from backend...');
const errorMessage = ref('');

onMounted(async () => {
  try {
    // !!! 로컬에서 실행 중인 Node.js 백엔드의 URL로 변경합니다 !!!
    const backendUrl = 'http://localhost:3000/'; // Node.js 백엔드가 3000번 포트에서 실행된다고 가정

    const response = await fetch(backendUrl);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const text = await response.text();
    backendMessage.value = text;
  } catch (error) {
    console.error('Failed to fetch from backend:', error);
    errorMessage.value = `Error fetching from backend: ${error.message}`;
    backendMessage.value = 'Failed to load message from backend.';
  }
});
</script>