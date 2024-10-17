<template>
  <div>
    <HeaderComponent :itemCount="itemCount" @toggle-cart="toggleCart"/>
    
    <div v-if="!showCart">
      <div class="products-grid">
        <div v-for="product in availableProducts"
             :key="product.id" class="product-card"
        >
          <img :src="product.image" :alt="product.name"/>
          <h3> {{ product.name }}</h3>
          <p> {{ formatCurrency(product.price) }}</p>
          <button @click="addToCart(product)">Dodaj do koszyka</button>
        </div>
      </div>
    </div>
    
    <div v-if="showCart" class="modal-overlay">
      <div class="modal">
        <button @click="toggleCart" class="close-modal">X</button>
        <ProductList
          
        /> 
      </div>
    </div>
  </div>
</template>

<script>
  import HeaderComponent from "./components/HeaderComponent.vue";
  import ProductList from "./components/ProductList.vue";
  import OrderSummary from "./components/OrderSummary.vue";
  
  export default {
    components: {
      HeaderComponent,
      ProductList,
      OrderSummary
    },
    data() {
      return {
        showCart: false,
        products: [],
        availableProducts: [
          {
            id: 1,
            image: "https://via.placeholder.com/200x150",
            name: "Produkt #1",
            price: 19.99
          }
        ]
      }
    },
    computed: {
      itemCount() {
        return this.products.reduce((total, product) => total + product.quantity, 0);
      },
      subTotal() {
        return this.products.reduce((total, product) => {
          return total + product.price * product.quantity;
        }, 0);
      },
      tax() {
        return this.subTotal * 0.19;
      }
    },
    methods: {
      generateProducts(count) {
        const names = ["computer", "flower", "monitor", "dvd", "game", "car", "book", "console", "keyboard", "telephone"];

        for (let i = 0; i < count; i++) {
          const randName = names[Math.ceil(Math.random() * names.length)];

          const newProduct = {
            id: this.availableProducts.length + 1,
            image: "https://via.placeholder.com/200x150?" + randName,
            name: `Produkt #${this.availableProducts.length + 1}`,
            price: (Math.random() * 100).toFixed(2),
          };
          
          this.availableProducts.push(newProduct);
        }
      },
      addToCart(product) {
        const exisitingProduct = this.products.find(p => p.id === product.id);
        if (exisitingProduct) {
          exisitingProduct.quantity++;
        } else {
          this.products.push({...product, quantity: 1});
        }
      },
      updateProductQuantity({ id, quantity}) {
        const product = this.products.find(p => p.id === id);
        
        if (product) product.quantity = quantity;
      },
      removeProduct(productId) {
        this.products = this.products.filter(p => p.id !== productId);
      },
      formatCurrency(value) {
        return Number(value).toLocaleString("pl-PL", { style: "currency", currency: "PLN"});
      },
      toggleCart() {
        if (this.products.length > 0) this.showCart = !this.showCart;
      }
    },
    mounted() {
      this.generateProducts(30);
    }
  }
</script>

<style scoped>

</style>
