<template>
  <div id="app">
    <ThreeJs />
  </div>
</template>

<script>
import ThreeJs from './components/ThreeJs.vue'

export default {
  name: 'App',
  components: {
    ThreeJs
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
  margin-top: 60px;
}
</style>
