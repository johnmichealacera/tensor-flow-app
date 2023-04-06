<template>
  <image-scanner />
</template>

<script>
import ImageScanner from './components/ImageScanner.vue'

export default {
  name: 'App',
  components: {
    ImageScanner
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
}
</style>
