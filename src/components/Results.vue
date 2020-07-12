<template>
    <div class="my-4" id="results">
			<b-button variant="info" v-on:click="toggleManaProductionPane()">{{manaProductionStatus}} Mana Production Averages</b-button>
			<p class="py-3">After drawing {{totalCardsDrawn}} cards, on average you will have</p>
			<b-row class="flex f-row">
				<div class="summary-box f-column" 
						v-for="item in items"
						:key="item.id"
						:class="{
							blue: item.type === 'lands', 
							yellow: item.type === 'rocks',
							green: item.type === 'dorks'}">
					<p class="display-4">{{item.resultsTypeAverage}}</p>
					<p class="text-capitalize">{{item.type}}</p> 
				</div>
			</b-row>
			<b-button variant="danger" class="my-4" v-on:click="toggleDetailsPane()">{{showDetailsStatus}} Details</b-button>
			<table class="table table-striped my-3" v-if="showDetails">
				<thead >
					<tr>
						<th scope="col">Type</th>
						<td scope="col">1</td>
						<td scope="col">2</td>
						<td scope="col">3</td>
						<td scope="col">4</td>
						<td scope="col">5</td>
						<td scope="col">6</td>
						<td scope="col">7</td>
						<td scope="col">8</td>
						<td scope="col">9</td>
						<td scope="col">10</td>
					</tr>
				</thead>
				<tbody>
					<tr v-for="item in items" :key="item.id">
						<th scope="row" class="text-capitalize">{{item.type}}</th>
						<td v-for="result in item.results[item.type]" :key="result" >{{result}}%</td>
					</tr>
				</tbody>
			</table>
    </div>
</template>

<script>
  export default {
		name: 'Results',
		props: { 
			items: Array,
			colors: Array,
			totalCardsDrawn: Number 
		},
		data: function() {
      return {
				showDetails: false,
				showDetailsStatus: "Show",
				manaProductionDetails: false,
				manaProductionStatus: "Show"
			}
    },

    methods: {

      toggleDetailsPane(){
				this.showDetails = !this.showDetails;
				this.showDetails ? this.showDetailsStatus = "Hide" : this.showDetailsStatus = "Show";
			},
			
			toggleManaProductionPane(){
				this.manaProductionDetails = !this.manaProductionDetails;
				this.manaProductionDetails ? this.manaProductionStatus = "Hide" : this.manaProductionStatus = "Show";
      }
    }
  }
</script>

<style>
	.flex {
		display: flex;
		align-items: center;
		justify-content: space-evenly;
	}

	.f-row {
		flex-direction: row;
	}

	.f-column {
		flex-direction: column;
	}

	.green { background: #158c51; }
	.yellow { background: #ceae1a; }
	.blue { background: #205a86; }

	.summary-box {
		width: 27%;
		height: 125px;
		border-radius: 5px;
		color: white;
		box-shadow: 0px 1px 4px 0px rgba(16, 17, 25, 0.6);
	}


</style>
