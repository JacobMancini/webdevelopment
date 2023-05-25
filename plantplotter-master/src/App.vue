<template>
  <PlantsPage />
  <br>
  <DrawPage />
</template>

<script>
import DrawPage from './components/DrawPage.vue'
import PlantsPage from './components/PlantsPage.vue'

export default {
  name: 'App',
  components: {
    DrawPage,
    PlantsPage,
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
