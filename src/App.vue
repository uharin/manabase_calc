<template>
    <div id="app">
      <b-container fluid="md" class="main-container">
        <img alt="EDH logo" src="./assets/MTG_commander.png">
        <BaseForm/>
        <Results/>
      </b-container>
    </div>
</template>

<script>
  import BaseForm from './components/BaseForm.vue';
  import Results from './components/Results.vue';

  export default {
    name: 'App',
    components: { BaseForm, Results }
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
