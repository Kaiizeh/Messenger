<script setup>
import { faEdit, faSearch } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { v4 as uuidv4 } from 'uuid';
import ConversationCard from './ui/ConversationCard.vue';
import { computed, inject, onMounted, ref } from 'vue';
import Modal from './ui/Modal.vue';

const { socket, userID, username, currentRoom } = inject("AppProvider");

const rooms = ref([]);
const searchRoom = ref("");
const isCreateRoom = ref(false);

const users = ref();
const searchUser = ref("");
const usersSelected = ref([]);

onMounted(() => {
    socket.on("getRooms", (rRooms) => {
        rooms.value = [...rRooms];
    });
    socket.on("newRoom", (newRoom) => {
        rooms.value.push(newRoom);
    });
    socket.on("notification", (newMessage, roomID) => {
        for (let index in rooms.value) {
            if (rooms.value[index].id === roomID) {
                rooms.value[index].messages.unshift(newMessage);
            }
        }
    });
});

const roomFiltred = computed(() => {
    if (searchRoom.value.length < 3) return rooms.value;
    return rooms.value.filter((room) => room.name.indexOf(searchRoom.value) !== -1);
});

const userFiltred = computed(() => {
    if (searchUser.value.length < 3) return null;
    return users.value.filter((user) => user.name.indexOf(searchUser.value) !== -1);
});

const wantCreateRoom = () => {
    socket.emit("getUsers");
    socket.on("getUsers", (rUsers) => users.value = rUsers);
    isCreateRoom.value = true;
};

const handleCancel = () => {
    isCreateRoom.value = false;
};

const addUser = (user) => {
    usersSelected.value.push({ id: user.id, name: user.name });
    searchUser.value = user.name;
};

const createRoom = () => {
    usersSelected.value.push({ id: userID.value, name: username.value });
    const newRoom = {
        name: "Conversation",
        id: uuidv4(),
        messages: [],
        createdAt: new Date(),
        lastUpdate: new Date()
    };
    socket.emit("createRoom", newRoom, usersSelected.value);
    rooms.value.push(newRoom);
    isCreateRoom.value = false;
}

const select = (room) => {
    if (currentRoom.value?.id) {
        socket.emit("leaveRoom", currentRoom.value.id);
    }
    currentRoom.value = room;
}
</script>

  

<template>
    <div class="cl__container">
        <div class="w-full flex justify-between items-center text-2xl mb-2 px-2 font-bold">
            <span>
                Discussions
            </span>
            <button class="btn btn-md btn-circle btn-ghost hover:btn-accent text-base" @click="wantCreateRoom">
                <FontAwesomeIcon :icon="faEdit" />
            </button>
        </div>
        <div class="input-research">
            <div class="input-icon">
                <FontAwesomeIcon :icon="faSearch" />
            </div>
            <input type="text" placeholder="Recherche..." v-model="searchRoom" />
        </div>
        <div class="cl__list">
            <ConversationCard v-for="room in roomFiltred" @click="select(room)" :roomName="room?.name"
                :lastUpdate="new Date(room?.lastUpdate).toLocaleDateString()"
                :lastMessage="room?.messages[0]?.content ?? null" :notificationNumber="0" :isSaw="false" :isPin="false"
                :isSelected="currentRoom?.id == room.id" class="mt-2" />
        </div>


        <Modal title="Nouvelle discussion" v-if="isCreateRoom" :handleConfirm="createRoom" :handleCancel="handleCancel">
            <div>
                <div class="form-control w-full max-w-xs mt-2 relative">
                    <input type="text" placeholder="Envoyer un message à ..."
                        class="input input-bordered w-full max-w-xs bg-slate-200" v-model="searchUser" />
                    <ul class="dropdown-content z-[1] menu p-2 shadow bg-slate-300 rounded-box w-52" v-if="userFiltred">
                        <li v-for="user in userFiltred" @click="addUser(user)"
                            class="cursor-pointer hover:bg-slate-400 p-2 rounded-lg">
                            {{ user.name }}
                        </li>
                    </ul>
                </div>
            </div>
        </Modal>
    </div>
</template>

  

<style lang="scss" scoped>
.cl__container {
    height: 100%;
    width: 20rem;
    margin-right: 1rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.input-research {
    height: 3rem;
    width: 90%;
    background: var(--secondary);
    border-radius: .7rem;
    margin: auto;
    display: flex;
    overflow: hidden;

    input {
        background: inherit;
        width: calc(100% - 3rem);

        &:focus {
            outline: none;
        }

        &::placeholder {
            color: var(--gray-100);
        }
    }
}

.input-icon {
    color: var(--gray-100);
    height: 3rem;
    width: 3rem;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;

    &:hover {
        background: var(--info);
        transition: .3s ease-in-out;
        color: var(--white);
    }
}

.cl__list {
    height: calc(100% - 5rem);
    overflow-y: auto;
    margin: .5rem 0;
}
</style>