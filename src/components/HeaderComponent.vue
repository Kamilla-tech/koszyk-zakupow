<template>
  <header class="container">
    <span class="count">
      Ilość produktów w koszyku:
      {{ itemCount}}
    </span>
    <button @click="$emit('toggle-cart')"><font-awesome-icon :icon="['fas', 'cart-shopping']" /></button>
  </header>
</template>

<script>
  export default {
    props: {
      itemCount: {
        type: Number,
        required: true
      }
    }
  }
</script>

<style scoped>
  .container {
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
  }
  
  .count {
    margin-right: 10px;
    color: #333;
  }
</style>