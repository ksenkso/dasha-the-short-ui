<template>
  <form>
    <textarea name="" id="" cols="30" rows="10" v-model="text" />
    <select v-model="chatAlias">
      <option value="test">Тестовая группа</option>
      <option value="genshin_conference">Конференц-зал геншин импакта</option>
    </select>
    <button
      :disabled="isLoading || !text"
      @click="send"
    >send</button>
  </form>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'MessageForm',
  data() {
    return {
      text: '',
      chatAlias: 'test',
      isLoading: false,
    };
  },
  methods: {
    send() {
      this.isLoading = true;
      fetch(import.meta.env.VITE_APP_API_URL + '/sendMessage', {
        method: 'POST',
        body: JSON.stringify({ text: this.text, chatAlias: this.chatAlias }),
        headers: {
          'Content-Type': 'application/json'
        }
      })
        .then(() => {
          this.text = '';
        })
        .catch((err) => {
          alert('Ошибка: ' + err.message);
          console.error(err);
        })
        .finally(() => {
          this.isLoading = false;
        });
    }
  }
});
</script>
<style scoped>
form {
  display: flex;
  flex-direction: column;
  row-gap: 12px;
}
</style>
