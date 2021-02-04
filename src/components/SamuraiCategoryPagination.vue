<template>
    <div class="samurai-category-pagination-container">
        <div class="left-part">
            <router-link class="link" v-if="currentPage > firstValue" :to="previousPage(currentPage)"><h1 class="text-paginator-previous">Anterior</h1></router-link>
        </div>
        <div class="center-part">
            <div class="square-container">
                <router-link class="link" :to="selectPage(firstValue)">
                <div class="square" v-if="firstValue!=currentPage">
                    <h1 class="square-number">{{firstValue}}</h1>
                </div>
                <div class="square-selected" v-if="firstValue==currentPage">
                    <h1 class="square-number">{{firstValue}}</h1>
                </div>
                </router-link>
            </div>
            <div class="square-container" v-for="(page ,index) of filterPages()" v-bind:key="index">
                <router-link class="link" :to="selectPage(page)">
                <div class="square" v-if="page!=currentPage">
                    <h1 class="square-number">{{page}}</h1>
                </div>
                <div class="square-selected" v-if="page==currentPage">
                    <h1 class="square-number">{{page}}</h1>
                </div>
                </router-link>
            </div>
            <div class="square-container">
                <router-link class="link" :to="selectPage(lastValue)">
                <div class="square" v-if="lastValue!=currentPage">
                    <h1 class="square-number">{{lastValue}}</h1>
                </div>
                <div class="square-selected" v-if="lastValue==currentPage">
                    <h1 class="square-number">{{lastValue}}</h1>
                </div>
                </router-link>
            </div>
        </div>
        <div class="right-part">
            <router-link class="link" v-if="currentPage < lastValue" :to="nextPage(currentPage)"><h1 class="text-paginator-last">Siguiente</h1></router-link>
        </div>
        <!--
        
        <div class="square-container">
            <router-link class="square-link">1</router-link>
        </div>
        
        -->
    </div>
</template>

<script>
export default {
  name: 'SamuraiCategoryPagination',
  data() {
    return {
      currentPage: 0,
      firstValue: 0,
      lastValue: 0,
      pages: [],
    };
  },
 props:{
    totalPages: {
      type: Number,
      required: true,
    },
    pageParameter: {
      type: String,
      default: 'page',
    },
 },
 methods:{
    nextPage(currentPage){
        console.log('router',this.$route.query);
        if(currentPage <= this.totalPages){
            return { //retorna un objeto con una propiedad query
                query: {
                ...this.$route.query,
                [this.pageParameter]: currentPage + 1,
                },
            };
        }
        else{
            alert('para hombre no puedes seguir subiendo')
        }
    },
    selectPage(page){
            return {
                query: {
                ...this.$route.query,
                [this.pageParameter]: page,
                },
            };
    },
    previousPage(currentPage) {
        if(currentPage >= 1){
            return {
                query: {
                ...this.$route.query,
                [this.pageParameter]: currentPage - 1,
                },
            };
        }
        else{
            alert('para hombre no puedes seguir bajando :V ')
        }
    },
    filterPages(){
        var tempArray = [];
        console.log('currentPage',this.currentPage);
        var actualValue = this.currentPage - 2;
        
        if(this.currentPage == this.firstValue){
            tempArray = this.pages.slice(0,2);
            return tempArray
        }
        else if(this.currentPage == this.lastValue) {
            tempArray = this.pages.slice(this.pages.length - 2 ,this.pages.length);
            return tempArray
        }
        else{
            if(this.pages[actualValue - 1]){
                tempArray.push(this.pages[actualValue - 1])
            }
            tempArray.push(this.pages[actualValue]);        
            if(this.pages[actualValue + 1]){
                tempArray.push(this.pages[actualValue + 1])
            }       
            return tempArray
        }
    }
 },
 created(){     
     for(let i = 1 ; i < this.totalPages - 1 ; i++){
         this.pages.push(i+1);
     }
 },
 mounted(){
    this.firstValue = 1;
    this.lastValue = this.totalPages;
    this.currentPage = parseInt(this.$route.query[this.pageParameter], 10) || 1;
    if(this.currentPage >= this.firstValue && this.currentPage <= this.lastValue ){
        this.$emit('send-message', this.currentPage);
    }
    
},
 watch: {
    $route(to) {
      this.currentPage = parseInt(to.query[this.pageParameter], 10) || 1;
      if(this.currentPage >= this.firstValue && this.currentPage <= this.lastValue ){
        this.$emit('send-message', this.currentPage);
      }
    },
 },
};
</script>

<style scoped>

    @import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;1,100;1,200;1,300&display=swap');

    /* Contenedor Principal*/

    .samurai-category-pagination-container{
        display: flex;
        width: 100%;
        height: 100px;
    }

    /* Parte Izquierda (El botón que dice anterior) */

    .left-part{
        width: 100%;
    }

    .text-paginator-previous{
        text-align: right;
        font-family: Poppins;
        font-size: 14px;
        padding-top: 8px;
        font-weight: 500;
    }

    /* Los numeros de la Páginación */

    .center-part{
        width: 100%;
    }

    .square-container{
        padding: 0;
        margin-left: 5px;
        margin-top: 13px;
        min-width: 34px;
        min-height: 34px;
        display: inline-flex;
    }

    .link{
        text-decoration: none;
        color: black;
    }

    .square{
        min-width: 34px;
        min-height: 34px;
        border-radius: 5px;
        background:transparent;
        color: black;
    }

    .square:hover{
        background: #e9ecef;       
    }

    .square-selected{
        min-width: 34px;
        min-height: 34px;
        border-radius: 5px;
        background: black;
        color: white;
    }

    .square-number{
        padding: 20%;
        margin: 0%;
        font-family: Poppins;
        font-size: 14px;
        font-weight: 500;
        text-align: center;        
    }
    /* Parte Derecha (El botón que dice siguiente) */

    .right-part{
        width: 100%;
    }

    .text-paginator-last{
        text-align: left;
        font-family: Poppins;
        font-size: 14px;
        padding-top: 8px;
        font-weight: 500;
    }

    @media (max-width:1000px){
        .left-part{
            width: 15%;
            margin-right: 0%;
            margin-left: 0%;
        }
        .center-part{
            width: 70%;
        }   
        .right-part{
            width: 15%;
            margin-right: 0%;
            margin-left: 0%;
        }
    }
</style>