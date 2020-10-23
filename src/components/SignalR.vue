<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img
          :src="require('../assets/sr-logo.png')"
          class="my-3"
          contain
          height="200"
        />
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <p>Here be messages:</p>
        <div class="message-wrapper">
          <ul >
            <li v-for="message in messages" :key="message">
               {{ message }} 
            </li>
          </ul>
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <div>
          <v-input ><v-text-field label="Please enter a message"> </v-text-field></v-input>
        </div>
      </v-col>
    </v-row>

  </v-container>
</template>

<script lang="ts">
  import Vue from 'vue'
  import signalR from '@microsoft/signalr';
const connection = new signalR.HubConnectionBuilder()
    .withUrl("dwxros.service.signalr.net/")
    .configureLogging(signalR.LogLevel.Information)
    .build();

  export default Vue.extend({
    name: 'SignalR',
    data: () => ({
      messages: ["1","2"],
    }),
    methods: {
      async start() {
        try {
            await connection.start();
            console.log("connected");
        } catch (err) {
            console.log(err);
            // setTimeout(() => this.start(), 5000);
        }
      },
      // connection.onclose(async () => {
      //   await this.start();
      // })
    },
    beforeMount() {
        this.start();
    }
  })
</script>
