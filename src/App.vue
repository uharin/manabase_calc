<template>
    <div id="app">
      <b-container fluid="md" class="main-container">
        <BaseForm/>
      </b-container>
    </div>
</template>

<script>
  import BaseForm from './components/BaseForm.vue';

  export default {
    name: 'App',
    components: { BaseForm }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Montserrat');

  #app {
    font-family: 'Montserrat', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #1d3557;
    background-color:#1d3557;
    padding:3em;
  }
  .main-container {
    background-color: #a8dadc;
    margin: auto;
    padding: 50px;
    border-radius: 25px;
  }
</style>
