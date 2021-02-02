<template>
    <div class="samurai-category-t1 category mb-4 mb-md-5" :style="styles">
        <div class="bannner-category-content">
            <samurai-banner-home class="banner-category-t1">
                <template v-slot:banner-template="data">
                    <swiper
                        :options="swiperOption"
                    >
                        <swiper-slide v-for="p in data.banners" :key="p.id">
                            <a :href="p.ruta" class="banner-category-t1-slide">
                                <div
                                    class="main-banner-img embed-responsive-21by9"
                                    :style="'background-image: url(' + p.banner + ');'"
                                >
                                    <div class="container-banner">
                                        <div class="container" :class="contentPosition">
                                            <div :class="textPosition">
                                                <!-- Si no han cargado banners responsive -->
                                                <h1
                                                    class="banner-title"
                                                    v-if="p.nombre"
                                                    :style="{ color: bannerTitleColor }"
                                                >
                                                    {{ p.nombre }}
                                                </h1>
                                                <h2
                                                    class="banner-subtitle"
                                                    v-if="p.subtitulo"
                                                    :style="{
                                                        color: bannerSubtitleColor
                                                    }"
                                                >
                                                    {{ p.subtitulo }}
                                                </h2>

                                                <p
                                                    class="banner-description"
                                                    v-if="p.descripcion"
                                                    v-html="p.descripcion"
                                                    :style="{
                                                        color: bannerSubtitleColor
                                                    }"
                                                ></p>
                                                <button
                                                    class="btn sm-btn sm-btn-banner"
                                                    v-if="p.boton"
                                                    :href="p.ruta"
                                                >
                                                    {{ p.boton }}
                                                </button>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </a>
                        </swiper-slide>
                        <div slot="button-prev" v-if="arrows" class="banner-category-t1-button-prev">
                            <font-awesome-icon :icon="chevronleft" class="fa-2x" />
                        </div>
                        <div slot="button-next" v-if="arrows" class="banner-category-t1-button-next">
                            <font-awesome-icon :icon="chevronright" class="fa-2x" />
                        </div>
                        <div
                            v-if="dots"
                            class="swiper-pagination"
                            slot="pagination"
                        ></div>
                    </swiper>
                </template>
            </samurai-banner-home>
        </div>
        <div class="w-100 content-bar">
            <div class="container">
                <div class="row">
                    <!-- API Breadcrumb -->
                    <div class="col-12">
                        <div class="row align-items-center justify-content-center">
                            <div class="col-12 col-md order-2 order-md-1">
                                <samurai-category-breadcrumb class="d-flex justify-content-center justify-content-md-start"></samurai-category-breadcrumb>
                            </div>
                            <!-- <div class="order-1 order-md-2 d-none d-md-block">
                                <div class="title text-center mb-0" ><h1 class="mb-0">{{ categoria.nombre }}</h1></div>
                            </div> -->
                            <div class="col-12 col-md text-center text-md-right pagination-products order-3 d-none d-md-block">
                                <!-- componente de paginación -->
                                {{ currentRange  }} 
                                de 
                                {{ filteredProducts.length }}
                                resultados
                            </div>
                        </div>
                    </div>
                </div>
            </div> 
        </div>
        <div class="container container-category">
            <div class="row">
                <div class="col-lg-3 col-12">
                    <!-- Filter -->

                    <div 
                        class="products-categoryt1-filters-wrapper mt-4 mb-4 mb-lg-0 scrollbar"
                        :class="visibleFilters ? 'open' : 'closed'"
                    >
                        <p @click="visibleFilters = !visibleFilters" class="">
                            Filtros
                            <span class="d-lg-none">
                                <chevron-down-icon />
                            </span>
                        </p>
                         <div class="d-block d-sm-none">
                                 <!-- responsivo -->
                                <p class="my-0 orden">Ordenar por:</p>
                                <SamuraiCategoryOrderBy />
                            </div>
                        <BCollapse v-model="visibleFilters" class="tabpanel" role="tabpanel">
                           
                            
                            <!-- <p class="mb-4">
                                Filtros
                            </p> -->
                            <SamuraiCategoryFilters 
                            :showFilterPrices="showFilterPrices"
                            :colorByName="colorByName"
                            /> 
                        </BCollapse>
                    </div>
                </div>

                <div class=" col-lg-9 col-12 wrapper-category" id="containerCategory">
                    <div class="row mt-4 mb-md-3">
                        
                        <div class="d-none d-xl-flex ml-auto grid-change-category col justify-content-end">
                            <div class="col d-none d-lg-flex align-items-center">
                                <div class="view ml-auto" @click="setColumns(false); setList(false)">
                                    <view-module-icon :class="{ 'highlight' : !columns && !list}"/>
                                </div>
                                <div class="view ml-3" @click="setColumns(true); setList(false)">
                                    <view-comfy-icon :class="{ 'highlight' : columns && !list}"/>
                                </div>
                                <div class="view ml-3" @click="setColumns(false); setList(true)">
                                    <table-of-contents-icon :class="{ 'highlight' : !columns && list}"/>
                                </div>
                            </div>

                            <div class="d-none d-md-flex align-items-center" >
                                 <p class="my-0 orden">Ordenar por:</p>
                                <SamuraiCategoryOrderBy />
                            </div>
                        </div>
                    </div>
                    <div>
                        <!-- Productos Categoria -->
                        <SamuraiProductsByCategory  
                            class="row" 
                            :columns="columns" 
                            :list="list"
                            :loanConfig="loanConfig"
                            :productsClass="productClases"
                            :productStyle="productsGridRowGap"
                        >
                            <template name="loader">
                                <div class="row" >
                                    <div class="col-12 col-sm-6 col-lg-4 col-xl-3 mb-5" v-for="i in [{id:1}, {id:2}, {id:3}, {id:4}]" :key="i.id">
                                        <SkeletonBox 
                                            width="100%"
                                            height="200px"
                                            class="rounded-lg"
                                        />
                                        <SkeletonBox 
                                            width="88%"
                                            height="14px"
                                            class="mt-3"
                                        />
                                        <SkeletonBox 
                                            width="45%"
                                            height="14px"
                                            class="mt-2"
                                        />
                                    </div>
                                </div>
                            </template> 
                        </SamuraiProductsByCategory>

                        <div class="d-flex justify-content-center">
                            <SamuraiCategoryPagination @pagination-changed="scrollTop" />
                        </div>
                    </div>
                </div>
                
            </div>
        </div>
        <SamuraiProductEmailLoan/>
    </div>
</template>
<script>
    import { mapState, mapGetters } from 'vuex';
    import VueScrollTo from 'vue-scrollto';
    import axios from 'axios';
    import "swiper/dist/css/swiper.css";
    import { swiper, swiperSlide } from "vue-awesome-swiper";
    import {
        faChevronRight,
        faChevronLeft
    } from "@fortawesome/free-solid-svg-icons";
    
    export default {
        components: {
            swiper,
            swiperSlide
        },
        props:{
            columns:{
                type:Boolean,
                default: false
            },
            list:{
                type:Boolean,
                default: false
            },
            breadcrumbBackground:{
                type: String,
                default: "#f2f2f2"
            },
            breadcrumbColor:{
                type: String,
                default: "#333c4a"
            },
            breadcrumbHoverColor:{
                type: String,
                default: "#333c4a"
            },
            checkboxBackground:{
                type: String,
                default: "#333333"
            },
            viewTypeButtonsActiveColor:{
                type: String,
                default: "#333333"
            },
            viewTypeButtonsColor:{
                type: String,
                default: "#dadada"
            },

            mobileFiltersButtonColor:{
                type: String,
                default: "#7f8287"
            },
            mobileFiltersButtonBorderColor:{
                type: String,
                default: "#7f8287"
            },
            mobileFiltersButtonBackground:{
                type: String,
                default: "#fff"
            },
            mobileFiltersButtonHoverColor:{
                type: String,
                default: "#fff"
            },
            mobileFiltersButtonHoverBackground:{
                type: String,
                default: "#333c4a"
            },
            mobileFiltersButtonHoverBorderColor:{
                type: String,
                default: "#333c4a"
            },
            showFilterPrices:{
				type:Boolean,
				default:true
            },
            loanConfig:{
                title:"Configuracion para q los productos no se dirijan al carro de compra sino al modal de prestamo",
                type:Boolean,
                default:false
            },
			colorByName:{
				type:Boolean,
				default:false
			},

            //Banner

            alignX: {
                type: String,
                default: "center"
            },
            alignY: {
                type: String,
                default: "center"
            },
            bannerTitleColor: {
                type: String,
                default: "white"
            },
            bannerSubtitleColor: {
                type: String,
                default: "white"
            },
            textAlign: {
                type: String,
                default: ""
            },
            dots: {
                type: Boolean,
                default: false
            },
            arrows: {
                type: Boolean,
                default: false
            }           
        },
        data(){
            return{
                visibleFilters: true,
                APP_URL: process.env.MIX_APP_URL,
                swiperOption: {
                    slidesPerView: 1,
                    spaceBetween: 0,
                    navigation: {
                        nextEl: ".banner-category-t1-button-next",
                        prevEl: ".banner-category-t1-button-prev"
                    },
                    pagination: {
                        el: ".swiper-pagination"
                    }
                }
            }
        },
        computed:{
            ...mapState({
                general: 'general',
                categories: 'categories',
                categoria(state){

                    return state.categories.all.find(category => category.id == state.general.idMenu)
                },
                currentPage(state){
                    return state.products.currentPage
                },
                itemsPerPage(state){
                    return state.products.itemsPerPage
                },
                currentRange(state){

                    let from = ((this.currentPage * this.itemsPerPage) - this.itemsPerPage) + 1;
                    let to = this.currentPage * this.itemsPerPage;           

                    if(to > this.filteredProducts.length){
                        to = this.filteredProducts.length;
                    }

                    return `${from} - ${to}`;
                },
                productClases(state){
                    
                    let classes = '';
                    let productsCategoryOptions = state.general.globalJson.productsCategory;
                    
                    if(productsCategoryOptions.props && productsCategoryOptions.props.resposiveGridColumns){
                        classes += (productsCategoryOptions.props.resposiveGridColumns == 2) ? 'col-6' : 'col-12';
                    }

                    if(!this.list && !this.columns){
                        classes += ' d-flex  col-xl-4 col-sm-6 col-md-6 col-lg-6'
                    }
                    
                    if(!this.list && this.columns){
                        classes += ' d-flex col-sm-6 col-md-6 col-lg-6 col-xl-3'
                    }
                    
                    if(this.list && !this.columns){
                        classes += ' col-12 listed mb-4 bg-light pl-0'
                    }
                    
                    return classes;
                },
                productsGridRowGap(state){
                    let productsCategoryOptions = state.general.globalJson.productsCategory;
                    
                    if(productsCategoryOptions.props && productsCategoryOptions.props.gridGap){
                        return 'margin-bottom: ' + productsCategoryOptions.props.gridGap;
                    }

                    return 'margin-bottom: 1.5rem';
                }
            }),
            ...mapGetters({
                filteredProducts: 'filteredProducts',
                paginatedProducts: 'paginatedProducts'
            }),
            styles(){

                return `
                    --pc-t1-view-type-color: ${this.viewTypeButtonsColor};
                    --pc-t1-view-type-active-color: ${this.viewTypeButtonsActiveColor};
                    --pc-t1-checkbox-background: ${this.checkboxBackground};
                    --pc-t1-breadcrumb-background: ${this.breadcrumbBackground};
                    --pc-t1-breadcrumb-color: ${this.breadcrumbColor};
                    --pc-t1-breadcrumb-hover-color: ${this.breadcrumbHoverColor};

                    --pc-t1-mobile-filters-button-color: ${this.mobileFiltersButtonColor};
                    --pc-t1-mobile-filters-button-border-color: ${this.mobileFiltersButtonBorderColor};
                    --pc-t1-mobile-filters-button-background: ${this.mobileFiltersButtonBackground};
                    --pc-t1-mobile-filters-button-hover-color: ${this.mobileFiltersButtonHoverColor};
                    --pc-t1-mobile-filters-button-hover-background: ${this.mobileFiltersButtonHoverBackground};
                    --pc-t1-mobile-filters-button-hover-border-color: ${this.mobileFiltersButtonHoverBorderColor};
                `;
            },
            //Banner
            chevronright() {
                return faChevronRight;
            },
            chevronleft() {
                return faChevronLeft;
            },
            contentPosition() {
                let result = "";
                switch (this.alignY) {
                    case "top":
                        result += "justify-content-start ";
                        break;

                    case "center":
                        result += "justify-content-center ";
                        break;

                    case "bottom":
                        result += "justify-content-end ";
                        break;

                    default:
                        result += "justify-content-center ";
                        break;
                }
                switch (this.alignX) {
                    case "left":
                        result += "align-items-start ";
                        break;

                    case "center":
                        result += "align-items-center ";
                        break;

                    case "right":
                        result += "align-items-end ";
                        break;

                    default:
                        result += "align-items-center ";
                        break;
                }
                return result;
            },
            textPosition() {
                let result = "";
                switch (this.textAlign) {
                    case "left":
                        result += "text-left ";
                        break;

                    case "center":
                        result += "text-center ";
                        break;

                    case "right":
                        result += "text-right ";
                        break;

                    default:
                        result += "";
                        break;
                }
                return result;
            }
        },
        mounted(){
            if(window.innerWidth < 992){
                setTimeout(()=>{
                    this.visibleFilters = false;
                }, 1);
            }
        },
        methods:{
            scrollTop(){
                VueScrollTo.scrollTo('#containerCategory', 800, {
					container: 'body',
				    easing: 'ease-in-out',
				    offset: -100,
				    force: true,
				    cancelable: true,
				    x: false,
				    y: true
				});
            },
            setColumns(value){
                this.columns = value;
            },
            setList(value){
                this.list = value;
            }
        },
        created() {
            this.$store.dispatch("getBanners");
        },
    }
</script>
<style lang="scss">
    .samurai-category-t1 .content-bar {
        background-color: var(--pc-t1-breadcrumb-background);
        padding: 4px 0px;
    }

    .samurai-category-t1 .title h1{
        font-size: 1.125rem;
        letter-spacing: 1.08px;
        text-transform: capitalize;
        color: #333c4a;
        font-weight: 600;
    }
    .samurai-category-t1 .pagination-products {
         font-size: 12px;
         font-weight: 500;
         font-stretch: normal;
         font-style: normal;
         line-height: 1.67;
         letter-spacing: 0.66px;
         color: #333c4a;
    }

    .samurai-category-t1 .samurai-pagination {
        font-size: 0.8rem;
    }
    
    .samurai-category-t1 .samurai-pagination .page-item {
        margin: 0 2px;
    }

    .samurai-category-t1 .samurai-pagination .page-link{
        color: var(--pc-t1-breadcrumb-color);
        padding: 0.5rem .53rem;
        border: 0;
        border-radius: 5px;
        transition: background-color .3s ease, color .3s ease;
    }

    .samurai-category-t1 .page-item.active .page-link{
        background-color: var(--pc-t1-breadcrumb-hover-color);
        color: #fff;
    }

    .samurai-category-t1 .page-item.disabled{
        display: none;
    }

    .samurai-category-t1 .pagination-next-item,
    .samurai-category-t1 .pagination-prev-item{
        margin: 0;
        
        a{
            padding: 0.5rem 0.5rem;
        }
    }

    .grid-change-category{
        p, select{
            font-size: 12px;
            font-weight: 500;
            font-stretch: normal;
            font-style: normal;
            line-height: 2.92;
            letter-spacing: 0.72px;
            color: #9b9b9b;
        }  
    }


    .samurai-category-t1 .view{
        color: var(--pc-t1-view-type-color) ;
        position: relative;
    }
    .samurai-category-t1 .orden {
        color: #9b9b9b;
    }
    .samurai-category-t1 .view svg{
        height: 1.75rem;
        width: 1.75rem;
        transition: all .3s ease;
    }
    .samurai-category-t1 .view > .highlight{
        color: var(--pc-t1-view-type-active-color);
    }
    .samurai-category-t1 .table-of-contents-icon svg{
        transform: scale(-1);
    }
    
    .samurai-category-t1 .select-filter .vs__dropdown-toggle{
        border: none;
        outline: none;
    }

    // .samurai-category-t1 .select-filter .vs--unsearchable .vs__search{
    //     display: none;
    // }

    .samurai-category-t1 .select-filter .vs__selected{
        width: 98px;
        font-size: 12px;
        font-weight: 500;
        font-stretch: normal;
        font-style: normal;
        line-height: 2.92;
        letter-spacing: 0.72px;
    }

    .samurai-category-t1 .select-filter .vs__actions{
        padding: 4px 0 0 0px;
    }

    .samurai-category-t1 .select-filter .vs--unsearchable .vs__search{
        padding: 0;
    }

    .samurai-category-t1 .select-filter .vs--single.vs--open .vs__selected{
        position: relative;
    }

    .samurai-category-t1 .select-filter .vs__dropdown-menu li{
        font-size: 0.8rem;
        font-weight: 500;
    }

    .samurai-category-t1 .custom-control-input:checked~.custom-control-label::before {
        border-color: var( --pc-t1-checkbox-background);
        background-color: var( --pc-t1-checkbox-background);
    }

    .samurai-category-t1 .custom-control-input:focus~.custom-control-label::before{
        box-shadow: none;
    }

    .samurai-category-t1 .products-categoryt1-filters-wrapper{
		position: sticky;
	    top: 18vh;
        max-height: 68vh;
        overflow-y: auto; 
    }
    //Titulo Filtro 
    .products-categoryt1-filters-wrapper > p{
        font-size: 14px;
        font-weight: 600;
        text-transform: uppercase;
        font-stretch: normal;
        font-style: normal;
        line-height: 2.5;
        letter-spacing: 0.84px;
        color: #333333;

        > span{
            transform-origin: center;
            transition: transform .3s ease-out;
        }
    }

    .products-categoryt1-filters-wrapper.open > p > span{
        transform: rotate(-180deg); 
    }

    @media (min-width: 350px) {

        .samurai-category-t1 .samurai-pagination {
            font-size: 0.9rem;
        }

        .samurai-category-t1 .samurai-pagination .page-link{
            padding: 0.5rem .6rem;
        }
    }
    
    @media (min-width: 425px) {

        .samurai-category-t1 .samurai-pagination .page-link{
            padding: 0.5rem .83rem;
        }

        .samurai-category-t1 .pagination-next-item,
        .samurai-category-t1 .pagination-prev-item{
            a{
                padding: 0.5rem 0.83rem;
            }
        }

        .samurai-category-t1 .samurai-pagination .page-item {
            margin: 0 5px;
        }
    }

    
	@media (min-width: 992px){
		 .samurai-category-t1 .products-categoryt1-filters-wrapper {
			max-width: 16rem;
			padding-right: 1rem;
        }
        .products-categoryt1-filters-wrapper > p{
            pointer-events: none;
        }
    }
    
    // Ajuste de boton de filtros
    @media (max-width: 991px){

        .products-categoryt1-filters-wrapper > p{
            padding: 0.8rem 0.7rem 0.7rem 1rem;
            border-radius: 5px;
            transition: color 0.3s ease, background-color 0.3s ease, border 0.3s ease, border-color 0.3s ease, border 0.3s ease;
	        font-size: 0.88rem;
            font-weight: 500;
            line-height: normal;
            letter-spacing: 1.25px;

            border: 1px solid var( --pc-t1-mobile-filters-button-border-color) ;
            background-color: var( --pc-t1-mobile-filters-button-background) ;
            color: var( --pc-t1-mobile-filters-button-color) ;

            &:hover{
                background-color: var( --pc-t1-mobile-filters-button-hover-background) ;
                border-color: var( --pc-t1-mobile-filters-button-hover-border-color);
                color: var( --pc-t1-mobile-filters-button-hover-color) ;
            }
        }
    }

    //Banner
.samurai-category-t1 {
    .banner-category-t1 .main-banner-img {
        width: 100%;
        padding-top: 25%;
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
    }
    .banner-category-t1 .container {
        height: 100%;
        display: flex;
        flex-direction: column;
    }

    .banner-category-t1 .banner-title {
        line-height: 1;
    }

    .banner-category-t1 .banner-subtitle {
        font-size: 1.75rem;
    }
    .banner-category-t1 .container-banner {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }
    .banner-category-t1 .banner-description,
    .banner-category-t1 .banner-description p {
        margin-bottom: 0.25rem;
    }
    .banner-category-t1-button-next,
    .banner-category-t1-button-prev {
        position: absolute;
        bottom: 20px;
        color: #fff;
        z-index: 1;
    }

    .banner-category-t1-button-prev{
        left: 24px;
    }

    .banner-category-t1-button-next{
        left: 80px;
    }

    .banner-category-t1-button-prev:hover,
    .banner-category-t1-button-next:hover,
    .swiper-button-disabled {
        opacity: 0.5;
    }


    @media (min-width: 576px) {

        .banner-category-t1-button-next,
        .banner-category-t1-button-prev {
            top: 50%;
            bottom: unset;
        }

        .banner-category-t1-button-next {
            left: unset;
            right: 3%;
            transform: translateX(50%);
        }
        .banner-category-t1-button-prev {
            left: 3%;
            transform: translateX(-50%);
        }

        .banner-category-t1 .main-banner-img {
            padding-top: 70%;
        }
    }

    @media (min-width: 1200px) {
        .banner-category-t1 .main-banner-img {
            padding-top: 10%;
        }
        .v-select .vs__dropdown-option{
            padding: 8px 30px;
        }
 
        .v-select .vs__dropdown-menu{
            width: 188px;
            height: 223px;    
        }
    }
}
.v-select .vs__dropdown-option--highlight{
    background: #ffffff !important;
    color: #1d70d3 !important;
  
}


</style>
