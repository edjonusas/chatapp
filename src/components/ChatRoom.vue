<template>
  <div>
    <h3>Welcome to ChatRoom {{ idChat }}</h3>
    <User>
      <template v-slot:[`user`]="{ user }">
        <ul>
          <li v-for="message of messages" :key="message.id">
            <ChatMessage
              :message="message"
              :owner="user.uid === message.sender"
            />
          </li>
        </ul>

        <input
          class="input is-primary"
          @keyup.enter="addMessage(user.uid)"
          v-model="newMessageText"
          type="text"
        />
        <button
          :disabled="!newMessageText || loading"
          class="button is-success"
          type="text"
          @click="addMessage(user.uid)"
        >
          Add Message
        </button>
      </template>
    </User>
  </div>
</template>

<script>
import User from "./User";
import { db } from "../firebase";
import ChatMessage from "./ChatMessage";

export default {
  components: {
    User,
    ChatMessage,
  },
  data() {
    return {
      newMessageText: "",
      loading: false,
      messages: [],
    };
  },
  computed: {
    idChat() {
      return this.$route.params.id;
    },
    messageCollection() {
      return db.doc(`chats/${this.idChat}`).collection("messages");
    },
  },
  firestore() {
    return {
      messages: this.messageCollection.orderBy("createdAt").limitToLast(10),
    };
  },
  methods: {
    async addMessage(uid) {
      this.loading = true;

      const { id: messageId } = this.messageCollection.doc();

      await this.messageCollection.doc(messageId).set({
        text: this.newMessageText,
        sender: uid,
        createdAt: Date.now(),
      });

      this.loading = false;
      this.newMessageText = "";
    },
  },
};
</script>

<style scoped>
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  min-width: 500px;
  background-color: #efefef;
  padding: 10px;
  border-radius: 0;
}

li {
  display: flex;
}
</style>