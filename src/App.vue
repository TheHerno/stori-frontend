<template>
  <v-app>
    <v-app-bar app>
      <v-app-bar-title>
        <a href="/">
          <img src="./assets/logo.png" alt="stori logo" />
        </a>
      </v-app-bar-title>
    </v-app-bar>
    <v-main>
      <userList />
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
