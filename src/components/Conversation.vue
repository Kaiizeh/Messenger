<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faEllipsisVertical, faMattressPillow, faMicrophone, faPaperclip, faPhone, faSearch } from '@fortawesome/free-solid-svg-icons';
import { faPaperPlane } from '@fortawesome/free-regular-svg-icons';
import Message from './ui/Message.vue';
import { inject, onMounted, ref, watch, computed } from 'vue';

const props = defineProps({
    room: Object,
});

const { user, socket, isCreateRoom, currentRoom } = inject("AppProvider");

const messages = ref([]);
const messageContent = ref();

const searchUser = ref("");
const contact = ref();
const users = ref([]);

watch(props, () => {
    socket.emit("joinRoom", props.room.id, user.value.id);
    messages.value = props.room.messages;
});

socket.on("getUsers", pUsers => {
    users.value = pUsers
});

onMounted(() => {
    messages.value = props.room.messages;
    socket.emit("joinRoom", props.room.id, user.value.id);
    socket.on("receivedMessage", (message) => {
        messages.value.unshift(message);
        socket.emit("seeMessage", new Date(), props.room.id);
    });
});

const sendMessage = () => {
    if (isCreateRoom.value) {
        createRoom();
        isCreateRoom.value = false;
    } else {
        const message = createMessagePayload();
        socket.emit("sendMessage", message, props.room.id);
        messages.value.unshift(message);
    }
    messageContent.value = "";
}

const createMessagePayload = () => ({
    authorID: user.value.id,
    author: user.value.name,
    content: messageContent.value,
    createdAt: new Date()
});

const createRoom = () => {
    const owner = { id: user.value.id, name: user.value.name };
    currentRoom.value.messages.unshift(createMessagePayload());
    currentRoom.value.name = `${contact.value.name}, ${user.value.name}`;
    socket.emit("createRoom", currentRoom.value, [contact.value, owner]);
    
    contact.value = null;
}

const userFiltred = computed(() => {
    if (searchUser.value.length < 3) return null;
    return users.value.filter((user) => user.name.indexOf(searchUser.value) !== -1);
});


const chooseContact = (pUser) => {
    contact.value = pUser;
    searchUser.value = "";
}
</script>

  

<template>
    <div class="c__container">
        <div class="flex w-full justify-between">
            <div class="flex flex-col w-[70%]" v-if="!isCreateRoom">
                <div class="c__title">{{ room?.name }}</div>
                <div class="c__subtitle">{{ room?.membersInfo }}</div>
            </div>
            <div class="flex flex-col w-[70%] relative" v-else>
                <div>
                    <input type="text" placeholder="Envoyer un message à ..."
                        class="input input-bordered input-accent w-full max-w-xs" v-model="searchUser"
                        @keyup.enter="chooseContact(userFiltred[0])" v-if="!contact"/>
                        <div v-else class="text-2xl font-bold">{{ contact.name }}</div>
                    <ul class="dropdown-content z-[1] menu p-2 shadow bg-slate-700 rounded-box w-52" v-if="userFiltred">
                        <li v-for="user in userFiltred" @click="chooseContact(user)"
                            class="cursor-pointer hover:bg-slate-600 p-2 rounded-lg">
                            {{ user.name }}
                        </li>
                    </ul>
                </div>

            </div>
            <div class="c__actions" v-if="!isCreateRoom">
                <button class="btn btn-ghost text-2xl hover:text-white">
                    <FontAwesomeIcon :icon="faSearch" />
                </button>
                <button class="btn btn-ghost text-2xl hover:text-white">
                    <FontAwesomeIcon :icon="faPhone" />
                </button>
                <button class="btn btn-ghost text-2xl hover:text-white">
                    <FontAwesomeIcon :icon="faMattressPillow" />
                </button>
                <button class="btn btn-ghost text-2xl hover:text-white">
                    <FontAwesomeIcon :icon="faEllipsisVertical" />
                </button>
            </div>
        </div>
        <div class="c__messages">
            <Message :message="message" v-for="message in messages" />
        </div>
        <div class="c__input-container">
            <button class="me-2" @click="() => socket.emit('joinRooms')">
                <FontAwesomeIcon :icon="faPaperclip" />
            </button>
            <div class="c__input">
                <input type="text" v-model="messageContent" @keyup.enter="sendMessage"/>
                <button @click="sendMessage">
                    <FontAwesomeIcon :icon="faPaperPlane" />
                </button>
            </div>
            <button class="ms-2">
                <FontAwesomeIcon :icon="faMicrophone" />
            </button>
        </div>
    </div>
</template>

  

<style lang="scss" scoped>
.c__container {
    width: calc(100% - 20rem);
    height: calc(100vh - 2rem);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.c__title {
    font-size: 30px;
    font-weight: bold;
    letter-spacing: 0.05cap;
}

.c__subtitle {
    font-size: 18px;
    color: var(--gray-100);
}

.c__actions {
    width: 20rem;
    display: flex;
    justify-content: space-between;
    padding: 0 1rem;
    color: var(--gray-200);
}

.c__input-container {
    width: 100%;
    display: flex;
    justify-content: space-between;
    height: 3rem;

    button {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 5%;
        aspect-ratio: 1/1;
        max-height: 4rem;
        border-radius: .75rem;
        transition: .3s;

        &:hover {
            background: var(--secondary);
        }

        &:active {
            transition: .1s;
            background: var(--info);
        }
    }
}

.c__input {
    width: 100%;
    border-radius: .75rem;
    background: var(--secondary);
    overflow: hidden;
    display: flex;

    input {
        width: calc(100% - 4rem);
        background: inherit;
        transition: .3s;
        font-size: 1.2rem;
        padding: .25rem 1rem;

        &:hover {
            background: var(--gray-200);
        }

        &:focus {
            outline: unset;
        }
    }

    button {
        width: 4rem;
        aspect-ratio: 1/1;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 0 .75rem .75rem 0;

        &:hover {
            background: var(--info);
        }

        &:active {
            transition: .1s;
            background: var(--info-focus);
        }
    }

}

.c__messages {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column-reverse;
    padding: .5rem;
    overflow-y: auto;
}
</style>