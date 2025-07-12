<script setup>
import { ref, onMounted } from 'vue'; // ref와 onMounted 훅 임포트

defineProps({
  msg: {
    type: String,
    required: true
  }
});

// 새로운 상태 변수 선언
const backendMessage = ref('Loading message from backend...');
const errorMessage = ref('');

// 컴포넌트가 마운트될 때 API 호출
onMounted(async () => {
  try {
    // 여기에 Node.js 백엔드의 URL을 입력하세요!
    // 예: const response = await fetch('https://my-program-path-backend-nodejs.onrender.com/');
    const backendUrl = 'https://program-path-backend-nodejs.onrender.com/'; // !!! 여기에 당신의 실제 백엔드 URL을 붙여넣으세요 !!!

    const response = await fetch(backendUrl);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const text = await response.text(); // 텍스트로 응답 받기
    backendMessage.value = text;
  } catch (error) {
    console.error('Failed to fetch from backend:', error);
    errorMessage.value = `Error fetching from backend: ${error.message}`;
    backendMessage.value = 'Failed to load message from backend.';
  }
});
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      You’ve successfully created a project with
      <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>.
    </h3>
    <hr>
    <h2>Message from Node.js Backend:</h2>
    <p v-if="backendMessage">{{ backendMessage }}</p>
    <p v-if="errorMessage" style="color: red;">{{ errorMessage }}</p>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>