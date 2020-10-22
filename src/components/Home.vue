<template>
  <div>
    <h3>Home</h3>

    <User>
      <template v-slot:[`user`]="{ user }">
        <div v-if="user">
          <UserProfile :user="user" />
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

export default {
  name: "Home",
  components: {
    Login,
    User,
    UserProfile,
  },
};
</script>

<style>
</style>