<template>
  <nav>
    <template v-if="this.$store.state.currentUser">
      <router-link to="/account">My Account</router-link> |
      <router-link to="/login" v-on:click="this.logout()" replace>
        Logout
      </router-link>
    </template>
    <template v-else>
      <router-link to="/login">Login</router-link> |
      <router-link to="/register">Register</router-link>
    </template>
    <router-link to="/home">Home</router-link> |
    <template v-if="isAdmin()">
      <router-link to="/users">Users</router-link> |
    </template>
    <router-link to="/about">About</router-link>
  </nav>
  <router-view />
</template>

<script lang="ts">
import { defineComponent } from "vue";

import User from "@/cls/model/User";
import { QuestionObject } from "@/cls/model/Question";

export default defineComponent({
  name: "App",
  data() {
    return {};
  },
  methods: {
    logout() {
      this.$store.dispatch("LOGOUT");
      this.$router.push("/login");
    },
    isAdmin() {
      return (
        this.$store.state.currentUser &&
        this.$store.state.currentUser.login === "admin"
      );
    },
  },
  beforeMount() {
    // eslint-disable-next-line @typescript-eslint/no-var-requires
    const users = require("./assets/mock-users.json");
    // eslint-disable-next-line @typescript-eslint/no-var-requires
    const questions = require("./assets/mock-questions.json");
    users.forEach((user: User) => this.$store.commit("ADD_USER", user));
    questions.forEach((question: QuestionObject) =>
      this.$store.commit("ADD_QUESTION", question)
    );
    this.$store.commit("LOGIN", { login: "admin", password: "admin" });
  },
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;
    padding: 20px;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
