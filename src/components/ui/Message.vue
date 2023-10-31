<script setup>
import { faCheckDouble } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { inject } from 'vue';

const { userID } = inject("AppProvider");
const props = defineProps({
    message: Object
});

const isAuthor = () => props.message.authorID === userID.value;

const getTime = () => {
    const time = new Date(props.message.createdAt)
    return time.toLocaleDateString() === new Date().toLocaleDateString() ? time.toLocaleTimeString().slice(0, time.toLocaleTimeString().length - 3) : time.toLocaleDateString();
}

</script>

<template>
    <div class="chat" :class="{'chat-end': isAuthor(), 'chat-start': !isAuthor()}">
        <div class="chat-image avatar">
            <div class="w-10 rounded-full">
                <img src="https://picsum.photos/300/300" />
            </div>
        </div>
        <div class="chat-header">
            {{ message.author }}
            <time class="text-xs opacity-50">{{ getTime() }}</time>
        </div>
        <div class="chat-bubble" :class="{'author': isAuthor(), 'sender': !isAuthor()}">{{ message.content ?? "no message" }}</div>
    </div>
</template>

<style lang="scss" scoped>

.sender {
    background: var(--secondary);
}

.author {
    background: var(--info);
}

.chat-bubble {
    font-size: 1.2rem;
}

</style>