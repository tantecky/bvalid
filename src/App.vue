<template>
  <Validator />
</template>

<script>
import Validator from './components/Validator.vue'

export default {
  name: 'App',
  components: {
    Validator,
  },
}
</script>

<style>
*,
*::after,
*::before {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  position: relative;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
