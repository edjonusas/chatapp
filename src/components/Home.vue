<template>
  <div>
    <h3>Home</h3>

    <User>
      <template v-slot:[`user`]="{ user }">
        <div v-if="user">
          <UserProfile :user="user" />
          <ChatList :uid="user.uid" />
        </div>
        <Login v-else />
      </template>
    </User>
  </div>
</template>

<script>
import Login from "./Login";
import User from "./User";
import UserProfile from "./UserProfile";
import ChatList from "./ChatList";

export default {
  name: "Home",
  components: {
    Login,
    User,
    UserProfile,
    ChatList,
  },
};
</script>

<style>
</style>