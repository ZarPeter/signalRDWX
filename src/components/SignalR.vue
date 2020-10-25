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
            <li v-for="(message, index) in messages" :key="index">
              <span v-if="message != ''">
               {{ message }} 
              </span>
            </li>
          </ul>
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <div>
          <v-input ><v-text-field label="Please enter a message" v-model="enteredMessage"> </v-text-field></v-input>
          <button v-on:click="sendMessage()"> Send </button>
        </div>
      </v-col>
    </v-row>

  </v-container>
</template>

<script lang="ts">
  import Vue from 'vue'
  import axios from 'axios'
  import { HttpClient, HubConnection, HubConnectionBuilder, HubConnectionState, LogLevel } from '@microsoft/signalr';
  
const connection = new HubConnectionBuilder()
    .withUrl('https://signalrrelaydwx.azurewebsites.net/api/')
    // .withUrl('https://dwxros.service.signalr.net/chat')
    .configureLogging(LogLevel.Information)
    .build();
const messagesLocal: string[] = [];
connection.on("newMessage", e => {
  console.log(e);
  const msg: string = e.message;
  messagesLocal.push(msg);
})
  export default Vue.extend({
    name: 'SignalR',
    data: () => ({
      messages: messagesLocal,
      enteredMessage: "",
    }),
    methods: {
      async start() {
        try {
            await connection.start().then(()=> {
              if(connection.state == HubConnectionState.Connected) {
                console.log("connected");
              }
              connection.invoke("send", "ff")
            });

        } catch (err) {
            console.log(err);
        }
      },
      async sendMessage() {
        try {
          console.log("ee");
          axios.post('https://signalrrelaydwx.azurewebsites.net/api/messages/', {message: this.enteredMessage})
        } catch(err) {
          console.log(err);
        }
      }
     },
    beforeMount() {
        connection.onclose(this.start);

        this.start();
    },
    beforeDestroy() {
      connection.stop();
    }
  })
</script>
