<template>
  <li class="row">
    <div class="detail">
      <div class="name">
        {{ product.name }}
      </div>
    </div>
    <div class="price">{{ formatCurrency(product.price) }}</div>
    <div class="quantity">
      Ilość:
      <input type="number" :value="quantity" @input="updateQuantity"> 
    </div>
    <div class="total" v-if="!isNaN(quantity)">
      {{ formatCurrency(product.price * quantity) }}
    </div>
    <div class="total" v-else>
      {{ formatCurrency(0 )}}
    </div>
    <div class="remove" @click="$emit('remove-product')">X</div>
  </li>
</template>

<script>
  export default {
    props: ["product"],
    data() {
      return {
        quantity: this.product.quantity
      };
    },
    methods: {
      updateQuantity(event) {
        const newQuantity = parseInt(event.target.value);
        this.quantity = isNaN(newQuantity) ? "" : newQuantity;

        this.$emit("update-quantity", {
          id: this.product.id,
          quantity: this.quantity
        });
      },
      formatCurrency(value) {
        return Number(value).toLocaleString("pl-PL", {style: "currency", currency: "PLN"});
      }
    }
  }
</script>

<style scoped>
  .row {
    display: flex;
    flex-direction: row;
    align-items: baseline;
    justify-content: space-between;
    margin-bottom: 10px;
  }
  
  .detail {
    display: flex;
    flex-direction: row;
    align-items: center;
  }
  
  .detail .name {
    margin-right: 10px;
  }
  
  .price {
    width: 100px;
    text-align: right;
  }
  
  .quantity {
    text-align: center;
  }
  
  .quantity > input {
    width: 60px;
    text-align: center;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .total {
    width: 100px;
    text-align: right;
  }
  
  .remove {
    line-height: 1;
    text-align: center;
    cursor: pointer;
  }
</style>