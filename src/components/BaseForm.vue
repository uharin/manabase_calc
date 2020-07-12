<template>
  <div class="px-3">
    <div class="title py-4 my-3">
      <img alt="EDH logo" src="../assets/MTG_commander.png">
      <h2>{{appInfo.name}}</h2>
      <p>by {{appInfo.author}}</p>
      <b-button variant="danger" v-if="fieldsDisabled" v-on:click="toggleFields()">Show Filters</b-button>
    </div>
    <div v-if="!fieldsDisabled">
      <div class="px-4 py-4 basic-filters">
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
                v-model="item.count" 
                :aria-label="item.type" 
                placeholder="0"  
                type="number">
              </b-form-input>
            </b-col>
          </b-row>
        </form>
      </div>
      <b-button class="my-4" variant="danger" v-on:click="toggleAdvFilters()">{{advFilterStatus}} Advanced Filters</b-button>
      <AdvancedFilter v-if="showAdvFilters" v-on:advFilterClick="updateColorTotal"/>
    </div>
    <b-button block class="mt-3" variant="info" v-on:click="calculatePercentPerDraw()">Calculate</b-button>
    <Results :totalCardsDrawn="totalCardsDrawn" :items="items" :colors="colors" />
    <b-button variant="primary" class="my-4" block v-on:click="calculatePercentPerDraw()">Show Next Draw</b-button>
  </div>
</template>

<script>
  import AdvancedFilter from './AdvancedFilter.vue';
  import Results from './Results.vue';

  export default {

    name: 'BaseForm',
    components: { AdvancedFilter, Results },
    data: function() {
      return {
        appInfo: {
          description: "This calculator allows you to see the average number of mana producing sources you should expect to see in your hand based on the number of cards drawn. The math is based on Hypergeometric Cumulative Probability ( P(X >= 1).",
          author: "Alex Koval",
          name: "Mana Base Calculator"
        },
        advFilterStatus: "Show",
        calculationIsCompleted: false,
        cardsInDeck: 99,
        colors: ["White", "Blue", "Black", "Red", "Green", "Colorless"],
        fieldsDisabled: false,
        partnerFlag: false,
        showAdvFilters: false,
        showAdvFilterResults: false,
        showDetails: [],
        totalCardsDrawn: 0,
        items: [
          { 
            id: 1, 
            type:"lands", 
            count: 0, 
            results: [],
            resultsTypeAverage: 0,
            averages: {
              white: 0,
              blue: 0,
              black: 0,
              red: 0,
              green: 0,
              colorless: 0},
          },
          { 
            id: 2, 
            type:"rocks", 
            count: 0, 
            results: [],
            resultsTypeAverage: 0,
            averages: {
              white: 0,
              blue: 0,
              black: 0,
              red: 0,
              green: 0,
              colorless: 0
            },
          },
          { 
            id: 3, 
            type:"dorks", 
            count: 0, 
            results: [],
            resultsTypeAverage: 0,
            averages: {
              white: 0,
              blue: 0,
              black: 0,
              red: 0,
              green: 0,
              colorless: 0
            }
          }
        ],
      }
    },

    methods: {

      toggleAdvFilters(){ 
        this.showAdvFilters = !this.showAdvFilters;
        this.showAdvFilters ? this.advFilterStatus = "Hide" : this.advFilterStatus = "Show";
      },

      togglePartnerFlag(){ this.partnerFlag = !this.partnerFlag; },

      toggleFields(){ this.fieldsDisabled = !this.fieldsDisabled; },

      updateColorTotal(values, itemIndex){
        let editedItem = this.items[itemIndex];
        Object.assign(editedItem, values);
        return editedItem;
      },



      calculatePercentPerDraw() {

        // Calculate the probability for the current draw

        // The first time calculating the probability calculate it for an opening hand of 7
        if (this.totalCardsDrawn === 0) {
          this.totalCardsDrawn = 7;

          // Calculate mana-production averages
          this.calculateManaProductionAverages();

          // Hide the filters for better result-viewing
          this.showAdvFilterResults = false;
        }

        // tracks total cards drawn in this instance of simulation 
        let cardsDrawn = this.totalCardsDrawn;


        for(let item of this.items){

          // store results of lands, rocks, dorks
          let tempResults = [];
          // store rounded value of possibilty for type in hand
          let tempResultsTypeAverage = 0;

          tempResults[item.type] = [];
        
          for (let j = 1; j <= 10; j++) {
            let currentResult = 1 - this.calculateHypDist(j - 1, cardsDrawn, parseInt(item.count), parseInt(this.cardsInDeck));

            if (currentResult < 1e-6) {
              currentResult = 0;
            }

            // Since we aren't calculating for 0 cards drawn, reset array to normal indexes
            let roundedResult = (currentResult * 100).toFixed(2);
            tempResults[item.type][j - 1] = roundedResult;

            if (roundedResult > 50) {
              tempResultsTypeAverage = j;
            }
          }

          item.results = tempResults;
          item.resultsTypeAverage = tempResultsTypeAverage;
        }

        console.log("ITEMS ARE ", this.items);
        
        if (this.fieldsDisabled === false) { this.fieldsDisabled = true }

        this.showDetails.push(false);
        this.totalCardsDrawn++;
        this.calculationIsCompleted = true;

      },

      calculateManaProductionAverages() {

        // Calculates the average that each mana source of its type can produce each type of mana
        // This only is relevant if the user filled in the Advanced Fields

        for (let item of this.items) {
          for (let color in item.totals) {
            if (parseInt(item.totals[color]) > 0) {
              
              this.showAdvFilterResults = true;

              item.averages[color] = ((parseInt(item.totals[color]) / parseInt(item.count)) * 100).toFixed(2)
            }
          }
        }
      },

      calculateHypDist(x, cards_drawn, total_copies, deck_size) {

        // Calculation formula derived from https://gist.github.com/adamnovak/f34e6cf2c08684752a9d

            let smaller_set, larger_set;

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
    background-color: #f1faee;
    border-radius: 10px;
  }
  .title {
    background-color: #a8dadc;
    border-radius: 10px;
  }
</style>
