<template>
  <v-container fluid>
    <chat-messages
        :current-question="currentQuestion"
        :messages="messages"
        :is-chat-started="isChatStarted"
    />
    <chat-field
        v-model="isTyping"
        :current-question="currentQuestion"
        :questions="questions"
        :is-error="isError"
        @next-message="nextMessage"
        @start-chat="startChat"
    />
  </v-container>
</template>

<script>
import http from "../http/http";
import { delay } from "../helpers";

import ChatField from "@/components/ChatField";
import ChatMessages from "@/components/ChatMessages";

export default {
  name: "Chat",
  components: {ChatMessages, ChatField},
  data: () => ({
    currentQuestion: 0,
    messages: [],
    questions: [],
    isChatStarted: false,
    robotTypingTime: 1500,
    isTyping: false,
    isError: false
  }),

  async created() {
    await http.get('questions.json')
      .then(res => {
        this.questions = res.data
      })
      .catch((e) => {
        this.questions = []
        this.isError = true
        throw new Error(e)
      })
  },

  methods: {
    nextMessage(message) {
      this.isTyping = true
      this.messages.push(message);
      ++this.currentQuestion;

      delay(this.robotTypingTime)
          .then(() => {
            this.questions[this.currentQuestion] && this.messages.push(...this.questions[this.currentQuestion]);
            this.isTyping = false
          });
    },

    startChat() {
      this.isChatStarted = true
      this.isTyping = true
      delay(this.robotTypingTime)
          .then(() => {
            this.messages = [...this.questions[this.currentQuestion]];
            this.isTyping = false
          });
    }
  }
}
</script>

<style>
</style>