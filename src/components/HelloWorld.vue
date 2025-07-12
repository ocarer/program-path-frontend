<script setup>
import { ref, onMounted } from 'vue';

defineProps({
  msg: {
    type: String,
    required: true
  }
});

const backendMessage = ref('Loading message from backend...');
const errorMessage = ref('');

onMounted(async () => {
  try {
    // !!! 여기에 Render에서 복사한 당신의 실제 Node.js 백엔드 URL을 붙여넣으세요 !!!
    const backendUrl = 'https://program-path-backend-nodejs.onrender.com/';

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