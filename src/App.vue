<template>
  <div id="app">
    <TodoApp />
  </div>
</template>

<script>
import TodoApp from './components/TodoApp.vue'

export default {
  name: 'App',
  components: {
    TodoApp
  }
}
</script>

<style lang="scss" scoped>

#app{
  font-family: sans-serif;
}

</style>