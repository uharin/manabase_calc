<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <BaseForm msg="Welcome to Your Vue.js App"/>
    <AdvancedFilter/>
  </div>
</template>

<script>
  import BaseForm from './components/BaseForm.vue';
  import AdvancedFilter from './components/AdvancedFilter.vue';

  export default {
    name: 'App',
    components: { BaseForm, AdvancedFilter }
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
