<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import ConversationsList from './components/ConversationsList.vue';
import Conversation from './components/Conversation.vue';
</script>

  

<template>
  <div class="h-screen w-full p-5 flex">
    <ConversationsList/>
    <Conversation title="Hello world" description="45 membres, 24 en ligne"/>
  </div>
</template>

  

<style scoped>

  

</style>