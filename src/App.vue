<script setup>
import { ref, onMounted } from 'vue';
import ChatMessages from './components/ChatMessages.vue';
import ChatInput from './components/ChatInput.vue';

const messages = ref([]);
const userName = ref('');
const isNameSet = ref(false);
const showNameInput = ref(false); 
onMounted(() => {
  const storedName = localStorage.getItem('userName');
  if (storedName) {
    userName.value = storedName;
    isNameSet.value = true;
  }

  // Load messages from localStorage
  const storedMessages = localStorage.getItem('chatMessages');
  if (storedMessages) {
    messages.value = JSON.parse(storedMessages);
  }
});

// Save the entered name to localStorage
const saveName = () => {
  if (userName.value.trim() === '') return;
  localStorage.setItem('userName', userName.value);
  isNameSet.value = true;
  showNameInput.value = false; // Hide input after saving
};

// Function to add a new message
const addMessage = (newMessage) => {
  const newMsg = { id: Date.now(), name: userName.value, text: newMessage };
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

    <!-- If name is not set, show input -->
    <div v-if="!isNameSet" class="name-input">
      <input v-model="userName" placeholder="Enter your name..." />
      <button @click="saveName">Save Name</button>
    </div>

    <!-- If name is set, show chat -->
    <div v-else>
      <p>Welcome, <strong>{{ userName }}</strong>! 🎉</p>
      
      <!-- Change Name Button -->
      <button @click="showNameInput = !showNameInput">Change Name</button>
      
      <!-- Show Name Input if user wants to change name -->
      <div v-if="showNameInput" class="name-input">
        <input v-model="userName" placeholder="Enter new name..." />
        <button @click="saveName">Update Name</button>
      </div>

      <ChatMessages :messages="messages" @delete-message="deleteMessage" />
      <ChatInput @send-message="addMessage" />
    </div>
  </div>
</template>

<style>

.container {
  width: 400px;
  margin: 0 auto;
  text-align: center;
}
.name-input {
  margin-bottom: 17px;
  
}
button {
  margin-top: 5px;
  padding: 5px 10px;
  cursor: pointer;
}
</style>
