<template>
  <div class="home">
    <div class="container">
<!--  -->
      <div class="jumbotron">
        <h1 class="display-4">Hello, world!</h1>
        <p class="lead">This is a simple hero unit, a simple jumbotron-style component for calling extra attention to featured content or information.</p>
        <hr class="my-4">
        <p>It uses utility classes for typography and spacing to space content out within the larger container.</p>
        <a class="btn btn-primary btn-lg" href="#" role="button">Learn more</a>
      </div>
<!--  -->
      <h1>Add new product</h1>
      <ul>
        <li v-for="error in errors" class="text-danger">{{error}}</li>
      </ul>
      Name: <input v-model="newProductName" type="text">
      Price: <input v-model="newProductPrice" type="text">
      Description: <input v-model="newProductDescription" type="text">
      SupplierId: <input v-model="newProductSupplierId" type="text">
      <button v-on:click="createProduct()" class="btn btn-primary">Create</button>
<!--  -->
      <div>
        <button v-on:click="setSortAttribute('name')" class="btn btn-secondary">Sort by name</button>
        <button v-on:click="setSortAttribute('price')" class="btn btn-secondary">Sort by price</button>
      </div>
      <h1>Search products</h1>
      <input type="text" v-model="searchFilter" list="names">
      <datalist id="names">
        <option v-for="product in products">{{ product.name }}</option>
      </datalist>
      <div class="row" is="transition-group" name="slide-right">
        <div v-for="product in orderBy(filterBy(products, searchFilter, 'name', 'price'), sortAttribute, sortOrder)" class="col-md-4 mb-2" v-bind:key="product.id">
            <div class="card">
            <img class="card-img-top" v-bind:src="product.images[0]" alt="Card image cap">
            <div class="card-body">
              <h5 class="card-title">{{ product.name }}</h5>
              <p class="card-text">Supplier: {{ product.supplier["name"] }}</p>
              <p class="card-text">Price: {{ product.price }}</p>
              <a v-bind:href="`/#/products/${product.id}`" class="btn btn-primary">Go somewhere</a>
            </div>
          </div>
        </div>
      </div>
<!--  -->
    </div>
  </div>
</template>


<style>
  /* Vue.js fade */
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s
  }
  .fade-enter, .fade-leave-to {
    opacity: 0
  }

  /* Vue.js slide-right */
  .slide-right-enter-active {
    transition: all .3s ease;
  }
  .slide-right-leave-active {
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-right-enter, .slide-right-leave-to {
    transform: translateX(10px);
    opacity: 0;
  }

  /* Vue.js slide-left */
  .slide-left-enter-active {
    transition: all .3s ease;
  }
  .slide-left-leave-active {
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-left-enter, .slide-left-leave-to {
    transform: translateX(-10px);
    opacity: 0;
  }
</style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductSupplierId: "",
      searchFilter: "",
      sortAttribute: "name",
      sortOrder: 1,
      errors: []
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/products").then(
      function(response) {
        console.log(response.data);
        this.products = response.data;
      }.bind(this)
    );
  },
  methods: {
    setCurrentProduct: function(inputProduct) {
      this.setCurrentProduct = inputProduct;
    },
    setSortAttribute: function(inputAttribute) {
      if (this.sortOrder === 1) {
        this.sortOrder = -1;
      } else {
        this.sortOrder = 1;
      }
      this.sortAttribute = inputAttribute;
    },
    createProduct: function() {
      console.log("createProduct");
      this.errors = [];
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        supplier_id: this.newProductSupplierId
      };
      axios
        .post("http://localhost:3000/api/products", params)
        .then(
          function(response) {
            console.log(response);
            this.product.push(response.data);
            this.newProductName = "";
            this.newProductPrice = "";
            this.newProductDescription = "";
            this.newProductSupplierId = "";
          }.bind(this)
        )
        .catch(
          function(error) {
            console.log(error.response.data.errors);
            this.errors = error.response.data.errors;
          }.bind(this)
        );
    }
  },
  computed: {}
 };
 </script>