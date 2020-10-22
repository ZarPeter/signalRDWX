<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <SignalR msg="Welcome to Your Vue.js + TypeScript App"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import SignalR from '@/components/SignalR.vue'; // @ is an alias to /src

@Component({
  components: {
    SignalR,
  },
})
export default class Home extends Vue {}
</script>
