<script setup>
import { ref, onMounted } from 'vue';
import ChatMessages from './components/ChatMessages.vue';
import ChatInput from './components/ChatInput.vue';

const messages = ref([]);

// Load messages from localStorage
onMounted(() => {
  const storedMessages = localStorage.getItem('chatMessages');
  if (storedMessages) {
    messages.value = JSON.parse(storedMessages);
  }
});

// Function to add a new message
const addMessage = (newMessage) => {
  const newMsg = { id: Date.now(), text: newMessage };
  messages.value.push(newMsg);
  saveMessages();
};

// Function to delete a message
const deleteMessage = (id) => {
  messages.value = messages.value.filter(msg => msg.id !== id);
  saveMessages();
};

// Save messages to localStorage
const saveMessages = () => {
  localStorage.setItem('chatMessages', JSON.stringify(messages.value));
};
</script>

<template>
  <div class="container">
    <h2>Vue Chat App</h2>
    <ChatMessages :messages="messages" @delete-message="deleteMessage" />
    <ChatInput @send-message="addMessage" />
  </div>
</template>

<style>
*{
  scrollbar-width: none;
}
.container {
  width: 400px;
  margin: 0 auto;
  text-align: center;
}
body{
  background: linear-gradient(45deg, rgb(207, 216, 241), #fff);
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;

}
h2{
  font-family: 'Courier New', Courier, monospace;
  font-size: 23px;
  font-weight: 400;
  border-bottom: 4px solid rgb(146, 146, 238);
}
</style>
