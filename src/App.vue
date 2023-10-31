<script setup>
import { v4 as uuidv4 } from 'uuid';
import RoomList from './components/RoomList.vue';
import Conversation from './components/Conversation.vue';
import { onMounted, provide, ref } from 'vue';
import { io } from "socket.io-client";
import Modal from './components/ui/Modal.vue';

const currentRoom = ref();
const user = ref({});
const needName = ref(false);
const isCreateRoom = ref(false);
const socket = io("http://localhost:3000", {
  transports: ["websocket", "polling"],
});

provide("AppProvider", {
  user,
  currentRoom,
  isCreateRoom,
  socket
});

onMounted(() => {
  user.value.id = window.localStorage.getItem("userID");
  if (!user.value.id) {
    user.value.id = uuidv4();
    window.localStorage.setItem("userID", user.value.id);
  }

  socket.emit("initialization", user.value.id);

  socket.on("getUser", (pUser) => {
    if (!pUser) {
      needName.value = true;
      return;
    }
    user.value = pUser;
  });
});

const handleConfirm = () => {
  if (username.value.length < 3) return;
  const user = {
    name: user.value.name,
    id: user.value.id,
    rooms: []
  };
  socket.emit("createUser", user);
  needName.value = false;
}
</script>

  

<template>
  <div class="container-app">
    <RoomList />
    <Conversation :room="currentRoom" v-if="currentRoom || isCreateRoom" />


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