<template>
  <v-app>
    <v-app-bar app>
      <a href="/">
        <img src="./assets/logo.png" alt="stori logo" />
      </a>
    </v-app-bar>
    <v-main>
      <userList></userList>
    </v-main>
  </v-app>
</template>
<script>
import userList from "./components/userList.vue";

export default {
  components: {
    userList
  }
}
</script>
