<template>
  <section class="container">
    <h3>Lista produktów w koszyku:</h3>
    <ul class="products">
      <ProductTitle
          v-for="product in products"
          :key="product.id"
          :product="product"
          @update-quantity="$emit('update-quantity', $event)"
          @remove-product="$emit('remove-product', product.id)"
      />
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
  .products {
    list-style-type: none;
    padding: 0 5px 0 0;
    margin: 0;
    max-height: 300px;
    overflow-y: auto;
  }
</style>