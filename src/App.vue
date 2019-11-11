<template>
  <div id="app">
    <InfoEdit> </InfoEdit>
  </div>
</template>

<script>
import InfoEdit from './components/InfoEdit.vue'

export default {
  name: 'app',
  components: {
    InfoEdit
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
