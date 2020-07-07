<template>
  <div>
    <h1>Mana Base Calculator</h1>
    <h2>by Alex Koval</h2>
    <div>
      <p>This calculator allows you to see the average number of mana producing sources you should expect to see in your hand based on the number of cards drawn. The math is based on Hypergeometric Cumulative Probability ( P(X >= 1 ).</p>
      <p>Are you using partner commanders?</p>
      <button>Yes</button><button>No</button>
      <form>
        <label class="label" for="land-count">Lands in deck:</label>
        <input type="number" id="land-count" class="form-control"/>
        <label class="label" for="land-count">Lands in deck:</label>
        <input type="number" id="land-count" class="form-control"/>
        <label class="label" for="land-count">Lands in deck:</label>
        <input type="number" id="land-count" class="form-control"/>
      </form>
      <b-button>Show Advanced Filters</b-button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'BaseForm',
    props: {
      msg: String
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
