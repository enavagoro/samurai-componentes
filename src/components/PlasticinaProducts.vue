<template>
	<div class="plasticina-products">
		<div class="card-container" v-for="web of webs" v-bind:key="web.id">
			<a v-bind:href="web.url" target="_blank">
				<!-- :href="web.url"-->
				<!-- *ngFor="let enterprise of enterprises; index as i" -->
				<div class="card">
					<!--  (click)="enterpriseOptions(i)" -->
					<div class="image-card-container">
						<img class="image-card" src="https://upload-icon.s3.amazonaws.com/uploads/icons/png/5772763941542186926-512.png">
          </div>
						<h1 class="card-title">{{web.name}}</h1> <!-- {{enterprise.name}} -->
						<h1 class="card-subtitle"></h1> <!-- {{enterprise.direccion}} -->
					</div>
			</a>
		</div>

        <div class="paginator-container">
            <PlasticinaPagination :total-pages="12" />
        </div>
	</div>
</template>

<script>
    import PlasticinaPagination from '@/components/PlasticinaPagination.vue'

    export default {
        name: 'PlasticinaProducts',
        components: {
            PlasticinaPagination,
        },
        data: function (){
            return {
            webs: [ {id:1,name:'Majus',url:'https://vase.cl/majus/'},
                    {id:2,name:'Cometazul',url:'http://cometazul.com/'},
                    {id:3,name:'Jazmín',url:'https://vase.cl/jazmin/'},
                    {id:4,name:'Fisalis',url:'https://vase.cl/fisalis/'}],
            };
        },
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

	@media (max-width:900px) {
		.card-container {
			margin-top: 15%;
			display: block;
		}

		.card {
			width: 80%;
			margin-left: 10%;
			height: 310px;
		}
	}    
</style>

