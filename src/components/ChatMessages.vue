<template>
  <v-row class="chat-messages__wrapper">
    <ul>
      <li v-for="(message, index) in messages"
          :key="index"
          :class="message.owner"
      >
        {{ message.text }}
      </li>
    </ul>
  </v-row>
</template>

<script>
export default {
  name: "ChatMessages",

  props: {
    messages: {
      type: Array,
      required: true,
      default: () => []
    },
    currentQuestion: {
      type: Number,
      required: true,
      default: 0
    }
  }
}
</script>

<style scoped lang="scss">
.chat-messages__wrapper {
  width: 100%;
  height: 410px;
  overflow-y: scroll;

  ul {
    width: 100%;
    margin: 0;

    li {
      display: inline-block;
      clear: both;
      padding: 20px;
      border-radius: 30px;
      margin-bottom: 2px;
      font-family: Helvetica, Arial, sans-serif;

      &.him {
        background: #eee;
        float: left;
      }

      &.me {
        float: right;
        background: #0084ff;
        color: #fff;

        &:last-of-type {
          border-bottom-right-radius: 30px;
        }
      }
    }

    .him + .me {
      border-bottom-right-radius: 5px;
    }

    .me + .me {
      border-top-right-radius: 5px;
      border-bottom-right-radius: 5px;
    }
  }
}
</style>