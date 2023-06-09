<template>
  <form>
    <textarea
      name=""
      id=""
      cols="30"
      rows="10"
      v-model="text"
      placeholder="Type something..."
      @keydown.enter.prevent="send"
    />
    <button
      :disabled="disabled || !text"
      @click="send"
    >send</button>
  </form>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'MessageForm',
  props: {
    disabled: Boolean,
  },
  emits: ['send'],
  data() {
    return {
      text: '',
    };
  },
  methods: {
    send() {
      const message = this.text.trim();

      if (message.length) {
        this.$emit('send', message);
        this.text = '';
      }
    }
  }
});
</script>
<style scoped>
form {
  display: flex;
  column-gap: 12px;
}

textarea {
  background-color: var(--color-tint);
  border: none;
  resize: none;
  line-height: 1.333em;
  height: 6em;
  width: 100%;
  color: white;
  padding: 4px;
}

textarea:focus {
  outline: none;
}

button {
  background-color: var(--color-tint);
  color: white;
  border: none;
  cursor: pointer;
}
</style>
