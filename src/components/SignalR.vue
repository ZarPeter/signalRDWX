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
  import { HubConnection, HubConnectionBuilder, LogLevel } from '@microsoft/signalr';
const connection = new HubConnectionBuilder()
    .withUrl('https://signalrrelaydwx.azurewebsites.net/api/')
    .configureLogging(LogLevel.Information)
    .build();
const messagesLocal = [""];
connection.on("newMessage", e => {
  console.log(e);
  const msg: string = e.message;
  messagesLocal.push(msg);
})
  export default Vue.extend({
    name: 'SignalR',
    data: () => ({
      messages: messagesLocal,
    }),
    methods: {
      async start() {
        try {
            await connection.start().then(()=> connection.invoke("messages", ""));
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
