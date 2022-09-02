<template>
<div  id="app" >
	<div class="container text-start">
		<div class="data-ch">
			<Linechart :data=items />
		</div>
		<br>
		<h4> รายละเอียดพืชแต่ละครั้ง </h4>

		<div id="list_oder" class="row align-items-start">
			<div class="col">
				<div class="row">
					<div v-for="(item, index) in items">
						<button type="button" class="i text-muder fw-lighter fs-6 mt-0 btn btn-lg btn-primary"  v-on:click="say(item.id)"
							style="margin-top: 10px; padding-right: 40px;">
							ครั้งที่ - {{ item.id }} - {{ item.created_at }}
							</button>
						</div>
					</div>
					<div id="button_selector">
				<button style="background-color: brown;">one</button>
				<button style="background-color: palevioletred;">two</button>
				<button style="background-color: skyblue;">three</button>
				<button style="background-color: greenyellow;">four</button>
				</div>

			</div>
			
			<div class="col">
				<Info :id=id />
			</div>
		</div>

		<br>
		<br>

		<div id="bt-back" class="black_button">
			<NuxtLink to="/"> Back </NuxtLink>
		</div>


	</div>
</div>
</template>



<script>
export default {
	data() {
		return {
			items: [],
			id: 1,
		};
	},
	mounted() {
		this.fetchAPI()
	},
	methods: {
		async fetchAPI() {
			const url = "http://192.168.1.100:8000/greenhouses/?skip=0&limit=1000";
			const response = await this.$axios.$get(url)

			this.items = response;
		},
		say: function (id) {
			this.id = id
		}
	},
};
</script>



<style>
#button_selector{
	background-color: #4CAF50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}	
.tital {
	text-align: center;
	color: brown;
}

.black_button {
	text-align: right;
	font-size: 30px;
}

.info_plant_list {
	font-size: 20px;

}

* {
	margin: 0;
	padding: 0;

}
#app{
	background-color: #dbdbdb;
}
.data-ch{
	background-color: #fff;
	border-radius: 20px;
	
}
h4{
	border-style: solid;
	background-color: #fff;
	padding: 10px;
	border-radius: 10px;
	width: 20rem;
	text-align: center;
}
#list_oder{
	padding-left: 110px;
}
#bt-back{
	background-color: #fff;
	border-radius: 10px;
	width: 100px;
	text-align: center;
	margin-bottom: 20px;
	margin-left: 80rem;

}
.i{
	color: #fff;
	
	padding: 3px;
	margin-bottom: 0.4rem;	
}



</style>




