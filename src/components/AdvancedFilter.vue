<template>
  <div class="px-3">
    <b-button class="my-4" variant="danger" v-on:click="toggleAdvFilters()">{{advFilterStatus}} Advanced Filters</b-button>
    <div v-if="showAdvancedFilters" class="px-4 py-4 adv-filters">
      <h3>Advanced Filters</h3>
      <p class="text-justify pt-3"><strong>NOTE: </strong>{{sectionInfo}}</p>
      <b-input-group v-for="item in items" :key="item[i]" class="mb-1 pt-4">
        <label>How many {{item}} produce the following mana?</label>
        <b-row>
          <b-col class="text-left py-2 px-2 color-column" v-for="color in colors" :key="color[i]" v-bind:class="color">
            <label class="text-capitalize">{{color}}</label><b-form-input :aria-label="item-color" placeholder="0"  type="number"></b-form-input>
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
        colors: ["white", "blue", "black", "red", "green", "colorless"],
        items: ["lands", "rocks", "dorks"],
        sectionInfo: "In the case of mana sources that can produce multiple types of mana colors (ex. Shock lands, Rainbow lands), increase the counter for each color that land could possibly produce (Fetch lands included). The colorless field is reserved for lands that only produce colorless mana.",
        showAdvancedFilters: false,
        advFilterStatus: "Show"
      }
    },
    methods: {
      toggleAdvFilters(){ 
        this.showAdvancedFilters = !this.showAdvancedFilters;
        this.showAdvancedFilters ? this.advFilterStatus = "Hide" : this.advFilterStatus = "Show";
      },
    }
  }
</script>

<style scoped>
  .adv-filters {
    background-color: #B2DFDB;
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
