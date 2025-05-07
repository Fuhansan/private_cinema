<template>
  <div class="main-layout">
    <div class="video-section">
      <VideoPlayer />
    </div>
    <div class="chat-section">
      <ChatPanel />
    </div>
  </div>
</template>

<script setup lang="ts">
import VideoPlayer from './VideoPlayer.vue'
import ChatPanel from './ChatPanel.vue'
</script>

<style scoped>
.main-layout {
  display: flex;
  height: 97vh;
  width: 97vw;
  overflow: hidden;
}
.video-section {
  flex: 2;
  background: #111;
  display: flex;
  align-items: center;
  justify-content: center;
}
.chat-section {
  flex: 1;
  background: #f5f5f5;
  display: flex;
  flex-direction: column;
}
@media (max-width: 600px) {
  .main-layout {
    flex-direction: column;
    height: 100vh;
    width: 100vw;
  }
  .video-section {
    flex: none;
    height: 45vh;
    min-height: 180px;
    max-height: 60vh;
    width: 100vw;
    border-radius: 0;
    box-shadow: none;
  }
  .chat-section {
    flex: 1;
    height: 100%;
    min-height: 50%;
    width: 100vw;
    border-radius: 0;
    box-shadow: none;
  }
}
</style>
