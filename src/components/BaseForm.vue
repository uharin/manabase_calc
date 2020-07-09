<template>
  <div class="px-3">
    <h2>{{appInfo.name}}</h2>
    <p>by {{appInfo.author}}</p>
    <div class="px-4 py-4 basic-filters">
      <b-button class="my-4" variant="primary">TEST</b-button>
      <p class="text-justify">{{appInfo.description}}</p>
      <p>Are you using partner commanders?</p>
      <b-button-group>
        <b-button :pressed="partnerFlag" v-on:click="togglePartnerFlag" variant="info">Yes</b-button>
        <b-button :pressed="!partnerFlag" v-on:click="togglePartnerFlag" variant="info">No</b-button>
      </b-button-group>
      <form class="pt-3">
        <b-row>
          <b-col v-for="item in items" :key="item.id">
            <label><strong class="text-capitalize">{{item.type}}</strong> in deck</label>
            <b-form-input 
              v-on:click="printTotals()" 
              v-model="item.total" 
              :aria-label="item.type" 
              placeholder="0"  
              type="number">
            </b-form-input>
          </b-col>
        </b-row>
      </form>
    </div>
    <AdvancedFilter v-on:advFilterClick="onChildClick"/>
    <b-button block class="mt-3" variant="info">Calculate</b-button>
  </div>
</template>

<script>
  import AdvancedFilter from './AdvancedFilter.vue';

  export default {

    name: 'BaseForm',
    components: { AdvancedFilter },
    data: function() {
      return {
        appInfo: {
          description: "This calculator allows you to see the average number of mana producing sources you should expect to see in your hand based on the number of cards drawn. The math is based on Hypergeometric Cumulative Probability ( P(X >= 1).",
          author: "Alex Koval",
          name: "Mana Base Calculator"
        },
        items: [
          { id: 1, type:"lands", total: 0 },
          { id: 2, type:"rocks", total: 0 },
          { id: 3, type:"dorks", total: 0 }
        ],
        partnerFlag: false,
        fromChild: '',
        counter: 0 
      }
    },

    methods: {

      onChildClick(value){
          console.log("value ",  value)
      },

      printTotals(){ console.log(this.items); },

      togglePartnerFlag(){ this.partnerFlag = !this.partnerFlag; },

      calculateManaProductionAverages() {
            // Calculates the average that each mana source of its type can produce each type of mana
            // This only is relevant if the user filled in the Advanced Fields
            for (let type in this.color_production_totals) {
                for (let color in this.color_production_totals[type]) {
                    if (parseInt(this.color_production_totals[type][color]) > 0) {
                        this.show_advanced_filter_results_flag = true;
                        this.color_production_averages[type][color] = ((parseInt(this.color_production_totals[type][color]) / parseInt(this.copy_count[type])) * 100).toFixed(2)
                    }
                }
            }
        },
        hyp(x, cards_drawn, total_copies, deck_size) {
            // Calculation formula derived from https://gist.github.com/adamnovak/f34e6cf2c08684752a9d
            let smaller_set,
                larger_set;

            // Make sure copies is fewer than cards drawn
            if (total_copies < cards_drawn) {
                smaller_set = total_copies;
                larger_set = cards_drawn
            } else {
                smaller_set = cards_drawn;
                larger_set = total_copies;
            }

            let h = 1,
                s = 1,
                k = 0,
                i = 0;

            // Refer to Gist above for deeper explanation on how this calculation works
            while (i < x) {
                while (s > 1 && k < smaller_set ) {
                    h = h * (1 - larger_set / (deck_size - k));
                    s = s * (1 - larger_set / (deck_size - k));
                    k = k + 1;
                }
                h = h * (smaller_set - i) * (larger_set - i) / (i + 1) / (deck_size - smaller_set - larger_set + i + 1);
                s = s + h;
                i = i + 1;
            }
            while (k < smaller_set ) {
                s = s * (1 - larger_set / (deck_size - k));
                k = k + 1;
            }

            return s;
        }
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
