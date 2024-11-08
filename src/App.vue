<script setup lang="ts">
import { RouterView, useRoute } from "vue-router";
import Overlay from "./components/Overlay.vue";
import { useAppStore } from "./stores/app";
import { storeToRefs } from "pinia";
import Chatbox from "./frontend/components/chatbox/Chatbox.vue";
import { ref, watch } from "vue";

const appStore = useAppStore();

const route = useRoute();

watch(
  () => route.fullPath,
  () => {
    console.log(route.fullPath);

    if (route.fullPath.split("/").includes("admin"))
      isHiddenChatBox.value = true;
  }
);

const { isShowOverlay } = storeToRefs(appStore);

const isShowBox = ref(false);

const isHiddenChatBox = ref(false);

const handleShowChat = () => {
  isShowBox.value = true;
};

const handleClick = () => {
  isShowBox.value = false;
};
</script>

<template>
  <RouterView />
  <Overlay v-if="isShowOverlay" />

  <div v-if="!isHiddenChatBox" class="chat-box">
    <i
      v-if="!isShowBox"
      @click="handleShowChat"
      style="font-size: 30px; color: #6b87e9; cursor: pointer"
      class="pi pi-discord"
    ></i>
    <Chatbox @click="handleClick" v-if="isShowBox" />
  </div>
</template>

<style scoped lang="scss">
.chat-box {
  position: sticky;
  left: 0;
  bottom: 10px;
  margin: 20px;

  .pi-discord {
    &:hover {
      cursor: pointer;
      transform: scale(1.3);
      transition: all 0.3s ease;
    }
  }
}

.app-container {
  display: flex;

  .main {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 1;
    height: 100vh;
    overflow-y: auto;
  }
}

.web-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
