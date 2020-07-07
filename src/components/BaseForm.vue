<template>
  <div class="px-3">
    <h2>{{appInfo.name}}</h2>
    <p>by {{appInfo.author}}</p>
    <div class="px-4 py-4 basic-filters">
      <p class="text-justify">{{appInfo.description}}</p>
      <p>Are you using partner commanders?</p>
      <b-button-group>
        <b-button variant="info">Yes</b-button>
        <b-button :pressed.sync="commanderFlag" variant="info">No</b-button>
      </b-button-group>
      <form class="pt-3">
        <b-row>
          <b-col v-for="item in items" :key="item[i]">
            <label><strong class="text-capitalize">{{item.type}}</strong> in deck</label><b-form-input v-on:click="printTotals()" v-model="item.total" :aria-label="item.type" placeholder="0"  type="number"></b-form-input>
          </b-col>
        </b-row>
      </form>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'BaseForm',
    data: function() {
      return {
        appInfo: {
          description: "This calculator allows you to see the average number of mana producing sources you should expect to see in your hand based on the number of cards drawn. The math is based on Hypergeometric Cumulative Probability ( P(X >= 1).",
          author: "Alex Koval",
          name: "Mana Base Calculator"
        },
        items: [
          { type:"lands", total: 0 },
          { type:"rocks", total: 0 },
          { type:"dorks", total: 0 }
        ],
        commanderFlag: false
      }
    },
    methods: {
      printTotals(){ console.log(this.items); },

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .basic-filters {
    background-color: #B2DFDB;
    border-radius: 10px;
  }
</style>
