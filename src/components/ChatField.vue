<template>
  <v-row
      :style="{
      position: 'absolute',
      bottom: 0,
      left: 0,
      right: 0,
      padding: '10px',
      zIndex: 100,
      background: '#ffffff'
    }"
  >
    <v-col class="py-0" cols="12" md="12">
      <v-textarea
          v-model="input"
          :error-messages="chatInputErrors"
          filled
          no-resize
          label="Type your message"
          :disabled="!isChatStarted || isBtnDisabled"
      />
    </v-col>
    <v-btn
        v-if="isChatStarted"
        class="ml-auto mr-2"
        @click="send"
        x-large
        color="primary"
        :disabled="isBtnDisabled"
    >
      Send Message
    </v-btn>
    <v-btn
        v-else
        class="ml-auto mr-2"
        @click="startChat"
        x-large
        color="primary"
    >
      Let's chat
    </v-btn>
  </v-row>
</template>

<script>
export default {
  name: "ChatField",

  props: {
    currentQuestion: {
      type: Number,
      required: true,
      default: 0
    },
    questions: {
      type: Array,
      required: true,
      default: () => []
    }
  },

  data: () => ({
    input: '',
    chatInputErrors: [],
    answers: {
      name: '',
      age: 0,
      location: '',
      feeling: '',
      hobby: '',
    },
    isChatStarted: false,
    isBtnDisabled: false
  }),

  watch: {
    currentQuestion(val) {
      if (val === this.questions.length - 1) {
        this.isBtnDisabled = true;
      }
    }
  },

  methods: {
    send() {
      if (this.input.length) {
        this.chatInputErrors = []; // reset errors array

        let question = {};
        if (this.questions[this.currentQuestion]) { // if questions exist
          question = this.questions[this.currentQuestion].find(q => q.ask);

          if (question.ask !== undefined) {
            this.answers[question.ask] = this.input;
          }
        }

        this.$emit("next-message", {
          text: this.input,
          ...!!question && {ask: question.ask}, // if question exist - add property "ask"
          owner: "me"
        });

        this.input = "";
      } else {
        this.chatInputErrors.push("This field is required"); // set errors
      }
    },

    startChat() {
      this.isChatStarted = true
      this.$emit('start-chat')
    }
  }
}
</script>

<style scoped>
</style>