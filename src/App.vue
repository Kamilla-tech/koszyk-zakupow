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
          :products="products"
          @update-quantity="updateProductQuantity"
          @remove-product="removeProduct"
        /> 
        
        <OrderSummary :subTotal="subTotal" :tax="tax"/>
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
            image: "https://placehold.co/200x150",
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
        
        for (let i = 0; i < count; i++) {
          
          const newProduct = {
            id: this.availableProducts.length + 1,
            image: "https://placehold.co/200x150",
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
  .products-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 20px;
    margin-top: 10px;
  }
  
  .product-card {
    padding: 20px;
    border: 1px solid #ddd;
    text-align: center;
  }
  
  .product-card img {
    max-width: 100%;
    margin-bottom: 10px;
  }
  
  .product-card button {
    padding: 5px;
    background-color: #007bff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
    color: #fff;
  }
  
  .product-card button:hover {
    background-color: #0056b3;
  }
  
  .modal-overlay {
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.7);
  }
  
  .modal {
    position: absolute;
    padding: 20px;
    width: 80%;
    max-width: 600px;
    background-color: #fff;
    border-radius: 10px;
  }
  
  .close-modal {
    position: absolute;
    top: 10px;
    right: 10px;
    width: 20px;
    height: 20px;
    background-color: #f44336;
    color: #fff;
    border: none;
    border-radius: 50px;
    font-size: 0.8rem;
    line-height: 1;
    cursor: pointer;
  }
  
  .close-modal:hover {
    background-color: #d32f2f;
  }
</style>
