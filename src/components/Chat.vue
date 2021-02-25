<template>
  <v-container fluid>
    <chat-messages
        :current-question="currentQuestion"
        :messages="messages"
    />
    <chat-field
        :current-question="currentQuestion"
        :questions="questions"
        @next-message="nextMessage"
    />
  </v-container>
</template>

<script>
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
  }),

  created() {
    this.messages = [...this.questions[this.currentQuestion]]
  },

  methods: {
    nextMessage(message) {
      this.messages.push(message);
      ++this.currentQuestion;

      this.questions[this.currentQuestion] && this.messages.push(...this.questions[this.currentQuestion])
    }
  }
}
</script>

<style>
</style>