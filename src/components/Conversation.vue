<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faEllipsisVertical, faMattressPillow, faMicrophone, faPaperclip, faPhone, faSearch } from '@fortawesome/free-solid-svg-icons';
import { faPaperPlane } from '@fortawesome/free-regular-svg-icons';

defineProps({
    title: String,
    description: String

})
</script>

  

<template>
    <div class="c__container">
        <div class="flex w-full">
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
            Hello
        </div>
        <div class="c__input-container">
            <button class="me-2">
                <FontAwesomeIcon :icon="faPaperclip" />
            </button>
            <div class="c__input">
                <input type="text" class=""/>
                <button>
                    <FontAwesomeIcon :icon="faPaperPlane" />
                </button>
            </div>
            <button class="ms-2">
                <FontAwesomeIcon :icon="faMicrophone" />
            </button>
        </div>
    </div>
</template>

  

<style scoped>
.c__container {
    width: 100%;
    height: 100%;
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
    width: 30%;
    display: flex;
    justify-content: space-between;
    padding: 0 1rem;
    color: var(--gray-200);
}

.c__input-container {
    width: 100%;
    display: flex;
    justify-content: space-between;
}

.c__input-container > button {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 5%;
    aspect-ratio: 1/1;
    border-radius: .75rem;
}

.c__input-container > button:hover {
    background: var(--secondary);
}

.c__input {
    width: 100%;
    border-radius: .75rem;
    background: var(--secondary);
    overflow: hidden;
    display: flex;
}

.c__input > input {
     width: 95%;
     background: inherit;
     padding: .5rem;
     transition: .3s;
}

.c__input > input:hover {
    background: var(--gray-200);
}

.c__input > input:focus {
    outline: unset;
}

.c__input > button {
    width: 5%;
    aspect-ratio: 1/1;
    display: flex;
    justify-content: center;
    align-items: center;
}

.c__input > button:hover {
    background: var(--info);
}

.c__input > button:active {
    transition: .1s;
    background: var(--info-focus);
}

.c__messages {
    width: 100%;
    height: 85%;
    display: flex;
    flex-direction: column-reverse;
    padding: .5rem;
}
</style>