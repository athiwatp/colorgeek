<template>
  <div id="app">
    <app-header></app-header>
    <router-view></router-view>
  </div>
</template>

<script>
import firebase from 'firebase';
import { mapActions } from 'vuex';
import AppHeader from './components/Header';

export default {
  name: 'app',
  components: {
    AppHeader,
  },
  created() {
    firebase.auth().onAuthStateChanged((user) => {
      // User is signed in.
      if (user) {
        let nickname = '';
        if (user.displayName) {
          nickname = user.displayName;
        }
        this.setCurrentUser({ nickname, uid: user.uid });
      } else {
        // No user is signed in.
        this.clearCurrentUser();
      }
    });
  },
  methods: {
    ...mapActions({
      setCurrentUser: 'setCurrentUser',
      clearCurrentUser: 'clearCurrentUser',
    }),
  },
};
</script>

<style>
  body {
    background-color: #f3f3ff;
  }
</style>
