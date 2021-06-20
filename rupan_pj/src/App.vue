<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <RupanSubtitle />
  </div>
</template>

<script>
import RupanSubtitle from './components/RupanSubtitle.vue'

export default {
  name: 'App',
  components: {
    RupanSubtitle
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #000000;
  margin-top: 60px;
}
</style>
