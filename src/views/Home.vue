<template>
  <div class="home">
    <h1>New Product</h1>
    <div id="newProductForm">
      <div>
        Name: <input v-model="newProductName">
      </div>
      <div>
        Price: <input v-model="newProductPrice">
      </div>
      <div>
        Description: <input v-model="newProductDescription">
      </div>
      <div>
        Image URL: <input v-model="newProductImageUrl">
      </div>
      <button v-on:click="createProduct()">Create Product</button>
    </div>

    <h1>All Products</h1>
    <div v-for="product in products">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.name">
      <div>
        <button v-on:click="showProduct(product)">Product Info</button>
      </div>
      <div v-if="product === currentProduct">
        <p>Price: {{ product.formatted.price }}</p>
        <p>Description: {{ product.description }}</p>
        <div>
          <h4>Edit Product</h4>
          <div>
            Name: <input v-model="product.name">
          </div>
          <div>
            Price: <input v-model="product.price">
          </div>
          <div>
            Description: <input v-model="product.description">
          </div>
          <div>
            Image URL: <input v-model="product.image_url">
          </div>
          <button v-on:click="updateProduct(product)" class="btn btn-primary">Edit Product</button>
          <button v-on:click="destroyProduct(product)" class="btn btn-danger">Delete Product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
  img{
    width: 500px;
  }
</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
      currentProduct: {}
    };
  },
  created: function() {
    axios.get("/api/products")
      .then(response => {
        this.products = response.data;
      });
  },
  methods: {
    createProduct: function() {
      console.log("Create Product");
      var params = {
                    name: this.newProductName,
                    price: this.newProductPrice,
                    description: this.newProductDescription,
                    image_url: this.newProductImageUrl
                    };
      axios.post("/api/products", params)
        .then(response => {
          console.log("Success", response.data);
          this.products.push(response.data);
        });
    },
    showProduct: function(inputProduct) {
      if (this.currentProduct === inputProduct) {
        this.currentProduct = {};
      } else {
        this.currentProduct = inputProduct;
      }
    },
    updateProduct: function(inputProduct) {
      var params = {
                    name: inputProduct.name,
                    price: inputProduct.price,
                    description: inputProduct.description,
                    image_url: inputProduct.image_url
                    };
      axios.patch("/api/products/" + inputProduct.id, params)
        .then(response => {
          console.log("Success", response.data);
          // inputProduct = response.data;
        });
    },
    destroyProduct: function(inputProduct) {
      axios.delete("/api/products/" + inputProduct.id)
        .then(response => {
          console.log("Success" ,response.data);
          var index = this.products.indexOf(inputProduct);
          this.products.splice(index,1);
        });
    }
  }
};
</script>