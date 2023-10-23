<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faCheckDouble, faThumbTack } from '@fortawesome/free-solid-svg-icons';

defineProps({
    roomName: String,
    lastUpdate: String,
    lastMessage: String,
    notificationNumber: Number,
    isSaw: Boolean,
    isPin: Boolean
});

</script>

<template>
    <div class="cc__container">
        <div class="cc__image">
            <img src="https://picsum.photos/300/300"/>
        </div>
        <div class="cc__content">
            <div class="cc__title">
                <span>{{ roomName }}</span>
                <div>
                    <FontAwesomeIcon :icon="faCheckDouble" class="me-2"/>
                    <span>{{ lastUpdate }}</span>
                </div>
            </div>
            <div class="cc__description">
                <span class="cc__lastMessage">{{ lastMessage }}</span>
                <div class="flex justify-center items-center">
                    <div class="cc__notification"> {{ notificationNumber }}</div>
                    <FontAwesomeIcon :icon="faThumbTack" v-if="isPin" class="ms-2"/>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.cc__container {
    display: flex;
    background: var(--secondary);
    padding: 1rem;
    height: 6rem;
    width: 100%;
    border-radius: 1.5rem;
    cursor: pointer;
}

.cc__image {
    height: 4rem;
    width: 4rem;
    border-radius: 1rem;
    overflow: hidden;
    margin-right: 1rem;
}

.cc__image > img {
    object-fit:fill;
}

.cc__content {
    display: flex;
    flex-direction: column;
    width: calc(100% - 5rem);
}

.cc__title {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-bottom: .25rem;
    font-size: 18px;
}

.cc__title > div {
    font-size: 14px;
}

.cc__title > div > svg {
    color: var(--gray-200);
}

.cc__description {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.cc__lastMessage {
    color: var(--gray-200);
    font-size: 14px;
}

.cc__notification {
    width: 1.5rem;
    height: 1.5rem;
    border-radius: 100%;
    background: var(--info);
    color: var(--white);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 14px;
}
</style>