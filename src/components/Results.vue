<template>
    <div id="results">
			<p>After drawing 7 cards, on average you will have</p>
			<b-row class="flex f-row">
				<div class="summary-box f-column" style="background: #205a86;"><p class="display-4">1</p><p>Lands</p></div>
				<div class="summary-box" style="background: #ceae1a;"><p class="display-4">2</p><p>Artifacts</p></div>
				<div class="summary-box" style="background: #158c51;"><p class="display-4">3</p><p>Dorks</p></div>
			</b-row>
			<b-button variant="danger" class="my-4" v-on:click="toggleDetailsPane()">{{showDetailsStatus}} Details</b-button>
			<table v-if="showDetails" class="my-3">
				<thead class="table table-striped">
					<tr>
						<th scope="col">Source</th>
						<th scope="col">White</th>
						<th scope="col">Blue</th>
						<th scope="col">Black</th>
						<th scope="col">Red</th>
						<th scope="col">Green</th>
						<th scope="col">Colorless Only</th>
					</tr>
					<tbody>
						<tr>
							<th scope="row">Lands</th>
							<td>chance</td>
						</tr>
						<tr>
							<th scope="row">Mana Rocks</th>
							<td >chance</td>
						</tr>
						<tr>
							<th scope="row">Dorks</th>
							<td >chance</td>
						</tr>
						</tbody>
				</thead>
			</table>
			<b-button variant="primary" class="my-4" block>Show Next Draw</b-button>
    </div>
</template>

<script>
  export default {
		name: 'Results',
		data: function() {
      return {
				showDetails: false,
				showDetailsStatus: "Show"
			}
    },

    methods: {

      toggleDetailsPane(){
				this.showDetails = !this.showDetails;
				this.showDetails ? this.showDetailsStatus = "Hide" : this.showDetailsStatus = "Show";
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

	.summary-box {
		width: 27%;
		height: 125px;
		border-radius: 5px;
		color: white;
		box-shadow: 0px 1px 4px 0px rgba(16, 17, 25, 0.6);
	}


</style>
