<template>
  <div class="video-player">
    <!-- 这里可以集成视频播放器，比如video标签或第三方播放器组件 -->
    <video controls width="100%" height="auto">
      <source src="https://cdn.pixabay.com/video/2025/03/28/268290_large.mp4" type="video/mp4" />
      您的浏览器不支持 video 标签。
    </video>
  </div>
</template>

<script setup lang="ts">
// 这里可以添加视频播放相关的逻辑
</script>

<style scoped>
.video-player {
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #232526 0%, #414345 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.25);
  transition: box-shadow 0.3s;
}
.video-player:hover {
  box-shadow: 0 16px 48px 0 rgba(31, 38, 135, 0.35);
}
.video-player video {
  border-radius: 16px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.18);
  background: #000;
}
@media (max-width: 600px) {
  .video-player {
    border-radius: 0;
    box-shadow: none;
    height: 100%;
    min-height: 120px;
    max-height: 60vh;
    padding: 0;
  }
  .video-player video {
    border-radius: 0;
    width: 100vw;
    height: 100%;
    min-height: 120px;
    max-height: 60vh;
  }
}
</style>