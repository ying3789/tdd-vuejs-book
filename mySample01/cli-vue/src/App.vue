<template>
  <div id="app">
    <MyClient/>
  </div>
</template>

<script>
import MyClient from './components/MyClient.vue'

export default {
  name: 'App',
  components: {
    MyClient
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  /* color: #2c3e50; */
}
</style>
