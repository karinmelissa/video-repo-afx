<template>
  <main>
    <input-video :getItems="getItems"/>
    <video-container :videos="videos" :getItems="getItems"/>
  </main>
</template>

<script setup lang="ts">
import InputVideo from './components/InputVideo.vue';
import VideoContainer from './components/VideoContainer.vue'
import axios from "axios";
import { ref, onMounted } from "vue";
const apiAWs = 'https://e5if1zx2z8.execute-api.us-east-1.amazonaws.com/test/items';
const videos = ref([]);

const getItems = async () => {
  try {
    const response = await axios.get(apiAWs);
    videos.value = response.data;
  } catch (error) {
    console.error('Error al obtener datos de la API:', error);
  }
};

onMounted(() => {
  getItems();
});
</script>

