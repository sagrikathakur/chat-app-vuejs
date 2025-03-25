<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps(['messages']);
const emit = defineEmits(['delete-message']);

// Format timestamp
const formatTimestamp = (timestamp) => {
  const date = new Date(timestamp);
  return date.toLocaleTimeString('en-IN', {
    hour: '2-digit',
    minute: '2-digit',
    hour12: true,
  }) + ' | ' + date.toLocaleDateString('en-IN', {
    day: '2-digit',
    month: 'short',
  });
};
</script>

<template>
  <div class="chat-messages">
    <transition-group name="fade">
      <div v-for="(msg, index) in messages" :key="msg.createdAt" class="message">
        <p><strong>{{ msg.person }}:</strong> {{ msg.message }}</p>
        <small>{{ formatTimestamp(msg.createdAt) }}</small>
        <button @click="emit('delete-message', index)">delete</button>
      </div>
    </transition-group>
  </div>
</template>

<style>
.chat-messages {
  max-height: 300px;
  overflow-y: auto;
  padding: 10px;
}
.message {
  background: #f3f3f3;
  padding: 8px;
  margin: 5px 0;
  border-radius: 6px;
  position: relative;
  font-family: 'Courier New', Courier, monospace;

}
body.dark .message {
  background:#333;
  color: white;
}
small {
  display: block;
  font-size: 0.8rem;
  color: white;
}

/* Fade Animations */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
