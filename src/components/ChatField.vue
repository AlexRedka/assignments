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
          filled
          name="input-7-1"
          label="Type your message"
          :error-messages="chatInputErrors"
          v-model="input"
      ></v-textarea>
    </v-col>
    <v-btn
        class="ml-auto mr-2"
        @click="send"
        x-large
        color="primary"
        dark
    >
      Send Message
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
    }
  }),

  methods: {
    send() {
      if (this.input.length) {
        this.chatInputErrors = []; // reset errors array

        let question = {};
        if (this.questions[this.currentQuestion]) {
          question = this.questions[this.currentQuestion].find(q => q.ask);

          if (question && question.ask !== undefined) {
            this.answers[question.ask] = this.input;
          }
        }

        this.$emit("next-message", {
          text: this.input,
          ...!!question && { ask: question.ask }, // if question exist - add property "ask"
          owner: "me"
        });

        this.input = "";
      } else {
        this.chatInputErrors.push("This field is required"); // set errors
      }
    }
  }
}
</script>

<style scoped>
</style>