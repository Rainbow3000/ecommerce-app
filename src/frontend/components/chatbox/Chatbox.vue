<template>
  <div class="chatbox">
    <i @click="emit('click')" class="pi pi-times"></i>
    <div class="chatbox-messages" ref="messageBox">
      <div
        class="chat-message"
        v-for="(message, index) in messages"
        :key="index"
        :class="{ user: message.isUser }"
        :style="message.isUser && 'margin-left:200px'"
      >
        <span :style="message.isUser && 'display: flex; align-items: left'" class="message-text">{{ message.text }}</span>
      </div>
    </div>
    <div class="chatbox-input">
      <input
        v-model="input"
        type="text"
        placeholder="Nhập câu hỏi..."
        @keyup.enter="sendMessage"
      />
      <button @click="sendMessage">Gửi</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from "vue";

const emit = defineEmits(["click"]);

// Danh sách tin nhắn mẫu
const messages = ref([
  { text: "Xin chào! Bạn muốn hỏi gì về sản phẩm?", isUser: false },
  { text: "Bên cửa hàng có còn sản phẩm không?", isUser: true },
  { text: "Bên mình hiện vẫn đang còn hàng.", isUser: false },
]);

// Giá trị nhập liệu
const input = ref("");

// Gửi tin nhắn
const sendMessage = () => {
  if (input.value.trim() === "") return;
  messages.value.push({ text: input.value, isUser: true });

  // Giả lập trả lời tự động
  setTimeout(() => {
    messages.value.push({
      text: "Cảm ơn bạn! Chúng tôi sẽ trả lời sớm nhất.",
      isUser: false,
    });
    scrollToBottom();
  }, 1000);

  input.value = "";
  scrollToBottom();
};

// Cuộn xuống cuối danh sách tin nhắn
const messageBox = ref(null);
const scrollToBottom = () => {
  nextTick(() => {
    if (messageBox.value) {
      messageBox.value.scrollTop = messageBox.value.scrollHeight;
    }
  });
};

// Cuộn xuống cuối khi component được tải
onMounted(scrollToBottom);
</script>

<style scoped lang="scss">
.chatbox {
  display: flex;
  flex-direction: column;
  width: 400px;
  height: 500px;
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
  background-color: #f9f9f9;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  position: relative;

  .pi-times {
    position: absolute;
    right: 10px;
    top: 10px;
    cursor: pointer;
  }
}

.chatbox-messages {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
  background-color: #fff;
}

.chat-message {
  padding: 8px 12px;
  margin-bottom: 10px;
  border-radius: 12px;
  max-width: 70%;
}

.chat-message.user {
  background-color: #e0f7fa;
  align-self: flex-end;
  text-align: right;
}

.chat-message:not(.user) {
  background-color: #eceff1;
  align-self: flex-start;
}

.chat-message .message-text {
  font-size: 14px;
  color: #333;
}

.chatbox-input {
  display: flex;
  padding: 10px;
  border-top: 1px solid #ddd;
  background-color: #f7f7f7;
}

.chatbox-input input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
}

.chatbox-input button {
  margin-left: 10px;
  padding: 8px 12px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.chatbox-input button:hover {
  background-color: #0056b3;
}
</style>
