<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <index></index>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import index from './views/index/index'

export default {
  name: 'App',
  components: {
    index
  }
}
</script>

<style>
#app {
  
}
</style>
