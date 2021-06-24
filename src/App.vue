<template>
  <div id="app">
    <Plan/>
  </div>
</template>

<script>
import Plan from './components/Plan.vue'

export default {
  name: 'App',
  components: {
    Plan
  },
  data: () => ({
    list: {
      1: {
        type: 'INCOME',
        comment: 'Some income comment',
        id: 1,
      },
      2: {
        type: 'OUTCOME',
        comment: 'Some outcome comment',
        id: 2
      },
    }
  })
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
