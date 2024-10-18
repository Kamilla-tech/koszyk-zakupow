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
      <input type="number" :value="qunatity" @input="updateQunatity"> 
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
        this.quantity = isNaN(newQunatity) ? "" : newQunatity;

        this.$emit("update-qunatity", {
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
  
  
</style>