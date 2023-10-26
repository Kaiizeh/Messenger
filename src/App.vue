<script setup>
import { v4 as uuidv4 } from 'uuid';
import ConversationsList from './components/ConversationsList.vue';
import Conversation from './components/Conversation.vue';
import { onMounted, provide, ref } from 'vue';
import { io } from "socket.io-client";

const conversation = ref();
const userID = ref();
const socket = io("http://localhost:3000", {
  transports: ["websocket", "polling"],
});

provide("AppProvider", {
  userID,
  conversation,
  socket
});

onMounted(() => {
  userID.value = window.localStorage.getItem("userID");
  if (userID.value) {
    userID.value = uuidv4();
    window.localStorage.setItem("userID", userID.value);
  }
  
  socket.emit("joinRooms", userID.value);
});
</script>

  

<template>
  <div class="container-app">
    <ConversationsList />
    <Conversation title="Hello world" description="45 membres, 24 en ligne" room="DrzSA423F" />
  </div>
</template>

  

<style scoped>
.container-app {
  display: flex;
  height: 100vh;
  width: 100vw;
  padding: 1rem;
}
</style>