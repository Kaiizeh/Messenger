<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faEllipsisVertical, faMattressPillow, faMicrophone, faPaperclip, faPhone, faSearch } from '@fortawesome/free-solid-svg-icons';
import { faPaperPlane } from '@fortawesome/free-regular-svg-icons';
import Message from './ui/Message.vue';
import { inject, onMounted, ref } from 'vue';

const props = defineProps({
    title: String,
    description: String,
    room: String
});

const { userID, socket } = inject("AppProvider");

const messages = ref([]);
const messageContent = ref();

onMounted(() => {
    socket.emit('joinRoom', props.room);
    socket.on("receivedMessage", (message) => {
        messages.value.unshift(message);
        socket.emit("seeMessage", new Date(), props.room);
    });
});

const sendMessage = () => {
    const message = createMessagePayload()
    socket.emit("sendMessage", message, props.room);
    messages.value.unshift(message);
    messageContent.value = "";
}

const createMessagePayload = () => ({
    authorID: userID.value,
    author: "Kéziah THIBO",
    content: messageContent.value,
    createdAt: new Date()
})
</script>

  

<template>
    <div class="c__container">
        <div class="flex w-full justify-between">
            <div class="flex flex-col w-[70%]">
                <div class="c__title">{{ title }}</div>
                <div class="c__subtitle">{{ description }}</div>
            </div>
            <div class="c__actions">
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
            <Message :message="message" v-for="message in messages"/>
        </div>
        <div class="c__input-container">
            <button class="me-2">
                <FontAwesomeIcon :icon="faPaperclip" />
            </button>
            <div class="c__input">
                <input type="text" v-model="messageContent"/>
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
    width: 100%;
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