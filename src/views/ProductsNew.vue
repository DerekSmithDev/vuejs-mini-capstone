<template>
  <div class="home">
    <div class="container">
       <h1>Add new product</h1>
      <ul>
        <li v-for="error in errors" class="text-danger">{{error}}</li>
      </ul>
      Name: <input v-model="newProductName" type="text">
      Chef: <input v-model="newProductPrice" type="text">
      Ingredients: <input v-model="newProductDescription" type="text">
      Directions: <input v-model="newProductSupplierId" type="text">
      <button v-on:click="createProduct()" class="btn btn-primary">Create</button>
     </div>
  </div>
</template>
 <style>
</style>
 <script>
var axios = require("axios");
 export default {
  data: function() {
    return {
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductSupplierId: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
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
            this.newProductName = "";
            this.newProductPrice = "";
            this.newProductDescription = "";
            this.newProductSupplierId = "";
            this.$router.push("/");
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

<!-- newProductName
newProductPrice
newProductDescription
newProductSupplierId -->