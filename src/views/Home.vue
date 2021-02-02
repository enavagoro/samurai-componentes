<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <PlasticinaProducts />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import PlasticinaProducts from '@/components/PlasticinaProducts.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld,
    PlasticinaProducts,
  },
  /*
  watch:{
    $route(to){
      this.$refs.console.value += `\nRoute Change detected: page: ${to.query.page} / tweetPages: ${to.query.tweetPages}`;
    }
  }
  */
}
</script>
