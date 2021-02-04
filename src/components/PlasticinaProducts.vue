<template>
	<div class="plasticina-products">
		<div class="card-container" v-for="pokemon of pokemons" v-bind:key="pokemon.id">
			<a v-bind:href="pokemon.url" target="_blank">
				<!-- :href="web.url"-->
				<!-- *ngFor="let enterprise of enterprises; index as i" -->
				<div class="card">
					<!--  (click)="enterpriseOptions(i)" -->
					<div class="image-card-container">
						<img class="image-card" src="https://upload-icon.s3.amazonaws.com/uploads/icons/png/5772763941542186926-512.png">
          </div>
						<h1 class="card-title">{{pokemon.name}}</h1> <!-- {{enterprise.name}} -->
						<h1 class="card-subtitle"></h1> <!-- {{enterprise.direccion}} -->
					</div>
			</a>
		</div>

			<!--
		    <textarea
				ref="console"
				style="font-family: inerhit; min-width: 320px; max-width: 640px; width: 100%"
			/>
			-->

        <div class="paginator-container">
            <SamuraiCategoryPagination :total-pages="1000" @send-message="handleSendMessage" />
        </div>
	</div>
</template>

<script>
	import PlasticinaPagination from '@/components/PlasticinaPagination.vue';
	import SamuraiCategoryPagination from '@/components/SamuraiCategoryPagination.vue';
	import axios from 'axios';

    export default {
        name: 'PlasticinaProducts',
        components: {
            SamuraiCategoryPagination,
        },
        data: function (){
            return {
			pokemons: [],
			limit: 10,
			offset: 10,
			page: 1
            };
		},
		methods:{
			chargeData:function(limit,offset){
				axios.get('https://pokeapi.co/api/v2/pokemon?limit='+limit+'&offset='+offset)
					.then(response => {
						this.pokemons = response.data.results;
						console.log('response',response)
					})
					.catch(e => {
						this.errors.push(e)
					})
			},
			handleSendMessage(value) {+ 
				// Our event handler gets the event, as well as any
				// arguments the child passes to the event
				this.chargeData(this.limit,this.offset * value);
			}
		},
		/*
		mounted(){
			this.page = parseInt(this.$route.query[this.pageParameter], 10) || 1;
			console.log('que pasa',this.page);
			
		},
		 watch: {
			$route(to) {
				this.page = parseInt(to.query[this.pageParameter], 10) || 1;
				console.log('-----page',this.page);
				this.chargeData(this.limit,this.offset * this.page);
			},
		},*/
		/*
		created(){
			//inicial charge
			if(this.page==1){
				this.chargeData(this.limit,this.offset * this.page);
			}
		},
		*/
		
    }
</script>

<style scoped>
    .plasticina-products{
        width: 95%;
		margin-left: 2.5%;
    }

    .card-container {
		margin-top: 1.5%;
		display: inline-flex;
	}

	.card {
		width: 230px;
		height: 250px;
		background: white;
		margin-left: 50px;
		margin-top: 3%;
		margin-bottom: 2%;
		padding-top: 1%;
		padding-bottom: 2.5%;
		border-radius: 10px;
		-webkit-box-shadow: 0px 2px 2px 2px rgba(184, 184, 184, .5);
		-moz-box-shadow: 0px 2px 2px 2px rgba(184, 184, 184, .5);
		box-shadow: 0px 2px 2px 2px rgba(184, 184, 184, .5);
		cursor: pointer;
		transition: .5s;
	}

	.card:hover {
		transition: .5s;
		transform: scale(1.1);
	}

	.item-card {
		border-radius: 10px;
	}

	.card-title {
		margin-top: 10%;
		font-family: 'Roboto', sans-serif;
		font-weight: 600;
		font-size: 20px;
		color: #313158;
		text-align: center;
	}

	.card-subtitle {
		margin-top: 10%;
		font-size: 14px;
		color: #616161;
		text-align: center;
	}

	.image-card-container {
		width: 50%;
		margin-top: 5%;
		margin-bottom: 5%;
		margin-left: 25%;
	}

	.image-card {
		width: 80%;
		margin-left: 10%;
	}

	a {
		text-decoration: none;
	}

	.paginator-container{
		padding-top: 10%;
		padding-bottom: 10%;
		width: 70%;
		margin-left: 15%;
	}

	@media (max-width:1000px) {
		.card-container {
			margin-top: 15%;
			display: block;
		}

		.card {
			width: 80%;
			margin-left: 10%;
			height: 310px;
		}

		.paginator-container{
			padding-top: 10%;
			padding-bottom: 10%;
			width: 100%;
			margin-left: 0%;
		}
	}    
</style>

