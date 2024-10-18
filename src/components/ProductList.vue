<template>
  <section class="container">
    <h3>Lista produktów w koszyku:</h3>
    <ul class="products">
      
    </ul>
  </section>
</template>

<script>
  import ProductTitle from "./ProductTitle.vue";
  
  export default {
    components: {
      ProductTitle
    },
    props: ["products"]
  }
</script>

<style scoped>

</style>