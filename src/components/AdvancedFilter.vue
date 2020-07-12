<template>
  <div class="px-3">
    <div class="px-4 py-4 adv-filters">
      <h3>Advanced Filters</h3>
      <p class="text-justify pt-3"><strong>NOTE: </strong>{{sectionInfo}}</p>
      <b-input-group v-for="(item, parentIndex) in items" :key="item.id" class="mb-1 pt-4">
        <label>How many {{item.type}} produce the following mana?</label>
        <b-row>
          <b-col class="py-2 px-2 color-column" v-for="(color, index) in colors" :key="index" v-bind:class="color">
            <label class="text-capitalize">{{color}}</label>
            <b-form-input 
              v-on:click="advFilterClick(item, parentIndex)"
              v-model="item.totals[color]" 
              :aria-label="item-color" 
              placeholder="0"  
              type="number">
            </b-form-input>
          </b-col>
        </b-row>
      </b-input-group>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'AdvancedFilter',
    data() {
      return {
        items: [
         {
            id: 1,
            type: "lands",
            totals: {
              white: 0, 
              blue: 0, 
              black: 0, 
              red: 0, 
              green: 0, 
              colorless: 0
            },
          },
          {
            id: 2,
            type: "rocks",
            totals: {
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
            type: "dorks",
            totals: {
              white: 0, 
              blue: 0, 
              black: 0, 
              red: 0, 
              green: 0, 
              colorless: 0
            },
          }
        ],
        colors: [ "white", "blue", "black", "red", "green", "colorless"],
        sectionInfo: "In  the case of mana sources that can produce multiple types of mana colors (ex. Shock lands, Rainbow lands), increase the counter for each color that land could possibly produce (Fetch lands included). The colorless field is reserved for lands that only produce colorless mana.",
      }
    },
    methods: {

      printTotals(){ console.log(this.items); },

      advFilterClick(item, itemIndex){
        this.$emit('advFilterClick', item, itemIndex)
      }
    }
  }
</script>

<style scoped>
  .adv-filters {
    background-color: #f1faee;
    border-radius: 10px;
  }
  .color-column {
    border-radius: 10px;
    margin: .2em;
    color: white;
  }
  .white {
    background-color: #BDBDBD;
  }
  .blue {
    background-color: #29B6F6;
  }
  .black {
    background-color: #424242;
  }
  .red {
    background-color: #E53935;
  }
  .green {
    background-color: #43A047;
  }
  .colorless {
    background-color: #6D4C41;
  }
</style>
