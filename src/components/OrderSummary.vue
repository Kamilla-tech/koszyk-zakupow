<template>
 <section class="container">
   <div class="summary">
     <ul>
       <li>
         Cała kwota: <span>{{ formatCurrency(subTotal) }}</span>
       </li>
       <li>
         Podatek: <span>{{ formatCurrency(tax) }}</span>
       </li>
       <li class="total">
         Podsumowanie: <span> {{ formatCurrency (subTotal + tax) }}</span>
       </li>
     </ul>
   </div>
 </section>
</template>

<script>
  export default {
    name: "OrderSummary",
    props: ["subTotal", "tax"],
    methods: {
      formatCurrency(value) {
        return Number(value).toLocaleString("pl-PL", {style:"currency", currency: "PLN"});
      }
    }
  }
</script>

<style scoped>
  .summary {
    text-align: right;
  }
  
  .summary ul {
    list-style: none;
  }
  
  .summary ul li {
    padding: 0.5rem 0;
  }
  
  .summary ul li span {
    display: inline-block;
    width: 30%;
  }
  
  .summary ul li.total {
    font-weight: bold;
  }
  
</style>