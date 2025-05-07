<template>
  <div class="chat-panel">
    <!-- 这里可以集成聊天面板内容，如消息列表和输入框 -->
    <div class="messages" ref="messagesContainer">
      <template v-for="msg in messages" :key="msg.time + '-' + msg.text">
        <div v-if="msg.showTime" class="msg-time">{{ msg.time }}</div>
        <div
          class="message"
          :class="msg.isSelf ? 'self' : 'other'"
        >
          
          <div v-if="!msg.isSelf" style="display: flex;">
            <img  class="avatar" :src="msg.avatar"/>
            <div class="bubble_right">
                <div class="nickname" v-if="!msg.isSelf">{{ msg.nickname }}</div>
                <div class="text">{{ msg.text }}</div>
            </div>
          </div>
          <div v-if="msg.isSelf" style="display: flex;">
            <div class="bubble_right">
                <div class="nickname" v-if="!msg.isSelf">{{ msg.nickname }}</div>
                <div class="text">{{ msg.text }}</div>
            </div>
            <img  class="avatar" :src="msg.avatar" />
          </div>
        </div>
      </template>
    </div>
    <div class="input-area">
      <input v-model="input" @keyup.enter="sendMessage" placeholder="输入消息..." />
      <button @click="sendMessage">发送</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, nextTick } from 'vue'
const messages = ref([
  // 示例初始消息
  { text: '欢迎来到私人影院~😺', isSelf: false, avatar: 'http://localhost:3000/SmellyCat2.png', nickname: '系统', time: "", showTime: true }
])
const input = ref('')
const messagesContainer = ref<HTMLElement | null>(null)
function scrollToBottom() {
  nextTick(() => {
    if (messagesContainer.value) {
      messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight
    }
  })
}
function sendMessage() {
  if (input.value.trim()) {
    const now = new Date()
    const timeStr = now.getHours().toString().padStart(2, '0') + ':' + now.getMinutes().toString().padStart(2, '0')
    let showTime = false
    if (messages.value.length === 0 || messages.value[messages.value.length-1].time !== timeStr) showTime = true
    messages.value.push({ text: input.value, isSelf: true, avatar: 'http://localhost:3000/SmellyCat.png', nickname: '我', time: timeStr, showTime })
    scrollToBottom()
    setTimeout(() => {
      messages.value.push({ text: '对方回复: ' + input.value, isSelf: false, avatar: 'http://localhost:3000/SmellyCat2.png', nickname: '小明', time: timeStr, showTime: false })
      scrollToBottom()
    }, 600)
    input.value = ''
  }
}
</script>

<style scoped>
.chat-panel {
  width: 97%;
  height: 97%;
  background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
  display: flex;
  flex-direction: column;
  border-radius: 18px;
  box-shadow: 0 4px 24px 0 rgba(31, 38, 135, 0.10);
  overflow: hidden;
  max-width: 720px;
  margin: auto auto;

}
.messages {
  flex: 1;
  overflow-y: auto;
  padding: 18px 16px 8px 16px;
  background: transparent;
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.msg-time {
  text-align: center;
  color: #a0aec0;
  font-size: 13px;
  margin: 18px 0 8px 0;
  letter-spacing: 1px;
}
.message {
  display: flex;
  align-items: flex-end;
  margin-bottom: 8px;
  gap: 8px;
}
.message.self {
  flex-direction: row-reverse;
  align-self: flex-end;
}
.message.self .avatar {
  margin-left: 12px;
  margin-right: 0;
  box-shadow: 0 4px 16px rgba(59,130,246,0.18);
  border: 2.5px solid #3b82f6;
  background: #fff;
  align-self: flex-end;
}

.message.other .avatar {
  margin-right: 12px;
  margin-left: 0;
  box-shadow: 0 2px 8px rgba(31,38,135,0.08);
  border: 2px solid #e0e7ef;
  background: #fff;
  align-self: flex-end;
}
.bubble {
  max-width: 71%;
  min-width: 36px;
  padding: 14px 22px 12px 22px;
  border-radius:  8px 22px  22px   22px ;
  background: linear-gradient(90deg, #e3e9f7 60%, #cfd8dc 100%);
  color: #222;
  font-size: 15px;
  box-shadow: 0 4px 18px 0 rgba(31,38,135,0.10);
  position: relative;
  word-break: break-all;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border: 1.5px solid #e2e8f0;
  transition: background 0.2s, box-shadow 0.2s;
}
.message.self .bubble {
  background: linear-gradient(90deg, #60a5fa 0%, #38bdf8 100%);
  color: #fff;
  border-radius:  1px 22px  22px  22px  ;
  align-items: flex-end;
  border: 2px solid #38bdf8;
  box-shadow: 0 6px 24px 0 rgba(59,130,246,0.18);
}
.message.other .bubble {
  background: linear-gradient(90deg, #f1f5f9 60%, #e0e7ef 100%);
  color: #222;
  border: 1.5px solid #cbd5e1;
  box-shadow: 0 2px 12px 0 rgba(31,38,135,0.06);
}


.bubble_right {
  max-width: 71%;
  min-width: 36px;
  padding: 14px 22px 12px 22px;
  border-radius: 8px 22px 22px 22px;
  background: linear-gradient(90deg, #e3e9f7 60%, #cfd8dc 100%);
  color: #222;
  font-size: 15px;
  box-shadow: 0 4px 18px 0 rgba(31,38,135,0.10);
  position: relative;
  word-break: break-all;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border: 1.5px solid #e2e8f0;
  transition: background 0.2s, box-shadow 0.2s;
}
.message.self .bubble_right {
  background: linear-gradient(90deg, #60a5fa 0%, #38bdf8 100%);
  color: #fff;
  border-radius: 22px 22px 8px 22px;
  align-items: flex-end;
  border: 2px solid #38bdf8;
  box-shadow: 0 6px 24px 0 rgba(59,130,246,0.18);
}
.message.other .bubble_right {
  background: linear-gradient(90deg, #f1f5f9 60%, #e0e7ef 100%);
  color: #222;
  border: 1.5px solid #cbd5e1;
  box-shadow: 0 2px 12px 0 rgba(31,38,135,0.06);
}


.nickname {
  font-size: 13px;
  color: #38bdf8;
  margin-bottom: 2px;
}
.text {
  font-size: 15px;
  line-height: 1.7;
}
.input-area {
  display: flex;
  padding: 14px 16px;
  border-top: 1px solid #e2e8f0;
  background: #f8fafc;
  align-items: center;
}
.input-area input {
  flex: 1;
  padding: 10px 14px;
  border: 1.5px solid #cbd5e1;
  border-radius: 8px;
  margin-right: 10px;
  font-size: 15px;
  outline: none;
  transition: border 0.2s;
  background: #fff;
  color: #222;
}
.input-area input:focus {
  border: 1.5px solid #60a5fa;
}
.input-area button {
  padding: 8px 22px;
  border: none;
  background: linear-gradient(90deg, #38bdf8 60%, #6366f1 100%);
  color: #fff;
  border-radius: 8px;
  cursor: pointer;
  font-size: 15px;
  font-weight: 500;
  box-shadow: 0 2px 8px rgba(59,130,246,0.08);
  transition: background 0.2s, box-shadow 0.2s;
}
.input-area button:hover {
  background: linear-gradient(90deg, #6366f1 60%, #38bdf8 100%);
  box-shadow: 0 4px 16px rgba(59,130,246,0.18);
}
.avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  object-fit: cover;
  box-shadow: 0 2px 8px rgba(31,38,135,0.10);
  transition: box-shadow 0.2s;
}



@media (max-width: 600px) {
  .chat-panel {
    border-radius: 0;
    box-shadow: none;
    padding: 0;
    min-width: 0;
    max-width: 100%;
    height: 100%;
  }
  .messages {
    padding: 8px 2vw 64px 2vw;
    gap: 1px;
    max-height: calc(100vh - 64px);
  }
  .msg-time {
    font-size: 11px;
    margin: 10px 0 4px 0;
  }
  .message {
    margin-bottom: 4px;
    gap: 4px;
    flex-wrap: wrap;
    width: 100%;
  }
  .bubble, .bubble_right {
    max-width: 92vw;
    min-width: 20vw;
    padding: 8px 4vw 8px 4vw;
    font-size: 13px;
    border-radius: 14px 14px 14px 6px;
    word-break: break-word;
  }
  .message.self .bubble, .message.self .bubble_right {
    border-radius: 14px 6px 14px 14px;
  }
  .message.other .bubble, .message.other .bubble_right {
    border-radius: 14px 14px 6px 14px;
  }
  .nickname {
    font-size: 11px;
    margin-bottom: 1px;
    word-break: break-all;
  }
  .text {
    font-size: 13px;
    line-height: 1.5;
    word-break: break-word;
  }
  .input-area {
    padding: 8px 2vw;
    flex-direction: row;
    gap: 4px;
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100vw;
    z-index: 10;
    box-sizing: border-box;
    background: #f8fafc;
  }
  .input-area input {
    padding: 8px 8px;
    font-size: 13px;
    border-radius: 6px;
    margin-right: 6px;
    min-width: 0;
  }
  .input-area button {
    padding: 6px 12px;
    font-size: 13px;
    border-radius: 6px;
  }
  .avatar {
    width: 24px;
    height: 24px;
    min-width: 24px;
    min-height: 24px;
  }
  .bubble_right {
    align-items: flex-start;
    padding: 8px 4vw 8px 4vw;
  }
  .message.self .bubble_right {
    align-items: flex-end;
  }
}
</style>
