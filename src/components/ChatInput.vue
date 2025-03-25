<script setup>
import { ref } from 'vue';
import { Picker } from 'emoji-mart';

const newMessage = ref('');
const emit = defineEmits(['send-message']);
const showEmojiPicker = ref(false);

// Send message
const sendMessage = () => {
  if (!newMessage.value.trim()) return;
  emit('send-message', newMessage.value);
  newMessage.value = '';
};

// Add emoji
const addEmoji = (emoji) => {
  newMessage.value += emoji.native;
  showEmojiPicker.value = false;
};
</script>

<template>
  <div class="chat-input">
    <button @click="showEmojiPicker = !showEmojiPicker">😀</button>
    <input v-model="newMessage" placeholder="Type a message..." @keyup.enter="sendMessage" />
    <button @click="sendMessage">Send</button>

    <!-- Emoji Picker -->
    <div v-if="showEmojiPicker">
      <Picker @select="addEmoji" />
    </div>
  </div>
</template>

<style>
.chat-input {
  display: flex;
  gap: 10px;
  align-items: center;
  padding: 10px;
}
input {
  flex-grow: 1;
  padding: 8px;
  border-radius: 13px;
  border: none;
}

button {
  padding: 6px 10px;
  cursor: pointer;
  display: flex;
  transition:background 0.2s ease;
}
button:hover{
  background:rgb(212, 207, 207);
}
</style>
