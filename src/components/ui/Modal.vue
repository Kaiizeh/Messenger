<script setup>
import { faClose } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';

defineProps({
    title: String,
    handleConfirm: Function,
    handleCancel: Function
});
</script>

<template>
    <div class="m__mask">
        <div class="m__container">
            <div class="m__header">
                <div>{{ title }}</div>
                <button v-if="handleCancel" @click="handleCancel">
                    <FontAwesomeIcon :icon="faClose" />
                </button>
            </div>
            <div class="m__body">
                <slot></slot>
            </div>
            <div class="m__footer">
                <button @click="handleConfirm">Valider</button>
                <button v-if="handleCancel" @click="handleCancel">Annuler</button>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.m__mask {
    position: absolute;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(5px);
    display: flex;
    justify-content: center;
    align-items: center;
}

.m__container {
    width: 30rem;
    max-height: 20rem;
    background: var(--white);
    border-radius: 10px;
    padding: 10px;
    color: var(--primary);
}

.m__header {
    width: 100%;
    display: flex;
    justify-content: space-between;

    button {
        background: hsl(0, 0%, 90%);
        width: 2rem;
        height: 2rem;
        border-radius: .5rem;

        &:hover {
            background: hsl(0, 100%, 64%);
            color: var(--white);
            transition: .3s;
        }

        &:active {
            background: hsl(0, 100%, 50%);
        }
    }

    div {
        font-size: 1.4rem;
        font-weight: bold;
    }
}

.m__body {
    width: 100%;
    display: flex;
    justify-content: center;
}

.m__footer {
    margin-top: 1rem;
    float: right;

    button {
        padding: .5rem;
        margin: 0 .5rem;
        border-radius: .5rem;
        transition: .3s;
        color: var(--white);
        &:nth-child(1) {
            background-color: #22c55e;
            &:hover {
                background-color: #16a34a;
            }
        }

        &:nth-child(2) {
            background-color: #ef4444;
            &:hover {
                background-color: #dc2626;
            }
        }
    }
}
</style>