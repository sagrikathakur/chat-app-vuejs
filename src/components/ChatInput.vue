<script setup>
import { ref } from 'vue';
import 'emoji-picker-element';

const newMessage = ref('');
const showEmojiPicker = ref(false);
const emit = defineEmits(['send-message']);

// Function to send a message
const sendMessage = () => {
  if (newMessage.value.trim() === '') return;
  emit('send-message', newMessage.value);
  newMessage.value = '';
};

// Function to add an emoji to the input field
const addEmoji = (event) => {
  newMessage.value += event.detail.unicode;
};
</script>

<template>
  <div class="input-area">
    <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Type a message..." />
    <button @click="sendMessage">Send</button>
    <button @click="showEmojiPicker = !showEmojiPicker">Emoji-sticker</button>

    <emoji-picker v-if="showEmojiPicker" @emoji-click="addEmoji"></emoji-picker>
  </div>
</template>

<style>
.input-area {
  display: flex;
  gap: 5px;
  position: relative;
}
input {
  flex: 1;
  padding: 8px;
  border: none;
  border-radius: 14px;
}
button {
  padding: 8px;
  border-radius: 6px;
}
emoji-picker {
  position: absolute;
  bottom: 50px;
  right: 0;
  z-index: 1000;
}
</style>
