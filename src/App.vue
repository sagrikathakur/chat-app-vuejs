<script setup>
import { ref, onMounted } from 'vue';
import ChatMessages from './components/ChatMessages.vue';
import ChatInput from './components/ChatInput.vue';

const userName = ref('');
const isNameSet = ref(false);
const messages = ref([]);
const darkMode = ref(false);

// Predefined random replies
const autoReplies = [
  "I'm fine, how about you?",
  "aur bata",
  "UI needs more modifications",
  "Good night",
  "had lunch?",
  "What are you up to?",
  "Dont panic!! every thing gonna be alright!",
  "This is awesome website for UI inspiration.",
];

// Load name and messages
onMounted(() => {
  const storedName = localStorage.getItem('userName');
  if (storedName) {
    userName.value = storedName;
    isNameSet.value = true;
    loadMessages();
  }
  
  darkMode.value = localStorage.getItem('darkMode') === 'true';
  document.body.classList.toggle('dark', darkMode.value);
});

// Save name
const saveName = (event) => {
  event.preventDefault();
  if (!userName.value.trim()) return;
  localStorage.setItem('userName', userName.value);
  isNameSet.value = true;
  loadMessages();
};

// Load messages
const loadMessages = () => {
  messages.value = JSON.parse(localStorage.getItem('messages') || '[]');
};

// Save messages
const saveMessages = () => {
  localStorage.setItem('messages', JSON.stringify(messages.value));
};

// Send new message
const sendMessage = (message) => {
  const newMsg = {
    person: userName.value,
    type: 'sent',
    message,
    createdAt: Date.now(),
  };
  messages.value.push(newMsg);
  saveMessages();

  // Generate auto-reply
  setTimeout(() => {
    const replyMsg = {
      person: "Shrinat prabhu",
      type: 'received',
      message: autoReplies[Math.floor(Math.random() * autoReplies.length)],
      createdAt: Date.now(),
    };
    messages.value.push(replyMsg);
    saveMessages();
  }, 1000);
};

// Delete message
const deleteMessage = (index) => {
  messages.value.splice(index, 1);
  saveMessages();
};

// Toggle dark mode
const toggleDarkMode = () => {
  darkMode.value = !darkMode.value;
  localStorage.setItem('darkMode', darkMode.value);
  document.body.classList.toggle('dark', darkMode.value);
};

// Change user name
const changeName = () => {
  localStorage.removeItem('userName');
  isNameSet.value = false;
};
</script>

<template>
  <div class="container">
    <h2>Vue Chat App</h2>

    <!-- Dark Mode Toggle -->
    <button @click="toggleDarkMode">
      {{ darkMode ? " Light Mode" : "Dark Mode" }}
    </button>

    <!-- Name Input Form -->
    <form v-if="!isNameSet" @submit="saveName" class="name-form">
      <label>Enter Your Name:</label>
      <input v-model="userName" required />
      <button type="submit">Start Chat</button>
    </form>

    <!-- Chat Interface -->
    <div v-else>
      <p>Welcome, <strong>{{ userName }}</strong>! 
        <button @click="changeName">Change Name</button>
      </p>

      <ChatMessages :messages="messages" @delete-message="deleteMessage" />
      <ChatInput @send-message="sendMessage" />
    </div>
  </div>
</template>

<style>
/* Light Mode */
.container {
  width:80vw;
  margin: 0 auto;
  text-align: center;
  background: white;
  color: black;
  padding: 20px;
  border-radius: 8px;
  height: 100vh;
  
}

/* Dark Mode */
body.dark .container {
  background: #121212;
  color: white;
}
</style>
