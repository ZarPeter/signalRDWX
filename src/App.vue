<template>
  <v-app>
    <v-app-bar
      app
      color="#59b4d9"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="SignalR Logo"
          class="shrink mr-2"
          contain
          :src="require('./assets/sr-logo.png')"
          transition="scale-transition"
          width="140"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <SignalR/>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import SignalR from './components/SignalR.vue';

export default Vue.extend({
  name: 'App',

  components: {
    SignalR,
  },

  data: () => ({
    //
  }),
});
</script>
