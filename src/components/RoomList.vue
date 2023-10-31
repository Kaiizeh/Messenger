<script setup>
import { faEdit, faSearch } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { v4 as uuidv4 } from 'uuid';
import ConversationCard from './ui/ConversationCard.vue';
import { computed, inject, onMounted, ref } from 'vue';

const { socket, currentRoom, isCreateRoom, user } = inject("AppProvider");

const rooms = ref([]);
const notifications = ref([]);
const searchRoom = ref("");

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

const wantCreateRoom = () => {
    currentRoom.value = {
        name: null,
        id: uuidv4(),
        messages: [],
        createdAt: new Date(),
        lastUpdate: new Date()
    };
    isCreateRoom.value = true;
    socket.emit("getUsers");
};

const select = (room) => {
    if (currentRoom.value?.id) {
        socket.emit("leaveRoom", currentRoom.value.id);
    }
    if (isCreateRoom.value) isCreateRoom.value = false;
    currentRoom.value = room;
}

const hasNotification = (target) => {
    for (const room of user.value.rooms) {
        if (target.id === room.id) {
            return new Date(room.lastCheck) < new Date(target.lastUpdate) && target.messages[0].author !== user.value.id;
        }
    }

    return false;
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
                :lastMessage="room?.messages[0]?.content ?? null" :hasNotification="hasNotification(room)" :isSaw="false"
                :isPin="false" :isSelected="currentRoom?.id == room.id" class="mt-2" />
        </div>
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