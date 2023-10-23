<script setup>
import { faSearch } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import ConversationCard from './ui/ConversationCard.vue';
</script>

  

<template>
    <div class="cl__container">
        <div class="input-research">
            <div class="input-icon">
                <FontAwesomeIcon :icon="faSearch" />
            </div>
            <input type="text" placeholder="Recherche"/>
        </div>
        <div class="mt-3">
            <ConversationCard roomName="Office chat" lastUpdate="4 m" lastMessage="Salut! Ca va ?" :notificationNumber="5" :isSaw="false" :isPin="false"/>
        </div>
    </div>
</template>

  

<style scoped>
.cl__container {
    height: 100%;
    padding: 1.25rem;
    width: 25rem;
}
.input-research {
    height: 3rem;
    width: 90%;
    background: var(--secondary);
    border-radius: .7rem;
    margin: auto;
    display: flex;
    overflow: hidden;
}

.input-research > input {
    background: inherit;
    width: 16.5rem;
}

.input-research > input:focus {
    outline: none;
}

.input-research > input::placeholder, .input-icon {
    color: var(--gray-100);
}

.input-icon {
    height: 3rem;
    aspect-ratio: 1/1;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}

.input-icon:hover {
    background: var(--info);
    transition: .3s ease-in-out;
    color: var(--white);
}

</style>