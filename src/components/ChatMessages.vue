<script setup>
defineProps(['messages']);
const emit = defineEmits(['delete-message']);
</script>

<template>
  <div class="chat-box">
    <div v-for="message in messages" :key="message.id" class="message">
      <span>{{ message.text }}</span>
      <button @click="emit('delete-message', message.id)">Delete</button>
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
  background: rgb(115, 169, 227);
  color: white;
  border: none;
  padding: 5px;
  cursor: pointer;
  border-radius: 3px;
}
button:hover {
  background: rgb(180, 184, 246);
}
</style>
