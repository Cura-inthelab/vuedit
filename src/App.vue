<template>
  <div id="app">
    <command-menu></command-menu>
  </div>
</template>

<script>
import CommandMenu from './components/CommandMenu'

export default {
  components: {CommandMenu},
  name: 'App'
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
</style>
