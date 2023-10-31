<script setup>
import { v4 as uuidv4 } from 'uuid';
import RoomList from './components/RoomList.vue';
import Conversation from './components/Conversation.vue';
import { onMounted, provide, ref } from 'vue';
import { io } from "socket.io-client";
import Modal from './components/ui/Modal.vue';

const currentRoom = ref();
const rooms = ref();
const userID = ref();
const username = ref();
const needName = ref(false);
const socket = io("http://localhost:3000", {
  transports: ["websocket", "polling"],
});

provide("AppProvider", {
  userID,
  username,
  currentRoom,
  socket
});

onMounted(() => {
  userID.value = window.localStorage.getItem("userID");
  if (!userID.value) {
    userID.value = uuidv4();
    window.localStorage.setItem("userID", userID.value);
  }

  socket.emit("initialization", userID.value);

  socket.on("getUser", (user) => {
    if (!user) {
      needName.value = true;
      return;
    }
    userID.value = user.id;
    username.value = user.name;
  });
});

const handleConfirm = () => {
  if (username.value.length < 3) return;
  window.localStorage.setItem("username", username.value);
  const user = {
    name: username.value,
    id: userID.value,
    roomsID: []
  };
  socket.emit("createUser", user);
  needName.value = false;
}
</script>

  

<template>
  <div class="container-app">
    <RoomList :rooms="rooms" />
    <Conversation :room="currentRoom" v-if="currentRoom" />


    <Modal title="Définir votre nom." :handleConfirm="handleConfirm" :handleCancel="null" v-if="needName">
      <div class="form-control w-full max-w-xs mt-2">
        <input type="text" placeholder="Saisir votre nom ici..." class="input input-bordered w-full max-w-xs bg-slate-200"
          v-model="username" />
      </div>
    </Modal>
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