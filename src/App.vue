<template>
  <div id="app">
    <testing-postman></testing-postman>
  </div>
</template>

<script>
import TestingPostman from './components/TestingPostman'

export default {
  name: 'App',
  components: {
    TestingPostman
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body{
    background: #dedede;
}
</style>
