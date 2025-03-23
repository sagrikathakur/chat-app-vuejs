<script setup>
defineProps(['messages']);
const emit = defineEmits(['delete-message']);
</script>

<template>
  <div class="chat-box">
    <div v-for="message in messages" :key="message.id" class="message">
      <strong>{{ message.name }}:</strong> <span>{{ message.text }}</span>
      <button @click="emit('delete-message', message.id)">🗑️</button>
    </div>
  </div>
</template>

<style>
.chat-box {
  height: 300px;
  border: 1px solid #ccc;
  overflow-y: auto;
  padding: 10px;
  margin-bottom: 10px;
}
.message {
  background: #f1f1f1;
  margin: 5px;
  padding: 8px;
  border-radius: 5px;
  text-align: left;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
button {
  background: rgb(188, 181, 247);
  color: white;
  border: none;
  padding: 5px;
  cursor: pointer;
  border-radius: 3px;
  margin-bottom: 17px;
}
button:hover {
  background: rgb(155, 167, 243);
}
</style>
