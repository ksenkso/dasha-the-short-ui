<template>
  <div class="client">
    <div class="dialogs">
      <button
        v-for="dialog in dialogs"
        :key="dialog.alias"
        class="dialog"
        :class="{ active: currentDialog === dialog.alias }"
        @click="currentDialog = dialog.alias"
      >
        {{ dialog.title }}
      </button>
    </div>
    <div class="messages">
      <div class="messages-list">
        <div
          v-for="message in messages"
          :key="message.id"
          class="message"
        >

        </div>
      </div>
      <MessageForm
        :disabled="isLoading"
        @send="send"
      />
    </div>
  </div>
</template>

<style scoped>
.client {
  display: flex;
  border-radius: 3px;
  background-color: var(--color-bg);
}

.dialogs {
  display: flex;
  flex-direction: column;
}

.dialog {
  padding: 12px 8px;
  cursor: pointer;
  border: none;
  background-color: var(--color-tint);
  color: white;
  text-align: left;
}

.dialog.active {
  background-color: var(--color-active);
}

.messages {
  width: 100%;
}
</style>
<script setup lang="ts">
import MessageForm from "./components/MessageForm.vue";
import { ref } from "vue";

const dialogs = [
  {
    title: 'Тестовая группа',
    alias: 'test',
  },
  {
    title: 'Конференц-зал геншин импакта',
    alias: 'genshin_conference',
  },
];
const currentDialog = ref('test');
const isLoading = ref(false);
const messages = ref([]);
const send = (text: string) => {
  isLoading.value = true;
  fetch(import.meta.env.VITE_APP_API_URL + '/sendMessage', {
    method: 'POST',
    body: JSON.stringify({ text, chatAlias: currentDialog.value }),
    headers: {
      'Content-Type': 'application/json'
    }
  })
    .catch((err) => {
      alert('Ошибка: ' + err.message);
      console.error(err);
    })
    .finally(() => {
      isLoading.value = false;
    });
}
</script>
