<template>
    <div id="app">
      <b-container fluid="md" class="main-container">
        <img alt="EDH logo" src="./assets/MTG_commander.png">
        <BaseForm/>
        <AdvancedFilter/>
        <b-button block class="mt-3" variant="info">Calculate</b-button>
      </b-container>
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
  @import url('https://fonts.googleapis.com/css?family=Montserrat');

  #app {
    font-family: 'Montserrat', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    background-color:#34495E;
    padding:3em;
  }
  .main-container {
    background-color: #41B883;
    margin: auto;
    padding: 50px;
    border-radius: 25px;
  }
</style>
