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
        @next-message="nextMessage"
        @start-chat="startChat"
    />
  </v-container>
</template>

<script>
import {delay} from "../helpers";

import ChatField from "@/components/ChatField";
import ChatMessages from "@/components/ChatMessages";

export default {
  name: "Chat",
  components: {ChatMessages, ChatField},
  data: () => ({
    currentQuestion: 0,
    messages: [],
    questions: [
      [
        {
          text: "Hi, I'm Peter!",
          owner: "him"
        },
        {
          text: "What's your name?",
          ask: "name",
          owner: "him"
        }
      ],
      [
        {
          text: "Nice to meet you!",
          owner: "him"
        },
        {
          text: "How was your day?",
          ask: "feeling",
          owner: "him"
        }
      ],
      [
        {
          text: "Where're you from?",
          ask: "location",
          owner: "him"
        }
      ],
      [
        {
          text: "Nice!",
          owner: "him"
        },
        {
          text: "How old are you?",
          ask: "age",
          owner: "him"
        }
      ],
      [
        {
          text: "What's your favorite hobby?",
          ask: "hobby",
          owner: "him"
        }
      ],
      [
        {
          text: "Wow, cool",
          owner: "him"
        }
      ]
    ],
    isChatStarted: false,
    robotTypingTime: 1500,
    isTyping: false
  }),

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