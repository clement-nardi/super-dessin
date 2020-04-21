<template>
  <div id="app">
    <div>Hello Romain!</div>
    <div>Hello Amandine!</div>
    <SuperDessin v-bind:width="800" v-bind:height="800"/>
  </div>
</template>

<script>
import SuperDessin from './components/SuperDessin.vue'

export default {
  name: 'App',
  components: {
    SuperDessin
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
</style>
