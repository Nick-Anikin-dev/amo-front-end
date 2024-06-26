<template>
  <LeadsTable msg="Welcome to Your Vue.js App"/>
</template>
<script>
import LeadsTable from './components/LeadsTable.vue'

export default {
  name: 'App',
  components: {
    LeadsTable
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
  margin: 60px;
}
</style>
