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
      <p>Price: {{ product.formatted.price }}</p>
      <p>Description: {{ product.description }}</p>
    </div>
  </div>
</template>

<style>
  img{
    width: 500px;
  }
  /*#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
  #nav {
    padding: 30px;
  }

  #nav a {
    font-weight: bold;
    color: #2c3e50;
  }

  #nav a.router-link-exact-active {
    color: #42b983;
  }

  #newProductForm {
    border-color: black;
    border: 20px;
  }*/
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
      newProductImageUrl: ""
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
    }
  }
};
</script>