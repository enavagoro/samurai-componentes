<template>
    <div class="contenedor-plasticina">
        <router-link v-if="currentPage > 1" :to="previusPage(currentPage)">anterior</router-link>
        <h1>pagina: {{currentPage }} / {{ totalPages }}</h1>
        <router-link v-if="currentPage < this.totalPages" :to="nextPage(currentPage)">siguiente</router-link>
    </div>
</template>

<script>
export default {
  name: 'PlasticinaPagination',
  data() {
    return {
      currentPage: 1,
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
    previusPage(currentPage) {
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
 },
 mounted(){
    this.currentPage = parseInt(this.$route.query[this.pageParameter], 10) || 1;
 },
 watch: {
    $route(to) {
      console.log('esta es la query que observamos arriba',to);
      this.currentPage = parseInt(to.query[this.pageParameter], 10) || 1;
    },
 },

};
</script>