<template>
  <div>
    <ul>
      <li v-for="chat of chats" :key="chat.id">
        <router-link :to="{ name: 'chat', params: { id: chat.id } }">
          {{ chat.id }}
        </router-link>
        <input v-model="usersId" type="text" placeholder="users id" />
        <button @click="addUser(chat.id)">add Member</button>
      </li>
    </ul>

    <button @click="createChatRoom()">Create New Chat Room</button>
  </div>
</template>

<script>
//import { firestore } from "firebase";
import { db } from "../firebase";

export default {
  props: ["uid"],
  data() {
    return {
      chats: [],
      usersId: "",
    };
  },
  firestore() {
    return {
      chats: db
        .collection("chats")
        .where("members", "array-contains", this.uid),
    };
  },
  methods: {
    addUser(id) {
      const membersArray = [this.uid];
      membersArray.push(this.usersId);
      db.collection("chats").doc(id).update({
        members: membersArray,
      });
    },

    async createChatRoom() {
      const newChat = await db.collection("chats").add({
        createdAt: Date.now(),
        owner: this.uid,
        members: [this.uid],
      });

      console.log(newChat);
    },
  },
};
</script>

<style>
</style>