<template>
  <div id="app">  
    <navbar-kanban></navbar-kanban>
    <router-view></router-view>
  </div>
</template>

<script>
import NavbarKanban from '@/components/NavbarKanban'
export default {
  name: 'app',
  components: {
    NavbarKanban
  }
}
</script>

<style>
#app {
  /* font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px; */
}
</style>
