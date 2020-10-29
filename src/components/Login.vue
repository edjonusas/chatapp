<template>
  <aside>
    <div v-if="newUser">
      <h3>Sign Up for a new Account</h3>
      <a href="#" @click="newUser = false">Returning user?</a>

      <div class="inputs-block">
        <input class="input" type="text" placeholder="First Name" />
        <input class="input" type="text" placeholder="Last Name" />
        <input class="input" type="text" placeholder="Country" />
        <input class="input" type="text" placeholder="City" />
        <select class="input" name="" id="">
          <option value="">select your gender</option>
          <option value="">male</option>
          <option value="">female</option>
          <option value="">other</option>
        </select>
        <input v-model="email" placeholder="email" type="email" class="input" />
        <input
          v-model="password"
          placeholder="password"
          type="password"
          class="input"
        />
      </div>
    </div>

    <div v-else>
      <h3>Sign in with Email</h3>
      <a href="#" @click="newUser = true">New user?</a>

      <input v-model="email" placeholder="email" type="email" class="input" />
      <input
        v-model="password"
        placeholder="password"
        type="password"
        class="input"
      />
    </div>

    <button
      class="button is-info"
      :class="{ 'is-loading': loading }"
      @click="signInOrCreateUser()"
    >
      {{ newUser ? "Sing up" : "Login" }}
    </button>
    <p class="has-text-danger" v-if="errorMessage">{{ errorMessage }}</p>
  </aside>
</template>

<script>
import { auth } from "../firebase";

export default {
  data() {
    return {
      auth,
      newUser: true,
      email: "",
      password: "",
      firsName: "",
      LastName: "",
      loading: false,
      errorMessage: "",
    };
  },
  methods: {
    async signInOrCreateUser() {
      this.loading = true;
      this.errorMessage = "";
      try {
        if (this.newUser) {
          await auth.createUserWithEmailAndPassword(this.email, this.password);
        } else {
          await auth.signInWithEmailAndPassword(this.email, this.password);
        }
      } catch (error) {
        this.errorMessage = error.message;
      }
      this.loading = false;
    },
  },
};
</script>

<style>
</style>