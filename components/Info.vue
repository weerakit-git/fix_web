<template>
	<div>
		<div class="graph">
			<Barchart :data=items />
		</div><br>
		<div id="show_img" class="row align-items-start">
			<div class="row">
			<div v-for="(item, key, index) in images" class="col-6 row-sm-6 allimg">
				
				<h4>{{  key }}</h4>
				<!-- <h6>noir  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; rgb</h6> -->
				<img :src="item.noir" alt="" class="img-size">
				<!-- <p>rgb</p> -->
				<img :src="item.rgb" alt="" class="img-size">
				<!-- <p>ndvi</p> -->
				<img :src="item.ndvi" alt="" class="img-size">
				<br><br>
				
			</div>
		</div>
	</div>
</div>
</template>


<script>
export default {
	props: {
		id: Number,
	},
	watch: {
		id: function (newVal, oldVal) { // watch it
			this.fetchAPI(newVal)
			this.fetchImage(newVal)
			
		},
	},
	data() {
		return {
			items: [100, 100, 100, 100],
			images: {},
		};
	},
	mounted() {
		this.fetchAPI(this.id)
		this.fetchImage(this.id)
	},
	methods: {
		async fetchAPI(id) {
			const url = "http://192.168.1.100:8000/greenhouses/" + id
			const response = await this.$axios.$get(url)
			this.items = [response.temp, response.ec, response.humid, response.co2];
		},
		async fetchImage(id) {
			const url = "http://192.168.1.100:8000/get_info/" + id
			const response = await this.$axios.$get(url)
			this.images = response;
		}
	},
}
</script>

<style>
.col_icon {
	text-align: right;
}

.grid-container {
	display: grid;
	grid-template-columns: auto auto auto;

	padding: 40px;
}

.grid-item {
	background-color: rgba(255, 255, 255, 0.8);
	border: 1px solid rgba(0, 0, 0, 0.8);
	padding: 5px;
	font-size: 20px;
	text-align: center;
}

.back_button {
	text-align: right;
	font-size: 2rem;
}

.graph {
	padding: 100px;
	background-color: #fff;
	border-radius: 20px;
}

.img-size {
	width: 150px;
	height: 100px;
}
.align-items-start{
	margin-left: -6rem;
}
.allimg img{
	width: 7rem;
	height: 8rem;
	margin: 0.2rem;
}
.row align-items-start{
	padding: 100px;
	background-color: #fff;
	border-radius: 20px;
}
#show_img{
   padding-left: 110px;
   /* width: 0.5rem; */
}
</style>