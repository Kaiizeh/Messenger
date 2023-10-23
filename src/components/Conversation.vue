<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faEllipsisVertical, faMattressPillow, faMicrophone, faPaperclip, faPhone, faSearch } from '@fortawesome/free-solid-svg-icons';

defineProps({
    title: String,
    description: String

})
</script>

  

<template>
    <div class="w-[100%] h-[100%]">
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
        <div class="c__input">
            <button>
                <FontAwesomeIcon :icon="faPaperclip" />
            </button>
            <input type="text"/>
            <button>
                <FontAwesomeIcon :icon="faMicrophone" />
            </button>
        </div>
    </div>
</template>

  

<style scoped>

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

.c__input {
    width: 100%;
}

.c__input > input {
     width: 80%;
     background: inherit;
     padding: .5rem;
}
</style>