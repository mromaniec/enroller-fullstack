<template>
  <div id="app">
    <h1>Witaj w systemie do zapisów na zajęcia</h1>

    <div v-if="authenticatedUsername">
      <UserPanel :username="authenticatedUsername" @logout="logMeOut()"></UserPanel>
      <MeetingsPage :username="authenticatedUsername"></MeetingsPage>
    </div>

    <div v-else>
        <button :class="signingup ? 'button-outline' : '' " @click="signingup = false" class ="button" href="#">Logowanie</button>
        <button :class="!signingup ? 'button-outline' : '' " @click="signingup = true" >Rejestracja</button>

      <div v-if="message" class="alert"> {{message}} </div>

      <LoginForm v-if="!signingup" @login="(user) => logMeIn(user)"></LoginForm>
      <LoginForm v-else @login="(user) => register(user)" button-label="Załóż konto"></LoginForm>
    </div>
  </div>
</template>

<script>
import "milligram";
import LoginForm from "./LoginForm";
import UserPanel from "./UserPanel";
import MeetingsPage from "./meetings/MeetingsPage";
import axios from "axios";

export default {
  components: {LoginForm, MeetingsPage, UserPanel},
  data() {
    return {
      message: '',
      signingup: false,
      authenticatedUsername: '',
    }
  },
  methods: {
    logMeIn(user) {
      this.authenticatedUsername = user.login;
    },
    logMeOut() {
      this.authenticatedUsername = '';
    },

    register(user) {
      axios.post('/api/participants', user)
          .then(response => {
            this.message="konto założone";
          })
          .catch(response => {
            this.message="nazwa istnieje!";
          });
    }
  }
}
</script>

<style>
#app {
  max-width: 1000px;
  margin: 0 auto;
}

.alert{
  padding: 5px;
  margin: 10px;
  border: 1px solid darkred;
  font-size: 15px;
  color: red;
  text-align: center;
}
</style>
