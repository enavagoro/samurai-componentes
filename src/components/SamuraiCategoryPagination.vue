<template>
    <div class="samurai-category-pagination-container" :style="styles">
        <div class="samurai-category-pagination">
            <div class="left-part desktop">
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
            <div class="right-part desktop">
                <router-link class="link" v-if="currentPage < lastValue" :to="nextPage(currentPage)"><h1 class="text-paginator-last">Siguiente</h1></router-link>
            </div>
            <div class="links-phone phone">
                <div class="left-part">
                    <router-link class="link" v-if="currentPage > firstValue" :to="previousPage(currentPage)"><h1 class="text-paginator-previous">Anterior</h1></router-link>
                </div>
                <div class="right-part">
                    <router-link class="link" v-if="currentPage < lastValue" :to="nextPage(currentPage)"><h1 class="text-paginator-last">Siguiente</h1></router-link>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'SamuraiCategoryPaginationT1',
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
    primaryColor: {
        type: String,
        default: '#333333'
    },
    secondaryColor: {
        type: String,
        default: '#e9ecef'
    },
    textColor: {
        type: String,
        default: '#333333',
    },
    textColorSelected: {
        type: String,
        default: 'white',
    },
    circleButton: {
        type: Boolean,
        default: false,
    }
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
        this.$emit('send-current-page', this.currentPage);
    }
},
 watch: {
    $route(to) {
      this.currentPage = parseInt(to.query[this.pageParameter], 10) || 1;
      if(this.currentPage >= this.firstValue && this.currentPage <= this.lastValue ){
        this.$emit('send-current-page', this.currentPage);
      }
    },
 },
 computed: {
     styles(){
         var borderRadius = this.circleButton ? '200px' : '5px' ;

         return {
            '--primary-color': this.primaryColor,
            '--secondary-color': this.secondaryColor,
            '--text-color': this.textColor,
            '--text-color-selected': this.textColorSelected,
            '--button-border-radius': borderRadius,
         }
     }
 }
};
</script>

<style lang="scss" scoped>

    @import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;1,100;1,200;1,300&display=swap');

    /* Contenedor Principal*/

    .samurai-category-pagination-container{
        display: flex;
        width: 100%;
        height: 100px;
    }

    /* contenedor de las partes y el que centra */

    .samurai-category-pagination{
        width:50%;
        display: flex;
        margin: 0 auto;
    }

    /* Parte Izquierda (El botón que dice anterior) */

    .left-part{
        width:10%;
    }

    .text-paginator-previous{
        text-align: right;
        font-family: Poppins;
        font-size: 14px;
        padding-top: 8px;
        padding-bottom:8px;
        margin: 0 auto;
        font-weight: 500;
    }

    /* Los numeros de la Páginación */

    .center-part{
        margin-left: 10%;
        margin-right: 10%;
        width: 100%;
        text-align: center;
        //margin-right: 5%;
    }

    .square-container{
        padding: 0;
        margin-left: 5px;
        margin-top: 0px;
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
        border-radius: var(--button-border-radius);
        background:transparent;
        border: solid 1px transparent;
        color: var(--text-color);
    }

    .square:hover{
        background: var(--secondary-color);       
        border: solid 1px var(--secondary-color);
    }

    .square-selected{
        min-width: 34px;
        min-height: 34px;
        border-radius: var(--button-border-radius);
        background-color: var(--primary-color);
        color: var(--text-color-selected);
        border: solid 1px var(--primary-color);
    }

    .square-number{
        padding-top: 8px;
        padding-left: 5px;
        padding-right: 5px;
        margin: 0%;
        font-family: Poppins;
        font-size: 14px;
        font-weight: 500;
        text-align: center;        
    }
    /* Parte Derecha (El botón que dice siguiente) */

    .right-part{
        width: 10%;
    }

    .text-paginator-last{        
        font-family: Poppins;
        font-size: 14px;
        padding-top: 8px;
        font-weight: 500;
        text-align: left;
    }

    .phone{
        display: none;
    }

    @media (max-width:800px){
        .desktop{
            display: none;
        }

        .phone{
            display: block;
        }

        .links-phone{
            margin-top: 10%;
            display:flex;
        }

        .samurai-category-pagination{
            width: 100%;
            display:block;
        }

        .center-part{
            width: 100%;
            margin-right: 0% !important;
            margin-left: 0%;
            margin: 0 auto;
        }

        .left-part{
            width: 100%;
            margin-right: 0%;
            margin-left: 0%;
        }

        .text-paginator-previous{
            text-align: left;
            font-family: Poppins;
            font-size: 14px;
            padding-top: 8px;
            padding-bottom:8px;
            margin: 0 auto;
            font-weight: 500;
            width: 100%;
        }        

        .right-part{
            width: 100%;
            margin-right: 0%;
            margin-left: 0%;
        }

        .text-paginator-last{
            text-align: right;        
            font-family: Poppins;
            font-size: 14px;
            padding-top: 8px;
            font-weight: 500;
            width: 100%;
        }
        
    }
</style>