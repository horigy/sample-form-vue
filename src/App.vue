<template>
  <div id="app">
    <CreateUser />
  </div>
</template>

<script>
import CreateUser from './components/CreateUser.vue'

export default {
  name: 'App',
  components: {
    CreateUser
  }
}
</script>

<style lang="sass">
#app
  font-family: Helvetica, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
</style>