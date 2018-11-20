<template>
  <div class="home">
    <div class="container">
      <div class="jumbotron">
        <h1 class="display-4">Hello, world!</h1>
        <p class="lead">This is a simple hero unit, a simple jumbotron-style component for calling extra attention to featured content or information.</p>
        <hr class="my-4">
        <p>It uses utility classes for typography and spacing to space content out within the larger container.</p>
        <a class="btn btn-primary btn-lg" href="#" role="button">Learn more</a>
      </div>

      <h1>Add new recipe</h1>
      <button v-on:click="createRecipe()" class="btn btn-primary">Create</button>

      <div class="row">
        <div v-for="recipe in recipes" class="col-md-4 mb-2">
          <div class="card">
            <img class="card-img-top" v-bind:src="recipe.image_url" alt="Card image cap">
            <div class="card-body">
              <h5 class="card-title">{{ recipe.title }}</h5>
              <p class="card-text">Chef: {{ recipe.chef }}</p>
              <p class="card-text">Ingredients: {{ recipe.ingredients }}</p>
              <a href="#" class="btn btn-primary">Go somewhere</a>
            </div>
          </div>
        </div>
      </div>
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
      message: "Welcome to Vue.js!",
      recipes: []
    };
  },
  created: function() {
    axios.get("http://localhost:3000/api/recipes").then(
      function(response) {
        console.log(response.data);
        this.recipes = response.data;
      }.bind(this)
    );
  },
  methods: {
    createRecipe: function() {
      console.log("createRecipe");
      var params = {
        input_title: "Test title",
        input_chef: "Test chef",
        input_ingredients: "Test ingredients",
        input_directions: "Test directions"
      };
      axios.post("http://localhost:3000/api/recipes", params).then(
        function(response) {
          console.log(response);
          this.recipes.push(response.data);
        }.bind(this)
      );
    }
  },
  computed: {}
};
</script>
